# SharePoint Online Lab Exercises - LAB-05a

> **Running scenario:** Continuing from LAB-05, Contoso Fleet's booking app lets anyone reserve a vehicle for any length of time — including two weeks, which starves everyone else. You'll add a real approval workflow with Power Automate: short reservations go through automatically, long ones need a manager's sign-off.

## Lab: Automating an Approval Workflow with Power Automate
### Objective:
Build a Power Automate flow that triggers a manager approval only when a business rule is met (reservation longer than 3 days) — not on every single item, which is what makes this a realistic flow instead of a toy one.

---

### Step 1: Extend the SharePoint List
1. Navigate to the `Contoso Fleet` site (from LAB-05) and open **Vehicle Reservations**.
2. Add a column: **Approval Status** (Choice: Pending, Approved, Rejected, Not Required), default value `Pending`.
3. If you don't already have it from LAB-05, also confirm the list has **StartDate** and **EndDate** as Date columns — the flow needs to calculate duration from them.

---

### Step 2: Create the Flow
1. Navigate to **Power Automate** (https://make.powerautomate.com).
2. **Create** > **Automated cloud flow**.
3. Name it `Fleet Reservation Approval`.
4. Trigger: **When an item is created** — connect to the `Contoso Fleet` site and the `Vehicle Reservations` list.

---

### Step 3: Only Approve What Actually Needs Approval
1. Add a **Compute** step: use an expression to calculate reservation length in days:
   ```
   div(sub(ticks(triggerOutputs()?['body/EndDate']), ticks(triggerOutputs()?['body/StartDate'])), 864000000000)
   ```
2. Add a **Condition**: `[computed days]` is greater than `3`.
3. **If yes** (long reservation — needs approval):
   - **Start and wait for an approval**:
     - Approval type: **Approve/Reject – First to respond**.
     - Title: `Vehicle reservation for @{triggerOutputs()?['body/Employee']} (@{...} days)`.
     - Assigned to: your fleet manager's email.
     - Details: include vehicle, employee, start/end dates.
   - **Update item**: set **Approval Status** using:
     ```
     if(equals(outputs('Start_and_wait_for_an_approval')?['body/outcome'], 'Approve'), 'Approved', 'Rejected')
     ```
4. **If no** (3 days or fewer — auto-approved):
   - **Update item**: set **Approval Status** to `Not Required`, skipping the manager entirely.
5. **Send an email (V2)** to the requester in both branches, using `@{outputs('Start_and_wait_for_an_approval')?['body/outcome']}` in the approval branch and a fixed "auto-approved" message in the other.

---

### Step 4: Test Both Paths
1. In `Vehicle Reservations`, add a **1-day** reservation. Confirm in **Power Automate** > **My flows** > **Fleet Reservation Approval** > **Run history** that it took the "no approval needed" branch and the item shows `Not Required` immediately.
2. Add a **5-day** reservation. Confirm the manager gets an approval request, and that approving/rejecting it updates **Approval Status** correctly and emails the requester.
3. Deliberately test a **boundary case**: exactly 3 days. Confirm it behaves the way your condition actually specifies (`greater than 3`, not `greater than or equal`) — boundary conditions are where real flows break in production.

---

### Step 5: Monitor and Harden
1. In **Run history**, open one successful and one (deliberately caused) failed run — try submitting a reservation with a blank **EndDate** to see how the flow handles bad data.
2. Add a check at the top of the flow (a **Condition** before the duration math) that skips gracefully or notifies you if `EndDate` is empty, instead of letting the flow error out.
3. Note the flow's connection: it runs under *your* identity by default. Discuss with the group what happens to this flow the day you leave the company — this is exactly why real deployments use a **service/dedicated account** or a **solution with an application user**, not a personal login.

### Challenge:
Add a second condition branch: if the same vehicle already has an **Approved** reservation overlapping the new request's dates, auto-reject with a clear message instead of even asking the manager — the flow should never bother a human with a request that's already impossible.

### Knowledge check:
Why does triggering the approval on a *condition* (duration > 3 days) make this a more realistic training example than approving every single item unconditionally? What's the operational cost of the "approve everything" version at a company with 300 employees?

---
