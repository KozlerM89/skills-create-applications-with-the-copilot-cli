---
description: Execute the implementation plan by processing and executing all tasks defined in tasks.md
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
  py: scripts/python/check_prerequisites.py --json --require-tasks --include-tasks
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before implementation)**:
- Check `.specify/extensions.yml` for entries under `hooks.before_implement` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Outline

1. Run `scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` from repo root and parse FEATURE_DIR and AVAILABLE_DOCS.

2. **Check checklists status** (if `FEATURE_DIR/checklists/` exists):
   - Count completed/incomplete items across all checklists.
   - If any incomplete, ask user to confirm proceed before continuing.

3. Load implementation context:
   - **REQUIRED**: `tasks.md`, `plan.md`
   - **IF EXISTS**: `data-model.md`, `contracts/`, `research.md`, `.specify/memory/constitution.md`, `quickstart.md`

4. **Project Setup Verification**: Create/verify `.gitignore` and other ignore files based on project tech stack.

5. Parse `tasks.md` and extract: task phases, dependencies, task details (ID, description, file paths, parallel markers [P]).

6. Execute implementation phase-by-phase:
   - Respect task dependencies: sequential tasks in order, parallel [P] tasks together.
   - Follow TDD if tests requested: test tasks before implementation tasks.
   - Mark completed tasks as `[X]` in `tasks.md`.

7. Progress tracking: report after each task; halt on non-parallel failures.

8. Completion validation: verify all tasks done, features match spec, tests pass.

Note: If `tasks.md` is incomplete, suggest running `speckit.tasks` first.

## Mandatory Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_implement` and dispatch accordingly.

## Completion Report

Final status with summary of completed work and all tasks marked [X].
