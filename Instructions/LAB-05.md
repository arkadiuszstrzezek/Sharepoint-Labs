# SharePoint Online Lab Exercises - LAB-05

> **Running scenario:** Contoso Consulting's employees keep emailing the office manager to book the company vehicles, and it's chaos. You'll fix this with a Power App backed by SharePoint lists — and along the way, learn why a SharePoint admin needs to care about Power Platform even though it's "not SharePoint."

## Lab 1: Planning and Configuring Customizations and Applications
### Objective:
Learn how Power Apps and SharePoint lists work together to build real business applications, and understand what a SharePoint admin needs to know even when they're not the one building the app.

### Why a SharePoint admin should care:
Power Apps built on SharePoint lists inherit SharePoint's permissions model, count against the same storage quotas, and can move data through connectors that your tenant's DLP policies (see LAB-07) either allow or block. "Just a citizen-developer app" is still your governance problem.

---

### Part A: Design the Data — SharePoint Lists & One-to-Many Relationships

Create two SharePoint lists on a new site called `Contoso Fleet`:

**List 1: `Company Vehicles`**

| ID | YearMakeModel | AssetCode | LicensePlate | Office |
|----|----------------------|-----------|----------|------------|
| 1 | 2020 Dodge Ram | 10-023 | Y2K 9D9 | Albany |
| 2 | 2016 Ford F150 | 10-034 | Q9T 9T5 | Albany |
| 3 | 2019 GMC Sierra | 10-122 | A7I 0Z5 | Fargo |
| 4 | 2020 Honda Ridgeline | 10-021 | J9B 1P8 | Schaumberg |
| 5 | 2021 Nissan Pathfinder | 10-301 | Q7K 7L7 | Schaumberg |

**List 2: `Vehicle Reservations`**

| ID | VehicleID | Employee | StartDate | EndDate |
|----|-----------|---------------|------------|------------|
| 1 | 3 | Mark Clark | 2026-08-25 | 2026-08-29 |
| 2 | 2 | Anna Sinclair | 2026-08-25 | 2026-08-27 |
| 3 | 4 | Laura Andrews | 2026-08-27 | 2026-08-29 |
| 4 | 1 | Sarah Green | 2026-08-25 | 2026-08-25 |
| 5 | 1 | John Freeman | 2026-08-26 | 2026-08-27 |

The `VehicleID` column in `Vehicle Reservations` links to the `ID` in `Company Vehicles` — a classic one-to-many relationship, the same pattern you'll see in almost every real SharePoint-backed business app.

**Checkpoint:** before building the app, check **Site Permissions** on `Contoso Fleet` — decide who should be able to edit `Company Vehicles` (fleet admins only) versus who should be able to add rows to `Vehicle Reservations` (everyone). List membership design is a permissions decision, not just a data-modeling one.

---

### Part B: Select a Vehicle

1. Open Power Apps Studio and create a new tablet app connected to `Contoso Fleet`.
2. Insert a vertical gallery, set its data source to `Company Vehicles`.
3. Change the layout to **Title**, set the title field to `YearMakeModel`.
4. Add a label above the gallery: **Choose A Vehicle.**

---

### Part C: Reservation Form

1. Add a label bound to the selection:
   ```powerapps
   gal_ChooseAVehicle.Selected.YearMakeModel
   ```
2. Add read-only fields for **Asset Code**, **License Plate**, and **Office**, each bound the same way (e.g. `gal_ChooseAVehicle.Selected.AssetCode`), with **DisplayMode** set to `DisplayMode.Disabled`.
3. Add input fields for the requester:
   - **Employee** (text input): `Default = Blank()`
   - **Start Date**, **End Date** (date pickers): `Default = Today()`

---

### Part D: Show Related Reservations

1. Add a second gallery, data source `Vehicle Reservations`, **Items** set to:
   ```powerapps
   Filter(
     'Vehicle Reservations',
     VehicleID = gal_ChooseAVehicle.Selected.ID
   )
   ```
2. Bind labels inside it to `ThisItem.Employee`, `ThisItem.StartDate`, `ThisItem.EndDate`.

---

### Part E: Add, Edit, Reset, Delete a Reservation

**Add** — Submit button `OnSelect`:
```powerapps
Patch(
  'Vehicle Reservations',
  Defaults('Vehicle Reservations'),
  {
    VehicleID: gal_ChooseAVehicle.Selected.ID,
    Employee: txt_Employee.Text,
    StartDate: dte_StartDate.SelectedDate,
    EndDate: dte_EndDate.SelectedDate
  }
);
Reset(txt_Employee); Reset(dte_StartDate); Reset(dte_EndDate);
```

**Edit** — set the reservations gallery's **Default** to `Defaults('Vehicle Reservations')`, prefill inputs from `gal_CurrentBookings.Selected`, then on Submit:
```powerapps
Patch(
  'Vehicle Reservations',
  Coalesce(
    LookUp('Vehicle Reservations', ID = gal_CurrentBookings.Selected.ID),
    Defaults('Vehicle Reservations')
  ),
  {
    VehicleID: gal_ChooseAVehicle.Selected.ID,
    Employee: txt_Employee.Text,
    StartDate: dte_StartDate.SelectedDate,
    EndDate: dte_EndDate.SelectedDate
  }
);
Reset(txt_Employee); Reset(dte_StartDate); Reset(dte_EndDate); Reset(gal_CurrentBookings);
```

**Reset** — a **New** button:
```powerapps
Reset(gal_CurrentBookings)
```

**Delete** — a trash icon inside the reservations gallery:
```powerapps
Remove(
  'Vehicle Reservations',
  LookUp('Vehicle Reservations', ID = ThisItem.ID)
)
```

---

### Part F: The Admin Angle — Close the Loop

1. **Check for double-booking:** the app as built lets two people reserve the same vehicle on overlapping dates. Add a `Notify()` warning (or block the `Patch`) if a `Filter` on `Vehicle Reservations` finds an overlapping `StartDate`/`EndDate` for the same `VehicleID`.
2. **Check the connector:** in Power Apps Studio, open **File** > **Data sources** and note the SharePoint connector is the only one in use. If someone later adds an HTTP or SQL connector, that's exactly the kind of change a DLP policy (LAB-07) is meant to catch.
3. **Check who can publish:** in the **Power Platform Admin Center**, look at who has Maker rights in your environment — publishing an app is a governance action, same as creating a site.

### Challenge:
Add a **Manager approval** step: before a reservation is written with `Patch`, require the Employee field to match a value that exists in a `Managers` list you look up — a small taste of the approval logic you'll build properly with Power Automate in LAB-05a.

### Knowledge check:
This app reads and writes two SharePoint lists directly, bypassing any custom SharePoint permission UI you might configure later. If you later break inheritance on `Company Vehicles` to restrict editing to fleet admins, will the app's edit buttons automatically respect that? Why or why not?

---

These exercises give hands-on experience planning, configuring, and using Power Apps to build a real, list-relationship-driven application on top of SharePoint Online — and thinking about it the way an admin has to, not just a maker.
