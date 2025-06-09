# SharePoint Online Lab Exercises - LAB-05a

## Lab: Automating Approval Workflow with Power Automate
### Objective:
Create a Power Automate flow that triggers an approval process when a new item is added to a SharePoint list.

---

### Step 1: Create a SharePoint List
1. Navigate to your SharePoint site.
2. Click **Settings** (gear icon) > **Site Contents** > **New** > **List**.
3. Name the list **Approval Requests**.
4. Add the following columns to the list:
   - **Title** (Single line of text) – Default column.
   - **Requester Name** (Single line of text).
   - **Request Details** (Multiple lines of text).
   - **Approval Status** (Choice: Pending, Approved, Rejected).
5. Save the list.

---

### Step 2: Connect Power Automate to SharePoint
1. Navigate to **Power Automate** (https://flow.microsoft.com).
2. Click **Create** > **Automated Cloud Flow**.
3. Name the flow **Approval Workflow**.
4. Select the trigger **When an item is created**.
5. Connect to your SharePoint site:
   - Select your **Site Address**.
   - Select the **Approval Requests** list.

---

### Step 3: Build the Approval Flow
1. **Add an Approval Action**:
   - Click **+ New Step**.
   - Search for **Start and wait for an approval**.
   - Select **Approve/Reject – First to respond**.
   - Configure the approval:
     - **Title**: Enter `Approval Request for: @{triggerOutputs()?['body/Title']}`.
     - **Assigned to**: Enter the email address of the approver.
     - **Details**: Enter `Requester: @{triggerOutputs()?['body/RequesterName']} - Details: @{triggerOutputs()?['body/RequestDetails']}`.
2. **Update the SharePoint List**:
   - Click **+ New Step**.
   - Search for **Update item**.
   - Configure the action:
     - **Site Address**: Select your SharePoint site.
     - **List Name**: Select **Approval Requests**.
     - **ID**: Select `ID` from the dynamic content.
     - **Approval Status**: Use the following expression:
       - `if(equals(outputs('Start_and_wait_for_an_approval')?['body/outcome'], 'Approve'), 'Approved', 'Rejected')`.
3. **Send Notification** (Optional):
   - Click **+ New Step**.
   - Search for **Send an email (V2)**.
   - Configure the email:
     - **To**: Enter the requester’s email address.
     - **Subject**: Enter `Your request has been @{outputs('Start_and_wait_for_an_approval')?['body/outcome']}`.
     - **Body**: Include the approval status and any comments.

---

### Step 4: Test the Flow
1. Navigate to the **Approval Requests** list in SharePoint.
2. Add a new item:
   - **Title**: Test Request.
   - **Requester Name**: Your name.
   - **Request Details**: Example details.
   - **Approval Status**: Leave as **Pending**.
3. Verify the flow execution:
   - Go to **Power Automate** > **My Flows** > **Approval Workflow** > **Run History**.
   - Confirm that the flow triggered successfully.
4. Check the approver’s email inbox for the approval request.
5. Approve or reject the request.
6. Verify that the **Approval Status** column in the SharePoint list updates accordingly.

---

### Step 5: Monitor and Optimize
1. Navigate to **Power Automate** > **My Flows**.
2. Open the **Approval Workflow** and review the run history for errors or delays.
3. Optimize the flow by adding conditions or additional actions as needed.

---