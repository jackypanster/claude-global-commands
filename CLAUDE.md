# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This is the Claude Code commands directory (`/Users/user/.claude/commands`) containing custom slash command definitions. Each markdown file defines a command that can be invoked in Claude Code sessions.

## Command Structure

Commands are defined as markdown files with YAML frontmatter specifying:
- `description`: Brief command description
- `allowed-tools`: Specific tools the command can use (optional)

The command body contains instructions in Chinese describing the command's functionality and execution steps.

## Available Commands

- `/test-and-fix` - Run tests and automatically fix failures (supports npm test, npm run test:*, npm run lint, npm run fix:*)
- `/debug-error` - Debug and fix errors in the codebase
- `/refactor` - Refactor code following best practices
- `/optimize` - Optimize code performance
- `/security-review` - Perform security analysis
- `/docs` - Generate or update documentation

## Command Development

When creating new commands:
1. Use markdown format with YAML frontmatter
2. Include clear descriptions of the command purpose
3. Specify allowed-tools if command needs restricted tool access
4. Use `$ARGUMENTS` placeholder for user-provided parameters
5. Structure instructions as numbered steps for clarity

## Key Notes

- This directory defines command templates, not executable code
- Commands are invoked in actual project directories where they operate on real codebases
- The `allowed-tools` restriction in `/test-and-fix` limits it to specific npm commands for safety
- All command descriptions are currently in Chinese