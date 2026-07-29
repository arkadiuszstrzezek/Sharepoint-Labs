# SharePoint Online Lab Exercises - LAB-07

> **Running scenario:** Contoso Consulting's legal team just heard about the `Fabrikam Project` site and asked three questions: "What if someone overwrites a file by mistake? What if someone emails a client's credit card number by accident? And what if we get sued and need to freeze everything?" This lab answers all three.

## Lab 1: Managing Content Lifecycle in SharePoint Online
### Objective:
Learn how versioning protects content before you even get to compliance tooling.

### Steps:
1. **Organize content:**
   - On `Fabrikam Project` (from LAB-03), create a document library named **Project Documents** with folders for **Contracts** and **Deliverables**.
2. **Apply metadata (recap from LAB-04):**
   - Add **Document Type** and **Department** columns and tag a few sample documents.
3. **Enable and test versioning:**
   - Go to **Library Settings** > **Versioning Settings**, enable version history, retain **10** major versions.
   - Upload a document, edit it three times, then open **Version History** and restore an earlier version. Confirm this is a self-service undo — no admin ticket needed.
4. **Break it on purpose:**
   - Delete the current version of a file entirely (not the whole document, just via version history if your plan allows) and restore it — this is the difference between "oops, wrong edit" (versioning) and "the file is gone" (next: legal hold / recycle bin), which you'll contrast in Lab 3.

### Knowledge check:
Versioning protects against accidental overwrites. What does it *not* protect against that a determined bad actor could still do?

---

## Lab 2: Overview of the Microsoft Purview Compliance Portal
### Objective:
Get oriented in Microsoft Purview before diving into any one tool.

### Steps:
1. **Access the portal:**
   - Navigate to the **Microsoft Purview portal** (https://purview.microsoft.com).
2. **Tour the relevant sections:**
   - **Information Protection** — sensitivity labels (Lab 5).
   - **Data Loss Prevention** — DLP policies (Lab 4).
   - **eDiscovery** — content search and legal holds (Lab 3).
   - **Audit** — who did what, when, across Microsoft 365.
3. **Check the compliance score:**
   - Open **Compliance Manager**, review Contoso's current score, and pick the single highest-impact recommended action — you'll implement one improvement across Labs 3–5.

### Knowledge check:
Of Information Protection, DLP, and eDiscovery, which one is *preventive*, which is *reactive*, and which is *investigative*? Match each to Contoso legal's three original questions.

---

## Lab 3: Overview of the eDiscovery Mechanism
### Objective:
Learn to search, preserve, and export content for legal or investigative purposes — answering "what if we get sued?"

### Steps:
1. **Create an eDiscovery case:**
   - In Purview, go to **eDiscovery** > **eDiscovery (Standard)**, create a case named `Fabrikam Contract Dispute`.
2. **Search for content:**
   - Within the case, create a content search scoped to the `Fabrikam Project` site, with keywords matching your test documents and a date range covering when you created them.
   - Run the search and review the results — note the item count and locations.
3. **Apply a legal hold:**
   - Add a hold to the `Fabrikam Project` site from within the case.
   - **Prove it works:** try to permanently delete one of the held documents (delete it, then empty it from the site recycle bin). Confirm the content is still recoverable/discoverable because of the hold — this is the entire point of a legal hold versus relying on the recycle bin alone.
4. **Export results:**
   - Export the search results and note what's included (content plus a load file with metadata) — this is what would actually go to outside counsel.

### Challenge:
Remove the legal hold once your test is done, and document in one sentence why leaving test holds active is itself a governance problem (hint: think about what it does to Contoso's actual retention/storage posture).

### Knowledge check:
Why is a legal hold a stronger guarantee than telling employees "please don't delete anything related to this case"?

---

## Lab 4: Overview of the DLP Mechanism
### Objective:
Learn to prevent sensitive information from leaving the organization in the first place — answering "what if someone shares a credit card number?"

### Steps:
1. **Create a DLP policy:**
   - In Purview, go to **Data Loss Prevention** > **Policies**, create a new policy using the **Financial data** template (which includes credit card number detection).
   - Scope it to **SharePoint sites** and **OneDrive accounts**; include `Fabrikam Project` explicitly.
2. **Configure detection and action:**
   - Set the condition to detect content containing credit card numbers.
   - Set the action: block external sharing of the matching content, and show a policy tip to the user explaining why.
3. **Test the policy:**
   - Upload a test document containing a fake, clearly-non-real credit card number pattern (e.g., a sample number from Microsoft's own DLP testing guidance) to `Fabrikam Project`.
   - Try to share that document with the external Fabrikam guest account from LAB-03. Confirm the policy tip appears and sharing is blocked or requires override, per your configuration.
4. **Check the audit trail:**
   - Confirm the DLP match shows up in **Activity explorer** — this is how you'd know the policy actually fired in production, not just that you configured it.

### Knowledge check:
DLP and the "Specific people" sharing habit from LAB-03 both reduce data leakage risk. Why do you still need DLP even if every employee always shares carefully with "Specific people"?

---

## Lab 5: Data Classification and Sensitivity Labels
### Objective:
Learn to classify content so protection travels with the file, wherever it goes — closing the loop on all three of legal's questions.

### Steps:
1. **Create sensitivity labels:**
   - In **Information Protection**, create three labels: `Public`, `Internal`, `Confidential – Client Data`.
   - On `Confidential – Client Data`, configure encryption and restrict access to Contoso employees only (no external access, even with a valid share link).
2. **Publish a label policy:**
   - Create a label policy publishing all three labels to SharePoint and OneDrive, and set `Internal` as the tenant default.
3. **Apply and prove the label works:**
   - On `Fabrikam Project`, apply `Confidential – Client Data` to a document containing client financial details.
   - Try to open that document as an unauthenticated/external user (or simulate by removing the Fabrikam guest's access first) — confirm the encryption actually blocks access, not just that a label tag is showing.
4. **Monitor classification tenant-wide:**
   - Go to **Data Classification** in Purview and review the overview of labeled vs. unlabeled content — this is how you'd measure whether Contoso's rollout is actually being adopted, not just configured.

### Challenge:
Go back to Compliance Manager (Lab 2) and mark your chosen recommended action as implemented, using the DLP policy or sensitivity label you just built as the evidence. Watch the compliance score respond.

### Knowledge check:
A sensitivity label follows the file even if it's downloaded and emailed. A SharePoint permission does not. Given that, why isn't sensitivity labeling a full replacement for careful site/library permissions?

---

These exercises give hands-on experience managing content lifecycle and using Microsoft Purview to prevent, detect, and investigate data risk in SharePoint Online — the three questions every legal and security team eventually asks.
