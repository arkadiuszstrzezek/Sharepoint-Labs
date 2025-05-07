# SharePoint Online Lab Exercises - LAB-07

## Lab 1: Managing Content in SharePoint Online
### Objective:
Learn how to manage content effectively in SharePoint Online.

### Steps:
1. **Organize Content**:
   - Navigate to a SharePoint site and create a document library named **Project Documents**.
   - Add folders for different projects (e.g., Project A, Project B).
   - Upload sample documents to the library.
2. **Apply Metadata**:
   - Add metadata columns to the library (e.g., Document Type, Department).
   - Tag documents with appropriate metadata values.
3. **Enable Versioning**:
   - Go to **Library Settings** > **Versioning Settings**.
   - Enable version history and set the number of versions to retain (e.g., 10).
   - Save the changes.

---

## Lab 2: Overview of the Security and Compliance Center
### Objective:
Understand the features of the Microsoft Purview Compliance Portal (formerly Security and Compliance Center).

### Steps:
1. **Access the Compliance Portal**:
   - Navigate to the **Microsoft Purview Compliance Portal** (https://compliance.microsoft.com).
2. **Explore Key Features**:
   - Review sections like:
     - **Information Protection**: Configure sensitivity labels.
     - **Data Loss Prevention**: Create and manage DLP policies.
     - **eDiscovery**: Manage content searches and legal holds.
     - **Audit**: Review audit logs for user and admin activities.
3. **Review Compliance Score**:
   - Go to **Compliance Manager** and review the tenant's compliance score.
   - Identify areas for improvement.

---

## Lab 3: Overview of the eDiscovery Mechanism
### Objective:
Learn how to use the eDiscovery mechanism to search and manage content for legal or compliance purposes.

### Steps:
1. **Create an eDiscovery Case**:
   - Navigate to the **eDiscovery** section in the Compliance Portal.
   - Create a new case named **Legal Case 01**.
2. **Search for Content**:
   - Within the case, create a new content search.
   - Specify search criteria (e.g., keywords, date range, specific SharePoint sites).
   - Run the search and review the results.
3. **Apply a Legal Hold**:
   - Add a legal hold to a specific user or SharePoint site to preserve content.
   - Verify that the hold is applied successfully.
4. **Export Search Results**:
   - Export the search results for further analysis.

---

## Lab 4: Overview of the DLP Mechanism
### Objective:
Learn how to create and manage Data Loss Prevention (DLP) policies in SharePoint Online.

### Steps:
1. **Create a DLP Policy**:
   - Navigate to the **Data Loss Prevention** section in the Compliance Portal.
   - Create a new policy to protect sensitive information (e.g., credit card numbers, personal data).
   - Apply the policy to SharePoint and OneDrive.
2. **Configure Policy Settings**:
   - Set conditions for detecting sensitive information (e.g., content contains credit card numbers).
   - Define actions to take when a policy is violated (e.g., block access, notify users).
   - Save and publish the policy.
3. **Test the Policy**:
   - Upload a document containing sensitive information to a SharePoint library.
   - Verify that the DLP policy is triggered and the appropriate actions are taken.

---

## Lab 5: Data Classification and Management
### Objective:
Learn how to classify and manage data in SharePoint Online.

### Steps:
1. **Create Sensitivity Labels**:
   - Navigate to the **Information Protection** section in the Compliance Portal.
   - Create sensitivity labels (e.g., Confidential, Internal, Public).
   - Configure label settings, such as encryption and access restrictions.
2. **Publish Sensitivity Labels**:
   - Create a label policy and publish the labels to SharePoint and OneDrive.
3. **Apply Sensitivity Labels**:
   - Navigate to a SharePoint library and apply a sensitivity label to a document.
   - Verify that the label's settings (e.g., encryption) are applied.
4. **Monitor Data Classification**:
   - Go to **Data Classification** in the Compliance Portal.
   - Review the overview of sensitive information types and labeled content.

---

These exercises provide hands-on experience with managing content, ensuring compliance, and protecting sensitive data in SharePoint Online.