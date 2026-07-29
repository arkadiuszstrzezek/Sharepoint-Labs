# Ćwiczenia laboratoryjne SharePoint Online - LAB-05

> **Scenariusz przewodni:** Pracownicy Contoso Consulting nieustannie wysyłają maile do kierownika biura, żeby zarezerwować firmowe pojazdy, i panuje w tym chaos. Naprawisz to za pomocą aplikacji Power App opartej na listach SharePoint — a przy okazji dowiesz się, dlaczego administrator SharePoint musi dbać o Power Platform, mimo że "to nie jest SharePoint".

## Lab 1: Planowanie i konfigurowanie dostosowań i aplikacji
### Cel:
Poznać, jak Power Apps i listy SharePoint współpracują ze sobą, budując prawdziwe aplikacje biznesowe, oraz zrozumieć, co administrator SharePoint musi wiedzieć, nawet jeśli to nie on buduje aplikację.

### Dlaczego administrator SharePoint powinien się tym przejmować:
Aplikacje Power Apps zbudowane na listach SharePoint dziedziczą model uprawnień SharePoint, obciążają te same limity magazynu i mogą przesyłać dane przez konektory, na które zasady DLP Twojego tenanta (patrz LAB-07) albo pozwalają, albo nie. "To tylko aplikacja programisty-obywatela" nadal jest Twoim problemem governance.

---

### Część A: Zaprojektuj dane — listy SharePoint i relacje jeden-do-wielu

Utwórz dwie listy SharePoint na nowej witrynie o nazwie `Contoso Fleet`:

**Lista 1: `Company Vehicles`**

| ID | YearMakeModel | AssetCode | LicensePlate | Office |
|----|----------------------|-----------|----------|------------|
| 1 | 2020 Dodge Ram | 10-023 | Y2K 9D9 | Albany |
| 2 | 2016 Ford F150 | 10-034 | Q9T 9T5 | Albany |
| 3 | 2019 GMC Sierra | 10-122 | A7I 0Z5 | Fargo |
| 4 | 2020 Honda Ridgeline | 10-021 | J9B 1P8 | Schaumberg |
| 5 | 2021 Nissan Pathfinder | 10-301 | Q7K 7L7 | Schaumberg |

**Lista 2: `Vehicle Reservations`**

| ID | VehicleID | Employee | StartDate | EndDate |
|----|-----------|---------------|------------|------------|
| 1 | 3 | Mark Clark | 2026-08-25 | 2026-08-29 |
| 2 | 2 | Anna Sinclair | 2026-08-25 | 2026-08-27 |
| 3 | 4 | Laura Andrews | 2026-08-27 | 2026-08-29 |
| 4 | 1 | Sarah Green | 2026-08-25 | 2026-08-25 |
| 5 | 1 | John Freeman | 2026-08-26 | 2026-08-27 |

Kolumna `VehicleID` w `Vehicle Reservations` łączy się z `ID` w `Company Vehicles` — klasyczna relacja jeden-do-wielu, ten sam wzorzec, który zobaczysz w niemal każdej prawdziwej aplikacji biznesowej opartej na SharePoint.

**Punkt kontrolny:** przed zbudowaniem aplikacji sprawdź **Uprawnienia witryny** na `Contoso Fleet` — zdecyduj, kto powinien móc edytować `Company Vehicles` (tylko administratorzy floty) w porównaniu z tym, kto powinien móc dodawać wiersze do `Vehicle Reservations` (wszyscy). Projekt członkostwa listy to decyzja dotycząca uprawnień, nie tylko modelowania danych.

---

### Część B: Wybór pojazdu

1. Otwórz Power Apps Studio i utwórz nową aplikację na tablet połączoną z `Contoso Fleet`.
2. Wstaw galerię pionową, ustaw jej źródło danych na `Company Vehicles`.
3. Zmień układ na **Title**, ustaw pole tytułu na `YearMakeModel`.
4. Dodaj etykietę nad galerią: **Choose A Vehicle.**

---

### Część C: Formularz rezerwacji

1. Dodaj etykietę powiązaną z wyborem:
   ```powerapps
   gal_ChooseAVehicle.Selected.YearMakeModel
   ```
2. Dodaj pola tylko do odczytu dla **Asset Code**, **License Plate** i **Office**, każde powiązane w ten sam sposób (np. `gal_ChooseAVehicle.Selected.AssetCode`), z **DisplayMode** ustawionym na `DisplayMode.Disabled`.
3. Dodaj pola wejściowe dla osoby składającej wniosek:
   - **Employee** (pole tekstowe): `Default = Blank()`
   - **Start Date**, **End Date** (selektory dat): `Default = Today()`

---

### Część D: Pokaż powiązane rezerwacje

1. Dodaj drugą galerię, źródło danych `Vehicle Reservations`, **Items** ustawione na:
   ```powerapps
   Filter(
     'Vehicle Reservations',
     VehicleID = gal_ChooseAVehicle.Selected.ID
   )
   ```
2. Powiąż etykiety wewnątrz niej z `ThisItem.Employee`, `ThisItem.StartDate`, `ThisItem.EndDate`.

---

### Część E: Dodawanie, edytowanie, resetowanie, usuwanie rezerwacji

**Dodawanie** — przycisk Submit, `OnSelect`:
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

**Edycja** — ustaw **Default** galerii rezerwacji na `Defaults('Vehicle Reservations')`, wstępnie wypełnij pola z `gal_CurrentBookings.Selected`, a następnie na Submit:
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

**Resetowanie** — przycisk **New**:
```powerapps
Reset(gal_CurrentBookings)
```

**Usuwanie** — ikona kosza wewnątrz galerii rezerwacji:
```powerapps
Remove(
  'Vehicle Reservations',
  LookUp('Vehicle Reservations', ID = ThisItem.ID)
)
```

---

### Część F: Perspektywa administratora — zamknięcie pętli

1. **Sprawdź podwójne rezerwacje:** aplikacja w obecnej postaci pozwala dwóm osobom zarezerwować ten sam pojazd w nakładających się terminach. Dodaj ostrzeżenie `Notify()` (lub zablokuj `Patch`), jeśli `Filter` na `Vehicle Reservations` znajdzie nakładający się `StartDate`/`EndDate` dla tego samego `VehicleID`.
2. **Sprawdź konektor:** w Power Apps Studio otwórz **File** > **Data sources** i zauważ, że konektor SharePoint jest jedynym używanym. Jeśli ktoś później doda konektor HTTP lub SQL, to dokładnie ten rodzaj zmiany, który ma wychwycić zasada DLP (LAB-07).
3. **Sprawdź, kto może publikować:** w **Power Platform Admin Center** sprawdź, kto ma uprawnienia twórcy (Maker) w Twoim środowisku — publikowanie aplikacji to działanie governance, tak samo jak tworzenie witryny.

### Wyzwanie:
Dodaj krok **zatwierdzenia przez menedżera**: zanim rezerwacja zostanie zapisana za pomocą `Patch`, wymagaj, aby pole Employee pasowało do wartości istniejącej na liście `Managers`, którą sprawdzasz przez LookUp — mała próbka logiki zatwierdzania, którą zbudujesz porządnie za pomocą Power Automate w LAB-05a.

### Sprawdzenie wiedzy:
Ta aplikacja czyta i zapisuje bezpośrednio do dwóch list SharePoint, pomijając wszelki niestandardowy interfejs uprawnień SharePoint, który mógłbyś skonfigurować później. Jeśli później przerwiesz dziedziczenie na `Company Vehicles`, aby ograniczyć edycję do administratorów floty, czy przyciski edycji w aplikacji automatycznie to uwzględnią? Dlaczego tak lub dlaczego nie?

---

Te ćwiczenia dają praktyczne doświadczenie w planowaniu, konfigurowaniu i używaniu Power Apps do budowania prawdziwej aplikacji opartej na relacjach między listami na bazie SharePoint Online — i myślenia o tym tak, jak musi myśleć administrator, a nie tylko twórca aplikacji.
