# Ćwiczenia laboratoryjne SharePoint Online - LAB-09

> **Scenariusz przewodni:** Witryna `Fabrikam Project` Contoso Consulting (LAB-03) ma dokumenty, ale zespół wciąż rozmawia o nich przez e-mail. Kierownictwo chce mieć wszystko — czat, spotkania, pliki — w jednym miejscu i chce wiedzieć, co "społeczność" (Viva Engage) właściwie dodaje ponad to. Ten lab łączy to wszystko razem.

## Lab 1: Zarządzanie dodatkowymi aplikacjami w SharePoint Online
### Cel:
Poznać, jak aplikacje rozszerzają poszczególne witryny oraz jak katalog aplikacji tenanta kontroluje, co w ogóle wolno uruchomić.

### Kroki:
1. **Zbadaj dostępne aplikacje:**
   - Przejdź do **Centrum administracyjnego SharePoint** > **Więcej funkcji** > **Aplikacje** > **Otwórz**, a następnie wybierz **Katalog aplikacji**.
   - Przejrzyj, co jest już dostępne, i przeglądaj **Microsoft AppSource** w poszukiwaniu dodatkowych składników webowych/aplikacji.
2. **Dodaj aplikację do witryny:**
   - Na `Fabrikam Project` przejdź do **Zawartość witryny** > **Nowy** > **Aplikacja**.
   - Dodaj odpowiednią aplikację (np. składnik webowy raportu Power BI) i skonfiguruj ją.
3. **Włącz wdrażanie niestandardowych aplikacji:**
   - W Katalogu aplikacji zwróć uwagę na opcję **katalogu aplikacji kolekcji witryn** (ograniczonego do jednej witryny) w porównaniu z **katalogiem aplikacji tenanta** (dostępnym wszędzie).
   - Omówcie: dlaczego Contoso chciałoby, żeby witryna projektu klienckiego jak `Fabrikam Project` miała własny, ograniczony katalog aplikacji zamiast czerpać z tego ogólnotenantowego?
4. **Sprawdzenie governance:**
   - Przejrzyj, kto obecnie ma uprawnienia do przesyłania do katalogu aplikacji tenanta. To prawdziwa granica bezpieczeństwa — złośliwa lub słabo zbudowana niestandardowa aplikacja wdrożona w całym tenancie może dotknąć każdej witryny.

### Sprawdzenie wiedzy:
Jaka jest faktyczna różnica bezpieczeństwa między dodaniem przez użytkownika aplikacji z Microsoft AppSource do własnej witryny a przesłaniem przez administratora niestandardowej aplikacji do katalogu aplikacji tenanta?

---

## Lab 2: Integracja Microsoft Teams z SharePoint Online
### Cel:
Poznać obecny (zaczynający się od witryny) sposób łączenia witryny zespołu SharePoint z Microsoft Teams oraz kiedy starsza metoda, zaczynająca się od Teams, wciąż jest właściwym narzędziem.

> ⚠️ **Przepływ ostatnio się zmienił:** stara ścieżka wejścia do Teams i wybrania "Utwórz zespół z istniejącej witryny SharePoint" nie jest już głównym przepływem. Dziś punkt wejścia znajduje się na **samej witrynie SharePoint**.

### Kroki:
1. **Dodaj Teams od strony SharePoint (obecna metoda):**
   - Otwórz połączoną z grupą Witrynę zespołu, której jesteś właścicielem (np. `HR` z LAB-02).
   - Na stronie głównej witryny znajdź **Dodaj czat w czasie rzeczywistym** (lub **Dodaj Microsoft Teams** w panelu "Następne kroki", w prawym górnym rogu).
   - Przejdź przez krótkie wprowadzenie, a następnie wybierz, które zasoby SharePoint (strony, aktualności, listy, biblioteki dokumentów) mają być widoczne jako karty wewnątrz nowego zespołu Teams.
   - Potwierdź: to działa tylko dlatego, że `HR` jest już połączone z grupą Microsoft 365 (LAB-02, Lab 5) — witryna niepołączona z grupą (jak Witryna komunikacyjna) nie może zostać w ten sposób przekonwertowana.
2. **Metoda zapasowa (zaczynająca się od Teams, wciąż ważna):**
   - Jeśli "Dodaj czat w czasie rzeczywistym" nie jest dostępne, otwórz **Microsoft Teams** > **Utwórz zespół** > **Więcej opcji tworzenia zespołu** > **Z grupy** i wybierz grupę stojącą za `HR`.
   - Porównaj: ten sam wynik końcowy, inny punkt startowy — dobrze wiedzieć oba sposoby, gdy kolega opisuje starszy zrzut ekranu z bloga lub kursu.
3. **Dodaj biblioteki SharePoint jako karty:**
   - W powstałym zespole Teams otwórz kanał, kliknij **+**, aby dodać kartę, wybierz **Bibliotekę dokumentów** i połącz ją z biblioteką dokumentów `HR`.
   - Potwierdź, że członkowie zespołu mogą otworzyć i edytować dokument bezpośrednio wewnątrz Teams oraz zobaczyć tę samą aktualizację pliku na żywo w SharePoint.
4. **Skonfiguruj zasadę po stronie Teams:**
   - W **Centrum administracyjnym Teams** (https://admin.teams.microsoft.com) > **Aplikacje Teams** > **Zasady konfiguracji** potwierdź, że aplikacja SharePoint jest włączona dla odpowiedniej grupy użytkowników.

### Sprawdzenie wiedzy:
Dlaczego "Dodaj Microsoft Teams" pojawia się tylko na Witrynach zespołu połączonych z grupą, a nie na Witrynach komunikacyjnych, jak `Contoso Home` z LAB-02?

---

## Lab 3: Integracja Viva Engage z SharePoint Online
### Cel:
Zrozumieć, co Viva Engage (dawniej Yammer) dodaje ponad to, czego nie dają kanały Teams i SharePoint — rozmowę międzyzespołową, obejmującą całą organizację — i połączyć ją z witryną.

### Kroki:
1. **Włącz Viva Engage:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** > **Ustawienia** > **Ustawienia organizacji** > **Viva Engage** i potwierdź, że jest włączone dla Contoso.
   - Zwróć uwagę, że głębsza administracja na poziomie społeczności (tworzenie/moderowanie społeczności, ustawienia całej sieci) odbywa się we własnym obszarze administracyjnym Viva Engage, dostępnym z ikony koła zębatego wewnątrz samej aplikacji Viva Engage — osobna warstwa od przełącznika w centrum administracyjnym Microsoft 365.
2. **Utwórz społeczność i połącz ją z witryną:**
   - W Viva Engage utwórz społeczność o nazwie `Contoso All Hands`.
   - Na `Contoso Home` (LAB-02) edytuj stronę główną i dodaj składnik webowy **Konwersacje Viva Engage**, skonfigurowany tak, aby pokazywał społeczność `Contoso All Hands`.
3. **Wprowadź to również do Teams:**
   - W Microsoft Teams dodaj aplikację **Viva Engage** do zespołu i skonfiguruj ją, aby wyświetlała tę samą społeczność — teraz ta sama rozmowa jest dostępna z SharePoint, Teams i samodzielnej aplikacji Viva Engage.
4. **Porównaj to konkretnie z kanałem Teams:**
   - Opublikuj wiadomość na kanale zespołu `HR` (Lab 2) i wiadomość w `Contoso All Hands`. Omówcie, kto realnie może zobaczyć każdą z nich (kanał = tylko członkowie zespołu HR; społeczność = potencjalnie cała organizacja) — to jest właściwe kryterium decyzyjne "kanał Teams vs. społeczność Viva Engage", nie tylko lista funkcji.
5. **Skonfiguruj ustawienia ogólnoorganizacyjne:**
   - Wewnątrz ustawień administracyjnych Viva Engage przejrzyj uprawnienia do tworzenia społeczności (kto może zakładać nowe społeczności — otwarte dla wszystkich czy ograniczone?), ustawienia współpracy zewnętrznej oraz retencję danych.

### Sprawdzenie wiedzy:
Zespół Marketingu Contoso chce mieć przestrzeń do dzielenia się ogłoszeniami dla całej firmy, które mogliby zobaczyć i na które mogliby zareagować wszyscy spośród 300 pracowników. Czy powinien to być kanał Teams, czy społeczność Viva Engage? Uzasadnij to na podstawie tego, co właśnie zaobserwowałeś/aś o zasięgu odbiorców.

---

## Lab 4: Administracyjna integracja Teams i Viva Engage
### Cel:
Odejść od pojedynczych funkcji do ogólnotenantowego monitorowania i governance wszystkiego, co dotąd zintegrowano.

### Kroki:
1. **Monitoruj użycie:**
   - W **Centrum administracyjnym Microsoft 365** > **Raporty** > **Użycie** przejrzyj raporty adopcji dla Teams, SharePoint i Viva Engage obok siebie.
   - Ustal: czy Contoso faktycznie korzysta ze społeczności `Contoso All Hands` i zbudowanego zespołu `HR`, czy aktywność pozostała w mailu/czacie?
2. **Skonfiguruj zasady governance:**
   - W **Centrum administracyjnym Teams** ustaw zasadę dostępu gościa dla Teams co najmniej tak restrykcyjną, jak sufit udostępniania zewnętrznego SharePoint ustawiony w LAB-03 — niespójna zasada gości między Teams a SharePoint to częste, łatwe do uniknięcia ustalenie audytowe.
3. **Zaudytuj aktywność integracji:**
   - W portalu **Microsoft Purview** (LAB-07) przejdź do **Audytu** i wyszukaj aktywność związaną z aplikacją dodaną w Lab 1, konwersją Teams-z-witryny w Lab 2 oraz utworzeniem społeczności w Lab 3.
   - Potwierdź, że potrafisz odpowiedzieć "kto co zainstalował i kiedy" na wszystko, co zbudowałeś/aś w tym labie, korzystając wyłącznie z logu audytu.

### Wyzwanie:
Stwórz jednoakapitową "mapę integracji" dla Contoso: jaka zawartość żyje natywnie w SharePoint, jakie rozmowy odbywają się na kanałach Teams, a co należy do społeczności Viva Engage — z jednozdaniową regułą dla każdej, żeby nowy pracownik nie musiał zgadywać, gdzie coś opublikować.

### Sprawdzenie wiedzy:
Gdyby raporty użycia (krok 1) pokazały, że zespół `HR` i społeczność `Contoso All Hands` są prawie puste miesiąc po uruchomieniu, czy to problem techniczny, czy coś innego — i co faktycznie sprawdziłbyś/łabyś najpierw?

---

Te ćwiczenia dają praktyczne doświadczenie w integrowaniu i zarządzaniu governance Microsoft Teams i Viva Engage obok SharePoint Online, wykorzystując obecny (zaczynający się od witryny) przepływ integracji zamiast nieaktualnych zrzutów ekranu.
