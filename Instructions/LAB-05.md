# SharePoint Online Lab Exercises - LAB-05

## Lab 1: Planning and Configuring Customizations and Applications
### Objective:
Learn how to plan and configure customizations and applications in SharePoint Online using Power Automate and Power Apps.

### Steps:
Here’s the instruction converted to an English Markdown (`.md`) file. You can copy and save it as, for example, `reserve-vehicle-app.md`.


# Power Apps: Reserve A Vehicle App Using SharePoint List Relationships

## Table of Contents

- [Introduction: Reserve A Vehicle App](#introduction-reserve-a-vehicle-app)
- [SharePoint Lists & One‑to‑Many Relationships](#sharepoint-lists--one-to-many-relationships)
- [Select A Vehicle](#select-a-vehicle)
- [Reservation Form](#reservation-form)
- [Showing Related Reservations](#showing-related-reservations)
- [Adding A New Reservation](#adding-a-new-reservation)
- [Editing An Existing Reservation](#editing-an-existing-reservation)
- [Resetting The Form](#resetting-the-form)
- [Deleting A Reservation](#deleting-a-reservation)

---

## Introduction: Reserve A Vehicle App

The **Reserve A Vehicle** app enables company employees to book a vehicle for travel. The user selects a vehicle, enters reservation details (name, dates), and submits. Existing reservations can be edited or deleted.

---

## SharePoint Lists & One‑to‑Many Relationships

### List 1: `Company Vehicles`

| ID | YearMakeModel         | AssetCode | LicensePlate | Office     |
|----|-----------------------|-----------|--------------|------------|
| 1  | 2020 Dodge Ram        | 10-023    | Y2K 9D9      | Albany     |
| 2  | 2016 Ford F150        | 10-034    | Q9T 9T5      | Albany     |
| 3  | 2019 GMC Sierra       | 10-122    | A7I 0Z5      | Fargo      |
| 4  | 2020 Honda Ridgeline  | 10-021    | J9B 1P8      | Schaumberg |
| 5  | 2021 Nissan Pathfinder| 10-301    | Q7K 7L7      | Schaumberg |

### List 2: `Vehicle Reservations`

| ID | VehicleID | Employee      | StartDate  | EndDate    |
|----|-----------|---------------|------------|------------|
| 1  | 3         | Mark Clark    | 2021-01-25 | 2021-01-29 |
| 2  | 2         | Anna Sinclair | 2021-01-25 | 2021-01-27 |
| 3  | 4         | Laura Andrews | 2021-01-27 | 2021-01-29 |
| 4  | 1         | Sarah Green   | 2021-01-25 | 2021-01-25 |
| 5  | 1         | John Freeman  | 2021-01-26 | 2021-01-27 |
| 6  | 3         | Laura Andrews | 2021-02-01 | 2021-02-05 |
| 7  | 3         | Mark Clark    | 2021-02-10 | 2021-02-10 |
| 8  | 5         | Anna Sinclair | 2021-02-13 | 2021-02-14 |

The `VehicleID` column in `Vehicle Reservations` links to the `ID` in `Company Vehicles`, creating a one-to-many relationship.

---

## Select A Vehicle

1. Open Power Apps Studio and create a new tablet app.
2. Insert a vertical gallery and set its data source to `Company Vehicles`.
3. Change the layout to **Title** and set the title field to `YearMakeModel`.
4. Move the right-chevron icon to the left of the label and change its **Icon** property to `Icon.Cars`.
5. Add a label above the gallery with the text: **Choose A Vehicle.**

---

## Reservation Form

1. Add a label to display the selected vehicle:

   ```powerapps
    gal_ChooseAVehicle.Selected.YearMakeModel
   ````

2. Add the following read-only fields:

   * **Asset Code**:

     ```powerapps
     gal_ChooseAVehicle.Selected.AssetCode
     ```

   * **License Plate**:

     ```powerapps
     gal_ChooseAVehicle.Selected.LicensePlate
     ```

   * **Office**:

     ```powerapps
     gal_ChooseAVehicle.Selected.Office
     ```

   → Set their **DisplayMode** to `DisplayMode.Disabled`.

3. Add fields for user input:

   * **Employee** (text input): `Default = Blank()`
   * **Start Date**, **End Date** (date pickers): `Default = Today()`

---

## Showing Related Reservations

1. Add a blank gallery at the bottom of the screen and set its data source to `Vehicle Reservations`.

2. In the **Items** property, set:

   ```powerapps
   Filter(
     'Vehicle Reservations',
     VehicleID = gal_ChooseAVehicle.Selected.ID
   )
   ```

3. Inside the gallery, add labels bound to:

   * `ThisItem.Employee`
   * `ThisItem.StartDate`
   * `ThisItem.EndDate`

4. Add a header label above the gallery to indicate columns.

---

## Adding A New Reservation

1. Insert a **Send** icon and a label named **Submit**.
2. Use this code in their **OnSelect** property:

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
   Reset(txt_Employee);
   Reset(dte_StartDate);
   Reset(dte_EndDate);
   ```

---

## Editing An Existing Reservation

1. Add an **Edit** icon within the reservations gallery.

2. Set the gallery’s **Default** property to:

   ```powerapps
   Defaults('Vehicle Reservations')
   ```

3. Update defaults for input fields:

   * **Employee**:

     ```powerapps
     gal_CurrentBookings.Selected.Employee
     ```

   * **Start Date**, **End Date**:

     ```powerapps
     Coalesce(
       gal_CurrentBookings.Selected.StartDate,
       Today()
     )
     Coalesce(
       gal_CurrentBookings.Selected.EndDate,
       Today()
     )
     ```

4. Update the **OnSelect** for Submit:

   ```powerapps
   Patch(
     'Vehicle Reservations',
     Coalesce(
       LookUp(
         'Vehicle Reservations',
         ID = gal_CurrentBookings.Selected.ID
       ),
       Defaults('Vehicle Reservations')
     ),
     {
       VehicleID: gal_ChooseAVehicle.Selected.ID,
       Employee: txt_Employee.Text,
       StartDate: dte_StartDate.SelectedDate,
       EndDate: dte_EndDate.SelectedDate
     }
   );
   Reset(txt_Employee);
   Reset(dte_StartDate);
   Reset(dte_EndDate);
   Reset(gal_CurrentBookings);
   ```

---

## Resetting The Form

1. Add an **Add** icon and label named **New**.
2. Set the **OnSelect** to:

   ```powerapps
   Reset(gal_CurrentBookings)
   ```

---

## Deleting A Reservation

1. Insert a **Trash** icon in the gallery.
2. Use this code in **OnSelect**:

   ```powerapps
   Remove(
     'Vehicle Reservations',
     LookUp(
       'Vehicle Reservations',
       ID = ThisItem.ID
     )
   )
   ```

---

**Done!** You now have a full guide for building a Power Apps reservation system with SharePoint list relationships.

```

---

Let me know if you’d like a downloadable `.md` file or any tweaks!
```

---

These exercises provide hands-on experience with planning, configuring, and using Power Automate and Power Apps to create custom workflows and applications integrated with SharePoint Online.