# Replacing a Dataverse Interactive Dashboard with a Modern Component

## Context

This guide covers the full process of replacing a classic **Dataverse Interactive Dashboard** (Contact entity) in a Model-Driven App with a modern **Generative Page** (AI-assisted) or a **Custom Page** (manual canvas).

---

## Prerequisites

- Access to **Power Apps** (make.powerapps.com)
- A **Model-Driven App** containing the existing Interactive Dashboard
- Dataverse environment with the **Contact** entity
- (For Generative Page) Microsoft Copilot enabled on your tenant

---

## Phase 1 – Remove the Interactive Dashboard

### Step 1: Open the Model-Driven App in the App Designer

1. Go to [make.powerapps.com](https://make.powerapps.com)
2. Select your **environment** (top-right corner)
3. In the left navigation, click **Apps**
4. Find your Model-Driven App and click **Edit** (opens App Designer)

### Step 2: Remove the Dashboard from the Sitemap

1. In App Designer, open the **Navigation** section (sitemap editor)
2. Locate the group/area that contains the **Contact Interactive Dashboard**
3. Select the dashboard entry and click **Remove** or **Delete**
4. Save the sitemap

### Step 3: Publish the App (interim)

- Click **Publish** to apply the sitemap change before adding the new page
- This avoids conflicts during the new page addition

---

## Phase 2 – Add the Modern Replacement

Choose **Option A** (Generative Page — fast, AI-assisted) or **Option B** (Custom Page — full control).

---

### Option A: Generative Page (Recommended for speed)

> Requires Copilot features enabled in your environment.

#### Step 1: Add a new page via Copilot

1. In App Designer, click **+ Add page**
2. Select **Generative page** (AI-assisted option)
3. In the Copilot prompt field, type a description such as:
   ```
   Show a summary of active Contacts with their full name, email, phone, account name, and last activity date. Include a chart showing contacts by status.
   ```
4. Click **Generate**

#### Step 2: Review the generated page

- Copilot creates a canvas-based page with layout, controls, and data bindings
- Review the generated components (gallery, charts, labels)
- Use the **regenerate** or **refine** options if needed

#### Step 3: Customize (optional)

- Switch to **Edit in Canvas** to refine the layout with Power Fx
- Adjust filters, sorting, colors, and typography to match your branding
- Add any navigation buttons (e.g., "Open Contact Record")

#### Step 4: Add the page to the sitemap

1. Back in App Designer, go to **Navigation**
2. Add the new Generative Page to the area where the old dashboard was
3. Set a display name (e.g., `Contact Overview`)
4. Save the sitemap

---

### Option B: Custom Page (Full control)

#### Step 1: Create a new Custom Page

1. In App Designer, click **+ Add page**
2. Select **Custom page**
3. Give it a name (e.g., `ContactDashboardPage`)
4. Click **Create** — this opens the Canvas editor

#### Step 2: Build the page

1. Connect the **Contacts** Dataverse table as a data source
2. Add components:
   - **Gallery** — list of contacts with key fields
   - **Chart control** — contacts by status/city/account
   - **KPI tiles** — total contacts, new this month, etc.
   - **Search/Filter bar** — filter by status, account, city
3. Use **Power Fx** for dynamic filtering and navigation:
   ```
   Filter(Contacts, Status = "Active")
   ```
4. Add a **Navigate()** action on gallery items to open Contact records

#### Step 3: Save and publish the Custom Page

1. Click **File > Save**
2. Click **Publish**

#### Step 4: Add the page to the sitemap

1. In App Designer > **Navigation**, add the Custom Page
2. Set the display name and icon
3. Save and publish the app

---

## Phase 3 – Validate and Publish

### Step 1: Preview the app

1. In App Designer, click **Play** (preview mode)
2. Navigate to the new page
3. Verify:
   - Contact data loads correctly
   - Charts render as expected
   - Filters and search work
   - Clicking a contact opens the correct form

### Step 2: Check permissions

- Ensure all users/roles that had access to the old dashboard can access the new page
- Review **Security Roles** in the Power Platform Admin Center if needed

### Step 3: Final publish

1. Click **Publish** in App Designer
2. Confirm the old Interactive Dashboard is no longer in the navigation
3. Share the app with your users/teams

---

## Phase 4 – Clean Up (Optional)

### Remove the old Interactive Dashboard definition

1. Go to **make.powerapps.com > Solutions**
2. Open the solution containing your app
3. Find the **Dashboard** component (type: Dashboard) for the Contact entity
4. If it is no longer used anywhere, **delete** it from the solution
5. **Export** the solution (managed or unmanaged) for backup

### Export your solution

1. In Solutions, click **Export solution**
2. Choose **Unmanaged** (for continued development) or **Managed** (for deployment)
3. Click **Next > Export**
4. Save the `.zip` file for the next session

---

## Summary

| Step | Action | Tool |
|---|---|---|
| 1 | Remove dashboard from sitemap | App Designer |
| 2 | Create Generative Page or Custom Page | App Designer + Copilot / Canvas |
| 3 | Add new page to sitemap | App Designer |
| 4 | Validate in preview | App Designer Play |
| 5 | Publish app | App Designer |
| 6 | Clean up old dashboard component | Solutions |
| 7 | Export solution | Solutions |

---

## Notes for Next Session

- Upload your exported solution `.zip` file
- Share the **environment URL** and **solution name**
- Indicate whether you chose Generative Page or Custom Page so refinements can continue

---

## Appendix: Spec-Kit Workflow (Recommended for future features)

This project is configured with **[GitHub Spec Kit](https://github.com/github/spec-kit) v0.13.0** integrated with GitHub Copilot CLI. Use the slash commands below in your Copilot CLI session to drive any future feature work using Spec-Driven Development.

### Quick-Start Commands

```bash
# 1. Establish / update project principles
/speckit.constitution Create principles focused on modern UX using Fluent UI,
Dataverse-first data access, solution-based packaging, and Power Platform best practices.

# 2. Define what you want to build
/speckit.specify Replace the Contact Interactive Dashboard in the Model-Driven App
with a modern Custom Page using Creator Kit (DetailsList, SearchBox, CommandBar).

# 3. Clarify any gaps (optional but recommended)
/speckit.clarify

# 4. Generate a technical plan
/speckit.plan Use Power Apps Custom Page (canvas), Creator Kit Fluent UI components,
Dataverse Contacts table, solution-based deployment.

# 5. Break plan into tasks
/speckit.tasks

# 6. Execute all tasks
/speckit.implement

# 7. Check for remaining gaps after implementation
/speckit.converge
```

### Command Reference

| Command | Description |
|---|---|
| `/speckit.constitution` | Create or update project governing principles |
| `/speckit.specify` | Define what you want to build (requirements & user stories) |
| `/speckit.clarify` | AI asks up to 5 targeted questions to fill spec gaps |
| `/speckit.plan` | Create technical implementation plan |
| `/speckit.tasks` | Generate actionable task list from the plan |
| `/speckit.implement` | Execute all tasks and build the feature |
| `/speckit.converge` | Assess codebase vs spec and append any remaining work |

### Project Constitution

The project constitution is stored at `.specify/memory/constitution.md`. It establishes:
- **Dataverse-First** data access
- **Solution Packaging** for all components
- **Fluent UI / Creator Kit** for all new pages
- **Spec-Driven Development** as the mandatory workflow
- **Security-Role Compliance** for every new component

Run `/speckit.constitution` in any session to view or amend these principles.
