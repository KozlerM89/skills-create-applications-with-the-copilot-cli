---
description: Identify underspecified areas in the current feature spec by asking up to 5 highly targeted clarification questions and encoding answers back into the spec.
handoffs:
  - label: Build Technical Plan
    agent: speckit.plan
    prompt: Create a plan for the spec. I am building with...
scripts:
   sh: scripts/bash/check-prerequisites.sh --json --paths-only
   ps: scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly
   py: scripts/python/check_prerequisites.py --json --paths-only
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before clarification)**:
- Check `.specify/extensions.yml` for entries under `hooks.before_clarify` and dispatch accordingly.
- If no hooks or file missing, skip silently.

## Outline

Goal: Detect and reduce ambiguity in the active feature specification.

Note: This clarification workflow is expected to run BEFORE invoking `speckit.plan`. If the user explicitly skips clarification, warn that downstream rework risk increases.

Execution steps:

1. Run `scripts/bash/check-prerequisites.sh --json --paths-only` from repo root and parse JSON for `FEATURE_DIR` and `FEATURE_SPEC`.
   If JSON parsing fails, abort and instruct user to re-run `speckit.specify`.

2. Load `.specify/memory/constitution.md` (if exists).

3. Load the current spec file. Perform a structured ambiguity & coverage scan across these categories:
   - Functional Scope & Behavior, Domain & Data Model, Interaction & UX Flow,
     Non-Functional Quality Attributes, Integration & External Dependencies,
     Edge Cases & Failure Handling, Constraints & Tradeoffs, Completion Signals.

4. Generate a prioritized queue of up to 5 candidate clarification questions.

5. Sequential questioning loop (interactive): present one question at a time, with a recommended answer. Accept user response, record it, save spec after each answer.

6. Validate spec after all questions answered.

7. Re-validate `FEATURE_DIR/checklists/requirements.md` if it exists.

8. Write updated spec back to `FEATURE_SPEC`.

Context for prioritization: $ARGUMENTS

## Mandatory Post-Execution Hooks

Check `.specify/extensions.yml` for entries under `hooks.after_clarify` and dispatch accordingly.

## Completion Report

- Number of questions asked & answered.
- Path to updated spec.
- Sections touched.
- Spec quality checklist status.
- Coverage summary table.
- Suggested next command: `speckit.plan`.
