---
description: Assess the current codebase against the feature's spec, plan, and tasks, then append any remaining unbuilt work as new tasks to tasks.md so implement can complete it.
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

**Check for extension hooks (before convergence)**:
- Check `.specify/extensions.yml` for entries under `hooks.before_converge` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Goal

Close the gap between what the spec, plan, and tasks call for and what the codebase currently implements.
Read `spec.md`, `plan.md`, and `tasks.md` as the **sole source of intent** (with the constitution as governing constraints).
**APPEND-ONLY**: Only write is appending a new `## Phase N: Convergence` section to `tasks.md`.

## Execution Steps

### 1. Initialize Convergence Context

Run `scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` from repo root. Derive absolute paths:
- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

If any artifact is missing, STOP and instruct user to run:
- `speckit.specify` for missing spec
- `speckit.plan` for missing plan
- `speckit.tasks` for missing tasks

### 2. Load Artifacts

Load from `spec.md`: Functional Requirements, Success Criteria, User Stories.
Load from `plan.md`: Architecture decisions, data model, file/component touch-points.
Load from `tasks.md`: Task IDs and phase numbers (to compute next ID/phase).
Load constitution (if not a template): principle MUST/SHOULD statements.

### 3. Build Intent Inventory & Assess Codebase

For each requirement / acceptance criterion / constitution principle, inspect code in scope and classify findings:
- **`missing`**: required work absent entirely.
- **`partial`**: exists but doesn't fully satisfy requirement.
- **`contradicts`**: conflicts with stated intent or constitution MUST.
- **`unrequested`**: present but not called for.

### 4. Assign Severity

CRITICAL → constitution MUST violations or P1-blocking gaps.
HIGH → missing/partial core functional requirements.
MEDIUM → partial secondary requirements or unrequested additions.
LOW → minor gaps or polish.

### 5. Present Findings Summary

```
## Convergence Findings
| ID | Gap Type | Severity | Source | Evidence | Remaining Work |
```

### 6. Append Convergence Tasks (or report converged)

**If findings exist**: Append `## Phase N: Convergence` to `tasks.md` with new tasks:
`- [ ] T042 <description> per <source-ref> (<gap-type>)`

**If no findings**: Leave `tasks.md` unchanged. Report: **"✅ Converged."**

### 7. Provide Next Actions

On `tasks_appended`: recommend running `speckit.implement`.
On `converged`: recommend review / opening a PR.

## Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_converge` and dispatch accordingly.
