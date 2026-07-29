# Ćwiczenia laboratoryjne SharePoint Online - LAB-07

> **Scenariusz przewodni:** Dział prawny Contoso Consulting właśnie usłyszał o witrynie `Fabrikam Project` i zadał trzy pytania: "Co jeśli ktoś przez pomyłkę nadpisze plik? Co jeśli ktoś przypadkowo wyśle mailem numer karty kredytowej klienta? I co jeśli zostaniemy pozwani i będziemy musieli wszystko zabezpieczyć?" Ten lab odpowiada na wszystkie trzy pytania.

## Lab 1: Zarządzanie cyklem życia zawartości w SharePoint Online
### Cel:
Poznać, jak wersjonowanie chroni zawartość, zanim jeszcze sięgniesz po narzędzia zgodności.

### Kroki:
1. **Zorganizuj zawartość:**
   - Na `Fabrikam Project` (z LAB-03) utwórz bibliotekę dokumentów o nazwie **Project Documents**, z folderami **Contracts** i **Deliverables**.
2. **Zastosuj metadane (przypomnienie z LAB-04):**
   - Dodaj kolumny **Document Type** i **Department** i otaguj kilka przykładowych dokumentów.
3. **Włącz i przetestuj wersjonowanie:**
   - Przejdź do **Ustawienia biblioteki** > **Ustawienia wersjonowania**, włącz historię wersji, zachowaj **10** wersji głównych.
   - Prześlij dokument, edytuj go trzy razy, a następnie otwórz **Historię wersji** i przywróć wcześniejszą wersję. Potwierdź, że jest to samoobsługowe cofnięcie zmian — bez zgłoszenia do administratora.
4. **Zepsuj to celowo:**
   - Usuń całkowicie bieżącą wersję pliku (nie cały dokument, tylko poprzez historię wersji, jeśli Twój plan na to pozwala) i przywróć go — to jest różnica między "ups, zła edycja" (wersjonowanie) a "plik zniknął" (kolejny krok: przechowywanie prawne / kosz), którą skontrastujesz w Lab 3.

### Sprawdzenie wiedzy:
Wersjonowanie chroni przed przypadkowym nadpisaniem. Przed czym ono *nie* chroni, co zdeterminowany złośliwy użytkownik nadal mógłby zrobić?

---

## Lab 2: Przegląd portalu zgodności Microsoft Purview
### Cel:
Zorientować się w Microsoft Purview, zanim zagłębisz się w którekolwiek konkretne narzędzie.

### Kroki:
1. **Otwórz portal:**
   - Przejdź do **portalu Microsoft Purview** (https://purview.microsoft.com).
2. **Zwiedź istotne sekcje:**
   - **Ochrona informacji** — etykiety poufności (Lab 5).
   - **Zapobieganie utracie danych** — zasady DLP (Lab 4).
   - **eDiscovery** — wyszukiwanie zawartości i przechowywanie prawne (Lab 3).
   - **Audyt** — kto co zrobił i kiedy, w całym Microsoft 365.
3. **Sprawdź wynik zgodności:**
   - Otwórz **Compliance Manager**, przejrzyj obecny wynik Contoso i wybierz jedno, najbardziej wpływowe zalecane działanie — wdrożysz jedną poprawę w Labach 3–5.

### Sprawdzenie wiedzy:
Spośród Ochrony informacji, DLP i eDiscovery, które jest *prewencyjne*, które *reaktywne*, a które *dochodzeniowe*? Dopasuj każde do trzech oryginalnych pytań działu prawnego Contoso.

---

## Lab 3: Przegląd mechanizmu eDiscovery
### Cel:
Nauczyć się wyszukiwać, zabezpieczać i eksportować zawartość do celów prawnych lub dochodzeniowych — odpowiadając na pytanie "co jeśli zostaniemy pozwani?".

### Kroki:
1. **Utwórz sprawę eDiscovery:**
   - W Purview przejdź do **eDiscovery** > **eDiscovery (Standard)**, utwórz sprawę o nazwie `Fabrikam Contract Dispute`.
2. **Wyszukaj zawartość:**
   - W ramach sprawy utwórz wyszukiwanie zawartości ograniczone do witryny `Fabrikam Project`, ze słowami kluczowymi pasującymi do Twoich dokumentów testowych i zakresem dat obejmującym moment ich utworzenia.
   - Uruchom wyszukiwanie i przejrzyj wyniki — zwróć uwagę na liczbę elementów i lokalizacje.
3. **Zastosuj przechowywanie prawne (legal hold):**
   - Dodaj przechowywanie do witryny `Fabrikam Project` w ramach sprawy.
   - **Udowodnij, że działa:** spróbuj trwale usunąć jeden z zablokowanych dokumentów (usuń go, a następnie opróżnij go z kosza witryny). Potwierdź, że zawartość nadal jest odzyskiwalna/wykrywalna dzięki przechowywaniu — to cały sens przechowywania prawnego w porównaniu z poleganiem wyłącznie na koszu.
4. **Wyeksportuj wyniki:**
   - Wyeksportuj wyniki wyszukiwania i zwróć uwagę, co jest w nich zawarte (zawartość plus plik ładujący z metadanymi) — to jest to, co faktycznie trafiłoby do zewnętrznej kancelarii prawnej.

### Wyzwanie:
Usuń przechowywanie prawne po zakończeniu testu i udokumentuj w jednym zdaniu, dlaczego pozostawienie aktywnych testowych przechowywań samo w sobie jest problemem governance (podpowiedź: pomyśl, co to robi z rzeczywistym podejściem Contoso do retencji/magazynu).

### Sprawdzenie wiedzy:
Dlaczego przechowywanie prawne jest silniejszą gwarancją niż powiedzenie pracownikom "proszę nie usuwać niczego związanego z tą sprawą"?

---

## Lab 4: Przegląd mechanizmu DLP
### Cel:
Nauczyć się zapobiegać opuszczaniu organizacji przez wrażliwe informacje, zanim to w ogóle nastąpi — odpowiadając na pytanie "co jeśli ktoś udostępni numer karty kredytowej?".

### Kroki:
1. **Utwórz zasadę DLP:**
   - W Purview przejdź do **Zapobieganie utracie danych** > **Zasady**, utwórz nową zasadę korzystając z szablonu **Dane finansowe** (który obejmuje wykrywanie numerów kart kredytowych).
   - Ogranicz jej zakres do **witryn SharePoint** i **kont OneDrive**; uwzględnij jawnie `Fabrikam Project`.
2. **Skonfiguruj wykrywanie i działanie:**
   - Ustaw warunek wykrywania zawartości zawierającej numery kart kredytowych.
   - Ustaw działanie: zablokuj udostępnianie zewnętrzne pasującej zawartości i pokaż użytkownikowi podpowiedź zasad wyjaśniającą dlaczego.
3. **Przetestuj zasadę:**
   - Prześlij testowy dokument zawierający fałszywy, wyraźnie nieprawdziwy wzorzec numeru karty kredytowej (np. przykładowy numer z wytycznych testowych DLP samego Microsoft) do `Fabrikam Project`.
   - Spróbuj udostępnić ten dokument zewnętrznemu kontu gościa Fabrikam z LAB-03. Potwierdź, że pojawia się podpowiedź zasad, a udostępnianie jest zablokowane lub wymaga nadpisania, zgodnie z Twoją konfiguracją.
4. **Sprawdź ślad audytu:**
   - Potwierdź, że dopasowanie DLP pojawia się w **Eksploratorze aktywności** — tak dowiesz się, że zasada faktycznie zadziałała w produkcji, a nie tylko że ją skonfigurowałeś/aś.

### Sprawdzenie wiedzy:
DLP i nawyk udostępniania "Konkretnym osobom" z LAB-03 oba zmniejszają ryzyko wycieku danych. Dlaczego nadal potrzebujesz DLP, nawet jeśli każdy pracownik zawsze ostrożnie udostępnia "Konkretnym osobom"?

---

## Lab 5: Klasyfikacja danych i etykiety poufności
### Cel:
Nauczyć się klasyfikować zawartość, żeby ochrona podróżowała razem z plikiem, gdziekolwiek trafi — zamykając pętlę na wszystkie trzy pytania działu prawnego.

### Kroki:
1. **Utwórz etykiety poufności:**
   - W **Ochronie informacji** utwórz trzy etykiety: `Public`, `Internal`, `Confidential – Client Data`.
   - Na `Confidential – Client Data` skonfiguruj szyfrowanie i ogranicz dostęp wyłącznie do pracowników Contoso (bez dostępu zewnętrznego, nawet z ważnym linkiem udostępniania).
2. **Opublikuj zasadę etykiet:**
   - Utwórz zasadę etykiet publikującą wszystkie trzy etykiety do SharePoint i OneDrive, i ustaw `Internal` jako domyślną dla tenanta.
3. **Zastosuj i udowodnij, że etykieta działa:**
   - Na `Fabrikam Project` zastosuj `Confidential – Client Data` do dokumentu zawierającego dane finansowe klienta.
   - Spróbuj otworzyć ten dokument jako nieuwierzytelniony/zewnętrzny użytkownik (lub zasymuluj to, najpierw usuwając dostęp gościa Fabrikam) — potwierdź, że szyfrowanie faktycznie blokuje dostęp, a nie tylko że widoczna jest etykieta.
4. **Monitoruj klasyfikację w całym tenancie:**
   - Przejdź do **Klasyfikacji danych** w Purview i przejrzyj przegląd zawartości otagowanej i nieotagowanej — tak zmierzysz, czy wdrożenie w Contoso jest faktycznie przyjmowane, a nie tylko skonfigurowane.

### Wyzwanie:
Wróć do Compliance Manager (Lab 2) i oznacz wybrane zalecane działanie jako wdrożone, używając zasady DLP lub etykiety poufności, którą właśnie zbudowałeś/aś, jako dowodu. Obserwuj, jak reaguje wynik zgodności.

### Sprawdzenie wiedzy:
Etykieta poufności podąża za plikiem, nawet jeśli zostanie pobrany i wysłany mailem. Uprawnienie SharePoint tego nie robi. Biorąc to pod uwagę, dlaczego etykietowanie poufności nie jest pełnym zamiennikiem starannych uprawnień na poziomie witryny/biblioteki?

---

Te ćwiczenia dają praktyczne doświadczenie w zarządzaniu cyklem życia zawartości i korzystaniu z Microsoft Purview do zapobiegania, wykrywania i badania ryzyka związanego z danymi w SharePoint Online — trzy pytania, które prędzej czy później zadaje każdy dział prawny i bezpieczeństwa.
