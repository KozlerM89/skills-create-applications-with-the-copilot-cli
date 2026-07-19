---
description: "Task list for Contact Dashboard Modernisation"
---

# Tasks: Contact Dashboard Modernisation

**Input**: Design documents from `/specs/001-contact-dashboard-modernisation/`
**Prerequisites**: plan.md ✅ | spec.md ✅

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel with other tasks in the same phase
- **[Story]**: Which user story this task belongs to (US1–US4)

---

## Phase 1: Setup (Shared Infrastructure)

- [ ] T001 Verify Creator Kit v0.13+ is installed in the target Dataverse environment
- [ ] T002 [P] Open the target solution in make.powerapps.com and confirm solution is unmanaged

**Checkpoint**: Environment ready — Creator Kit confirmed, solution accessible.

---

## Phase 2: Remove Interactive Dashboard (Blocking Prerequisite)

**⚠️ CRITICAL**: Custom Page cannot be added until sitemap is cleared.

- [ ] T003 Open App Designer for the Model-Driven App
- [ ] T004 Locate the Contact Interactive Dashboard in the sitemap (Navigation section)
- [ ] T005 Remove the Interactive Dashboard entry from the sitemap
- [ ] T006 Save the sitemap and publish the app (interim publish)

**Checkpoint**: Interactive Dashboard removed and app published — Custom Page addition can now begin.

---

## Phase 3: Build the Custom Page — Foundational Data Layer (US1, US2)

**Goal**: Custom Page exists with Dataverse data connected and displayed in DetailsList.
**Independent Test**: Custom Page opens in preview and shows Contact records.

- [ ] T007 [US1] Create a new Custom Page named `ContactDashboardPage` in App Designer
- [ ] T008 [US1] Add Dataverse connector; connect Contact table (and Account via `parentaccountid` lookup)
- [ ] T009 [US1] Add Creator Kit **DetailsList** component; bind to Contact data source
- [ ] T010 [US1] Configure DetailsList columns: Full Name (`fullname`), Email (`emailaddress1`), Phone (`telephone1`), Account Name (`parentaccountid.name`), Last Activity Date (`modifiedon`)
- [ ] T011 [US1] Add Creator Kit **SearchBox** component; implement Power Fx filter:
  `Filter(Contacts, IsBlank(searchInput.SearchText) || StartsWith(fullname, searchInput.SearchText))`
- [ ] T012 [P] [US2] Add chart control; bind to Contact data grouped by `statuscode` field

**Checkpoint**: Data loads, columns visible, SearchBox filters results, chart displays status breakdown.

---

## Phase 4: Navigation and Quick Actions (US3, US4)

**Goal**: Row click opens Contact record; CommandBar allows creating new Contacts.

- [ ] T013 [US3] Add row-click `OnSelect` Power Fx formula to navigate to Contact form:
  `Navigate(ContactForm, ScreenTransition.None, {item: ThisItem})`
- [ ] T014 [US4] Add Creator Kit **CommandBar** component above the DetailsList
- [ ] T015 [US4] Add "New Contact" item to CommandBar; set `OnSelect`:
  `NewForm(ContactForm); Navigate(ContactForm, ScreenTransition.None)`

**Checkpoint**: Clicking a row opens the correct Contact; "New Contact" opens a blank Contact form.

---

## Phase 5: Integrate into Model-Driven App Sitemap

- [ ] T016 Save and publish `ContactDashboardPage` from the Canvas editor
- [ ] T017 In App Designer > Navigation, add `ContactDashboardPage` to the sitemap area where the old dashboard was
- [ ] T018 Set display name to `Contact Dashboard` and assign an appropriate icon
- [ ] T019 Save the sitemap and publish the Model-Driven App

**Checkpoint**: New page appears in app navigation; old dashboard entry is gone.

---

## Phase 6: Security Role Validation (US1–US4)

- [ ] T020 Identify Security Roles that previously had access to the Contact Interactive Dashboard
- [ ] T021 Assign those Security Roles to `ContactDashboardPage` in the solution
- [ ] T022 Validate access in a non-admin user session (sign in as a user with the mapped role)

**Checkpoint**: Non-admin users can open and use the Contact Dashboard Custom Page.

---

## Phase 7: Validation Against Spec

- [ ] T023 Play the app in App Designer preview mode
- [ ] T024 Verify SC-001: Interactive Dashboard no longer in navigation
- [ ] T025 Verify SC-002: Page loads in < 3 seconds with ≤ 500 records
- [ ] T026 Verify SC-003: All 5 columns visible in DetailsList
- [ ] T027 Verify SC-004: SearchBox filters list correctly
- [ ] T028 Verify SC-005: Row click opens correct Contact record
- [ ] T029 Verify SC-006: Non-admin role user can access the page
- [ ] T030 Verify SC-007: Solution exports without errors

---

## Phase 8: Solution Export & Commit

- [ ] T031 Export the solution as **unmanaged** from make.powerapps.com > Solutions
- [ ] T032 Create `solution-exports/` directory in the repository
- [ ] T033 Commit the exported `.zip` file to `solution-exports/ContactDashboardModernisation_<version>.zip`
- [ ] T034 Push and open PR referencing this task list

---

## Dependencies & Execution Order

- **Phase 1 (Setup)**: No dependencies.
- **Phase 2 (Remove Dashboard)**: Depends on Phase 1 — BLOCKS Phase 3.
- **Phase 3 (Build Page)**: Depends on Phase 2 — BLOCKS Phases 4–5.
- **Phase 4 (Nav & Actions)**: Depends on Phase 3; T013/T014/T015 can run in parallel within phase.
- **Phase 5 (Sitemap)**: Depends on Phase 4 (page must be published first).
- **Phase 6 (Security)**: Depends on Phase 5 (page must be in sitemap).
- **Phase 7 (Validation)**: Depends on Phases 5–6.
- **Phase 8 (Export)**: Depends on Phase 7 (all SC checks passing).
