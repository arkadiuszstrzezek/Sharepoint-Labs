# SharePoint Online Lab Exercises - LAB-03

> **Running scenario:** Contoso Consulting (from LAB-02) has just signed its first external client, **Fabrikam Inc.**, and needs to share project files with them without inviting the whole internet. You'll configure sharing at every level — tenant, site, and file — and see how each layer restricts the one below it.

## Lab 1: Configuring External Sharing
### Objective:
Learn how external sharing is layered in SharePoint Online — tenant policy sets the ceiling, and every level below can only be equal or stricter, never looser.

### Steps:
1. **Set the tenant-wide ceiling:**
   - Navigate to the **SharePoint Admin Center** > **Policies** > **Sharing**.
   - Review (don't necessarily change yet) the four levels:
     - **Anyone**: anonymous, no sign-in link.
     - **New and existing guests**: authenticated external users, added to Entra ID as guests.
     - **Existing guests only**: only guests already in the directory.
     - **Only people in your organization**: external sharing disabled entirely.
   - Set it to **New and existing guests** — Contoso wants Fabrikam to authenticate, not get anonymous links.
2. **Configure site-level sharing for the client site:**
   - Create (or reuse) a site named `Fabrikam Project`.
   - In **Active Sites**, select `Fabrikam Project` > **Sharing**.
   - Set the site's sharing level to **New and existing guests** — it cannot exceed the tenant setting from step 1, only match or restrict it further.
3. **Prove the ceiling effect (the actual lesson):**
   - Temporarily set the *tenant* policy to **Only people in your organization**.
   - Go back to the `Fabrikam Project` site's sharing setting — notice it is now capped at "Only people in your organization" too, regardless of what you chose in step 2.
   - Set the tenant policy back to **New and existing guests** afterward.

### Knowledge check:
If tenant sharing is set to "Existing guests only" but a site is set to "Anyone," what actually happens when someone tries to share a file on that site — and why?

---

## Lab 2: Types of External Sharing Mechanisms
### Objective:
Understand the different mechanisms for sharing content externally, and which one to use for Fabrikam.

### Steps:
1. **Share a file with a link:**
   - In `Fabrikam Project`, upload a sample proposal document.
   - Select the file > **Share**, and compare the link options:
     - **Anyone with the link** — no login required (only available if tenant/site policy allows it).
     - **People in your organization** — internal only.
     - **People with existing access** — no new permissions granted, just a shortcut.
     - **Specific people** — you name exactly who; safest for Fabrikam.
   - Choose **Specific people**, enter a test external email, set permission to **Can view**, and send.
2. **Share the whole site:**
   - Open `Fabrikam Project`, click **Share** (top-right of the site), enter the same external email, and assign the **Visitor** role.
   - Compare this to file-level sharing: site sharing gives ongoing access to everything the role permits, not just one document.
3. **Decide and justify:**
   - For a long-running client engagement like Fabrikam, would you standardize on site sharing or per-file sharing? Write down your reasoning — this becomes your governance rule in Lab 3.

### Knowledge check:
Why is "Specific people" almost always the safer default over "Anyone with the link," even when tenant policy allows anonymous links?

---

## Lab 3: Managing Content Sharing with Microsoft Entra ID and SharePoint Online
### Objective:
Learn how external sharing in SharePoint is really guest access in Microsoft Entra ID underneath, and how to monitor and revoke it.

### Steps:
1. **Configure guest access in Microsoft Entra ID:**
   - Navigate to the **Microsoft Entra admin center** (https://entra.microsoft.com).
   - Go to **External Identities** > **External collaboration settings**.
   - Review/configure:
     - **Guest user access restrictions** — how much of the directory a guest can see.
     - **Collaboration restrictions** — allow-list or deny-list specific domains (e.g., allow only `fabrikam.com`).
2. **Find the guest account SharePoint created for you:**
   - Still in Entra ID, go to **Users** > filter by **User type: Guest**.
   - Find the Fabrikam test account you shared with in Lab 2 — notice SharePoint auto-created it here the moment you shared.
3. **Monitor sharing activity:**
   - In the **SharePoint Admin Center**, check the **Reports** area (exact label may vary by tenant/update wave — look for sharing or usage reports) for external sharing activity.
   - Cross-check in the **Microsoft 365 Admin Center** > **Reports** > **Usage**, which also surfaces SharePoint external sharing stats.
4. **Revoke access two ways, and compare:**
   - **Narrow revoke:** go back to the file from Lab 2, click **Manage Access**, and remove the Fabrikam guest's link/permission. Confirm they lose access to *that file only*.
   - **Full revoke:** in Entra ID > **Users** > **Guest users**, delete the Fabrikam guest account entirely. Confirm this removes access *everywhere* they had it — sites, files, everything.

### Challenge:
Set up a **domain allow-list** in Entra ID external collaboration settings that only permits `fabrikam.com` guests, then try sharing with a `gmail.com` test address and confirm it's blocked. This is what most real client engagements actually configure instead of open guest sharing.

### Knowledge check:
Your security team asks: "How do we guarantee no one outside Fabrikam and our own company can ever get into the `Fabrikam Project` site?" Name the two settings (one in SharePoint, one in Entra ID) that together enforce this.

---

These exercises give hands-on, layered experience with external sharing — from the tenant policy ceiling down to a single file link, and the Entra ID guest accounts that make it all work.
