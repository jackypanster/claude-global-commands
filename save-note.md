---
allowed-tools: Bash, Read, Write, AskUserQuestion
description: Save a note to personal knowledge base with Johnny.Decimal classification
---

# Save Note to Knowledge Base

Load and follow the `note-manager` skill instructions for the **Save** function.
Also load `obsidian-markdown` skill for Obsidian syntax reference.

**Knowledge base location**: `~/workspace/notes/`

## Current structure
!`ls -1 ~/workspace/notes/`

## Task

Save the following content as a note:

$ARGUMENTS

## IMPORTANT: Git Sync (Mandatory)

After saving the note, MUST sync to remote:
```bash
cd ~/workspace/notes && git add -A && git commit -m "note: add/update [JD-ID] [标题]" && git pull --rebase && git push
```
