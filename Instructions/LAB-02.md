# SharePoint Online Lab Exercises - LAB-02

> **Running scenario:** You are the new SharePoint Administrator at **Contoso Consulting**, a company growing from 50 to 300 employees. Leadership wants a real intranet: a central hub, dedicated department sites (HR, IT, Marketing), and clear governance around templates and permissions. Every exercise below adds one piece of that intranet, so by the end of LAB-02 you'll have a small but working hub-and-spoke structure you can keep building on in later labs.

## Lab 1: Planning and Configuring Site Collections and Hub Sites
### Objective:
Learn how to plan and configure site collections and hub sites in SharePoint Online.

### Steps:
1. **Create the hub-to-be site:**
   - Navigate to the **SharePoint Admin Center** > **Active Sites** > **Create**.
   - Choose a **Communication Site** (Topic design) named `Contoso Home`.
2. **Create two department sites:**
   - Create a **Team Site** named `HR` and another named `IT`.
3. **Register the hub site:**
   - Select `Contoso Home` from **Active Sites**.
   - Click **Hub** > **Register as hub site**.
   - Give it a display name (`Contoso Hub`) and decide **who can associate sites with this hub** (leave empty = anyone; or restrict to yourself for now).
4. **Associate the department sites:**
   - Select `HR`, click **Hub** > **Associate with a hub site**, choose `Contoso Hub`, save.
   - Repeat for `IT`.
5. **Verify propagation:**
   - Open `HR` or `IT` — the Contoso Hub navigation bar and theme should now appear at the top of the site.

### PowerShell alternative (automation):
```powershell
New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/ContosoHome -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Contoso Home" -Template SITEPAGEPUBLISHING#0
Register-SPOHubSite -Site https://<your-tenant>.sharepoint.com/sites/ContosoHome -Principals @()
Add-SPOHubSiteAssociation -Site https://<your-tenant>.sharepoint.com/sites/HR -HubSite https://<your-tenant>.sharepoint.com/sites/ContosoHome
```

### Challenge:
- Add a third department site (`Marketing`) and associate it with the hub — **without using the UI**, only PowerShell.
- Change the hub site's theme/logo and confirm it propagates to all three associated sites.

### Knowledge check:
Why would you use a hub site instead of a classic subsite structure for HR, IT and Marketing? What do you lose by choosing subsites instead?

---

## Lab 2: SharePoint Classic vs. Modern — Understanding the Retirement
### Objective:
Understand SharePoint Classic architecture well enough to recognize it, and understand **why Microsoft is retiring it** — this is now core admin knowledge, not just history.

> ⚠️ **Important, current as of 2026:** the **Create** button in the modern SharePoint Admin Center only offers modern **Team Site** and **Communication Site** — there is no "Classic Team Site" option anymore. On top of that, Microsoft has been retiring classic **publishing** sites in stages: creation of new classic publishing sites (portal, blank site, publishing site, enterprise search center) was blocked from **September 15, 2025**, and as of **March 15, 2026** the temporary tenant-level opt-outs (e.g., for custom scripting) were shut off permanently. Existing classic sites still work, but you can no longer grow that footprint. This lab is written around that reality instead of around creating a new classic site, since that path is now closed for most tenants.

### Steps:
1. **Hunt for an existing classic site:**
   - In **Active Sites**, check the **Template** column (or run the PowerShell below) to see if your tenant already has any classic sites (`STS#0`, `SRCHCEN#0`, `CMSPUBLISHING#0`, etc.) — these are usually old default team sites or the root site collection.
   ```powershell
   Get-SPOSite -Limit All | Select-Object Url, Template
   ```
2. **Try to create one anyway (learning-by-failure):**
   - Attempt to create a classic team site via PowerShell:
   ```powershell
   New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/ClassicTest -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Classic Test" -Template STS#0
   ```
   - Note whether it succeeds, is silently converted to a modern template, or is blocked by tenant policy. Whatever happens, that result *is* the lesson: document it.
3. **Explore whatever classic site you found (or the one you just created, if it worked):**
   - Look for: subsites, the ribbon interface, classic web parts, `_layouts/15/` settings pages.
4. **Fill in the comparison table** (use it for Lab 3 too):

   | Aspect | Classic | Modern |
   |---|---|---|
   | Navigation | Ribbon | Command bar |
   | Structure | Subsites | Hub + associated sites |
   | Responsiveness | Poor / desktop-first | Fully responsive |
   | Web parts | Classic (Content Editor, etc.) | Modern (News, Highlighted Content...) |
   | Can still be created today? | | |

### Discussion:
- Why does Microsoft push customers off classic architecture (performance, mobile, security of custom scripting)?
- If Contoso still had an old classic intranet, what would your migration plan look like?

### Knowledge check:
Your manager asks you to "spin up a quick classic publishing site for a temporary campaign page." What do you tell them, and what do you propose instead?

---

## Lab 3: Overview of SharePoint Modern Architecture
### Objective:
Understand the architecture of SharePoint Modern sites through hands-on exploration, not just observation.

### Steps:
1. **Build a real page:**
   - On `Contoso Home` (or a new Communication Site), create a page and add at least three modern web parts: **News**, **Highlighted content**, and **Embed** (e.g., embed a YouTube video or a Microsoft Form).
2. **Test responsiveness:**
   - Resize your browser window down to phone width, or open the site in the **SharePoint mobile app**. Confirm the layout reflows automatically — no separate "mobile site" needed.
3. **Confirm Microsoft 365 Group integration:**
   - Open the `HR` Team Site (created in Lab 1) and check the **Conversations**/**Members** area — it's backed by an Microsoft 365 Group. Open Outlook/Teams and confirm the same group appears there.
4. **Automate a page edit (PnP):**
   ```powershell
   Add-PnPPageWebPart -Page "Home" -DefaultWebPartType News
   ```

### Challenge:
Recreate the same page layout using only PnP PowerShell (no UI) — this is exactly the kind of task a real admin automates for repeatable department site provisioning.

### Knowledge check:
List three concrete advantages a modern site gives your end users over the classic site you inspected in Lab 2.

---

## Lab 4: Templates and Structures for Classic and Modern Sites
### Objective:
Learn what site templates/designs are available today and when to use each.

### Steps:
1. **Inventory available modern designs:**
   - In **Create** > **Communication Site**, note the three designs offered: **Topic**, **Showcase**, **Blank**.
   - In **Create** > **Team Site**, note there is effectively one modern design (group-connected).
2. **List subsite templates via PowerShell** (subsites still use the classic template engine):
   ```powershell
   Get-PnPWebTemplates
   ```
3. **Build a decision matrix:**

   | Scenario | Recommended template |
   |---|---|
   | Department landing page for HR | Communication Site — Topic |
   | Product launch microsite | Communication Site — Showcase |
   | Project team with documents & planning | Team Site (Microsoft 365 Group) |
   | Quick internal announcement page, no group needed | Communication Site — Blank |

4. Fill in one more row yourself for a scenario relevant to Contoso Marketing, and justify your choice.

### Knowledge check:
Why did Microsoft consolidate dozens of classic templates down to essentially two modern site types?

---

## Lab 5: Overview of Microsoft 365 Groups
### Objective:
Understand how Microsoft 365 Groups drive membership and permissions behind the scenes of a Team Site — including what happens when membership changes.

### Steps:
1. **Create a Microsoft 365 Group:**
   - In the Microsoft 365 Admin Center, go to **Groups** > **Active Groups** > **Add a Group** > **Microsoft 365**.
   - Name it `Contoso Marketing`.
2. **Confirm the linked SharePoint site was auto-provisioned:**
   - Find its SharePoint site under **Active Sites** and confirm it matches the group name.
3. **Add a member through the group, not the site:**
   - In the Microsoft 365 Admin Center, add a test user to the `Contoso Marketing` group as a **Member**.
   - Go to the SharePoint site's **Site Permissions** — the user should already appear as a **Member**, without you touching SharePoint at all.
4. **Now remove them from the group:**
   - Remove the same user from the group in the Microsoft 365 Admin Center.
   - Refresh SharePoint site permissions and confirm the user's access is gone (sync can take a few minutes).
5. **Explore the wider integration:**
   - Open Teams and confirm you can "Create a team" from the `Contoso Marketing` group, reusing the same membership and SharePoint files.

### Knowledge check:
If a user complains they can't access a Team Site even though "IT added them last week," what's the first thing you check, given what you just observed about group-driven sync?

---

## Lab 6: Managing and Planning Sites
### Objective:
Practice the governance judgment calls a real SharePoint admin makes daily.

### Steps:
1. **Governance audit:**
   - Pick one site you created earlier (e.g., `HR`) and fill in this audit checklist:

     | Check | Current value | Meets best practice? |
     |---|---|---|
     | Sharing capability | | |
     | Permission inheritance broken anywhere? | | |
     | Navigation type (structural vs. managed) | | |
     | Regional settings / time zone | | |
     | Storage quota | | |

2. **Flat vs. subsites debate:**
   - In two sentences, argue **for** a flat hub-and-spoke structure for Contoso, then in two sentences argue **for** a deep subsite hierarchy. Which one actually fits Contoso's growth from 50 to 300 employees, and why?
3. **Fix one finding:**
   - Pick one row from your audit table that doesn't meet best practice and fix it (e.g., tighten sharing capability from "Anyone" to "Only people in your organization").

### Knowledge check:
Why is "flat structure with hub sites" generally recommended over deep subsite trees in modern SharePoint?

---

## Lab 7: Planning Hub Site Structure for Intranet
### Objective:
Design and implement a realistic intranet hub structure end-to-end.

### Steps:
1. **Design first, build second:**
   - On paper (or in a diagramming tool), sketch Contoso's intranet: `Contoso Hub` at the top, with `HR`, `IT`, and `Marketing` as spokes. Note which spokes will get sub-spokes later (e.g., IT → Helpdesk).
2. **Configure hub navigation:**
   - On `Contoso Hub`, edit the navigation bar to add links to each associated site, plus one external resource (e.g., the company's HR benefits portal).
3. **Test hub-scoped search:**
   - From inside the `HR` site, search for a document that only exists on `IT`. Because both are associated with the same hub, the result should surface in search — this is one of the concrete benefits hubs give you over disconnected sites.
4. **Add a rule:**
   - Decide and document who is allowed to associate new sites with `Contoso Hub` going forward (recall the "who can associate" setting from Lab 1) — this is a governance decision, not just a technical one.

### Challenge:
Add a fourth, nested layer: create a `Helpdesk` site and associate it with `IT` instead of directly with `Contoso Hub`. Confirm whether hub-scoped search from `Helpdesk` still surfaces `HR` content (test the actual behavior — don't assume).

### Knowledge check:
Your CEO asks "why can't we just use folders in one giant site instead of all these separate sites and a hub?" Give a two-sentence answer.

---

## Lab 8: Managing Site Permissions
### Objective:
Learn to manage, customize, and **verify** permissions for SharePoint Online sites — verification is the part most admins skip and regret.

### Steps:
1. **Grant permissions:**
   - On the `Marketing` site, go to **Site Permissions**, add a test user, and assign them the **Member** role.
2. **Create a custom permission level:**
   - Go to **Site Permissions** > **Advanced permissions settings** > **Permission Levels** > **Add a Permission Level**.
   - Base it on **Contribute** but name it `Contributor - No Delete`, and uncheck the delete permissions (list items, documents, versions).
   - Create a SharePoint group `Marketing Contributors (No Delete)` and assign it this custom level.
3. **Break inheritance:**
   - Open the site's main document library, go to **Library Settings** > **Permissions for this document library**, and stop inheriting permissions.
   - Assign only the `Marketing Contributors (No Delete)` group to this library, removing the default Members access.
4. **Verify — don't assume:**
   - Use **Site Permissions** > **Check Permissions**, enter your test user, and confirm exactly what access level they resolve to on the library.
   - Cross-check with PowerShell:
   ```powershell
   Get-PnPGroupMembers -Identity "Marketing Contributors (No Delete)"
   Get-PnPUserEffectivePermissions -Identity testuser@<your-tenant>.onmicrosoft.com -List "Documents"
   ```

### Challenge:
Audit **Owner** access across all three Contoso sites created in this lab (`Contoso Home`, `HR`, `Marketing`) using:
```powershell
Get-PnPGroupMembers -Identity "Site Owners"
```
run against each site in turn. Flag anyone who is an Owner on a site they shouldn't be.

### Knowledge check:
Why is "Check Permissions" (or `Get-PnPUserEffectivePermissions`) more trustworthy than just reading group membership lists when you need to answer "can this specific user open this specific document"?

---

These exercises give you hands-on, currently-accurate experience planning, configuring, and governing SharePoint Online sites and hub sites — including recognizing when a "classic" approach is no longer available and knowing what to reach for instead.
