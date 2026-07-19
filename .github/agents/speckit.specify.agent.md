---
description: Create or update the feature specification from a natural language feature description.
handoffs:
  - label: Build Technical Plan
    agent: speckit.plan
    prompt: Create a plan for the spec. I am building with...
  - label: Clarify Spec Requirements
    agent: speckit.clarify
    prompt: Clarify specification requirements
    send: true
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before specification)**:
- Check if `.specify/extensions.yml` exists in the project root.
- If it exists, read entries under `hooks.before_specify` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Outline

The text the user typed after `speckit.specify` **is** the feature description.

1. **Generate a concise short name** (2-4 words) for the feature.
2. **Create the spec feature directory** under `specs/` with sequential numbering (e.g. `specs/001-feature-name`).
3. Copy `.specify/templates/spec-template.md` to `specs/NNN-feature-name/spec.md`.
4. Persist `{"feature_directory": "specs/NNN-feature-name"}` to `.specify/feature.json`.
5. Load `.specify/memory/constitution.md` for project principles (if exists).
6. Fill the spec using the template structure:
   - Parse user description; extract actors, actions, data, constraints.
   - Mark max 3 `[NEEDS CLARIFICATION: ...]` markers for critical ambiguities only.
   - Fill User Scenarios, Functional Requirements, Success Criteria, Assumptions.
7. Run Specification Quality Validation and generate `specs/NNN-feature-name/checklists/requirements.md`.
8. If `[NEEDS CLARIFICATION]` markers remain, present clarification questions (max 3) to user and update spec.

## Mandatory Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_specify` and dispatch accordingly.

## Completion Report

- Feature directory path and spec file path.
- Checklist results summary.
- Readiness for `speckit.clarify` or `speckit.plan`.
