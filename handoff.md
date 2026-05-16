---
description: Bridge sessions across days/machines. WRITE (default) or RESUME ('resume' arg).
argument-hint: [resume]
allowed-tools: Bash, Read, Write, Edit
---

You are implementing the `handoff` skill defined at github.com/jackypanster/meta-skills/blob/main/handoff/README.md.
**Read the invariants carefully. Drop any → skill degrades to free-text journal.**

## Mode dispatch

- `$ARGUMENTS` empty → **WRITE**
- `$ARGUMENTS` == `resume` → **RESUME**
- Anything else → reject: `valid args: '' (WRITE) or 'resume'`

## Path resolution (both modes)

```
PROJECT_ROOT = $(git rev-parse --show-toplevel 2>/dev/null)
PROJECT = $(basename "$PROJECT_ROOT")  # fallback: "default" if no git
STORE = ~/workspace/meta-memory
HANDOFF = $STORE/$PROJECT/HANDOFF.md
ARCHIVE = $STORE/$PROJECT/archive
```

If `$STORE` missing → fail with: `Handoff store missing at $STORE. Either: (a) gh repo clone jackypanster/meta-memory, or (b) git init $STORE for local-only mode.`

## WRITE

1. If `$HANDOFF` exists: read it, preserve unfinished P1/P2 (carry-over). `✓`-marked items drop.
2. Collect signals in this order:
   - existing `$HANDOFF`
   - `git -C $PROJECT_ROOT log --since='12 hours ago' --oneline`
   - `git -C $PROJECT_ROOT status --short`
   - current session context (tasks done, blockers, paths touched, decisions made, machines/services involved)
   - any runtime task surface (TaskList state, todo state)
3. Draft new content per **Document schema** below.
4. Show diff vs existing.
5. `mkdir -p $STORE/$PROJECT && write $HANDOFF`.
6. `git -C $STORE add -A && git -C $STORE commit -m "handoff: $PROJECT $(date +%F)"`.
7. `git -C $STORE push` — best effort:
   - success → print `pushed`
   - failure (any reason) → print `WARN: push failed — N unpushed commits` where `N=$(git -C $STORE rev-list @{u}..HEAD --count 2>/dev/null)`. **NEVER fail WRITE on push error** (invariant 6).
8. If `$STORE` has no remote configured (`git -C $STORE remote -v` empty): commit locally and print `saved locally only — no remote configured`.

## RESUME

1. If `$STORE` has remote configured:
   - `git -C $STORE fetch` → on network error → print `WARN: offline, using local copy`, skip pull, continue to step 2.
   - `git -C $STORE merge --ff-only origin/$(git -C $STORE rev-parse --abbrev-ref HEAD)` → on ff-only failure → **STOP**, print exactly:
     ```
     STOP: cross-host divergence at $HANDOFF.
     Local commits since fork: git -C $STORE log @{u}..HEAD --oneline
     Remote commits since fork: git -C $STORE log HEAD..@{u} --oneline
     Manual resolution required. Do not retry /handoff resume until merged.
     ```
2. If `$HANDOFF` missing → print: `No handoff for $PROJECT. Run /handoff (no args) at end of day to write one.` Stop.
3. Output **exactly** this shape (no preamble, no postamble, no markdown fence):
   ```
   Yesterday: <one sentence — most consequential item from "What got done today">
   Open: P0 <n>, P1 <n>, P2 <n>  (✓ done not counted)
   First P0: <title>
     → <first concrete action: cmd / path / file>
     Blocked? <yes/no — only if waiting on external resource>
   Start?
   ```
4. **After user confirms `Start?`**, run cross-project surface (invariant 11 — borrow/deploy needs explicit approval):
   - Enumerate other projects' HANDOFFs ≤48h:
     ```
     find "$STORE" -mindepth 2 -maxdepth 2 -name HANDOFF.md \
       -not -path "*/$PROJECT/*" \
       -newermt "48 hours ago"
     ```
   - For each match: read "What got done today" (first 1–2 bullets).
   - Judge **borrow / deploy value for THIS machine** using:
     - Machine role from `~/.claude/SOUL.md` (if present) + multi-machine topology in `~/.claude/projects/<slug>/memory/MEMORY.md`
     - Local presence check (grep / file test) — skip if equivalent already installed
     - Role mismatch — skip if item is for a different role (e.g. GPU-only item on a non-GPU host)
   - If ≥1 candidate survives, output exactly:
     ```
     Cross-project (last 48h):
       [1] <project>: <one-line outcome> — <concrete action: clone X / install skill Y / port pattern Z>
       [2] ...
     Approve (e.g. "1,3" / "skip" / "all"):
     ```
   - Wait for user response. Execute selected `[N]` actions. Skipped candidates drop — next RESUME resurfaces them if still active in source HANDOFFs.
   - If zero candidates: print nothing, proceed to step 5.
5. **Drop into the first own-project P0 without asking.** Stop only if the first action involves: key rotation, destructive migration (DROP TABLE, rm -rf shared paths, force-push to main), or paid API spend. Those need explicit human auth.

## Document schema (WRITE follows this exactly)

```markdown
# HANDOFF — <today YYYY-MM-DD> → <tomorrow YYYY-MM-DD>

## What got done today

1. **<topic / ticket / artifact>** — <one-line outcome with concrete path or URL>
2. ...

## Tomorrow's TODO

### P0 — must-do first thing  (≤3 items, ~30min each)

1. **<task>** — <concrete first action: command, file, decision>
   - <sub-bullet only if multi-step>

### P1 — same day if possible

### P2 — nice to have / cleanup / future

## Key paths

\`\`\`
<machine-or-host> (<user>, <how-to-connect>):
  <path>                <one-line purpose>

Uncommitted:
  <path>                <action needed>
\`\`\`

## Open questions

- <falsifiable question with concrete next-step trigger>
```

Section headers can be in any language — pick one and keep it stable across handoffs in the same project so grep continuity holds.

## Invariants (drop any → skill degrades to free-text journal)

1. **P0 ≤ 3.** Demote excess to P1.
2. **Concrete action, not category.** Bad: `API key 安全`. Good: `Rotate NVIDIA key — revoke nvapi-Mzc... in NGC console, gen new, write ~/.hermes/.env`.
3. **Carry-over:** items not `✓` flow into tomorrow at same priority; `✓` items drop.
4. **Paths block includes machine/user/connect-method when remote.**
5. **Open questions are falsifiable** (trigger condition or deadline).
6. **Local-first, sync best-effort.** Local commit is the durable boundary, always runs. WRITE: push failure → warn never fail. RESUME: network failure → fall back to local; ff-only divergence → STOP, never auto-merge.
7. **RESUME drops into action without asking** (except human-auth listed above).
8. **One HANDOFF.md per project root.**
9. **Runtime-agnostic format.** No `.claude/` `.codex/` `~/.hermes/` paths in the HANDOFF.md itself or its store location.

## Anti-patterns

| Don't | Do |
|---|---|
| Free-text reflection ("today felt productive") | Concrete outcomes with artifact paths |
| `TODO: Continue working on X` | `TODO: Run score-research.sh, threshold ≥80%, write out/score.json` |
| Paths buried in prose | Dedicated `Key paths` block with machine + user + connect method |
| Silently overwrite user's hand edits | Read existing first, preserve unchanged sections, ask if unsure |
| Store under `.claude/HANDOFF.md` etc. | `~/workspace/meta-memory/<project>/HANDOFF.md` |
| Fail WRITE because `git push` failed | Local commit is durable; push failure is warning |
| Refuse RESUME when offline | Fall back to local; local is always readable |
| Auto-merge / rebase when ff-only fails | Stop, surface — divergence needs human inspection |
| Long preamble in RESUME output | Six lines exactly, ending with `Start?` |

## Lifecycle (archive)

When ALL P0/P1 items are `✓` (typically 1–3 days), next WRITE moves the current file to `$ARCHIVE/<YYYY-MM-DD>.md` and starts fresh. Archive is for grep, not human reading.

## Inline checkpointing

As P0 items finish during the day, prepend `✓` to the bullet inline. Don't rewrite the file mid-day — tomorrow's WRITE rebuilds.
