# SharePoint Online Lab Exercises - LAB-01

> **Running scenario:** It's your first day as SharePoint Administrator at **Contoso Consulting**, a company about to grow from 50 to 300 employees. Nobody has set up governance yet — you're starting from a mostly-empty tenant. This lab is Day 1: get oriented, and prove you can do the same basic task four different ways (UI, SharePoint PowerShell, PnP PowerShell, Graph API), because a real admin needs to know which tool fits which situation. LAB-02 onward builds Contoso's actual intranet on top of what you set up here.

## Lab 1: Role of a SharePoint Administrator
### Objective:
Understand what a SharePoint Administrator is actually responsible for, by finding real answers in the Admin Center — not just clicking through it.

### Steps:
1. Log in to the **Microsoft 365 Admin Center** (https://admin.microsoft.com).
2. Navigate to the **SharePoint Admin Center**.
3. **Scavenger hunt — answer these using only the Admin Center, and write down where you found each one:**
   - How many active sites does the tenant currently have, and how much total storage is in use?
   - Is there anything in **Deleted sites** right now? (If empty, note that — you'll intentionally put something there in Lab 2.)
   - What is the tenant's current external sharing level, under **Policies** > **Sharing**?
   - Name one setting under **Policies** > **Access control** and what it protects against.
4. **Discuss the role:**
   - Based on what you just found, list three decisions a SharePoint Administrator makes that a regular site owner cannot.

### Knowledge check:
A site owner asks you to "just turn on sharing with anyone" for their site. Based on what you found in step 3, what governs whether you even *can* say yes?

---

## Lab 2: Managing SharePoint Online with PowerShell
### Objective:
Learn to manage SharePoint Online with the SharePoint Online Management Shell — the tool for tenant-level administration (site creation, sharing capability, storage) that the UI can only do one click at a time.

### Prerequisites:
- Install the **SharePoint Online Management Shell**.
- Connect to SharePoint Online:
  ```powershell
  Connect-SPOService -Url https://<your-tenant>-admin.sharepoint.com -Credential (Get-Credential)
  ```
  > **Note:** if your tenant enforces MFA (most do), the `-Credential (Get-Credential)` prompt will fail. Drop the `-Credential` parameter entirely and just run `Connect-SPOService -Url https://<your-tenant>-admin.sharepoint.com` — it will open a modern, MFA-capable sign-in window instead.

### Steps:
1. **Retrieve a list of all site collections:**
   ```powershell
   Get-SPOSite
   ```
2. **Create a new site collection:**
   ```powershell
   New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/LabSite -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Lab Site"
   ```
3. **Update site collection properties:**
   ```powershell
   Set-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite -SharingCapability ExternalUserSharingOnly
   ```
4. **Verify the change landed, don't just assume it did:**
   ```powershell
   Get-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite | Select-Object Url, SharingCapability
   ```
5. **Remove the site collection:**
   ```powershell
   Remove-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite
   ```
6. **Close the loop from Lab 1:**
   - Run `Get-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite -IncludeRecycled` (or check **Deleted sites** in the Admin Center) and confirm `LabSite` is now sitting in the recycle bin, not gone forever.

### Challenge:
Script steps 2–3 as a single reusable function `New-ContosoSite -Name "TestSite" -Sharing ExternalUserSharingOnly` that wraps `New-SPOSite` and `Set-SPOSite` together — this is the shape of what real provisioning scripts look like.

### Knowledge check:
Why does `Remove-SPOSite` not permanently delete the site immediately? What real-world mistake does that default protect against?

---

## Lab 3: Using the SharePoint PnP PowerShell Module
### Objective:
Learn PnP PowerShell — the tool for *content-level* automation (lists, columns, items) that the SPO module in Lab 2 deliberately doesn't do.

### Prerequisites:
- Install the PnP PowerShell Module:
  ```powershell
  Install-Module -Name PnP.PowerShell
  ```
- Grant admin consent for the PnP Management Shell Entra ID app (one-time, per tenant). `Connect-PnPOnline -Interactive` uses a Microsoft-provided multi-tenant Entra ID application; unless it has already been consented to in your tenant, sign-in will fail with a "need admin approval" error. As a Global/SharePoint Administrator, run:
  ```powershell
  Register-PnPManagementShellAccess
  ```
  This opens a browser window where you approve the requested permissions for the whole tenant. It only needs to be done once.
- Connect to SharePoint Online:
  ```powershell
  Connect-PnPOnline -Url https://<your-tenant>.sharepoint.com -Interactive
  ```

### Steps:
1. **Create a new list:**
   ```powershell
   New-PnPList -Title "Lab List" -Template GenericList
   ```
2. **Add a column to the list:**
   ```powershell
   Add-PnPField -List "Lab List" -DisplayName "Student Name" -InternalName "StudentName" -Type Text
   ```
3. **Add an item to the list:**
   ```powershell
   Add-PnPListItem -List "Lab List" -Values @{"StudentName"="John Doe"}
   ```
4. **Export the list to a CSV file:**
   ```powershell
   Get-PnPListItem -List "Lab List" | Select-Object -ExpandProperty FieldValues | Export-Csv -Path "LabList.csv" -NoTypeInformation
   ```

### Challenge:
Try `Get-SPOSite` (Lab 2's module) and `Get-PnPList` (this module) back to back. Notice `Get-SPOSite` cannot see inside a site's lists, and `Get-PnPList` has no concept of tenant-wide sharing policy. Write one sentence explaining the boundary between what each module is *for*.

### Knowledge check:
You need to bulk-create 50 lists across 50 new sites with identical columns. Would you reach for `Get-SPOSite`/`New-SPOSite` (Lab 2) or PnP PowerShell (this lab)? Why?

---

## Lab 4: Using Microsoft Graph to Manage SharePoint
### Objective:
Learn Microsoft Graph — the tool you reach for when automation needs to run *outside* PowerShell entirely (a web app, a Power Automate custom connector, a mobile app).

### Prerequisites:
- Register an app in **Microsoft Entra ID** (entra.microsoft.com) and obtain an access token with appropriate Microsoft Graph permissions (e.g., `Sites.Read.All`).
- Use tools like Postman or the **Graph Explorer** (https://developer.microsoft.com/graph/graph-explorer) to try requests without writing a client app first.

### Steps:
1. **Retrieve a list of SharePoint sites:**
   - Endpoint:
     ```http
     GET https://graph.microsoft.com/v1.0/sites
     ```
   - Example response:
     ```json
     {
       "value": [
         {
           "id": "site-id",
           "name": "Communication Site",
           "webUrl": "https://<your-tenant>.sharepoint.com/sites/CommunicationSite"
         }
       ]
     }
     ```
2. **Create a new list in a site:**
   - Endpoint:
     ```http
     POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists
     ```
   - Body:
     ```json
     {
       "displayName": "Lab List",
       "list": {
         "template": "genericList"
       }
     }
     ```
3. **Add an item to the list:**
   - Endpoint:
     ```http
     POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items
     ```
   - Body:
     ```json
     {
       "fields": {
         "Title": "John Doe"
       }
     }
     ```
4. **Try it live:**
   - Open Graph Explorer, sign in, and run the `GET /sites` request for real against your tenant. Find your `Lab List` site's `id` in the response.

### Challenge:
You just did the same three things — create a list, add a column (skip for Graph), add an item — in PnP PowerShell (Lab 3) and now in Graph. List one scenario where only Graph would work (hint: think about anything that isn't a Windows machine with PowerShell installed).

### Knowledge check:
Fill in this decision table from what you've now done four ways — UI, `SPO` module, `PnP` module, Graph API:

| Task | Best tool | Why |
|---|---|---|
| One-off tenant sharing policy change | | |
| Bulk-provisioning 50 sites from a script | | |
| A custom web app that needs to read SharePoint lists | | |
| Training a new site owner who's never used PowerShell | | |

---

## Lab 5: Your First Real Admin Task in the Sandbox
### Objective:
Combine Labs 1–4 into one small, complete admin task — end to end, in the UI — before you start building Contoso's real intranet in LAB-02.

### Steps:
1. **Create a new site collection:**
   - Go to **Active Sites** > **Create** > **Team Site**, named `IT Sandbox`.
2. **Configure sharing settings:**
   - Navigate to **Policies** > **Sharing**, and set the *site-level* sharing for `IT Sandbox` to **Only people in your organization** — the strictest option, since this is just a scratch space, not client-facing.
3. **Generate some activity:**
   - Upload a couple of test files and add one list item, so there's something to monitor.
4. **Monitor site usage:**
   - Go to **Active Sites**, select `IT Sandbox`, and view the **Usage** tab for activity data.
5. **Clean up like a professional:**
   - Delete `IT Sandbox` when you're done — Contoso's real structure starts fresh in LAB-02, and leaving throwaway sandbox sites lying around is exactly the sprawl you'll be governing against later (LAB-06, LAB-08a).

### Knowledge check:
You now have four ways to have done Lab 5's first two steps (UI here, plus `New-SPOSite`/`Set-SPOSite` from Lab 2, PnP from Lab 3, Graph from Lab 4). Which one would you actually use on your first real day on the job, and which one would you use once Contoso has 300 employees and you're provisioning sites weekly? Justify the difference.

---

These exercises give you a working picture of the SharePoint Administrator role and hands-on fluency in all four ways of managing SharePoint Online — Admin Center, SharePoint PowerShell, PnP PowerShell, and Microsoft Graph — the toolkit every later lab in this course builds on.
