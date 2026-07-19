---
description: "Task list template for feature implementation"
---

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (required), spec.md (required)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel
- **[Story]**: Which user story this task belongs to (US1, US2, US3…)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

- [ ] T001 Create project structure per implementation plan
- [ ] T002 [P] Configure linting and formatting tools

---

## Phase 2: Foundational (Blocking Prerequisites)

**⚠️ CRITICAL**: No user story work can begin until this phase is complete.

- [ ] T003 [P] Set up core infrastructure components
- [ ] T004 Configure error handling and logging

**Checkpoint**: Foundation ready — user story implementation can now begin.

---

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal**: [Brief description]
**Independent Test**: [How to verify independently]

- [ ] T005 [P] [US1] Create [Component] in [file path]
- [ ] T006 [US1] Implement [Service/Logic] in [file path]
- [ ] T007 [US1] Add validation and error handling

**Checkpoint**: User Story 1 should be fully functional and testable independently.

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

- [ ] T008 [P] [US2] Create [Component] in [file path]
- [ ] T009 [US2] Implement [Service/Logic] in [file path]

---

## Phase N: Polish & Cross-Cutting Concerns

- [ ] TXXX [P] Documentation updates
- [ ] TXXX Code cleanup and refactoring
- [ ] TXXX Run quickstart.md validation

---

## Dependencies & Execution Order

- **Setup (Phase 1)**: No dependencies.
- **Foundational (Phase 2)**: Depends on Setup — BLOCKS all user stories.
- **User Stories (Phase 3+)**: All depend on Foundational; can proceed in parallel.
- **Polish (Final Phase)**: Depends on all desired user stories being complete.
