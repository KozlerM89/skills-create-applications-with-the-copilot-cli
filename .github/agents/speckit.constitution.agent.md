---
description: Create or update the project constitution from interactive or provided principle inputs, ensuring all dependent templates stay in sync.
handoffs:
  - label: Build Specification
    agent: speckit.specify
    prompt: Implement the feature specification based on the updated constitution. I want to build...
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution Checks

**Check for extension hooks (before constitution update)**:
- Check if `.specify/extensions.yml` exists in the project root.
- If it exists, read it and look for entries under the `hooks.before_constitution` key
- If the YAML cannot be parsed or is invalid, skip hook checking silently and continue normally
- Filter out hooks where `enabled` is explicitly `false`. Treat hooks without an `enabled` field as enabled by default.
- For each remaining hook, do **not** attempt to interpret or evaluate hook `condition` expressions:
  - If the hook has no `condition` field, or it is null/empty, treat the hook as executable
  - If the hook defines a non-empty `condition`, skip the hook and leave condition evaluation to the HookExecutor implementation
- For each executable hook, output the following based on its `optional` flag:
  - **Optional hook** (`optional: true`):
    ```
    ## Extension Hooks

    **Optional Pre-Hook**: {extension}
    Command: `/{command}`
    Description: {description}

    Prompt: {prompt}
    To execute: `/{command}`
    ```
  - **Mandatory hook** (`optional: false`):
    ```
    ## Extension Hooks

    **Automatic Pre-Hook**: {extension}
    Executing: `/{command}`
    EXECUTE_COMMAND: {command}

    Wait for the result of the hook command before proceeding to the Outline.
    ```
    After emitting the block above you MUST actually invoke the hook and wait for it to finish before continuing.
- If no hooks are registered or `.specify/extensions.yml` does not exist, skip silently

## Outline

You are updating the project constitution at `.specify/memory/constitution.md`.

Follow this execution flow:

1. Load the existing constitution at `.specify/memory/constitution.md`.
2. Collect/derive values for placeholders from user input or repo context.
3. Draft the updated constitution content replacing all placeholder tokens.
4. Consistency propagation checklist:
   - Read `.specify/templates/plan-template.md` and ensure Constitution Check aligns.
   - Read `.specify/templates/spec-template.md` for scope/requirements alignment.
   - Read `.specify/templates/tasks-template.md` and ensure task categorization reflects principles.
   - Read each installed Spec Kit command file (named `speckit.*` in `.github/agents/`).
5. Produce a Sync Impact Report as an HTML comment at the top of the constitution file.
6. Validate: no bracket tokens, version line matches report, dates ISO format.
7. Write the completed constitution back to `.specify/memory/constitution.md`.
8. Output a final summary: new version, bump rationale, files flagged for follow-up, suggested commit message.

## Post-Execution Checks

**Check for extension hooks (after constitution update)**:
- Check `.specify/extensions.yml` for entries under `hooks.after_constitution`.
- Apply the same hook-dispatch logic as Pre-Execution Checks above.
- If no hooks or file missing, skip silently.
