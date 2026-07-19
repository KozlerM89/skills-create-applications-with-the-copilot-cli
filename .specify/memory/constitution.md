# Contact Dashboard Modernisation — Constitution

## Core Principles

### I. Dataverse-First
All data must be read and written through Dataverse (Microsoft Dataverse).
Tables used in this project MUST exist as standard or custom Dataverse entities.
No direct REST/OData calls bypassing the Dataverse connector.

### II. Power Platform Solution Packaging
All components (Custom Pages, Generative Pages, Forms, Security Roles) MUST be
included in a managed or unmanaged solution for portability and deployment.
No ad-hoc environment changes outside the solution.

### III. Modern UX via Fluent UI (Creator Kit)
Every new page or custom component MUST use Fluent UI controls from the
Power Platform Creator Kit where available (CommandBar, DetailsList, SearchBox, etc.).
Classic Interactive Dashboards are explicitly out of scope and MUST NOT be re-introduced.

### IV. Spec-Driven Development
All feature work MUST follow the spec-kit workflow:
`speckit.constitution` → `speckit.specify` → `speckit.clarify` → `speckit.plan`
→ `speckit.tasks` → `speckit.implement` → `speckit.converge`.
No implementation may begin without an approved spec and plan.

### V. Security-Role Compliance
Every new page/component MUST be accessible to the relevant Dataverse Security Roles.
Security role assignments MUST be validated as part of the implementation checklist.

## Technology Standards

- **App type**: Model-Driven App (Power Apps)
- **Data layer**: Microsoft Dataverse — Contact entity (and related entities)
- **UI components**: Power Platform Creator Kit v0.13+ (Fluent UI)
- **Page type**: Custom Page (canvas) or Generative Page
- **Deployment**: Solution-based (managed for production, unmanaged for development)
- **Tool**: GitHub Copilot CLI with spec-kit v0.13.0 integration

## Development Workflow

1. Spec created via `speckit.specify` → stored in `specs/NNN-feature-name/spec.md`
2. Clarification via `speckit.clarify` (optional but recommended)
3. Technical plan via `speckit.plan` → stored in `specs/NNN-feature-name/plan.md`
4. Task breakdown via `speckit.tasks` → stored in `specs/NNN-feature-name/tasks.md`
5. Implementation via `speckit.implement`
6. Gap analysis via `speckit.converge`
7. Export solution and commit `.zip` artefact to repository

## Governance

This constitution supersedes all other development guidelines for this project.
All PRs must verify compliance with Principles I–V.
Amendments require updating this file and running `speckit.constitution` to propagate changes.

**Version**: 1.0.0 | **Ratified**: 2026-07-19 | **Last Amended**: 2026-07-19
