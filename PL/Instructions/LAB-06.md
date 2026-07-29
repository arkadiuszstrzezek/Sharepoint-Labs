# Ćwiczenia laboratoryjne SharePoint Online - LAB-06

> **Scenariusz przewodni:** Contoso Consulting ma już prawdziwe witryny, prawdziwych gości zewnętrznych i aplikację Power App zapisującą do prawdziwej listy. Kierownictwo właśnie zapytało: "co powstrzyma to przed wymknięciem się spod kontroli w miarę wzrostu do 300 osób?" To jest governance na poziomie tenanta — temat tego labu.

## Lab 1: Zarządzanie ustawieniami dostępu na poziomie tenanta
### Cel:
Nauczyć się konfigurować ustawienia, które obowiązują naraz w każdej witrynie tenanta, oraz zrozumieć, dlaczego funkcjonują osobno od ustawień poszczególnych witryn.

### Kroki:
1. **Dostęp do zasad tenanta:**
   - Przejdź do **Centrum administracyjnego SharePoint** > **Zasady** w nawigacji po lewej stronie.
2. **Przejrzyj udostępnianie (przypomnienie z LAB-03):**
   - W sekcji **Zasady** > **Udostępnianie** potwierdź poziom udostępniania zewnętrznego, domyślny typ linku i ustawienia wygasania linków — to jest sufit, który ogranicza każde ustawienie udostępniania na poziomie witryny.
3. **Skonfiguruj wylogowanie po bezczynności:**
   - W sekcji **Zasady** > **Kontrola dostępu** > **Wylogowanie po bezczynności sesji** włącz ustawienie i określ limit czasu (np. 15 minut).
   - Zwróć uwagę na zastrzeżenia: dotyczy to *całej organizacji* — nie można tego ustawić na poziomie witryny ani użytkownika — a propagacja zajmuje około 15 minut, bez wpływu na już otwarte sesje.
4. **Omówcie granicę:**
   - Zapytaj: które z dzisiejszych ustawień zespół IT Contoso mógłby delegować poszczególnym właścicielom witryn, a które absolutnie muszą pozostać scentralizowane? Wylogowanie po bezczynności sesji to dobry przykład "musi pozostać scentralizowane" — zapisz jeszcze jedno na podstawie tego, co widziałeś/aś we wcześniejszych labach (podpowiedź: sufit udostępniania zewnętrznego z LAB-03).

### Sprawdzenie wiedzy:
Dlaczego wylogowania po bezczynności sesji nie można skonfigurować dla poszczególnej witryny, w odróżnieniu od ustawień udostępniania?

---

## Lab 2: Zarządzanie ustawieniami magazynu SharePoint (poziom tenanta)
### Cel:
Poznać, jak magazyn jest alokowany w całym tenancie oraz kiedy przełączyć się z trybu automatycznego na ręczny.

### Kroki:
1. **Zobacz całkowite zużycie:**
   - W **Aktywnych witrynach** zwróć uwagę na podsumowanie ogólnego wykorzystania magazynu u góry strony.
2. **Zrozum tryb Automatyczny (domyślny):**
   - Przejdź do **Ustawienia** > **Limity magazynu witryny**.
   - W trybie **Automatycznym** każda witryna czerpie w razie potrzeby z jednej wspólnej puli tenanta (do 25 TB na witrynę) — w ogóle nie ustawiasz indywidualnych limitów.
3. **Przełącz na Ręczny i zobacz, co się zmienia:**
   - Przełącz na **Ręczny**. Zauważ, że limit każdej istniejącej witryny domyślnie skacze do maksimum 25 TB — tryb ręczny niczego automatycznie nie zmniejsza, tylko *odblokowuje* kontrolę na poziomie witryny.
   - Ustaw mały, celowy limit (np. 5 GB) na witrynie `Fabrikam Project` z LAB-03, żeby móc przetestować zachowanie ostrzeżeń w Lab 3.
4. **Monitoruj zużycie:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** > **Raporty** > **Użycie** i przejrzyj raport magazynu SharePoint — to Twoje źródło wczesnego ostrzegania, zanim poszczególne witryny zaczną osiągać limity.

### Sprawdzenie wiedzy:
Contoso rośnie szybko i nieprzewidywalnie. Czy zalecałbyś/łabyś zarządzanie magazynem Automatyczne czy Ręczne, i dlaczego ta odpowiedź może się zmienić, gdy Contoso będzie mieć kilka witryn z wrażliwymi, rozliczanymi kosztowo danymi (np. archiwum wideo rozliczane klientowi)?

---

## Lab 3: Zarządzanie ustawieniami magazynu poszczególnych witryn
### Cel:
Zastosować limity magazynu na poziomie witryny i zobaczyć system ostrzeżeń w akcji.

### Kroki:
1. **Zobacz zużycie witryny:**
   - W **Aktywnych witrynach** wybierz `Fabrikam Project` i sprawdź wartość **Wykorzystany magazyn**.
2. **Zapełnij go (bezpiecznie):**
   - Prześlij kilka większych plików testowych (lub wielokrotnie zduplikuj istniejący plik), aż witryna zbliży się do limitu 5 GB ustawionego w Lab 2.
3. **Zaobserwuj ostrzeżenie:**
   - Gdy zbliżysz się do progu, sprawdź, czy właściciele/członkowie witryny widzą ostrzeżenie o magazynie w produkcie. Zwróć uwagę, co się dzieje przy próbie przesłania czegoś powyżej 100% — nowe przesyłanie powinno zostać zablokowane, a istniejąca zawartość pozostaje dostępna.
4. **Podnieś limit i potwierdź:**
   - Zwiększ limit `Fabrikam Project` do 10 GB i potwierdź, że przesyłanie plików znów działa natychmiast, bez żadnego opóźnienia propagacji (w odróżnieniu od wylogowania po bezczynności sesji z Lab 1).

### Sprawdzenie wiedzy:
Jaka jest praktyczna różnica w *doświadczeniu użytkownika* między witryną, która osiąga limit magazynu, a użytkownikiem wylogowanym z powodu bezczynności? Które z nich jest bardziej uciążliwe dla witryny widocznej dla klienta, jak `Fabrikam Project`, i dlaczego?

---

## Lab 4: Zarządzanie ustawieniami tworzenia witryn
### Cel:
Kontrolować, kto może tworzyć witryny i w jaki sposób, równoważąc produktywność samoobsługową z rozrostem (sprawl).

### Kroki:
1. **Przejrzyj obecne ustawienia samoobsługi:**
   - Przejdź do **Ustawienia** > **Tworzenie witryn**.
   - Zwróć uwagę na trzy szerokie podejścia: zezwolić wszystkim na samoobsługę, ograniczyć tworzenie tylko do administratorów, lub skierować tworzenie przez **przepływ zatwierdzania** (ktoś składa wniosek o witrynę, zatwierdzający wyraża zgodę, zanim zostanie ona utworzona).
2. **Wybierz właściwe podejście dla Contoso:**
   - Contoso wyszło już poza etap "5 osób, wszyscy się znają". Skonfiguruj opcję **przepływu zatwierdzania** zamiast w pełni otwartej samoobsługi i ustaw siebie jako zatwierdzającego/ą.
3. **Przetestuj to:**
   - Jako użytkownik testowy (lub symulując wniosek) złóż nowy wniosek o utworzenie witryny i potwierdź, że trafia on do Twojej kolejki zatwierdzeń zamiast natychmiast się tworzyć.
4. **Ogranicz według grupy (opcjonalne wzmocnienie):**
   - W sekcji **Tworzenie witryn** ogranicz, kto może w ogóle *składać* wniosek, do konkretnej grupy zabezpieczeń (np. grupy "Site Owners"), a nie każdego pracownika.
5. **Ustaw sensowne wartości domyślne dla wszystkiego, co powstanie:**
   - Skonfiguruj domyślny limit magazynu, domyślną strefę czasową i domyślne ustawienia udostępniania dla nowych witryn — to jest to, co powstrzymuje falę zatwierdzonych witryn przed dryfowaniem każda w inne, niespójne ustawienia.

### Sprawdzenie wiedzy:
Jaki jest faktyczny kompromis, który podejmuje Contoso, przechodząc z otwartej samoobsługi na przepływ zatwierdzania? Wymień jedną rzecz, którą zyskują, i jedną, którą tracą.

---

## Lab 5: Zaawansowane zarządzanie tenantem i witrynami
### Cel:
Połączyć ustawienia tenanta z codziennym monitorowaniem kondycji.

### Kroki:
1. **Włącz/wyłącz funkcje tenanta:**
   - W **Ustawieniach** przejrzyj przełączniki, takie jak OneDrive **Żądanie plików**, ograniczenia klienta synchronizacji oraz katalog aplikacji kolekcji witryn — zdecyduj z uzasadnieniem (nie tylko "włącz wszystko"), które z nich pasują dzisiaj Contoso.
2. **Zbadaj dostępne szablony/projekty witryn:**
   - W **Aktywne witryny** > **Utwórz** przejrzyj obecne opcje Witryny zespołu i Witryny komunikacyjnej (Temat/Prezentacja/Pusta) — dogłębnie zrobiłeś/aś to w LAB-02 Lab 4; tutaj tylko potwierdź, że nic się od tego czasu nie zmieniło.
3. **Monitoruj kondycję tenanta:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** > **Kondycja** > **Kondycja usługi**.
   - Sprawdź obecny status SharePoint Online i wszelkie aktywne komunikaty.

### Wyzwanie:
Napisz jednostronicowy dokument "Contoso SharePoint Governance Baseline", łączący decyzje z Labów 1–5 tego pliku: sufit udostępniania, limit bezczynności, tryb magazynu, podejście do tworzenia witryn oraz domyślne ustawienia nowych witryn. To jest artefakt, który prawdziwy administrator SharePoint przekazuje kierownictwu — nie przegląd zrzutów ekranu, tylko dokument decyzyjny.

### Sprawdzenie wiedzy:
Gdybyś musiał/a uzasadnić tę linię bazową governance sceptycznemu kierownikowi działu, który chce po prostu "samoobsługi, bez biurokracji", jaki jest Twój najmocniejszy argument w jednym zdaniu?

---

Te ćwiczenia dają praktyczne doświadczenie w zarządzaniu ustawieniami na poziomie tenanta oraz specyficznymi dla witryny w SharePoint Online — a co ważniejsze, praktykę w decydowaniu, *które* ustawienie należy do którego poziomu.
