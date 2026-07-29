# Ćwiczenia laboratoryjne SharePoint Online - LAB-08

> **Scenariusz przewodni:** Biblioteka `Contracts` Contoso Consulting szybko rośnie, a ktoś musi ręcznie otworzyć każdą umowę, żeby wiedzieć, jakiego jest typu i kiedy wygasa. To dokładnie problem ręcznego tagowania, który ma rozwiązać **SharePoint Premium** (usługi zawartości AI wcześniej znane jako **SharePoint Syntex**).

> ⚠️ **Uwaga dotycząca nazewnictwa, aktualna na 2026 rok:** Microsoft zmienił markę SharePoint Syntex na **SharePoint Premium** pod koniec 2023 roku, a następnie w 2024 roku przemianował komponenty rozliczane w modelu pay-as-you-go konkretnie na **usługi przetwarzania dokumentów**. Nadal zobaczysz "Syntex" w niektórych menu, dokumentacji i treściach społeczności — traktuj to jako tę samą leżącą u podstaw funkcjonalność pod nowszą nazwą parasolową, a nie inny produkt.

## Lab 1: Zarządzanie zawartością przed jej automatyzacją
### Cel:
Zbudować realistyczną, nieuporządkowaną bibliotekę zawartości — dokładnie taką, na którą faktycznie ma być skierowany SharePoint Premium.

### Kroki:
1. **Zorganizuj zawartość:**
   - Na witrynie o nazwie `Contoso Legal` utwórz bibliotekę dokumentów o nazwie **Contracts**.
   - Dodaj foldery **Employment Contracts** i **Vendor Contracts**.
   - Prześlij 5–10 przykładowych dokumentów w stylu umów (prawdziwe szablony lub proste wygenerowane atrapy) z różnorodną zawartością.
2. **Włącz wersjonowanie (przypomnienie z LAB-07):**
   - Włącz historię wersji, zachowaj 10 wersji.
3. **Zastosuj metadane najpierw ręcznie:**
   - Dodaj kolumny **Contract Type** i **Expiration Date** i otaguj każdy dokument *ręcznie*.
   - Zmierz sobie czas. Zapisz, ile to zajęło dla zaledwie 10 dokumentów — porównasz to z automatycznym wynikiem w Lab 3.

### Sprawdzenie wiedzy:
Gdyby Contoso miało 10 000 umów zamiast 10, co poza samym "to wolno" psuje się w podejściu "otaguj ręcznie"?

---

## Lab 2: Przegląd SharePoint Premium (rozumienie treści)
### Cel:
Zrozumieć, co faktycznie robi SharePoint Premium oraz jak wygląda rzeczywistość licencjonowania.

### Kroki:
1. **Czym to jest?**
   - Omówcie: SharePoint Premium wykorzystuje modele AI do czytania dokumentów i automatycznego wypełniania kolumn metadanych — zamieniając ręczne tagowanie z Lab 1 w automatyczny krok przy przesyłaniu.
   - Kluczowe obszary funkcjonalności: **modele rozumienia dokumentów** (klasyfikacja + wyodrębnianie z Twoich własnych typów dokumentów), **gotowe modele/przetwarzanie formularzy** (faktury, paragony, umowy od razu po wyjęciu z pudełka) oraz **kolumny autouzupełniania** (definicje kolumn w języku naturalnym, bez konieczności trenowania w prostszych przypadkach).
2. **Sprawdź licencjonowanie i rozliczenia, zanim czegokolwiek dotkniesz:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** > **Rozliczenia** > **Twoje produkty** i potwierdź, co jest zawarte w obecnym planie Contoso.
   - Zwróć uwagę na prawdziwy haczyk: poza ograniczonym wliczonym wolumenem, funkcje SharePoint Premium rozliczane na podstawie zużycia wymagają połączonej **subskrypcji Azure** do rozliczeń pay-as-you-go — to już *nie* jest prosty przełącznik w centrum administracyjnym, to konfiguracja obejmująca wiele portali, w tym Azure. Właściciele budżetu muszą być zaangażowani, zanim cokolwiek włączysz.
3. **Włącz rozumienie treści:**
   - W **Centrum administracyjnym SharePoint** poszukaj **Usług zawartości** (lub **Premium**/**Syntex**, zależnie od aktualnej etykiety w Twoim tenancie) i potwierdź, że jest włączone.
   - Jeśli Twój tenant nie ma jeszcze połączonej subskrypcji Azure, udokumentuj to jako blokadę, zamiast zgadywać obejścia — to prawdziwa rozmowa z działem finansów IT, nie skrót laboratoryjny.

### Sprawdzenie wiedzy:
Dlaczego Microsoft mógłby celowo powiązać funkcję zawartości AI z rozliczeniami Azure opartymi na zużyciu zamiast po prostu dołączyć ją płasko do licencji SharePoint?

---

## Lab 3: Budowanie modelu rozumienia dokumentów
### Cel:
Wytrenować model, który w kilka sekund wykona to, co Lab 1 zrobił ręcznie.

### Kroki:
1. **Utwórz lub otwórz Content Center:**
   - W Centrum administracyjnym SharePoint przejdź do **Aktywne witryny** > **Utwórz** i poszukaj typu witryny **Content Center** (używanej do budowania i publikowania modeli Premium). Nazwij ją `Contoso Content Center`.
2. **Utwórz model rozumienia dokumentów:**
   - W Content Center utwórz model o nazwie `Contract Classification`.
   - Prześlij te same przykładowe dokumenty z Lab 1 jako przykłady treningowe i oznacz kluczowe encje, które chcesz wyodrębnić: **Contract Type**, **Expiration Date**.
   - Wytrenuj model, przejrzyj jego wskaźniki pewności i opublikuj go w bibliotece `Contracts`.
3. **Przetestuj go — i zmierz czas:**
   - Prześlij trzy *nowe* dokumenty umów do `Contracts`.
   - Potwierdź, że model automatycznie je klasyfikuje i wypełnia metadane — porównaj czas, jaki to zajęło, z Twoim ręcznym tagowaniem z Lab 1.
4. **Przetestuj skrajny przypadek:**
   - Prześlij jeden dokument, który celowo do tego nie pasuje (np. CV zamiast umowy). Potwierdź, że model albo odmawia pewnej klasyfikacji, albo oznacza niską pewność — dobrze działający model nie powinien z pewnością błędnie oznaczyć śmieciowych danych wejściowych.

### Sprawdzenie wiedzy:
Twój model został wytrenowany na 10 przykładowych umowach. Jakie ryzyko to stwarza, jeśli Contoso zacznie później otrzymywać umowy w bardzo innym formacie (np. od niedawno przejętej firmy z własnymi szablonami)?

---

## Lab 4: Przetwarzanie formularzy i ciągła automatyzacja
### Cel:
Rozszerzyć automatyzację na ustrukturyzowane formularze i połączyć ją z dalszymi procesami biznesowymi.

### Kroki:
1. **Utwórz model przetwarzania formularzy:**
   - W Content Center utwórz model o nazwie `Invoice Processing`, korzystając z przykładowych faktur jako danych treningowych.
   - Wytrenuj go do wyodrębniania: **Invoice Number**, **Vendor Name**, **Total Amount**.
   - Opublikuj go w nowej bibliotece o nazwie **Invoices**.
2. **Przetestuj go:**
   - Prześlij nowe faktury i potwierdź, że wyodrębnione pola poprawnie się wypełniają.
3. **Zastosuj retencję na wyodrębnionych metadanych:**
   - Utwórz etykietę retencji (np. **Retain for 7 Years**), opublikuj ją w `Contracts` i ustaw automatyczne stosowanie na podstawie wyodrębnionego **Contract Type = Vendor Contract** — pokazując, jak wynik Premium może napędzać retencję Purview (LAB-07), a nie tylko być kolumną.
4. **Połącz to z powiadomieniem:**
   - Korzystając z tego, czego nauczyłeś/aś się w LAB-05a, naszkicuj (lub zbuduj) przepływ Power Automate, który co tydzień sprawdza **Expiration Date** i wysyła e-mail do działu prawnego dla wszystkiego, co wygasa w ciągu 30 dni — naturalny kolejny krok, gdy metadane są już wiarygodne i automatyczne.
5. **Monitoruj i koryguj model:**
   - Przejrzyj bieżącą wydajność/pewność modelu na nowo przesyłanych dokumentach.
   - Celowo błędnie sklasyfikuj jeden dokument, popraw go ręcznie i sprawdź, czy model wykorzystuje tę poprawkę jako informację zwrotną do ponownego trenowania.

### Wyzwanie:
Oblicz przybliżony zwrot z inwestycji (ROI) dla Contoso: (czas zaoszczędzony na dokument z Lab 3) × (liczba umów miesięcznie) w porównaniu z kosztem zużycia Azure z Lab 2. To dokładnie ten argument, który administrator SharePoint musi przedstawić, żeby uzyskać zatwierdzenie budżetu — nie "to fajne AI", tylko liczba.

### Sprawdzenie wiedzy:
Dlaczego połączenie wyodrębnionych metadanych Premium z przepływem Power Automate przypominającym o wygaśnięciu (krok 4) tworzy więcej realnej wartości biznesowej niż samo wyodrębnianie?

---

Te ćwiczenia dają praktyczne doświadczenie w zarządzaniu zawartością i korzystaniu z rozumienia treści AI SharePoint Premium do automatyzacji i skalowania tagowania metadanych — w tym z realiami licencjonowania i kosztów, jakie się z tym wiążą.
