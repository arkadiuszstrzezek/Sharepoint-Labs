# Ćwiczenia laboratoryjne SharePoint Online - LAB-02

> **Scenariusz przewodni:** Jesteś nowym Administratorem SharePoint w **Contoso Consulting**, firmie rosnącej z 50 do 300 pracowników. Kierownictwo chce prawdziwego intranetu: centralnego hubu, dedykowanych witryn działowych (HR, IT, Marketing) oraz jasnego governance dotyczącego szablonów i uprawnień. Każde poniższe ćwiczenie dodaje jeden element tego intranetu, więc pod koniec LAB-02 będziesz mieć małą, ale działającą strukturę hub-and-spoke, na której będziesz mógł/mogła dalej budować w kolejnych labach.

## Lab 1: Planowanie i konfigurowanie kolekcji witryn oraz witryn hubowych
### Cel:
Nauczyć się planować i konfigurować kolekcje witryn oraz witryny hubowe w SharePoint Online.

### Kroki:
1. **Utwórz witrynę, która stanie się hubem:**
   - Przejdź do **Centrum administracyjnego SharePoint** > **Aktywne witryny** > **Utwórz**.
   - Wybierz **Witrynę komunikacyjną** (projekt Temat) o nazwie `Contoso Home`.
2. **Utwórz dwie witryny działowe:**
   - Utwórz **Witrynę zespołu** o nazwie `HR` oraz drugą o nazwie `IT`.
3. **Zarejestruj witrynę hubową:**
   - Wybierz `Contoso Home` w **Aktywnych witrynach**.
   - Kliknij **Hub** > **Zarejestruj jako witrynę hubową**.
   - Nadaj jej nazwę wyświetlaną (`Contoso Hub`) i zdecyduj, **kto może kojarzyć witryny z tym hubem** (pozostaw puste = każdy; lub ogranicz do siebie na razie).
4. **Skojarz witryny działowe:**
   - Wybierz `HR`, kliknij **Hub** > **Skojarz z witryną hubową**, wybierz `Contoso Hub`, zapisz.
   - Powtórz dla `IT`.
5. **Zweryfikuj propagację:**
   - Otwórz `HR` lub `IT` — pasek nawigacyjny i motyw Contoso Hub powinny teraz pojawić się na górze witryny.

### Alternatywa w PowerShell (automatyzacja):
```powershell
New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/ContosoHome -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Contoso Home" -Template SITEPAGEPUBLISHING#0
Register-SPOHubSite -Site https://<your-tenant>.sharepoint.com/sites/ContosoHome -Principals @()
Add-SPOHubSiteAssociation -Site https://<your-tenant>.sharepoint.com/sites/HR -HubSite https://<your-tenant>.sharepoint.com/sites/ContosoHome
```

### Wyzwanie:
- Dodaj trzecią witrynę działową (`Marketing`) i skojarz ją z hubem — **bez użycia interfejsu użytkownika**, tylko za pomocą PowerShell.
- Zmień motyw/logo witryny hubowej i potwierdź, że propaguje się on do wszystkich trzech skojarzonych witryn.

### Sprawdzenie wiedzy:
Dlaczego użyłbyś/łabyś witryny hubowej zamiast klasycznej struktury podwitryn dla HR, IT i Marketingu? Co tracisz, wybierając zamiast tego podwitryny?

---

## Lab 2: SharePoint Classic vs. Modern — zrozumienie wycofywania
### Cel:
Poznać architekturę SharePoint Classic na tyle dobrze, aby ją rozpoznać, oraz zrozumieć, **dlaczego Microsoft ją wycofuje** — to jest teraz podstawowa wiedza administratora, a nie tylko historia.

> ⚠️ **Ważne, aktualne na 2026 rok:** przycisk **Utwórz** w nowoczesnym Centrum administracyjnym SharePoint oferuje wyłącznie nowoczesną **Witrynę zespołu** i **Witrynę komunikacyjną** — nie ma już opcji "Klasyczna witryna zespołu". Ponadto Microsoft etapami wycofuje klasyczne witryny **publikacji**: tworzenie nowych klasycznych witryn publikacji (portal, pusta witryna, witryna publikacji, centrum wyszukiwania dla przedsiębiorstw) zostało zablokowane od **15 września 2025**, a od **15 marca 2026** tymczasowe opcje wyłączenia na poziomie tenanta (np. dla niestandardowych skryptów) zostały wyłączone na stałe. Istniejące klasyczne witryny nadal działają, ale nie możesz już zwiększać tego zasobu. Ten lab jest napisany pod kątem tej rzeczywistości, a nie tworzenia nowej klasycznej witryny, ponieważ ta ścieżka jest teraz zamknięta dla większości tenantów.

### Kroki:
1. **Poszukaj istniejącej klasycznej witryny:**
   - W **Aktywnych witrynach** sprawdź kolumnę **Szablon** (lub uruchom poniższy PowerShell), aby zobaczyć, czy Twój tenant ma już jakieś klasyczne witryny (`STS#0`, `SRCHCEN#0`, `CMSPUBLISHING#0` itd.) — są to zwykle stare domyślne witryny zespołu lub główna kolekcja witryn.
   ```powershell
   Get-SPOSite -Limit All | Select-Object Url, Template
   ```
2. **Spróbuj mimo to ją utworzyć (nauka przez porażkę):**
   - Spróbuj utworzyć klasyczną witrynę zespołu za pomocą PowerShell:
   ```powershell
   New-SPOSite -Url https://<your-tenant>.sharepoint.com/sites/ClassicTest -Owner admin@<your-tenant>.onmicrosoft.com -StorageQuota 1000 -Title "Classic Test" -Template STS#0
   ```
   - Zapisz, czy się to udało, zostało po cichu przekonwertowane na nowoczesny szablon, czy zostało zablokowane przez zasady tenanta. Cokolwiek się stanie, ten wynik *jest* lekcją: udokumentuj go.
3. **Zbadaj dowolną znalezioną klasyczną witrynę (lub tę, którą właśnie utworzyłeś/aś, jeśli się udało):**
   - Poszukaj: podwitryn, interfejsu wstążki, klasycznych składników webowych, stron ustawień `_layouts/15/`.
4. **Uzupełnij tabelę porównawczą** (przyda się też w Lab 3):

   | Aspekt | Klasyczna | Nowoczesna |
   |---|---|---|
   | Nawigacja | Wstążka | Pasek poleceń |
   | Struktura | Podwitryny | Hub + witryny skojarzone |
   | Responsywność | Słaba / desktop-first | W pełni responsywna |
   | Składniki webowe | Klasyczne (Edytor treści itd.) | Nowoczesne (Aktualności, Wyróżniona zawartość...) |
   | Czy nadal można ją dziś utworzyć? | | |

### Dyskusja:
- Dlaczego Microsoft odchodzi od klasycznej architektury (wydajność, urządzenia mobilne, bezpieczeństwo niestandardowych skryptów)?
- Gdyby Contoso nadal miało stary klasyczny intranet, jak wyglądałby Twój plan migracji?

### Sprawdzenie wiedzy:
Twój przełożony prosi Cię o "szybkie postawienie klasycznej witryny publikacji na tymczasową stronę kampanii". Co mu powiesz i co zaproponujesz zamiast tego?

---

## Lab 3: Przegląd nowoczesnej architektury SharePoint
### Cel:
Zrozumieć architekturę nowoczesnych witryn SharePoint poprzez praktyczne eksplorowanie, a nie tylko obserwację.

### Kroki:
1. **Zbuduj prawdziwą stronę:**
   - Na `Contoso Home` (lub nowej Witrynie komunikacyjnej) utwórz stronę i dodaj co najmniej trzy nowoczesne składniki webowe: **Aktualności**, **Wyróżniona zawartość** i **Osadzanie** (np. osadź film z YouTube lub formularz Microsoft Forms).
2. **Przetestuj responsywność:**
   - Zmniejsz okno przeglądarki do szerokości telefonu lub otwórz witrynę w **aplikacji mobilnej SharePoint**. Potwierdź, że układ automatycznie się dostosowuje — bez potrzeby osobnej "wersji mobilnej".
3. **Potwierdź integrację z grupą Microsoft 365:**
   - Otwórz witrynę zespołu `HR` (utworzoną w Lab 1) i sprawdź obszar **Konwersacje**/**Członkowie** — jest on oparty na grupie Microsoft 365. Otwórz Outlook/Teams i potwierdź, że ta sama grupa pojawia się tam.
4. **Zautomatyzuj edycję strony (PnP):**
   ```powershell
   Add-PnPPageWebPart -Page "Home" -DefaultWebPartType News
   ```

### Wyzwanie:
Odtwórz ten sam układ strony, korzystając wyłącznie z PnP PowerShell (bez interfejsu użytkownika) — to dokładnie ten rodzaj zadania, które prawdziwy administrator automatyzuje przy powtarzalnym provisioningu witryn działowych.

### Sprawdzenie wiedzy:
Wymień trzy konkretne przewagi, jakie nowoczesna witryna daje Twoim użytkownikom końcowym w porównaniu z klasyczną witryną, którą zbadałeś/aś w Lab 2.

---

## Lab 4: Szablony i struktury dla witryn klasycznych i nowoczesnych
### Cel:
Poznać, jakie szablony/projekty witryn są dziś dostępne i kiedy z każdego korzystać.

### Kroki:
1. **Zinwentaryzuj dostępne nowoczesne projekty:**
   - W **Utwórz** > **Witryna komunikacyjna** zanotuj trzy oferowane projekty: **Temat**, **Prezentacja**, **Pusta**.
   - W **Utwórz** > **Witryna zespołu** zanotuj, że jest praktycznie jeden nowoczesny projekt (połączony z grupą).
2. **Wylistuj szablony podwitryn za pomocą PowerShell** (podwitryny wciąż korzystają z klasycznego silnika szablonów):
   ```powershell
   Get-PnPWebTemplates
   ```
3. **Zbuduj macierz decyzyjną:**

   | Scenariusz | Zalecany szablon |
   |---|---|
   | Strona docelowa działu HR | Witryna komunikacyjna — Temat |
   | Mikrowitryna wprowadzenia produktu | Witryna komunikacyjna — Prezentacja |
   | Zespół projektowy z dokumentami i planowaniem | Witryna zespołu (grupa Microsoft 365) |
   | Szybka wewnętrzna strona ogłoszeń, bez potrzeby grupy | Witryna komunikacyjna — Pusta |

4. Uzupełnij samodzielnie jeszcze jeden wiersz dla scenariusza istotnego dla Marketingu Contoso i uzasadnij swój wybór.

### Sprawdzenie wiedzy:
Dlaczego Microsoft skonsolidował dziesiątki klasycznych szablonów praktycznie do dwóch nowoczesnych typów witryn?

---

## Lab 5: Przegląd grup Microsoft 365
### Cel:
Zrozumieć, jak grupy Microsoft 365 napędzają członkostwo i uprawnienia w tle Witryny zespołu — w tym, co się dzieje, gdy zmienia się członkostwo.

### Kroki:
1. **Utwórz grupę Microsoft 365:**
   - W Centrum administracyjnym Microsoft 365 przejdź do **Grupy** > **Aktywne grupy** > **Dodaj grupę** > **Microsoft 365**.
   - Nazwij ją `Contoso Marketing`.
2. **Potwierdź, że powiązana witryna SharePoint została automatycznie utworzona:**
   - Znajdź jej witrynę SharePoint w **Aktywnych witrynach** i potwierdź, że nazwa się zgadza z nazwą grupy.
3. **Dodaj członka przez grupę, nie przez witrynę:**
   - W Centrum administracyjnym Microsoft 365 dodaj użytkownika testowego do grupy `Contoso Marketing` jako **Członka**.
   - Przejdź do **Uprawnień witryny** witryny SharePoint — użytkownik powinien już figurować jako **Członek**, bez dotykania SharePoint w ogóle.
4. **Teraz usuń go z grupy:**
   - Usuń tego samego użytkownika z grupy w Centrum administracyjnym Microsoft 365.
   - Odśwież uprawnienia witryny SharePoint i potwierdź, że dostęp użytkownika zniknął (synchronizacja może zająć kilka minut).
5. **Zbadaj szerszą integrację:**
   - Otwórz Teams i potwierdź, że możesz "Utworzyć zespół" z grupy `Contoso Marketing`, ponownie wykorzystując to samo członkostwo i pliki SharePoint.

### Sprawdzenie wiedzy:
Jeśli użytkownik skarży się, że nie może uzyskać dostępu do Witryny zespołu, mimo że "IT dodało go w zeszłym tygodniu", co sprawdzisz najpierw, biorąc pod uwagę to, co właśnie zaobserwowałeś/aś odnośnie synchronizacji sterowanej przez grupę?

---

## Lab 6: Zarządzanie i planowanie witryn
### Cel:
Poćwiczyć decyzje governance, które prawdziwy administrator SharePoint podejmuje na co dzień.

### Kroki:
1. **Audyt governance:**
   - Wybierz jedną z wcześniej utworzonych witryn (np. `HR`) i uzupełnij tę listę kontrolną audytu:

     | Sprawdzenie | Aktualna wartość | Spełnia najlepsze praktyki? |
     |---|---|---|
     | Możliwość udostępniania | | |
     | Czy dziedziczenie uprawnień jest gdzieś przerwane? | | |
     | Typ nawigacji (strukturalna vs. zarządzana) | | |
     | Ustawienia regionalne / strefa czasowa | | |
     | Limit magazynu | | |

2. **Debata: struktura płaska vs. podwitryny:**
   - W dwóch zdaniach uzasadnij **za** płaską strukturą hub-and-spoke dla Contoso, a następnie w dwóch zdaniach uzasadnij **za** głęboką hierarchią podwitryn. Która z nich faktycznie pasuje do wzrostu Contoso z 50 do 300 pracowników i dlaczego?
3. **Popraw jedno ustalenie:**
   - Wybierz jeden wiersz z tabeli audytu, który nie spełnia najlepszych praktyk, i popraw go (np. zaostrz możliwość udostępniania z "Każdy" na "Tylko osoby w Twojej organizacji").

### Sprawdzenie wiedzy:
Dlaczego "płaska struktura z witrynami hubowymi" jest generalnie zalecana zamiast głębokich drzew podwitryn w nowoczesnym SharePoint?

---

## Lab 7: Planowanie struktury witryny hubowej dla intranetu
### Cel:
Zaprojektować i wdrożyć realistyczną strukturę hubu intranetowego od początku do końca.

### Kroki:
1. **Najpierw projekt, potem budowa:**
   - Na papierze (lub w narzędziu do diagramów) naszkicuj intranet Contoso: `Contoso Hub` na górze, z `HR`, `IT` i `Marketing` jako spoke'ami. Zanotuj, które spoke'i dostaną później pod-spoke'i (np. IT → Helpdesk).
2. **Skonfiguruj nawigację hubu:**
   - Na `Contoso Hub` edytuj pasek nawigacyjny, dodając linki do każdej skojarzonej witryny oraz jeden zasób zewnętrzny (np. portal świadczeń pracowniczych firmy).
3. **Przetestuj wyszukiwanie w zakresie hubu:**
   - Będąc wewnątrz witryny `HR`, wyszukaj dokument, który istnieje tylko w `IT`. Ponieważ obie są skojarzone z tym samym hubem, wynik powinien pojawić się w wyszukiwaniu — to jedna z konkretnych korzyści, jakie huby dają w porównaniu z niepołączonymi witrynami.
4. **Dodaj regułę:**
   - Zdecyduj i udokumentuj, kto będzie mógł kojarzyć nowe witryny z `Contoso Hub` w przyszłości (przypomnij sobie ustawienie "kto może kojarzyć" z Lab 1) — to decyzja governance, nie tylko techniczna.

### Wyzwanie:
Dodaj czwartą, zagnieżdżoną warstwę: utwórz witrynę `Helpdesk` i skojarz ją z `IT` zamiast bezpośrednio z `Contoso Hub`. Potwierdź, czy wyszukiwanie w zakresie hubu z `Helpdesk` nadal wyświetla zawartość `HR` (przetestuj rzeczywiste zachowanie — nie zakładaj).

### Sprawdzenie wiedzy:
Twój CEO pyta "dlaczego nie możemy po prostu użyć folderów w jednej wielkiej witrynie zamiast tych wszystkich osobnych witryn i hubu?" Odpowiedz w dwóch zdaniach.

---

## Lab 8: Zarządzanie uprawnieniami witryny
### Cel:
Nauczyć się zarządzać, dostosowywać i **weryfikować** uprawnienia dla witryn SharePoint Online — weryfikacja to część, którą większość administratorów pomija i tego żałuje.

### Kroki:
1. **Nadaj uprawnienia:**
   - Na witrynie `Marketing` przejdź do **Uprawnień witryny**, dodaj użytkownika testowego i przypisz mu rolę **Członek**.
2. **Utwórz niestandardowy poziom uprawnień:**
   - Przejdź do **Uprawnienia witryny** > **Zaawansowane ustawienia uprawnień** > **Poziomy uprawnień** > **Dodaj poziom uprawnień**.
   - Oprzyj go na **Współtworzenie**, ale nazwij `Contributor - No Delete` i odznacz uprawnienia do usuwania (elementy listy, dokumenty, wersje).
   - Utwórz grupę SharePoint `Marketing Contributors (No Delete)` i przypisz jej ten niestandardowy poziom.
3. **Przerwij dziedziczenie:**
   - Otwórz główną bibliotekę dokumentów witryny, przejdź do **Ustawienia biblioteki** > **Uprawnienia dla tej biblioteki dokumentów** i zatrzymaj dziedziczenie uprawnień.
   - Przypisz do tej biblioteki wyłącznie grupę `Marketing Contributors (No Delete)`, usuwając domyślny dostęp Członków.
4. **Zweryfikuj — nie zakładaj:**
   - Użyj **Uprawnienia witryny** > **Sprawdź uprawnienia**, wpisz swojego użytkownika testowego i potwierdź, jaki dokładnie poziom dostępu wynika dla niego w bibliotece.
   - Skrzyżuj to z PowerShell:
   ```powershell
   Get-PnPGroupMembers -Identity "Marketing Contributors (No Delete)"
   Get-PnPUserEffectivePermissions -Identity testuser@<your-tenant>.onmicrosoft.com -List "Documents"
   ```

### Wyzwanie:
Zaudytuj dostęp **Właściciela** we wszystkich trzech witrynach Contoso utworzonych w tym labie (`Contoso Home`, `HR`, `Marketing`), korzystając z:
```powershell
Get-PnPGroupMembers -Identity "Site Owners"
```
uruchomionego kolejno dla każdej witryny. Oznacz każdego, kto jest Właścicielem witryny, na której nie powinien nim być.

### Sprawdzenie wiedzy:
Dlaczego "Sprawdź uprawnienia" (lub `Get-PnPUserEffectivePermissions`) jest bardziej wiarygodne niż samo czytanie list członkostwa w grupach, gdy musisz odpowiedzieć na pytanie "czy ten konkretny użytkownik może otworzyć ten konkretny dokument"?

---

Te ćwiczenia dają Ci praktyczne, aktualne doświadczenie w planowaniu, konfigurowaniu i zarządzaniu governance witryn SharePoint Online oraz witryn hubowych — w tym rozpoznawanie, kiedy podejście "klasyczne" nie jest już dostępne, i wiedzę, po co sięgnąć zamiast tego.
