# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Repository

This is a **backup repository** for portable Claude Code slash commands. It serves as:
- A central place to version control custom commands
- A source for installing commands on new machines
- A shareable collection for other developers

**See README.md for installation instructions** (clone, copy, or symlink to `~/.claude/commands/`).

## Repository Purpose

This repository contains slash commands for Claude Code that implement **SpecKit** - a structured feature development workflow system. Commands are markdown files with YAML frontmatter that get expanded into prompts when invoked.

Most commands are **portable** and work in any project. Some (like test commands) need minor edits to match your Docker service names. A few are **project-specific** (rpg_subsystem_scaffold, prime_repositories).

## Command Structure

Commands follow this pattern:
```yaml
---
description: Brief description shown in command index
handoffs:
  - label: Button label
    agent: next.command
    prompt: Default prompt for handoff
    send: true  # Auto-send vs manual
---

## User Input
$ARGUMENTS

## Outline
[Execution instructions...]
```

## SpecKit Workflow

The core workflow progresses through phases, each with user checkpoints:

```
/speckit.constitution → /speckit.specify → /speckit.clarify → /speckit.plan → /speckit.checklist → /speckit.tasks → /speckit.analyze → /speckit.implement
```

| Phase | Command | Artifacts Generated |
|-------|---------|---------------------|
| 0 | `speckit.constitution` | `.specify/memory/constitution.md` |
| 1 | `speckit.specify` | `specs/<N>-<name>/spec.md`, feature branch |
| 2 | `speckit.clarify` | Updates to spec.md with clarifications |
| 3 | `speckit.plan` | `plan.md`, `research.md`, `data-model.md`, `contracts/`, `quickstart.md` |
| 4 | `speckit.checklist` | `checklists/<domain>.md` |
| 5 | `speckit.tasks` | `tasks.md` |
| 6 | `speckit.analyze` | Read-only consistency report (no files) |
| 7 | `speckit.taskstoissues` | GitHub issues |
| 8 | `speckit.implement` | Implementation code |

## Key Concepts

**Constitution**: Project principles stored at `.specify/memory/constitution.md`. Violations are always CRITICAL severity and block implementation.

**Checklists**: Test REQUIREMENTS quality, not implementation behavior. Items use markers like `[Gap]`, `[Ambiguity]`, `[Conflict]`.

**Task Format**: `- [ ] [TaskID] [P?] [Story?] Description with file path`
- `[P]` marks parallelizable tasks (different files, no dependencies)
- `[Story]` links to user stories (US1, US2, etc.)

**Analyze Phase**: Strictly read-only. Never modifies files.

## Script Dependencies

Commands rely on scripts at `.specify/scripts/bash/`:
- `create-new-feature.sh` - Branch creation and spec initialization
- `check-prerequisites.sh` - Validate feature context exists
- `setup-plan.sh` - Initialize planning artifacts
- `update-agent-context.sh` - Update agent-specific context files

## Standalone Commands

Non-workflow commands for common operations:
- `commit.md` - Lint, type-check, commit with conventional message
- `ship.md` - Merge to main and cleanup branches
- `test.md`, `test-hooks.md`, `test-components.md`, `test-a11y.md` - Run test suites
- `code-review.md` - Comprehensive code review
- `prep.md` - Prepare to discuss repository
