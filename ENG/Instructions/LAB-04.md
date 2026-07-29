# SharePoint Online Lab Exercises - LAB-04

> **Running scenario:** Contoso Consulting's `Fabrikam Project` site (from LAB-03) is filling up with documents, and people can't find anything. Your job in this lab is to fix that with metadata — first free-text columns, then a governed, tenant-wide taxonomy via the Term Store.

## Lab 1: What Is Metadata, and Why Does It Matter?
### Objective:
Understand what metadata is and why it beats folders for organizing content at scale.

### Steps:
1. **Look at metadata you already have:**
   - Open the `Fabrikam Project` document library.
   - Switch to **All Documents** view and note the built-in metadata columns: **Name**, **Modified**, **Modified By**, **File size**. These exist on every file automatically — metadata is *data about data*, not something you have to invent from scratch.
2. **Feel the pain folders cause:**
   - Create two folders: `Proposals` and `Reports`.
   - Upload the same sample document into both, once as `Proposal-Draft.docx` and once as `Report-Draft.docx`.
   - Now try to answer: "show me every Draft document, regardless of type." With folders alone, you can't — you'd have to open both folders and eyeball it.
3. **Plan the fix:**
   - Instead of more folders, list 3 metadata fields that would let you filter/search across the whole library at once: e.g., **Document Type**, **Status**, **Client**.

### Knowledge check:
Give one concrete search or filter you can do with metadata columns that you fundamentally cannot do with folders alone.

---

## Lab 2: Configuring Metadata Columns
### Objective:
Turn the plan from Lab 1 into real, working columns on the `Fabrikam Project` library.

### Steps:
1. **Create the columns:**
   - Go to the `Fabrikam Project` document library > **Library Settings** (or the **+ Add column** button in the list view) > **Create Column**.
   - Create:
     - **Document Type** (Choice: Proposal, Report, Invoice, Contract)
     - **Status** (Choice: Draft, In Review, Final)
     - **Client** (Single line of text — you'll fix this in Lab 3)
2. **Tag the existing documents:**
   - Go back to `Proposal-Draft.docx` and `Report-Draft.docx`, edit their properties, and fill in the new columns.
3. **Prove the value:**
   - Create a new view (or use **Group by**) that groups documents by **Status**. Now "show me everything still in Draft" is one click, across every folder, every document type.
4. **Add a default value:**
   - Set **Status**'s default value to `Draft`, so every new upload starts correctly tagged without anyone remembering to set it.

### Challenge:
Delete the two folders from Lab 1 entirely and rely only on metadata views (grouped/filtered by Document Type, Status) to browse the library. Notice this is the real recommendation Microsoft makes for modern libraries — flat structure, metadata-driven views.

### Knowledge check:
Why is a **default value** on a Choice column a small governance win, not just a convenience?

---

## Lab 3: Term Store Overview and Usage
### Objective:
Learn why free-text columns like `Client` (Lab 2) are a trap, and how the Term Store fixes it with a managed, reusable taxonomy.

### Steps:
1. **See the trap first:**
   - Go back to the `Client` column values you entered in Lab 2. If two people typed `Fabrikam`, `Fabrikam Inc`, and `Fabrikam Inc.` respectively, you now have three "different" clients in search and filters. This is exactly the problem Managed Metadata solves.
2. **Access the Term Store:**
   - Navigate to the **SharePoint Admin Center** > **Content services** > **Term store**.
3. **Create a term group:**
   - Create a new term group named `Contoso Metadata`, and add a short description.
4. **Create term sets:**
   - Within `Contoso Metadata`, create a term set named `Clients` with terms: `Fabrikam Inc.`, `Northwind Traders`.
   - Create a second term set named `Departments` with terms: `HR`, `IT`, `Marketing`, `Finance`.
5. **Replace the free-text column:**
   - On `Fabrikam Project`, create a new column of type **Managed Metadata** named `Client (Managed)`, linked to the `Clients` term set.
   - Migrate the library's existing documents to use it instead of the old free-text `Client` column, then delete the free-text column.
6. **Tag and test:**
   - Upload a new document and tag it using the managed metadata picker (notice it now enforces consistent spelling — no more typing).
   - Search for `Fabrikam` in the site search box and confirm the managed-metadata-tagged document surfaces reliably.

### Challenge:
Add a synonym to the `Fabrikam Inc.` term (e.g., "Fabrikam") in the Term Store, and confirm that searching the synonym still finds documents tagged with the canonical term. This is the specific problem synonyms solve that free-text columns never could.

### Knowledge check:
Your `HR`, `IT`, and `Marketing` sites from LAB-02 could all reuse the same `Departments` term set. What would go wrong if each site instead created its own local `Departments` Choice column?

---

These exercises build real judgment about *when* a simple column is enough and *when* you actually need the Term Store — a decision every SharePoint admin makes constantly.
