# SharePoint Online Lab Exercises - LAB-05

## Lab 1: Planning and Configuring Customizations and Applications
### Objective:
Learn how to plan and configure customizations and applications in SharePoint Online using Power Automate and Power Apps.

### Steps:
1. **Understand Customizations**:
   - Discuss the importance of customizations in SharePoint Online.
   - Examples of customizations:
     - Automating workflows with Power Automate.
     - Building custom forms and applications with Power Apps.
2. **Plan Customizations**:
   - Identify a business scenario (e.g., Leave Request Management, Task Tracking).
   - Define the data structure in SharePoint Online (e.g., lists for storing requests or tasks).
3. **Create a SharePoint List**:
   - Navigate to a SharePoint site.
   - Create a list named **Leave Requests** with the following columns:
     - **Employee Name** (Single line of text)
     - **Leave Type** (Choice: Vacation, Sick, Other)
     - **Start Date** (Date and Time)
     - **End Date** (Date and Time)
     - **Approval Status** (Choice: Pending, Approved, Rejected)

---

## Lab 2: Overview of Automation and Applications
### Objective:
Understand the mechanisms of automation and applications in SharePoint Online.

### Steps:
1. **Discuss Automation**:
   - Explain how Power Automate can be used to automate workflows in SharePoint Online.
   - Examples:
     - Sending approval requests.
     - Notifying users of changes in a list or library.
2. **Discuss Applications**:
   - Explain how Power Apps can be used to build custom applications.
   - Examples:
     - Creating a mobile-friendly form for submitting leave requests.
     - Building a dashboard for tracking tasks.

---

## Lab 3: Power Automate – Overview and Application
### Objective:
Learn how to create and use Power Automate flows with SharePoint Online.

### Steps:
1. **Create an Approval Flow**:
   - Navigate to **Power Automate** (https://flow.microsoft.com).
   - Create a new flow using the **Automated Cloud Flow** template.
   - Trigger: **When an item is created** in the **Leave Requests** list.
   - Actions:
     - **Start and wait for an approval**:
       - Approval type: **Approve/Reject – First to respond**.
       - Title: **Leave Request Approval**.
       - Assigned to: Manager's email.
     - **Update item**:
       - Update the **Approval Status** column in the **Leave Requests** list based on the approval outcome.
   - Save and test the flow by creating a new item in the **Leave Requests** list.
2. **Notify Employee of Approval Outcome**:
   - Add an action to send an email to the employee with the approval outcome.

---

## Lab 4: Power Apps – Overview and Application
### Objective:
Learn how to create a canvas app using Power Apps to interact with SharePoint Online data.

### Steps:
1. **Create a Canvas App**:
   - Navigate to **Power Apps** (https://make.powerapps.com).
   - Create a new **Canvas App** from blank.
   - Connect the app to the **Leave Requests** SharePoint list.
2. **Design the App**:
   - Add a form to the app for submitting leave requests:
     - Fields: **Employee Name**, **Leave Type**, **Start Date**, **End Date**.
     - Use the **SubmitForm** function to save data to the SharePoint list.
   - Add a gallery to display existing leave requests:
     - Connect the gallery to the **Leave Requests** list.
     - Display columns like **Employee Name**, **Leave Type**, and **Approval Status**.
3. **Integrate with Power Automate**:
   - Add a button to the app to trigger the approval flow created in Lab 3.
   - Use the **Power Automate** connector to call the flow from the app.
4. **Test the App**:
   - Publish the app and test it by submitting a leave request.
   - Verify that the approval flow is triggered and the **Approval Status** is updated in the SharePoint list.

---

## Lab 5: Advanced Customizations
### Objective:
Explore advanced customizations using Power Automate and Power Apps.

### Steps:
1. **Create Conditional Workflows**:
   - Modify the approval flow to include conditional logic:
     - If the leave type is **Sick**, auto-approve the request.
     - If the leave type is **Vacation**, send it for manager approval.
2. **Enhance the Canvas App**:
   - Add filtering and sorting options to the gallery.
   - Use conditional formatting to highlight requests based on their approval status.
3. **Secure the App**:
   - Use role-based access control to restrict access to certain features of the app (e.g., only managers can approve requests).

---

These exercises provide hands-on experience with planning, configuring, and using Power Automate and Power Apps to create custom workflows and applications integrated with SharePoint Online.