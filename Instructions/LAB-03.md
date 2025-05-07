# SharePoint Online Lab Exercises - LAB-03

## Lab 1: Configuring External Sharing
### Objective:
Learn how to configure external sharing settings in SharePoint Online.

### Steps:
1. **Enable External Sharing**:
   - Navigate to the **SharePoint Admin Center**.
   - Go to **Policies** > **Sharing**.
   - Set the sharing level to one of the following:
     - **Anyone**: Allows sharing with anyone, including anonymous users.
     - **New and Existing Guests**: Allows sharing with authenticated external users.
     - **Existing Guests Only**: Allows sharing only with users already in the directory.
     - **Only People in Your Organization**: Disables external sharing.
   - Save the changes.
2. **Configure Site-Level Sharing**:
   - Navigate to **Active Sites** in the SharePoint Admin Center.
   - Select a site and click **Sharing**.
   - Set the sharing level for the site (e.g., **Anyone**, **New and Existing Guests**).
   - Save the changes.

---

## Lab 2: Types of External Sharing Mechanisms
### Objective:
Understand the different mechanisms for external sharing in SharePoint Online.

### Steps:
1. **Share a File or Folder**:
   - Navigate to a document library in a SharePoint site.
   - Select a file or folder and click **Share**.
   - Choose one of the following sharing options:
     - **Anyone with the link**: Generates an anonymous link.
     - **People in your organization**: Restricts access to internal users.
     - **Specific people**: Allows sharing with specific external users.
   - Send the sharing link to an external user.
2. **Share a Site**:
   - Navigate to a SharePoint site.
   - Go to **Site Permissions** > **Invite People** > **Share Site Only**.
   - Enter the email address of an external user and assign a role (e.g., Member, Visitor).
   - Send the invitation.

---

## Lab 3: Managing Content Sharing with Entra ID and SharePoint Online
### Objective:
Learn how to manage external sharing using Entra ID (Azure AD) and SharePoint Online.

### Steps:
1. **Configure Guest Access in Entra ID**:
   - Navigate to the **Azure Active Directory Admin Center**.
   - Go to **External Identities** > **External Collaboration Settings**.
   - Configure the following settings:
     - **Guest user access restrictions**: Allow or block specific actions for guest users.
     - **Collaboration restrictions**: Restrict sharing to specific domains.
   - Save the changes.
2. **Monitor External Sharing**:
   - Navigate to the **SharePoint Admin Center**.
   - Go to **Reports** > **Sharing Activity**.
   - Review the sharing activity for external users.
3. **Revoke Access for External Users**:
   - Navigate to the **Azure Active Directory Admin Center**.
   - Go to **Users** > **Guest Users**.
   - Select a guest user and remove their access to specific resources.
4. **Remove Sharing Links**:
   - Navigate to a document library in a SharePoint site.
   - Select a file or folder and click **Manage Access**.
   - Remove any external sharing links.

---

These exercises provide hands-on experience with configuring and managing external sharing in SharePoint Online and Entra ID.