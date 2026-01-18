---
allowed-tools: Bash, Read, Write
description: Save a prompt and star it. Trigger: "save and star prompt", "保存并收藏prompt"
---

# Save and Star Prompt

Load and follow the `prompt-manager` skill instructions for the **Save** function.
Also load `obsidian-markdown` skill for Obsidian syntax reference.

**Vault location**: `~/workspace/prompt-vault/`

**IMPORTANT**: When saving the prompt, set `starred: true` in the frontmatter.

## Current vault structure
!`ls -1 ~/workspace/prompt-vault/ | grep -v "^\." | grep -v "README"`

## Task

Save the following prompt to the vault and mark it as starred:

$ARGUMENTS

## Workflow

1. Analyze the prompt content
2. Extract: title, author (if present), source (if present), tags (3-5)
3. Determine appropriate category based on content
4. Generate filename from title (use prompt's language)
5. Write file with YAML frontmatter (include `starred: true`)
6. Git sync to remote (MANDATORY):
   - `git add "[category]/[filename].md"`
   - `git commit -m "Add: [title] ⭐"`
   - `git pull --rebase && git push`
7. Confirm save location to user with sync status
