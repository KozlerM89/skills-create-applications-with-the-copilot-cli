# Implementation Plan: Contact Dashboard Modernisation

**Branch**: `001-contact-dashboard-modernisation` | **Date**: 2026-07-19 | **Spec**: [spec.md](./spec.md)

## Summary

Replace the classic Dataverse Interactive Dashboard (Contact entity) in the Model-Driven App with a Custom Page built on the Power Platform Creator Kit. The page will use a Fluent UI DetailsList for the contact list, a SearchBox for filtering, a CommandBar for quick actions, and a chart for status breakdown — all backed by the Dataverse Contact entity.

## Technical Context

**Platform**: Power Platform — Model-Driven App (make.powerapps.com)

**Primary Dependencies**: Microsoft Dataverse (Contact & Account entities), Power Platform Creator Kit v0.13+ (Fluent UI components), Power Fx

**Storage**: Dataverse — Contact entity (`cr_*` or standard fields), Account entity (lookup)

**Testing**: Manual validation in App Designer Play mode; checklist-based verification against acceptance scenarios

**Target Platform**: Model-Driven App > Custom Page (canvas-based)

**Performance Goals**: Page loads in < 3 seconds with ≤ 500 Contact records; search/filter responds in < 1 second

**Constraints**: Creator Kit must be pre-installed in the environment; no direct OData REST calls (Dataverse connector only); solution-based packaging mandatory

**Scale/Scope**: Single Custom Page replacing one Interactive Dashboard; no backend code changes required

## Constitution Check

*GATE: Must pass before Phase 0 research.*

| Principle | Compliant? | Notes |
|-----------|-----------|-------|
| I. Dataverse-First | ✅ | All data via Dataverse connector — Contact and Account tables only |
| II. Solution Packaging | ✅ | Custom Page added to existing solution; exported as unmanaged (dev) / managed (prod) |
| III. Modern UX via Fluent UI / Creator Kit | ✅ | DetailsList, SearchBox, CommandBar from Creator Kit; no classic controls |
| IV. Spec-Driven Development | ✅ | spec.md approved before plan created |
| V. Security-Role Compliance | ✅ | Security roles mapped in T013; validated before publish |

## Project Structure

### Documentation (this feature)

```text
specs/001-contact-dashboard-modernisation/
├── spec.md          ← requirements & user stories
├── plan.md          ← this file
└── tasks.md         ← actionable task list
```

### Power Platform Artefacts (outside repository — in solution)

```text
solution/
├── ContactDashboardPage    (Custom Page — canvas)
├── ContactDashboardPage.sitemap-entry  (Navigation entry in Model-Driven App)
└── SecurityRole.assignments            (Role → Custom Page mapping)
```

### Solution Export (committed to repository)

```text
solution-exports/
└── ContactDashboardModernisation_<version>.zip
```

## Phases

### Phase 1 — Environment Setup

- Verify Creator Kit v0.13+ is installed in the target environment.
- Open the target solution in make.powerapps.com.

### Phase 2 — Remove Interactive Dashboard

- Open App Designer for the Model-Driven App.
- Remove the Contact Interactive Dashboard entry from the sitemap.
- Publish (interim) to clear the sitemap before adding the new page.

### Phase 3 — Build the Custom Page

- Create a new Custom Page named `ContactDashboardPage`.
- Add Dataverse connector for Contact (and Account via lookup).
- Add Creator Kit components: CommandBar, SearchBox, DetailsList.
- Implement Power Fx data binding:
  - `Filter(Contacts, IsBlank(searchInput.SearchText) || StartsWith(fullname, searchInput.SearchText))` (delegable)
- Configure DetailsList columns: Full Name, Email, Business Phone, Account Name, Last Activity Date.
- Add "New Contact" CommandBar item with `NewForm()` / `Navigate()` action.
- Add row-click navigation: `Navigate(ContactForm, ScreenTransition.None, {item: ThisItem})`.
- Add chart control (status breakdown).
- Save and publish the Custom Page.

### Phase 4 — Integrate into Model-Driven App

- In App Designer > Navigation, add `ContactDashboardPage` to the sitemap area where the old dashboard was.
- Set display name: `Contact Dashboard`.
- Save and publish the Model-Driven App.

### Phase 5 — Security Role Validation

- Review existing Security Roles that had access to the Interactive Dashboard.
- Assign those roles to the new Custom Page in the solution.
- Verify in a non-admin user session.

### Phase 6 — Validation

- Play the app in App Designer preview.
- Execute all acceptance scenarios from spec.md.
- Confirm SC-001 through SC-007 are met.

### Phase 7 — Solution Export & Commit

- Export the solution as unmanaged (for development).
- Commit the `.zip` export to `solution-exports/` in the repository.

## Complexity Tracking

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| Delegation limit (Power Fx `Filter`) | Contacts table may exceed 500 rows | `Search()` is non-delegable; using `StartsWith` on indexed field keeps delegation intact |
