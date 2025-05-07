# SharePoint Online Lab Exercises - LAB-08

## Lab 1: Managing Content in SharePoint Online
### Objective:
Learn how to manage content effectively in SharePoint Online and prepare it for advanced processing with SharePoint Syntex.

### Steps:
1. **Organize Content**:
   - Navigate to a SharePoint site and create a document library named **Contracts**.
   - Add folders for different contract types (e.g., Employment Contracts, Vendor Contracts).
   - Upload sample documents to the library.
2. **Enable Versioning**:
   - Go to **Library Settings** > **Versioning Settings**.
   - Enable version history and set the number of versions to retain (e.g., 10).
   - Save the changes.
3. **Apply Metadata**:
   - Add metadata columns to the library (e.g., Contract Type, Expiration Date).
   - Tag documents with appropriate metadata values.

---

## Lab 2: Overview of SharePoint Syntex
### Objective:
Understand the purpose and capabilities of SharePoint Syntex.

### Steps:
1. **What is SharePoint Syntex?**:
   - Discuss how SharePoint Syntex uses AI to classify and extract metadata from documents.
   - Key features:
     - Document understanding models.
     - Form processing models.
     - Automatic metadata tagging.
2. **Enable SharePoint Syntex**:
   - Navigate to the **Microsoft 365 Admin Center**.
   - Go to **Setup** > **Automate content understanding**.
   - Enable SharePoint Syntex for your tenant.
3. **Create a Content Center**:
   - Navigate to the **SharePoint Admin Center**.
   - Go to **Active Sites** > **Create** > **Content Center**.
   - Name the site **Syntex Content Center** and save.

---

## Lab 3: Advanced Mechanisms of SharePoint Syntex
### Objective:
Learn how to create and use advanced document understanding and form processing models in SharePoint Syntex.

### Steps:
1. **Create a Document Understanding Model**:
   - Navigate to the **Syntex Content Center**.
   - Create a new document understanding model named **Contract Classification**.
   - Upload sample documents (e.g., employment contracts, vendor contracts).
   - Train the model to identify key phrases and patterns (e.g., contract type, expiration date).
   - Publish the model to the **Contracts** library.
2. **Test the Document Understanding Model**:
   - Upload new documents to the **Contracts** library.
   - Verify that the model automatically classifies the documents and extracts metadata.
3. **Create a Form Processing Model**:
   - Navigate to the **Syntex Content Center**.
   - Create a new form processing model named **Invoice Processing**.
   - Upload sample invoices and train the model to extract fields like:
     - Invoice Number
     - Vendor Name
     - Total Amount
   - Publish the model to a document library named **Invoices**.
4. **Test the Form Processing Model**:
   - Upload new invoices to the **Invoices** library.
   - Verify that the model extracts the specified fields and tags the documents with metadata.

---

## Lab 4: Automating Content Management with Syntex
### Objective:
Learn how to automate content management using SharePoint Syntex.

### Steps:
1. **Apply Retention Labels**:
   - Navigate to the **Syntex Content Center**.
   - Create a retention label (e.g., **Retain for 7 Years**) and publish it to the **Contracts** library.
   - Apply the label to documents based on metadata (e.g., Contract Type = Vendor Contract).
2. **Set Up Rules for Metadata Extraction**:
   - Configure rules in the **Contracts** library to trigger workflows based on extracted metadata.
   - Example: Notify the legal team when a contract's expiration date is within 30 days.
3. **Monitor Syntex Performance**:
   - Navigate to the **Syntex Content Center**.
   - Review the performance of document understanding and form processing models.
   - Adjust the models as needed to improve accuracy.

---

These exercises provide hands-on experience with managing content and using advanced features of SharePoint Syntex to automate and enhance content understanding.