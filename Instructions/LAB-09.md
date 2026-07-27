# SharePoint Online Lab Exercises - LAB-09

> **Running scenario:** Contoso Consulting's `Fabrikam Project` site (LAB-03) has documents, but the team keeps chatting about them over email. Leadership wants everything — chat, meetings, files — in one place, and wants to know what a "community" (Viva Engage) actually adds beyond that. This lab wires it all together.

## Lab 1: Managing Additional Applications in SharePoint Online
### Objective:
Learn how apps extend individual sites, and how the tenant App Catalog controls what's allowed to run at all.

### Steps:
1. **Explore available apps:**
   - Navigate to the **SharePoint Admin Center** > **More features** > **Apps** > **Open**, then select **App Catalog**.
   - Review what's already available, and browse **Microsoft AppSource** for additional web parts/apps.
2. **Add an app to a site:**
   - On `Fabrikam Project`, go to **Site Contents** > **New** > **App**.
   - Add a relevant app (e.g., a Power BI report web part) and configure it.
3. **Enable custom app deployment:**
   - In the App Catalog, note the option for a **site collection app catalog** (scoped to one site) versus the **tenant app catalog** (available everywhere).
   - Discuss: why would Contoso want a client engagement site like `Fabrikam Project` to have its own scoped app catalog instead of pulling from the tenant-wide one?
4. **Governance check:**
   - Review who currently has permission to upload to the tenant App Catalog. This is a genuine security boundary — a malicious or poorly-built custom app deployed tenant-wide can touch every site.

### Knowledge check:
What's the actual security difference between a user adding a Microsoft AppSource app to their own site versus an admin uploading a custom app to the tenant App Catalog?

---

## Lab 2: Integrating Microsoft Teams with SharePoint Online
### Objective:
Learn the current (site-first) way to connect a SharePoint team site to Microsoft Teams, and when the older Teams-first method is still the right tool.

> ⚠️ **Flow changed recently:** the old path of going into Teams and choosing "Create a team from an existing SharePoint site" isn't the primary flow anymore. Today, the entry point is on the **SharePoint site itself**.

### Steps:
1. **Add Teams from the SharePoint side (current method):**
   - Open a group-connected Team Site you own (e.g., `HR` from LAB-02).
   - On the site's home page, find **Add real-time chat** (or **Add Microsoft Teams** in the "Next steps" panel, top right).
   - Walk through the short tour, then select which SharePoint resources (pages, news, lists, document libraries) to surface as tabs inside the new Teams team.
   - Confirm: this only works because `HR` is already connected to a Microsoft 365 Group (LAB-02, Lab 5) — a non-group site (like a Communication Site) cannot be converted this way.
2. **Fallback method (Teams-first, still valid):**
   - If "Add real-time chat" isn't available, open **Microsoft Teams** > **Create team** > **More create team options** > **From group**, and select the group backing `HR`.
   - Compare: same end result, different starting point — good to know both when a colleague describes an older screenshot from a blog post or course.
3. **Add SharePoint libraries as tabs:**
   - In the resulting Teams team, open a channel, click **+** to add a tab, select **Document Library**, and link it to `HR`'s document library.
   - Confirm team members can open and edit a document directly inside Teams and see the same file update live in SharePoint.
4. **Configure the Teams-side policy:**
   - In the **Teams Admin Center** (https://admin.teams.microsoft.com) > **Teams apps** > **Setup policies**, confirm the SharePoint app is enabled for the relevant user group.

### Knowledge check:
Why does "Add Microsoft Teams" only appear on group-connected Team Sites and not on Communication Sites like `Contoso Home` from LAB-02?

---

## Lab 3: Integrating Viva Engage with SharePoint Online
### Objective:
Understand what Viva Engage (formerly Yammer) adds that Teams channels and SharePoint don't — cross-team, organization-wide conversation — and connect it to a site.

### Steps:
1. **Enable Viva Engage:**
   - Navigate to the **Microsoft 365 Admin Center** > **Settings** > **Org settings** > **Viva Engage**, and confirm it's enabled for Contoso.
   - Note that deeper community-level administration (creating/moderating communities, network-wide settings) happens inside Viva Engage's own admin area, reachable from the gear icon inside the Viva Engage app itself — a separate layer from the Microsoft 365 admin center toggle.
2. **Create a community and connect it to the site:**
   - In Viva Engage, create a community named `Contoso All Hands`.
   - On `Contoso Home` (LAB-02), edit the homepage and add a **Viva Engage Conversations** web part, configured to show the `Contoso All Hands` community.
3. **Bring it into Teams too:**
   - In Microsoft Teams, add the **Viva Engage** app to a team and configure it to display the same community — now the same conversation is reachable from SharePoint, Teams, and the standalone Viva Engage app.
4. **Compare it to a Teams channel, concretely:**
   - Post a message in the `HR` Team's channel (Lab 2), and a message in `Contoso All Hands`. Discuss who can realistically see each one (channel = HR team members only; community = potentially the whole org) — this is the actual decision criterion for "Teams channel vs. Viva Engage community," not just a feature checklist.
5. **Configure org-wide settings:**
   - Inside Viva Engage admin settings, review community creation permissions (who can spin up new communities — open to everyone, or restricted?), external collaboration settings, and data retention.

### Knowledge check:
Contoso's Marketing team wants a space to share company-wide announcements that anyone across all 300 employees might want to see and react to. Should that be a Teams channel or a Viva Engage community? Justify it using what you just observed about audience reach.

---

## Lab 4: Administrative Integration of Teams and Viva Engage
### Objective:
Step back from individual features to tenant-wide monitoring and governance of everything integrated so far.

### Steps:
1. **Monitor usage:**
   - In the **Microsoft 365 Admin Center** > **Reports** > **Usage**, review adoption reports for Teams, SharePoint, and Viva Engage side by side.
   - Identify: is Contoso actually using the `Contoso All Hands` community and the `HR` Team you built, or did activity stay in email/chat instead?
2. **Configure governance policies:**
   - In the **Teams Admin Center**, set a guest access policy for Teams that's at least as strict as the SharePoint external sharing ceiling you set in LAB-03 — inconsistent guest policy across Teams and SharePoint is a common, avoidable audit finding.
3. **Audit integration activity:**
   - In the **Microsoft Purview** portal (LAB-07), go to **Audit**, and search for activity related to the app you added in Lab 1, the Teams-from-site conversion in Lab 2, and the community creation in Lab 3.
   - Confirm you can answer "who installed what, and when" for everything you built in this lab, using nothing but the audit log.

### Challenge:
Produce a one-paragraph "integration map" for Contoso: which content lives natively in SharePoint, which conversations happen in Teams channels, and which belong in Viva Engage communities — with a one-sentence rule for each so a new employee doesn't have to guess where to post something.

### Knowledge check:
If usage reports (step 1) showed the `HR` Team and `Contoso All Hands` community both sitting nearly empty a month after launch, is that a technical problem or something else — and what would you actually check first?

---

These exercises provide hands-on experience integrating and governing Microsoft Teams and Viva Engage alongside SharePoint Online, using the current (site-first) integration flow rather than outdated screenshots.
