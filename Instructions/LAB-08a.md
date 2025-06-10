# SharePoint Online Lab Exercises - LAB-08a

## Lab 1: Overview of SharePoint Premium Advanced Management
### Objective:
Understand the features and capabilities of SharePoint Premium Advanced Management.

### Steps:
1. **What is SharePoint Premium Advanced Management?**:
   - Discuss the purpose of Premium Advanced Management:
     - Enhanced security and compliance features.
     - Advanced content management capabilities.
     - Improved monitoring and auditing tools.
   - Key features include:
     - Conditional access policies.
     - Advanced sharing controls.
     - Enhanced site lifecycle management.
2. **Enable Premium Advanced Management**:
   - Navigate to the **Microsoft 365 Admin Center**.
   - Go to **Billing** > **Products**.
   - Ensure that the **SharePoint Premium Advanced Management** license is assigned to your tenant.

---

## Lab 2: Navigating SharePoint Premium Advanced Management Settings
### Objective:
Explore and configure all settings available in SharePoint Premium Advanced Management.

### Steps:
1. **Access Advanced Management Settings**:
   - Navigate to the **SharePoint Admin Center**.
   - Go to **Settings** > **Advanced Management**.
2. **Configure Conditional Access Policies**:
   - Go to **Access Control** > **Conditional Access**.
   - Set up policies to restrict access based on:
     - Device compliance.
     - Location (e.g., block access from specific countries).
     - Session controls (e.g., limit session duration).
   - Save the changes.
3. **Enable Advanced Sharing Controls**:
   - Go to **Sharing** > **Advanced Sharing Settings**.
   - Configure settings such as:
     - Expiration dates for sharing links.
     - Restrict sharing to specific domains.
     - Require authentication for external sharing.
   - Save the changes.
4. **Configure Site Lifecycle Management**:
   - Go to **Site Management** > **Lifecycle Policies**.
   - Set up policies for:
     - Automatic site deletion after inactivity.
     - Notifications for site owners before deletion.
   - Save the changes.
5. **Enable Enhanced Auditing**:
   - Go to **Monitoring** > **Audit Logs**.
   - Enable advanced auditing for SharePoint activities, such as:
     - File access and modifications.
     - Sharing and permission changes.
     - Site creation and deletion.
   - Save the changes.

---

## Lab 3: Testing and Monitoring Advanced Management Features
### Objective:
Test the configured settings and monitor their impact on SharePoint Online.

### Steps:
1. **Test Conditional Access Policies**:
   - Attempt to access a SharePoint site from a non-compliant device or restricted location.
   - Verify that access is blocked or limited based on the configured policies.
2. **Test Advanced Sharing Controls**:
   - Share a document with an external user.
   - Verify that the sharing link expires as per the configured settings.
   - Attempt to share with a domain not allowed by the sharing restrictions.
3. **Test Site Lifecycle Management**:
   - Create a test site and leave it inactive.
   - Verify that notifications are sent to the site owner before automatic deletion.
4. **Monitor Audit Logs**:
   - Navigate to the **Microsoft Purview Compliance Portal**.
   - Go to **Audit** > **Search**.
   - Review logs for activities such as file access, sharing, and site management.

---

## Lab 4: Optimizing Advanced Management Settings
### Objective:
Learn how to optimize SharePoint Premium Advanced Management settings for your organization.

### Steps:
1. **Review Usage Reports**:
   - Navigate to the **Microsoft 365 Admin Center**.
   - Go to **Reports** > **Usage**.
   - Review reports for SharePoint activity and identify areas for improvement.
2. **Adjust Policies**:
   - Based on usage data, refine conditional access and sharing policies.
   - Example: Allow sharing with trusted external domains while blocking others.
3. **Train Administrators**:
   - Provide training for administrators on using advanced management features.
   - Share best practices for monitoring and managing SharePoint sites.

---

This lab provides hands-on experience with configuring and testing SharePoint Premium Advanced Management features.