# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This is the Claude Code commands directory (`/Users/user/.claude/commands`) containing custom slash command definitions for development workflow automation. Each markdown file defines a specific command that can be invoked in Claude Code sessions to streamline common development tasks.

## Command Structure

Commands are markdown files with YAML frontmatter containing:
- `description`: Brief command description for the user interface
- `allowed-tools`: Restricted tool access for security (optional)
- `argument-hint`: Parameter guidance for users (optional)

Command bodies contain structured instructions that define the command's execution logic and workflow steps.

## Available Commands

### Development & Testing
- `/test-and-fix` - Automated test execution and failure resolution (restricted to npm test commands)
- `/debug-error` - Error analysis and debugging workflow
- `/optimize` - Code performance optimization
- `/refactor` - Code refactoring following best practices
- `/docs` - Documentation generation and updates

### Git & Release Management
- `/branch-smart` - Intelligent branch operations (create, switch, merge, delete, sync)
- `/pr-ready` - Pre-pull request validation with comprehensive checks
- `/git-health` - Repository health analysis and cleanup recommendations
- `/release-prep` - Version bumping and changelog generation for releases
- `/commit-smart` - Intelligent commit message generation and staging
- `/sync-upstream` - Upstream synchronization for forked repositories
- `/fix-conflicts` - Merge conflict resolution assistance
- `/revert-safe` - Safe commit/merge reverting with impact analysis

### Security & Quality
- `/security-review` - Security vulnerability scanning and analysis

## Command Execution Model

Commands operate through a structured workflow:

1. **Status Assessment** - Commands begin by evaluating current project state using git commands and dynamic status checks (e.g., `!git branch --show-current`)
2. **Tool Restrictions** - Security-sensitive commands use `allowed-tools` to limit tool access (e.g., `/test-and-fix` only allows specific npm commands)
3. **Parameterization** - Commands accept arguments via `$ARGUMENTS` placeholder for customization
4. **Multi-step Execution** - Complex workflows break down into numbered steps with validation between stages

## Command Development Guidelines

When creating new commands:
- Use YAML frontmatter for metadata
- Structure instructions as numbered workflows
- Include dynamic status checks with `!command` syntax
- Implement appropriate tool restrictions for security
- Provide argument hints for user guidance
- Follow the established pattern of status → analysis → action → verification

## Security Considerations

- Commands with `allowed-tools` restrictions prevent unauthorized operations
- Git operations are scoped to prevent destructive actions
- Test commands are limited to standard npm scripts
- Security review commands focus on defensive analysis only