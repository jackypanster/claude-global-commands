---
allowed-tools: Bash, Read, Write
description: Save a prompt to personal prompt vault with auto-classification
---

# Save Prompt to Vault

Load and follow the `prompt-manager` skill instructions for the **Save** function.

**Vault location**: `~/workspace/prompt-vault/`

## Current vault structure
!`ls -1 ~/workspace/prompt-vault/ | grep -v "^\." | grep -v "README"`

## Task

Save the following prompt to the vault:

$ARGUMENTS

## Workflow

1. Analyze the prompt content
2. Extract: title, author (if present), source (if present), tags (3-5)
3. Determine appropriate category based on content
4. Generate filename from title (use prompt's language)
5. Write file with YAML frontmatter
6. Git sync to remote (MANDATORY):
   - `git add "[category]/[filename].md"`
   - `git commit -m "Add: [title]"`
   - `git pull --rebase && git push`
7. Confirm to user with sync status
