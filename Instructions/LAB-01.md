# SharePoint Online Lab Exercises- LAB-01

## Lab 1: Role of a SharePoint Administrator
### Objective:
Understand the responsibilities of a SharePoint Administrator and explore the SharePoint Admin Center.

### Steps:
1. Log in to the Microsoft 365 Admin Center.
2. Navigate to the **SharePoint Admin Center**.
3. Explore the following sections:
   - Active sites
   - Deleted sites
   - Sharing settings
   - Policies (e.g., external sharing, access control)
4. Discuss the role of a SharePoint Administrator in managing these settings.

---

## Lab 2: Managing SharePoint Online with PowerShell
### Objective:
Learn to use PowerShell to manage SharePoint Online.

### Prerequisites:
- Install the **SharePoint Online Management Shell**.
- Connect to SharePoint Online using the following command:
  ```powershell
  Connect-SPOService -Url https://<your-tenant>-admin.sharepoint.com -Credential (Get-Credential)
  ```

###  Steps:
- Retrieve a list of all site collections:
  ```powershell
  Get-SPOSite
  ```
- Create a new site collection:
  ```powershell
  New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/LabSite -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Lab Site"
  ```
- Update site collection properties:
  ```powershell
  Set-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite -SharingCapability ExternalUserSharingOnly
  ```
- Remove a site collection:
  ```powershell
  Remove-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite
  ```

## Lab 3: Using SharePoint PnP Module
### Objective:
Automate SharePoint Online tasks using the PnP PowerShell module.

### Prerequisites:
- Install the PnP PowerShell Module:
  ```powershell
  Install-Module -Name PnP.PowerShell
  ```
- Connect to SharePoint Online:
  ```powershell
  Connect-PnPOnline -Url https://<your-tenant>.sharepoint.com -UseWebLogin
  ```

 ### Steps:
- Create a new list:
  ```powershell
  New-PnPList -Title "Lab List" -Template GenericList
  ```
- Add a column to the list:
  ```powershell
  Add-PnPField -List "Lab List" -DisplayName "Student Name" -InternalName "StudentName" -Type Text
  ```
- Add an item to the list:
  ```powershell
  Add-PnPListItem -List "Lab List" -Values @{"StudentName"="John Doe"}
  ```
- Export the list to a CSV file:
  ```powershell
  Get-PnPListItem -List "Lab List" | Export-Csv -Path "LabList.csv" -NoTypeInformation
  ```

## Lab 4: Using Microsoft Graph to Manage SharePoint
### Objective:
Learn to use Microsoft Graph API to interact with SharePoint Online.

### Prerequisites:
- Register an app in Azure AD and obtain an access token.
- Use tools like Postman or Graph Explorer.

### Steps:
1. Retrieve a list of SharePoint sites:
 - Endpoint:
```powershell
GET https://graph.microsoft.com/v1.0/sites
  ```
 - Example response:
```powershell
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
2. Create a new list in a site:
- Endpoint:
```powershell
POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists
  ```
- Body:
```powershell
{
  "displayName": "Lab List",
  "list": {
    "template": "genericList"
  }
}
  ```
3. Add an item to the list:
- Endpoint:
```powershell
POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items
  ```
- Body:
```powershell
{
  "fields": {
    "Title": "John Doe"
  }
}
  ```

## Lab 5: Exploring SharePoint Admin Center
### Objective:
Perform common administrative tasks using the SharePoint Admin Center.


### Steps:

1. **Create a new site collection:**
   - Go to **Active Sites** > **Create** > **Team Site**.
   - Fill in the required details and create the site.
2. **Configure sharing settings:**
   - Navigate to **Policies** > **Sharing**.
   - Set sharing to **Anyone** or **Only people in your organization**.
3. **Monitor site usage:**
   - Go to **Active Sites** and select a site.
   - View the **Usage** tab for activity reports.


