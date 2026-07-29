# Ćwiczenia laboratoryjne SharePoint Online - LAB-01

> **Scenariusz przewodni:** To Twój pierwszy dzień jako Administrator SharePoint w **Contoso Consulting**, firmie, która ma urosnąć z 50 do 300 pracowników. Nikt jeszcze nie ustawił governance — zaczynasz od praktycznie pustego tenanta. To jest Dzień 1: zorientuj się w sytuacji i udowodnij, że potrafisz wykonać to samo podstawowe zadanie na cztery różne sposoby (UI, SharePoint PowerShell, PnP PowerShell, Graph API), ponieważ prawdziwy administrator musi wiedzieć, które narzędzie pasuje do jakiej sytuacji. LAB-02 i kolejne budują prawdziwy intranet Contoso na bazie tego, co skonfigurujesz tutaj.

## Lab 1: Rola Administratora SharePoint
### Cel:
Zrozumieć, za co faktycznie odpowiada Administrator SharePoint, znajdując prawdziwe odpowiedzi w Centrum administracyjnym — a nie tylko klikając po nim bez celu.

### Kroki:
1. Zaloguj się do **Centrum administracyjnego Microsoft 365** (https://admin.microsoft.com).
2. Przejdź do **Centrum administracyjnego SharePoint**.
3. **Polowanie na informacje — odpowiedz na poniższe pytania, korzystając wyłącznie z Centrum administracyjnego, i zapisz, gdzie znalazłeś/aś każdą odpowiedź:**
   - Ile aktywnych witryn ma obecnie tenant i ile łącznie miejsca w magazynie jest wykorzystane?
   - Czy w **Usuniętych witrynach** znajduje się obecnie coś? (Jeśli jest pusto, zapisz to — w Lab 2 celowo coś tam umieścisz.)
   - Jaki jest obecny poziom udostępniania zewnętrznego tenanta, w sekcji **Zasady** > **Udostępnianie**?
   - Wymień jedno ustawienie w sekcji **Zasady** > **Kontrola dostępu** i przed czym ono chroni.
4. **Omówcie rolę:**
   - Na podstawie tego, co właśnie znalazłeś/aś, wymień trzy decyzje, które podejmuje Administrator SharePoint, a których nie może podjąć zwykły właściciel witryny.

### Sprawdzenie wiedzy:
Właściciel witryny prosi Cię, żeby "po prostu włączyć udostępnianie dla każdego" na jego witrynie. Na podstawie tego, co znalazłeś/aś w kroku 3, co decyduje o tym, czy w ogóle *możesz* się na to zgodzić?

---

## Lab 2: Zarządzanie SharePoint Online za pomocą PowerShell
### Cel:
Nauczyć się zarządzać SharePoint Online za pomocą SharePoint Online Management Shell — narzędzia do administracji na poziomie tenanta (tworzenie witryn, możliwości udostępniania, magazyn), które w interfejsie można wykonać tylko pojedynczo, klikając.

### Wymagania wstępne:
- Zainstaluj **SharePoint Online Management Shell**.
- Połącz się z SharePoint Online:
  ```powershell
  Connect-SPOService -Url https://<your-tenant>-admin.sharepoint.com -Credential (Get-Credential)
  ```
  > **Uwaga:** jeśli tenant wymusza MFA (większość to robi), monit `-Credential (Get-Credential)` się nie powiedzie. Pomiń parametr `-Credential` całkowicie i uruchom po prostu `Connect-SPOService -Url https://<your-tenant>-admin.sharepoint.com` — otworzy się nowoczesne okno logowania obsługujące MFA.

### Kroki:
1. **Pobierz listę wszystkich kolekcji witryn:**
   ```powershell
   Get-SPOSite
   ```
2. **Utwórz nową kolekcję witryn:**
   ```powershell
   New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/LabSite -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Lab Site"
   ```
3. **Zaktualizuj właściwości kolekcji witryn:**
   ```powershell
   Set-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite -SharingCapability ExternalUserSharingOnly
   ```
4. **Zweryfikuj, czy zmiana się przyjęła, nie zakładaj tego z góry:**
   ```powershell
   Get-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite | Select-Object Url, SharingCapability
   ```
5. **Usuń kolekcję witryn:**
   ```powershell
   Remove-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite
   ```
6. **Zamknij pętlę z Lab 1:**
   - Uruchom `Get-SPOSite -Identity https://<your-tenant>.sharepoint.com/sites/LabSite -IncludeRecycled` (lub sprawdź **Usunięte witryny** w Centrum administracyjnym) i potwierdź, że `LabSite` znajduje się teraz w koszu, a nie zniknęło na zawsze.

### Wyzwanie:
Zamień kroki 2–3 w jedną wielokrotnego użytku funkcję `New-ContosoSite -Name "TestSite" -Sharing ExternalUserSharingOnly`, która łączy `New-SPOSite` i `Set-SPOSite` — to jest właśnie kształt, jaki mają prawdziwe skrypty provisioningu.

### Sprawdzenie wiedzy:
Dlaczego `Remove-SPOSite` nie usuwa witryny trwale od razu? Przed jakim realnym błędem chroni to domyślne zachowanie?

---

## Lab 3: Korzystanie z modułu SharePoint PnP PowerShell
### Cel:
Poznać PnP PowerShell — narzędzie do automatyzacji na poziomie *zawartości* (listy, kolumny, elementy), którego moduł SPO z Lab 2 celowo nie obsługuje.

### Wymagania wstępne:
- Zainstaluj moduł PnP PowerShell:
  ```powershell
  Install-Module -Name PnP.PowerShell
  ```
- Udziel zgody administratora dla aplikacji Entra ID PnP Management Shell (jednorazowo, na tenant). `Connect-PnPOnline -Interactive` korzysta z wielotenantowej aplikacji Entra ID dostarczonej przez Microsoft; jeśli nie została ona jeszcze zaakceptowana w Twoim tenancie, logowanie zakończy się błędem "wymagana zgoda administratora". Jako Administrator globalny/SharePoint uruchom:
  ```powershell
  Register-PnPManagementShellAccess
  ```
  Otworzy się okno przeglądarki, w którym zatwierdzasz żądane uprawnienia dla całego tenanta. Trzeba to zrobić tylko raz.
- Połącz się z SharePoint Online:
  ```powershell
  Connect-PnPOnline -Url https://<your-tenant>.sharepoint.com -Interactive
  ```

### Kroki:
1. **Utwórz nową listę:**
   ```powershell
   New-PnPList -Title "Lab List" -Template GenericList
   ```
2. **Dodaj kolumnę do listy:**
   ```powershell
   Add-PnPField -List "Lab List" -DisplayName "Student Name" -InternalName "StudentName" -Type Text
   ```
3. **Dodaj element do listy:**
   ```powershell
   Add-PnPListItem -List "Lab List" -Values @{"StudentName"="John Doe"}
   ```
4. **Wyeksportuj listę do pliku CSV:**
   ```powershell
   Get-PnPListItem -List "Lab List" | Select-Object -ExpandProperty FieldValues | Export-Csv -Path "LabList.csv" -NoTypeInformation
   ```

### Wyzwanie:
Uruchom `Get-SPOSite` (moduł z Lab 2) i `Get-PnPList` (ten moduł) jedno po drugim. Zauważ, że `Get-SPOSite` nie widzi wnętrza list witryny, a `Get-PnPList` nie ma pojęcia o zasadach udostępniania na poziomie tenanta. Napisz jedno zdanie wyjaśniające granicę między tym, do czego służy każdy moduł.

### Sprawdzenie wiedzy:
Musisz masowo utworzyć 50 list w 50 nowych witrynach, z identycznymi kolumnami. Sięgniesz po `Get-SPOSite`/`New-SPOSite` (Lab 2) czy po PnP PowerShell (ten lab)? Dlaczego?

---

## Lab 4: Korzystanie z Microsoft Graph do zarządzania SharePoint
### Cel:
Poznać Microsoft Graph — narzędzie, po które sięgasz, gdy automatyzacja musi działać *poza* PowerShellem całkowicie (aplikacja webowa, niestandardowy konektor Power Automate, aplikacja mobilna).

### Wymagania wstępne:
- Zarejestruj aplikację w **Microsoft Entra ID** (entra.microsoft.com) i uzyskaj token dostępu z odpowiednimi uprawnieniami Microsoft Graph (np. `Sites.Read.All`).
- Użyj narzędzi takich jak Postman lub **Graph Explorer** (https://developer.microsoft.com/graph/graph-explorer), aby wypróbować żądania bez pisania klienta od zera.

### Kroki:
1. **Pobierz listę witryn SharePoint:**
   - Endpoint:
     ```http
     GET https://graph.microsoft.com/v1.0/sites
     ```
   - Przykładowa odpowiedź:
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
2. **Utwórz nową listę w witrynie:**
   - Endpoint:
     ```http
     POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists
     ```
   - Treść żądania:
     ```json
     {
       "displayName": "Lab List",
       "list": {
         "template": "genericList"
       }
     }
     ```
3. **Dodaj element do listy:**
   - Endpoint:
     ```http
     POST https://graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items
     ```
   - Treść żądania:
     ```json
     {
       "fields": {
         "Title": "John Doe"
       }
     }
     ```
4. **Wypróbuj na żywo:**
   - Otwórz Graph Explorer, zaloguj się i uruchom żądanie `GET /sites` naprawdę, względem swojego tenanta. Znajdź `id` witryny swojej `Lab List` w odpowiedzi.

### Wyzwanie:
Właśnie zrobiłeś/aś te same trzy rzeczy — utworzenie listy, dodanie kolumny (pomiń dla Graph), dodanie elementu — w PnP PowerShell (Lab 3), a teraz w Graph. Wymień jeden scenariusz, w którym zadziała tylko Graph (podpowiedź: pomyśl o czymkolwiek, co nie jest maszyną Windows z zainstalowanym PowerShellem).

### Sprawdzenie wiedzy:
Uzupełnij tę tabelę decyzyjną na podstawie tego, co właśnie zrobiłeś/aś na cztery sposoby — UI, moduł `SPO`, moduł `PnP`, Graph API:

| Zadanie | Najlepsze narzędzie | Dlaczego |
|---|---|---|
| Jednorazowa zmiana zasad udostępniania tenanta | | |
| Masowe provisionowanie 50 witryn ze skryptu | | |
| Niestandardowa aplikacja webowa, która musi odczytywać listy SharePoint | | |
| Szkolenie nowego właściciela witryny, który nigdy nie używał PowerShell | | |

---

## Lab 5: Twoje pierwsze prawdziwe zadanie administracyjne w piaskownicy
### Cel:
Połączyć Laby 1–4 w jedno małe, kompletne zadanie administracyjne — od początku do końca, w interfejsie użytkownika — zanim zaczniesz budować prawdziwy intranet Contoso w LAB-02.

### Kroki:
1. **Utwórz nową kolekcję witryn:**
   - Przejdź do **Aktywne witryny** > **Utwórz** > **Witryna zespołu**, nazwij ją `IT Sandbox`.
2. **Skonfiguruj ustawienia udostępniania:**
   - Przejdź do **Zasady** > **Udostępnianie** i ustaw udostępnianie *na poziomie witryny* dla `IT Sandbox` na **Tylko osoby w Twojej organizacji** — najbardziej restrykcyjną opcję, ponieważ to tylko przestrzeń robocza, a nie coś, co widzi klient.
3. **Wygeneruj trochę aktywności:**
   - Prześlij kilka plików testowych i dodaj jeden element listy, żeby było co monitorować.
4. **Monitoruj wykorzystanie witryny:**
   - Przejdź do **Aktywne witryny**, wybierz `IT Sandbox` i zobacz kartę **Użycie**, aby zobaczyć dane o aktywności.
5. **Posprzątaj jak profesjonalista:**
   - Usuń `IT Sandbox`, gdy skończysz — prawdziwa struktura Contoso zaczyna się od nowa w LAB-02, a zostawianie porzuconych witryn typu piaskownica to dokładnie ten rozrost (sprawl), z którym będziesz walczyć w ramach governance później (LAB-06, LAB-08a).

### Sprawdzenie wiedzy:
Masz teraz cztery sposoby na wykonanie pierwszych dwóch kroków Lab 5 (UI tutaj, plus `New-SPOSite`/`Set-SPOSite` z Lab 2, PnP z Lab 3, Graph z Lab 4). Którego użyłbyś/łabyś faktycznie pierwszego dnia w prawdziwej pracy, a którego, gdy Contoso ma już 300 pracowników i tworzysz witryny co tydzień? Uzasadnij różnicę.

---

Te ćwiczenia dają Ci praktyczny obraz roli Administratora SharePoint oraz praktyczną biegłość we wszystkich czterech sposobach zarządzania SharePoint Online — Centrum administracyjne, SharePoint PowerShell, PnP PowerShell i Microsoft Graph — zestaw narzędzi, na którym bazuje każdy kolejny lab w tym kursie.
