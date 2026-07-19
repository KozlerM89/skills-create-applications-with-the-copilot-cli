---
description: Generate an actionable, dependency-ordered tasks.md for the feature based on available design artifacts.
handoffs:
  - label: Analyze For Consistency
    agent: speckit.analyze
    prompt: Run a project analysis for consistency
    send: true
  - label: Implement Project
    agent: speckit.implement
    prompt: Start the implementation in phases
    send: true
scripts:
  sh: scripts/bash/setup-tasks.sh --json
  ps: scripts/powershell/setup-tasks.ps1 -Json
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before tasks generation)**:
- Check `.specify/extensions.yml` for entries under `hooks.before_tasks` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Outline

1. **Setup**: Run `scripts/bash/setup-tasks.sh --json` from repo root and parse FEATURE_DIR, TASKS_TEMPLATE, and AVAILABLE_DOCS.

2. **Load design documents** from FEATURE_DIR:
   - **Required**: `plan.md`, `spec.md`
   - **Optional**: `data-model.md`, `contracts/`, `research.md`, `quickstart.md`
   - Load `.specify/memory/constitution.md` (if exists).

3. **Execute task generation workflow**:
   - Extract tech stack and project structure from `plan.md`.
   - Extract user stories with priorities (P1, P2, P3…) from `spec.md`.
   - Map entities and contracts to user stories.
   - Generate tasks organized by user story (each independently testable).
   - Create dependency graph showing user story completion order.

4. **Generate `tasks.md`** using `.specify/templates/tasks-template.md` as structure:
   - Phase 1: Setup (project initialization)
   - Phase 2: Foundational (blocking prerequisites)
   - Phase 3+: One phase per user story (in priority order)
   - Final Phase: Polish & cross-cutting concerns

## Task Generation Rules

Every task MUST follow: `- [ ] [TaskID] [P?] [Story?] Description with file path`

**Format**: `- [ ] T001 [P] [US1] Create Contact model in src/models/contact.py`

## Mandatory Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_tasks` and dispatch accordingly.

## Completion Report

- Total task count, task count per user story.
- Parallel opportunities.
- MVP scope (typically User Story 1 only).
