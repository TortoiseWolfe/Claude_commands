# Claude_commands

A backup repository of slash commands for [Claude Code](https://claude.ai/code). Use this repo to:
- Install commands on a new machine
- Share commands with others
- Version control your custom commands
- Sync commands across multiple systems

Includes **SpecKit** (a structured feature development workflow) plus standalone utilities for common development tasks.

---

## Installation

Claude Code discovers commands from `.claude/commands/` directories. Commands here are `.md` files that expand into prompts when invoked.

### User-Level (Global)

Install commands for all projects on your system:

```bash
# Option 1: Clone entire repo
git clone https://github.com/YOUR_USERNAME/Claude_commands.git ~/.claude/commands

# Option 2: Copy specific commands
cp path/to/Claude_commands/prep.md ~/.claude/commands/
cp path/to/Claude_commands/commit.md ~/.claude/commands/

# Option 3: Symlink to keep in sync with this backup
ln -s /path/to/Claude_commands ~/.claude/commands
```

### Project-Level (Per-Repo)

Install commands for a specific project:

```bash
mkdir -p your-project/.claude/commands
cp path/to/Claude_commands/*.md your-project/.claude/commands/
```

### Verify Installation

Run `/help` in Claude Code to see available commands, or type `/` to see the command list.

---

## Portability Guide

| Symbol | Meaning |
|--------|---------|
| **Portable** | Works anywhere, no changes needed |
| **Customizable** | Edit Docker service names, paths, or project-specific details |
| **Project-specific** | Designed for a specific codebase |

---

# SpecKit Workflow Commands

The SpecKit workflow progresses through phases with user checkpoints at each stage.

**Portability: Portable** (requires `.specify/` directory structure in target project)

## **/speckit.workflow**

Execute the complete SpecKit workflow from feature description to implementation. Orchestrates all phases in sequence with checkpoints for user approval between each stage. Supports flags like `--skip-clarify`, `--to-plan`, `--dry-run` to customize execution.

## **/speckit.constitution**

Create or update the project constitution at `.specify/memory/constitution.md`. Defines immutable project principles that become non-negotiable gates for all subsequent phases. Handles semantic versioning of constitution changes and propagates updates to dependent templates.

## **/speckit.specify**

Create feature branch and initial specification from a natural language description. Generates branch name, checks for conflicts, creates `spec.md` using the spec template, and produces an initial requirements quality checklist. Focuses on WHAT and WHY, not HOW.

## **/speckit.clarify**

Identify and resolve ambiguities in the current feature spec through targeted questions. Presents one question at a time with recommended answers. Updates spec atomically after each accepted answer. Covers functional scope, data model, UX flow, non-functional requirements, and edge cases.

## **/speckit.plan**

Generate implementation plan and design artifacts from the specification. Creates `research.md` (technical decisions), `data-model.md` (entities and relationships), `contracts/` (API specifications), and `quickstart.md` (integration scenarios). Validates against constitution principles.

## **/speckit.checklist**

Generate requirements quality checklists - "unit tests for requirements." Items validate specification completeness, clarity, and measurability rather than implementation behavior. Uses markers like `[Gap]`, `[Ambiguity]`, `[Conflict]` to flag issues.

## **/speckit.tasks**

Generate actionable, dependency-ordered task list from design artifacts. Organizes tasks by user story with phases: Setup, Foundational, User Stories (by priority), and Polish. Uses strict format: `- [ ] [TaskID] [P?] [Story?] Description with file path`.

## **/speckit.analyze**

Perform non-destructive cross-artifact consistency analysis across spec, plan, and tasks. Detects duplications, ambiguities, coverage gaps, and constitution violations. Assigns severity (CRITICAL/HIGH/MEDIUM/LOW) and provides remediation recommendations. **Strictly read-only.**

## **/speckit.taskstoissues**

Convert tasks from `tasks.md` into GitHub issues. Verifies remote is GitHub before proceeding. Preserves task dependencies and creates linked issues.

## **/speckit.implement**

Execute the implementation plan by processing all tasks in `tasks.md`. Verifies checklist completion first, creates ignore files for detected technologies, then executes tasks phase-by-phase. Marks tasks complete as they finish.

---

# Standalone Commands

## **/prep** — Portable

Prepare to discuss the current repository. Explores codebase structure and key files to build context for conversation.

## **/commit** — Portable

Run linter and type-check, then commit changes with a descriptive message. Uses conventional commits format (feat/fix/docs/refactor/test/chore). Only commits if all quality checks pass.

## **/ship** — Portable

Ship the current feature branch: commit pending changes, merge to main with `--no-ff`, and clean up stray branches. Enforces Docker-first development pattern where applicable.

## **/code-review** — Portable

Perform comprehensive code review covering security, performance, code quality, and test coverage. **Fixes issues directly** rather than just documenting them. Scans for XSS, injection vulnerabilities, missing memoization, duplicate code, and dead code.

## **/test** — Customizable

Run the comprehensive test suite (~180 test files). Executes in batches to avoid OOM in Docker/WSL2 environments. Covers atomic components, molecular components, organisms, lib/services/hooks.

> **To customize**: Edit Docker service name (`spoketowork`) and test commands to match your project.

## **/test-hooks** — Customizable

Run only React hook tests. Fast execution (<1 min).

> **To customize**: Edit Docker service name and test path.

## **/test-components** — Customizable

Run only React component tests (~5 min). Covers all components in atomic design structure.

> **To customize**: Edit Docker service name and test path.

## **/test-a11y** — Customizable

Run accessibility tests using Pa11y (~1 min). Checks WCAG 2.1 AA compliance. Requires dev server running.

> **To customize**: Edit Docker service name and test command.

## **/test-fail** — Customizable

Run only the known failing tests to investigate isolation issues. Useful for debugging state leaks between tests in `singleFork` mode.

> **To customize**: Edit Docker service name and test file paths.

## **/clean-start** — Customizable

Clean build artifacts and restart development environment. Stops Docker containers, removes `.next` and `node_modules/.cache`, then starts fresh containers.

> **To customize**: Edit script path (`./scripts/clean-start.sh`) to match your project.

---

# Utility Commands

## **/extract-linkedin** — Portable

Extract and clean a LinkedIn data export archive. Keeps profile-relevant files (positions, skills, recommendations) and deletes irrelevant data (ads, messages, receipts).

## **/clean-transcript** — Portable

Clean transcript files for GPT context engineering. Removes conversational filler and verbal tics while preserving educational content. Reorganizes into markdown sections.

## **/rpg_subsystem_scaffold** — Project-specific

Scaffold a new subsystem for the RPG-Suite WordPress plugin. Creates directory structure and base class following established patterns.

> **Note**: This command is specific to the RPG-Suite project and won't work elsewhere without modification.

## **/prime_repositories** — Project-specific

Load essential context about RPG-Suite and ZZZ repositories. Reads main readme files, integration plans, and core source files.

> **Note**: Contains hardcoded paths to specific repositories. Modify paths before using in other contexts.
