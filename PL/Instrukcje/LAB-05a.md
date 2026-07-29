# Ćwiczenia laboratoryjne SharePoint Online - LAB-05a

> **Scenariusz przewodni:** Kontynuując LAB-05, aplikacja rezerwacyjna Contoso Fleet pozwala każdemu zarezerwować pojazd na dowolny okres — w tym na dwa tygodnie, co pozbawia wszystkich innych możliwości. Dodasz prawdziwy przepływ zatwierdzania za pomocą Power Automate: krótkie rezerwacje przechodzą automatycznie, długie wymagają podpisu menedżera.

## Lab: Automatyzacja przepływu zatwierdzania za pomocą Power Automate
### Cel:
Zbudować przepływ Power Automate, który uruchamia zatwierdzenie przez menedżera tylko wtedy, gdy spełniona jest reguła biznesowa (rezerwacja dłuższa niż 3 dni) — a nie przy każdym pojedynczym elemencie, co czyni to realistycznym przepływem, a nie zabawką.

---

### Krok 1: Rozszerz listę SharePoint
1. Przejdź do witryny `Contoso Fleet` (z LAB-05) i otwórz **Vehicle Reservations**.
2. Dodaj kolumnę: **Approval Status** (Wybór: Pending, Approved, Rejected, Not Required), wartość domyślna `Pending`.
3. Jeśli nie masz ich jeszcze z LAB-05, potwierdź również, że lista ma **StartDate** i **EndDate** jako kolumny typu Data — przepływ musi na ich podstawie obliczyć czas trwania.

---

### Krok 2: Utwórz przepływ
1. Przejdź do **Power Automate** (https://make.powerautomate.com).
2. **Utwórz** > **Zautomatyzowany przepływ w chmurze**.
3. Nazwij go `Fleet Reservation Approval`.
4. Wyzwalacz: **Kiedy element zostanie utworzony** — połącz z witryną `Contoso Fleet` i listą `Vehicle Reservations`.

---

### Krok 3: Zatwierdzaj tylko to, co naprawdę tego wymaga
1. Dodaj krok **Compute**: użyj wyrażenia, aby obliczyć długość rezerwacji w dniach:
   ```
   div(sub(ticks(triggerOutputs()?['body/EndDate']), ticks(triggerOutputs()?['body/StartDate'])), 864000000000)
   ```
2. Dodaj **Warunek**: `[obliczone dni]` jest większe niż `3`.
3. **Jeśli tak** (długa rezerwacja — wymaga zatwierdzenia):
   - **Rozpocznij i czekaj na zatwierdzenie**:
     - Typ zatwierdzenia: **Zatwierdź/Odrzuć – pierwszy, kto odpowie**.
     - Tytuł: `Vehicle reservation for @{triggerOutputs()?['body/Employee']} (@{...} days)`.
     - Przypisane do: adres e-mail Twojego menedżera floty.
     - Szczegóły: uwzględnij pojazd, pracownika, daty rozpoczęcia/zakończenia.
   - **Zaktualizuj element**: ustaw **Approval Status** za pomocą:
     ```
     if(equals(outputs('Start_and_wait_for_an_approval')?['body/outcome'], 'Approve'), 'Approved', 'Rejected')
     ```
4. **Jeśli nie** (3 dni lub mniej — zatwierdzenie automatyczne):
   - **Zaktualizuj element**: ustaw **Approval Status** na `Not Required`, pomijając menedżera całkowicie.
5. **Wyślij e-mail (V2)** do wnioskodawcy w obu gałęziach, używając `@{outputs('Start_and_wait_for_an_approval')?['body/outcome']}` w gałęzi zatwierdzania i stałej wiadomości "zatwierdzone automatycznie" w drugiej.

---

### Krok 4: Przetestuj obie ścieżki
1. W `Vehicle Reservations` dodaj rezerwację na **1 dzień**. Potwierdź w **Power Automate** > **Moje przepływy** > **Fleet Reservation Approval** > **Historia przebiegów**, że przepływ wybrał gałąź "zatwierdzenie niepotrzebne", a element od razu pokazuje `Not Required`.
2. Dodaj rezerwację na **5 dni**. Potwierdź, że menedżer otrzymuje prośbę o zatwierdzenie oraz że jej zatwierdzenie/odrzucenie poprawnie aktualizuje **Approval Status** i wysyła e-mail do wnioskodawcy.
3. Celowo przetestuj **przypadek graniczny**: dokładnie 3 dni. Potwierdź, że zachowuje się zgodnie z tym, co faktycznie mówi Twój warunek (`greater than 3`, a nie `greater than or equal`) — przypadki graniczne to miejsce, w którym prawdziwe przepływy zawodzą w produkcji.

---

### Krok 5: Monitoruj i wzmocnij
1. W **Historii przebiegów** otwórz jeden udany i jeden (celowo wywołany) nieudany przebieg — spróbuj złożyć rezerwację z pustym **EndDate**, żeby zobaczyć, jak przepływ radzi sobie ze złymi danymi.
2. Dodaj sprawdzenie na początku przepływu (**Warunek** przed obliczeniami czasu trwania), które elegancko pomija lub powiadamia Cię, jeśli **EndDate** jest puste, zamiast pozwolić przepływowi zakończyć się błędem.
3. Zwróć uwagę na połączenie przepływu: domyślnie działa on pod Twoją tożsamością. Omówcie w grupie, co stanie się z tym przepływem w dniu, gdy odejdziesz z firmy — to dokładnie dlatego prawdziwe wdrożenia używają **konta serwisowego/dedykowanego** lub rozwiązania z użytkownikiem aplikacji, a nie osobistego loginu.

### Wyzwanie:
Dodaj drugą gałąź warunku: jeśli ten sam pojazd ma już **Zatwierdzoną** rezerwację nakładającą się na daty nowego wniosku, odrzuć automatycznie z jasnym komunikatem, zamiast w ogóle pytać menedżera — przepływ nigdy nie powinien zawracać głowy człowiekowi wnioskiem, który jest już niemożliwy do spełnienia.

### Sprawdzenie wiedzy:
Dlaczego uruchamianie zatwierdzenia na podstawie *warunku* (czas trwania > 3 dni) czyni to bardziej realistycznym przykładem szkoleniowym niż bezwarunkowe zatwierdzanie każdego elementu? Jaki jest operacyjny koszt wersji "zatwierdzaj wszystko" w firmie liczącej 300 pracowników?

---
