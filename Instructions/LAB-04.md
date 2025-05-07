# SharePoint Online Lab Exercises - LAB-04

## Lab 1: Planning and Configuring Metadata
### Objective:
Understand the concept of metadata and learn how to plan and configure metadata in SharePoint Online.

### Steps:
1. **Understand Metadata**:
   - Discuss what metadata is and its importance in organizing and classifying content.
   - Examples of metadata:
     - Document type
     - Author
     - Created date
     - Department
2. **Plan Metadata for a Document Library**:
   - Identify key metadata fields for a document library (e.g., Project Name, Document Type, Status).
   - Discuss how metadata can improve search and filtering.
3. **Configure Metadata Columns**:
   - Navigate to a document library in a SharePoint site.
   - Go to **Library Settings** > **Create Column**.
   - Create the following columns:
     - **Project Name** (Single line of text)
     - **Document Type** (Choice: Proposal, Report, Invoice)
     - **Status** (Choice: Draft, In Review, Final)
   - Save the changes.

---

## Lab 2: What Are Metadata?
### Objective:
Understand the concept of metadata and its role in SharePoint Online.

### Steps:
1. **Definition of Metadata**:
   - Metadata is data that provides information about other data.
   - Examples in SharePoint:
     - File name
     - Modified date
     - Custom columns (e.g., Department, Category)
2. **Benefits of Metadata**:
   - Improves searchability and filtering.
   - Enables better organization of content.
   - Facilitates automation and workflows.
3. **Explore Default Metadata**:
   - Navigate to a document library in a SharePoint site.
   - Review default metadata fields like **Name**, **Modified**, and **Modified By**.

---

## Lab 3: Term Store Overview and Usage
### Objective:
Learn about the Term Store in SharePoint Online and its applications.

### Steps:
1. **Access the Term Store**:
   - Navigate to the **SharePoint Admin Center**.
   - Go to **Content Services** > **Term Store**.
2. **Create a Term Group**:
   - In the Term Store, create a new term group (e.g., "Company Metadata").
   - Add a description for the group.
3. **Create a Term Set**:
   - Within the term group, create a term set (e.g., "Departments").
   - Add terms to the set (e.g., HR, IT, Marketing, Finance).
4. **Apply Managed Metadata to a Library**:
   - Navigate to a document library in a SharePoint site.
   - Go to **Library Settings** > **Create Column**.
   - Create a column of type **Managed Metadata**.
   - Link the column to the term set created in the Term Store.
5. **Tag Documents with Metadata**:
   - Upload a document to the library.
   - Edit the document properties and assign values from the managed metadata term set.

---

These exercises provide hands-on experience with planning, configuring, and using metadata and the Term Store in SharePoint Online.