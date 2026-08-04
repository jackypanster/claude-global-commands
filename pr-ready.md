---
allowed-tools: Bash(git status), Bash(git diff), Bash(npm test), Bash(gh pr create), Bash(gh pr merge)
description: "Create PR and merge to main branch with essential checks. Trigger: create PR, 提PR, merge to main. Do NOT use for: code review (use code-review-fix), diff inspection only."
---

## PR Creation & Merge Workflow

**Current Branch:** !`git branch --show-current`
**Target:** $ARGUMENTS (default: main)

### Pre-PR Checks
1. Code quality: !`npm test && npm run lint`
2. Changes summary: !`git diff origin/main --stat`
3. Commit history: !`git log origin/main..HEAD --oneline`

### Create & Merge PR
1. **Create PR**: !`gh pr create --base main --fill`
2. **Auto-merge**: !`gh pr merge --auto --squash` (or --merge/--rebase)

### Output
- PR URL
- Merge status
- Brief change summary
