# Feature Specification: Contact Dashboard Modernisation

**Feature Branch**: `001-contact-dashboard-modernisation`
**Created**: 2026-07-19
**Status**: Draft
**Input**: Replace the Contact Interactive Dashboard in the Model-Driven App with a modern Custom Page using Creator Kit (DetailsList, SearchBox, CommandBar).

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Browse and Search Active Contacts (Priority: P1)

As a sales representative, I want to view a modern, filterable list of active Contacts — including full name, email, phone, account name, and last activity date — so that I can quickly find and act on contact records without navigating the old dashboard.

**Why this priority**: This is the core replacement of the Interactive Dashboard; all other stories depend on data being visible.

**Independent Test**: Open the new Custom Page in the Model-Driven App and verify that the Contacts table loads with the required fields. Apply a search filter and confirm results narrow accordingly.

**Acceptance Scenarios**:

1. **Given** I am on the Contact Dashboard Custom Page, **When** the page loads, **Then** all active Contacts are displayed in a DetailsList with columns: Full Name, Email, Phone, Account Name, Last Activity Date.
2. **Given** the list is visible, **When** I type in the SearchBox, **Then** the list filters to Contacts whose name or email contains the search term.
3. **Given** no Contacts match the filter, **When** the search returns empty, **Then** a friendly empty-state message is shown.

---

### User Story 2 - View Contacts by Status via Chart (Priority: P2)

As a sales manager, I want to see a chart of Contacts grouped by status so that I can monitor pipeline health at a glance.

**Acceptance Scenarios**:

1. **Given** I am on the Contact Dashboard Custom Page, **When** the page loads, **Then** a chart is displayed showing the count of Contacts broken down by Status (Active, Inactive, etc.).
2. **Given** I apply a filter in the SearchBox, **When** the list updates, **Then** the chart updates to reflect only the filtered Contacts.

---

### User Story 3 - Navigate to a Contact Record (Priority: P2)

As a sales representative, I want to click on a Contact in the list and open their full record so that I can take action (edit, call, email).

**Acceptance Scenarios**:

1. **Given** the Contacts list is visible, **When** I click a Contact row, **Then** the Contact's Model-Driven form opens in the same app.
2. **Given** the form opens, **When** I navigate back, **Then** I return to the Contact Dashboard Custom Page.

---

### User Story 4 - Perform Quick Actions via CommandBar (Priority: P3)

As a sales representative, I want a CommandBar above the list with a "New Contact" button so that I can create a record without leaving the dashboard.

**Acceptance Scenarios**:

1. **Given** the CommandBar is visible, **When** I click "New Contact", **Then** the new Contact form opens.
2. **Given** I save the new Contact, **When** I return to the dashboard, **Then** the new Contact appears in the list.

---

### Edge Cases

- What happens when Dataverse returns 0 Contacts? → Show empty-state text "No contacts found."
- What happens when the user lacks read access to the Contact table? → Show a permission error message; do not crash the page.
- What happens when Creator Kit is not installed in the environment? → Document as a prerequisite; page will fail to load without it.

---

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The Custom Page MUST display the Dataverse Contact entity fields: Full Name, Email (Email Address 1), Business Phone, Account Name (via lookup), and Last Activity Date (or Modified On as fallback).
- **FR-002**: The Custom Page MUST include a SearchBox that filters the Contact list client-side or via Power Fx delegation.
- **FR-003**: The Custom Page MUST include a Creator Kit DetailsList as the primary contact list control.
- **FR-004**: The Custom Page MUST include a Creator Kit CommandBar with at minimum a "New Contact" button.
- **FR-005**: Clicking a row in the DetailsList MUST navigate to the Contact's Model-Driven form.
- **FR-006**: The Custom Page MUST be added to the Model-Driven App sitemap in place of the removed Interactive Dashboard.
- **FR-007**: The Custom Page MUST be included in the solution (unmanaged for development, managed for deployment).
- **FR-008**: All relevant Security Roles that previously had access to the Interactive Dashboard MUST be granted access to the new Custom Page.

### Key Entities *(include if feature involves data)*

- **Contact**: Standard Dataverse entity. Key attributes used: `fullname`, `emailaddress1`, `telephone1`, `parentaccountid` (lookup → Account.name), `modifiedon` / `lastusedincampaign`.
- **Account**: Related entity, referenced via Contact's `parentaccountid` lookup to display Account Name.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The Interactive Dashboard is removed from the sitemap and no longer accessible via app navigation.
- **SC-002**: The Contact Dashboard Custom Page loads in under 3 seconds with up to 500 records (standard Dataverse gallery delegation).
- **SC-003**: All five required columns (Full Name, Email, Phone, Account Name, Last Activity Date) are visible in the DetailsList.
- **SC-004**: SearchBox successfully filters the list for any typed string.
- **SC-005**: Clicking a row opens the correct Contact record form.
- **SC-006**: The new page is accessible to all Security Roles that previously accessed the dashboard.
- **SC-007**: The solution containing the Custom Page exports without errors.

---

## Assumptions

- Creator Kit v0.13+ is already installed in the target Dataverse environment.
- The user performing setup has System Customizer or System Administrator rights.
- The existing Interactive Dashboard is on the Contact entity and is contained within a solution.
- "Last Activity Date" will be represented by `modifiedon` if no dedicated activity-tracking field exists.
- The Model-Driven App is in an unmanaged solution during development.
