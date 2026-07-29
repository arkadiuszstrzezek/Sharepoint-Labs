# SharePoint Online Lab Exercises - LAB-08

> **Running scenario:** Contoso Consulting's `Contracts` library is growing fast, and someone has to open every single contract manually to know what type it is and when it expires. This is exactly the manual-tagging problem **SharePoint Premium** (the AI content services previously branded **SharePoint Syntex**) exists to solve.

> ⚠️ **Naming note, current as of 2026:** Microsoft rebranded SharePoint Syntex to **SharePoint Premium** in late 2023, then in 2024 renamed the pay-as-you-go, consumption-billed components specifically to **document processing services**. You'll still see "Syntex" in some menus, docs, and community content — treat it as the same underlying capability under a newer umbrella name, not a different product.

## Lab 1: Managing Content Before Automating It
### Objective:
Set up a realistic, messy content library — the kind SharePoint Premium is actually meant to be pointed at.

### Steps:
1. **Organize content:**
   - On a site named `Contoso Legal`, create a document library named **Contracts**.
   - Add folders for **Employment Contracts** and **Vendor Contracts**.
   - Upload 5–10 sample contract-style documents (real templates, or generate simple mock ones) with varying content.
2. **Enable versioning (recap from LAB-07):**
   - Enable version history, retain 10 versions.
3. **Apply metadata manually first:**
   - Add **Contract Type** and **Expiration Date** columns and tag every document *by hand*.
   - Time yourself. Write down how long it took for just 10 documents — you'll compare this against the automated result in Lab 3.

### Knowledge check:
If Contoso has 10,000 contracts instead of 10, what breaks about the "tag it by hand" approach besides just "it's slow"?

---

## Lab 2: Overview of SharePoint Premium (Content Understanding)
### Objective:
Understand what SharePoint Premium actually does and the licensing reality behind it.

### Steps:
1. **What is it?**
   - Discuss: SharePoint Premium uses AI models to read documents and automatically populate metadata columns — turning Lab 1's manual tagging into an automatic step on upload.
   - Key capability areas: **document understanding models** (classify + extract from your own document types), **prebuilt/form processing models** (invoices, receipts, contracts out of the box), and **autofill columns** (natural-language column definitions, no training needed for simpler cases).
2. **Check licensing and billing before touching anything:**
   - Navigate to the **Microsoft 365 Admin Center** > **Billing** > **Your products**, and confirm what's included in Contoso's current plan.
   - Note the real gotcha: beyond a limited included volume, SharePoint Premium's consumption-based features require an **Azure subscription** linked for pay-as-you-go billing — this is *not* a simple admin-center toggle anymore, it's a cross-portal setup involving Azure. Budget owners need to be looped in before you flip anything on.
3. **Enable content understanding:**
   - In the **SharePoint Admin Center**, look for **Content services** (or **Premium**/**Syntex**, depending on your tenant's current label) and confirm it's enabled.
   - If your tenant hasn't linked an Azure subscription yet, document that as a blocker rather than guessing at workarounds — this is a real conversation with IT finance, not a lab shortcut.

### Knowledge check:
Why might Microsoft deliberately tie an AI content feature to consumption-based Azure billing instead of bundling it flatly into the SharePoint license?

---

## Lab 3: Building a Document Understanding Model
### Objective:
Train a model to do in seconds what Lab 1 did by hand.

### Steps:
1. **Create or open a Content Center:**
   - In the SharePoint Admin Center, go to **Active Sites** > **Create**, and look for the **Content Center** site type (used for building and publishing Premium models). Name it `Contoso Content Center`.
2. **Create a document understanding model:**
   - In the Content Center, create a model named `Contract Classification`.
   - Upload the same sample documents from Lab 1 as training examples, and label the key entities you want extracted: **Contract Type**, **Expiration Date**.
   - Train the model, review its confidence scores, and publish it to the `Contracts` library.
3. **Test it — and time it:**
   - Upload three *new* contract documents to `Contracts`.
   - Confirm the model automatically classifies them and fills in metadata — compare the time this took against your manual-tagging timing from Lab 1.
4. **Stress-test it:**
   - Upload one document that's deliberately a bad fit (e.g., a résumé instead of a contract). Confirm the model either declines to classify it confidently or flags low confidence — a well-behaved model should not confidently mislabel garbage input.

### Knowledge check:
Your model was trained on 10 sample contracts. What risk does that create if Contoso later starts receiving contracts in a very different format (e.g., from a newly-acquired company with its own templates)?

---

## Lab 4: Form Processing and Ongoing Automation
### Objective:
Extend automation to structured forms, and connect it to downstream business processes.

### Steps:
1. **Create a form processing model:**
   - In the Content Center, create a model named `Invoice Processing`, using sample invoices as training data.
   - Train it to extract: **Invoice Number**, **Vendor Name**, **Total Amount**.
   - Publish it to a new library named **Invoices**.
2. **Test it:**
   - Upload new invoices and confirm the extracted fields populate correctly.
3. **Apply retention on top of extracted metadata:**
   - Create a retention label (e.g., **Retain for 7 Years**), publish it to `Contracts`, and set it to auto-apply based on the extracted **Contract Type = Vendor Contract** — showing how Premium's output can drive Purview retention (LAB-07), not just sit as a column.
4. **Chain it to a notification:**
   - Using what you learned in LAB-05a, sketch (or build) a Power Automate flow that checks `Expiration Date` weekly and emails the legal team for anything expiring within 30 days — the natural next step once metadata is reliable and automatic.
5. **Monitor and correct the model:**
   - Review the model's ongoing performance/confidence on newly uploaded documents.
   - Deliberately misclassify one document, correct it manually, and check whether the model surfaces that correction as retraining feedback.

### Challenge:
Calculate a rough ROI statement for Contoso: (time saved per document from Lab 3) × (contracts per month) versus the Azure consumption cost from Lab 2. This is the exact argument a SharePoint admin has to make to get budget approved — not "it's cool AI," but a number.

### Knowledge check:
Why does chaining Premium's extracted metadata into a Power Automate expiration-reminder flow (step 4) create more real business value than the extraction alone?

---

These exercises give hands-on experience managing content and using SharePoint Premium's AI content understanding to automate and scale metadata tagging — including the licensing and cost realities that come with it.
