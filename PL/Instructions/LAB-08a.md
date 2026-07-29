# Ćwiczenia laboratoryjne SharePoint Online - LAB-08a

> **Scenariusz przewodni:** Contoso Consulting urosło do 300 osób, dziesiątek witryn i aktywnego projektu klienckiego (`Fabrikam Project`). Podstawowe zasady udostępniania (LAB-03) oraz governance magazynu/tworzenia witryn (LAB-06) już nie wystarczają — kierownictwo chce dowodu, że tylko właściwe osoby widzą dane HR i finansowe, oraz że martwe witryny są automatycznie sprzątane. To jest **SharePoint Advanced Management (SAM)**.

> ⚠️ **Uwaga strukturalna, aktualna na 2026 rok:** funkcje Advanced Management są podzielone między dwa miejsca w lewej nawigacji Centrum administracyjnego SharePoint — **Zasady > Kontrola dostępu** (kontrole sesji/urządzeń/sieci/ograniczonego dostępu, część darmowa, część licencjonowana w ramach SAM) oraz dedykowany węzeł najwyższego poziomu **Advanced Management** (raporty governance, cykl życia witryn, wykrywanie ograniczonej zawartości). Jeśli etykieta menu poniżej nie zgadza się dokładnie z tym, co widzisz, użyj pola wyszukiwania centrum administracyjnego — Microsoft często przetasowuje ten obszar, co samo w sobie warto zauważyć w swoim zespole.

## Lab 1: Przegląd i licencjonowanie SharePoint Advanced Management
### Cel:
Zrozumieć, co SAM dodaje ponad standardowy governance SharePoint, oraz potwierdzić, że Contoso faktycznie ma licencję na jego użycie, zanim zaczniesz wokół tego planować.

### Kroki:
1. **Zdefiniuj zakres:**
   - Omówcie, co SAM dodaje ponad to, co już skonfigurowałeś/aś: drugą, niezależną warstwę ograniczenia dostępu (**kontrola ograniczonego dostępu**), widoczność nadmiernego udostępniania (**raporty governance dostępu do danych**), zautomatyzowaną obsługę nieaktywnych witryn (**zarządzanie cyklem życia witryn**) oraz kontrolę nad tym, jak zawartość pojawia się w wyszukiwaniu/Copilot (**wykrywanie ograniczonej zawartości**).
2. **Sprawdź licencjonowanie — nie zakładaj:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** > **Rozliczenia** > **Twoje produkty**.
   - Potwierdź: SAM jest automatycznie zawarty z licencją **Microsoft 365 Copilot**; tenanty bez Copilota potrzebują go jako **samodzielnego dodatku**. Zanotuj, w jakiej sytuacji jest Contoso.
3. **Znajdź funkcję w centrum administracyjnym:**
   - W lewej nawigacji **Centrum administracyjnego SharePoint** zlokalizuj pozycję najwyższego poziomu **Advanced Management** oraz osobno **Zasady > Kontrola dostępu**.

### Sprawdzenie wiedzy:
Dlaczego ma znaczenie, zanim zbudujesz plan wdrożenia, czy SAM jest zawarty dzięki licencji Copilot, czy zakupiony jako samodzielny dodatek?

---

## Lab 2: Zasady kontroli dostępu
### Cel:
Skonfigurować warstwę kontroli dostępu, która znajduje się *pod* normalnymi uprawnieniami SharePoint — może powiedzieć nie, nawet gdy nadanie uprawnienia mówi tak.

### Kroki:
1. **Przypomnij sobie, co jest już darmowe/podstawowe:**
   - W sekcji **Zasady** > **Kontrola dostępu** wróć do **Wylogowania po bezczynności sesji** (skonfigurowanego w LAB-06) — to nie wymaga licencji SAM.
2. **Skonfiguruj warunki sieciowe i urządzeń:**
   - W sekcji **Kontrola dostępu** > **Niezarządzane urządzenia** ustaw zasadę dla witryny `Fabrikam Project`: zezwól na dostęp tylko przez przeglądarkę (zablokuj pobieranie/synchronizację) z urządzeń, które nie są przyłączone do domeny ani zgodne.
   - W sekcji **Kontrola dostępu** > **Lokalizacja sieciowa** ogranicz dostęp do nazwanych lokalizacji, jeśli Contoso ma stałe zakresy IP biura — w przeciwnym razie omówcie, dlaczego ta kontrola ma mniejsze znaczenie dla w większości zdalnej siły roboczej.
3. **Skonfiguruj kontrolę ograniczonego dostępu (flagowa funkcja SAM):**
   - Utwórz witrynę o nazwie `Contoso HR` (jeśli jeszcze jej nie masz).
   - W sekcji **Kontrola dostępu** > **Kontrola ograniczonego dostępu** zastosuj zasadę ograniczającą dostęp *wyłącznie* do grupy zabezpieczeń Microsoft 365 "HR Team" — nawet Administrator globalny bez członkostwa w grupie HR powinien mieć zablokowane przeglądanie jej zawartości.
   - **Udowodnij to, nie tylko skonfiguruj:** zaloguj się jako (lub zasymuluj) użytkownik, który jest Właścicielem witryny, ale *nie* należy do grupy zabezpieczeń HR, i potwierdź, że teraz ma odmowę dostępu, mimo że jego poziom uprawnień SharePoint mówi co innego.
4. **Aktualizacja 2026 — deleguj odpowiedzialnie:**
   - Zwróć uwagę na nowszą możliwość delegowania zarządzania RAC do administratorów witryn z wymaganą notatką uzasadniającą, zamiast trzymania tego scentralizowanego wyłącznie przy administratorach SharePoint. Omówcie, czy Contoso jest gotowe do delegowania tego, czy powinno trzymać to scentralizowane, dopóki program jest nowy.

### Sprawdzenie wiedzy:
Użytkownik jest Właścicielem witryny na `Contoso HR` z pełnymi uprawnieniami nadanymi w SharePoint. Kontrola ograniczonego dostępu go wyklucza. Kto wygrywa i dlaczego istnieje ten dwuwarstwowy model zamiast po prostu naprawienia uprawnienia SharePoint?

---

## Lab 3: Raportowanie governance i cykl życia witryn
### Cel:
Przejść od reaktywnych poprawek do proaktywnej, ogólnotenantowej widoczności i sprzątania.

### Kroki:
1. **Uruchom raport governance dostępu do danych:**
   - W węźle **Advanced Management** uruchom raport na `Fabrikam Project` i `Contoso HR`, szukając **nadmiernego udostępniania** — linków udostępnionych dla "Każdego" lub osobom spoza oczekiwanych grup.
   - Zidentyfikuj co najmniej jedno ustalenie i je napraw (zaostrz link, usuń nieoczekiwanego gościa zewnętrznego), korzystając z tego, czego nauczyłeś/aś się w LAB-03.
2. **Skonfiguruj zarządzanie cyklem życia witryn:**
   - Ustaw zasadę oznaczającą witryny bez aktywności przez 90+ dni, powiadamiającą właściciela witryny przed jakimkolwiek usunięciem, zamiast usuwać od razu.
   - Zastosuj ją i sprawdź, jak wygląda witryna "oznaczona do przeglądu" z perspektywy właściciela.
3. **Skonfiguruj wykrywanie ograniczonej zawartości:**
   - Na `Contoso HR` włącz wykrywanie ograniczonej zawartości, żeby jej zawartość była wykluczona z wyszukiwania w całym tenancie i odpowiedzi Copilot dla użytkowników spoza grupy HR — nawet jeśli technicznie mieliby dostęp na poziomie pliku jakąś inną drogą.

### Wyzwanie:
Uruchom raport governance dostępu do danych po raz drugi po poprawkach z Lab 2 i Lab 3 i potwierdź, że ustalenie o nadmiernym udostępnianiu zniknęło. Raport, którego nigdy nie uruchamiasz ponownie, to nie governance, to jednorazowe sprzątanie.

### Sprawdzenie wiedzy:
Dlaczego dokument może być indywidualnie udostępniony użytkownikowi, a mimo to poprawnie wykluczony z wyników Copilot/wyszukiwania tego samego użytkownika, gdy wykrywanie ograniczonej zawartości jest włączone? Jaki realny problem to zamyka?

---

### Korzyści z SharePoint Advanced Management (co właśnie udowodniłeś/aś, praktycznie)
- **Lepsze bezpieczeństwo** — Kontrola ograniczonego dostępu zablokowała legalnego Właściciela witryny w Lab 2; to prawdziwa druga warstwa, nie slogan.
- **Lepsza zgodność** — raporty cyklu życia i governance dają dowody, a nie tylko intencje, na potrzeby audytów.
- **Usprawnione zarządzanie** — zasady cyklu życia usuwają problem "kto będzie pamiętał, żeby to posprzątać".
- **Widoczność i kontrola** — raport o nadmiernym udostępnianiu w Lab 3 wychwycił coś, czego ręczny przegląd uprawnień prawdopodobnie by nie wyłapał.

Ten lab skupił się na konfigurowaniu i *udowadnianiu* — a nie tylko opisywaniu — wartości SharePoint Advanced Management dla zabezpieczania i optymalizacji rosnącego tenanta SharePoint Online.
