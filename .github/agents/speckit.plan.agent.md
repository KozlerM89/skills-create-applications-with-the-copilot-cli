---
description: Execute the implementation planning workflow using the plan template to generate design artifacts.
handoffs:
  - label: Create Tasks
    agent: speckit.tasks
    prompt: Break the plan into tasks
    send: true
  - label: Create Checklist
    agent: speckit.checklist
    prompt: Create a checklist for the following domain...
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before planning)**:
- Check `.specify/extensions.yml` for entries under `hooks.before_plan` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Outline

1. **Setup**: Run `scripts/bash/setup-plan.sh --json` from repo root and parse JSON for FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH.

2. **Load context**: Read FEATURE_SPEC and `.specify/memory/constitution.md`. Load IMPL_PLAN template.

3. **Execute plan workflow**:
   - Fill Technical Context (mark unknowns as "NEEDS CLARIFICATION")
   - Fill Constitution Check section
   - Evaluate gates (ERROR if violations unjustified)
   - Phase 0: Generate `research.md` (resolve all NEEDS CLARIFICATION)
   - Phase 1: Generate `data-model.md`, `contracts/`, `quickstart.md`
   - Re-evaluate Constitution Check post-design

## Mandatory Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_plan` and dispatch accordingly.

## Completion Report

Report branch, IMPL_PLAN path, and generated artifacts.

## Phases

### Phase 0: Outline & Research

1. Extract unknowns from Technical Context → research tasks.
2. Consolidate findings in `research.md`: Decision, Rationale, Alternatives considered.

**Output**: `research.md` with all NEEDS CLARIFICATION resolved.

### Phase 1: Design & Contracts

**Prerequisites**: `research.md` complete.

1. Extract entities from feature spec → `data-model.md`.
2. Define interface contracts (if applicable) → `contracts/`.
3. Create quickstart validation guide → `quickstart.md`.

**Output**: `data-model.md`, `contracts/*`, `quickstart.md`.

## Key rules

- Use absolute paths for filesystem operations.
- ERROR on gate failures or unresolved clarifications.
