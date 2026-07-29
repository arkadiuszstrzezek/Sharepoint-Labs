# Ćwiczenia laboratoryjne SharePoint Online - LAB-03

> **Scenariusz przewodni:** Contoso Consulting (z LAB-02) właśnie podpisało pierwszego klienta zewnętrznego, **Fabrikam Inc.**, i musi udostępnić mu pliki projektowe bez zapraszania całego internetu. Skonfigurujesz udostępnianie na każdym poziomie — tenanta, witryny i pliku — i zobaczysz, jak każda warstwa ogranicza tę poniżej.

## Lab 1: Konfigurowanie udostępniania zewnętrznego
### Cel:
Poznać, jak warstwowo zbudowane jest udostępnianie zewnętrzne w SharePoint Online — zasady tenanta ustawiają sufit, a każdy poziom poniżej może być tylko równie restrykcyjny lub bardziej, nigdy luźniejszy.

### Kroki:
1. **Ustaw sufit dla całego tenanta:**
   - Przejdź do **Centrum administracyjnego SharePoint** > **Zasady** > **Udostępnianie**.
   - Przejrzyj (niekoniecznie zmieniaj jeszcze) cztery poziomy:
     - **Każdy**: anonimowo, bez linku logowania.
     - **Nowi i istniejący goście**: uwierzytelnieni użytkownicy zewnętrzni, dodawani do Entra ID jako goście.
     - **Tylko istniejący goście**: tylko goście już obecni w katalogu.
     - **Tylko osoby w Twojej organizacji**: udostępnianie zewnętrzne całkowicie wyłączone.
   - Ustaw na **Nowi i istniejący goście** — Contoso chce, żeby Fabrikam się uwierzytelniało, a nie dostawało anonimowe linki.
2. **Skonfiguruj udostępnianie na poziomie witryny dla witryny klienta:**
   - Utwórz (lub użyj ponownie) witrynę o nazwie `Fabrikam Project`.
   - W **Aktywnych witrynach** wybierz `Fabrikam Project` > **Udostępnianie**.
   - Ustaw poziom udostępniania witryny na **Nowi i istniejący goście** — nie może on przekroczyć ustawienia tenanta z kroku 1, może je co najwyżej dorównać lub bardziej ograniczyć.
3. **Udowodnij efekt sufitu (właściwa lekcja):**
   - Tymczasowo ustaw zasady *tenanta* na **Tylko osoby w Twojej organizacji**.
   - Wróć do ustawienia udostępniania witryny `Fabrikam Project` — zauważ, że teraz jest ono również ograniczone do "Tylko osoby w Twojej organizacji", niezależnie od tego, co wybrałeś/aś w kroku 2.
   - Ustaw z powrotem zasady tenanta na **Nowi i istniejący goście**.

### Sprawdzenie wiedzy:
Jeśli udostępnianie na poziomie tenanta jest ustawione na "Tylko istniejący goście", ale witryna jest ustawiona na "Każdy", co faktycznie się dzieje, gdy ktoś próbuje udostępnić plik na tej witrynie — i dlaczego?

---

## Lab 2: Rodzaje mechanizmów udostępniania zewnętrznego
### Cel:
Zrozumieć różne mechanizmy udostępniania treści na zewnątrz i wybrać, którego użyć dla Fabrikam.

### Kroki:
1. **Udostępnij plik za pomocą linku:**
   - W `Fabrikam Project` prześlij przykładowy dokument propozycji.
   - Wybierz plik > **Udostępnij** i porównaj opcje linku:
     - **Każdy z linkiem** — bez wymogu logowania (dostępne tylko, jeśli zezwalają na to zasady tenanta/witryny).
     - **Osoby w Twojej organizacji** — tylko wewnętrznie.
     - **Osoby z istniejącym dostępem** — bez nadawania nowych uprawnień, tylko skrót.
     - **Konkretne osoby** — wskazujesz dokładnie, kto; najbezpieczniejsze dla Fabrikam.
   - Wybierz **Konkretne osoby**, wpisz testowy adres e-mail zewnętrzny, ustaw uprawnienie na **Może przeglądać** i wyślij.
2. **Udostępnij całą witrynę:**
   - Otwórz `Fabrikam Project`, kliknij **Udostępnij** (prawy górny róg witryny), wpisz ten sam adres e-mail zewnętrzny i przypisz rolę **Odwiedzający**.
   - Porównaj to z udostępnianiem na poziomie pliku: udostępnianie witryny daje ciągły dostęp do wszystkiego, na co pozwala rola, a nie tylko do jednego dokumentu.
3. **Zdecyduj i uzasadnij:**
   - Dla długotrwałej współpracy z klientem, jak Fabrikam, czy ustandaryzowałbyś/łabyś udostępnianie na poziomie witryny czy pliku? Zapisz swoje uzasadnienie — stanie się to Twoją zasadą governance w Lab 3.

### Sprawdzenie wiedzy:
Dlaczego "Konkretne osoby" jest niemal zawsze bezpieczniejszą domyślną opcją niż "Każdy z linkiem", nawet gdy zasady tenanta pozwalają na anonimowe linki?

---

## Lab 3: Zarządzanie udostępnianiem treści za pomocą Microsoft Entra ID i SharePoint Online
### Cel:
Zrozumieć, że udostępnianie zewnętrzne w SharePoint jest tak naprawdę pod spodem dostępem gościa w Microsoft Entra ID, oraz nauczyć się to monitorować i odwoływać.

### Kroki:
1. **Skonfiguruj dostęp gościa w Microsoft Entra ID:**
   - Przejdź do **Centrum administracyjnego Microsoft Entra** (https://entra.microsoft.com).
   - Przejdź do **Tożsamości zewnętrzne** > **Ustawienia współpracy zewnętrznej**.
   - Przejrzyj/skonfiguruj:
     - **Ograniczenia dostępu użytkowników-gości** — ile z katalogu gość może zobaczyć.
     - **Ograniczenia współpracy** — lista dozwolonych lub zablokowanych konkretnych domen (np. zezwól tylko na `fabrikam.com`).
2. **Znajdź konto gościa utworzone przez SharePoint:**
   - Wciąż w Entra ID przejdź do **Użytkownicy** > filtruj po **Typ użytkownika: Gość**.
   - Znajdź testowe konto Fabrikam, któremu udostępniłeś/aś w Lab 2 — zauważ, że SharePoint automatycznie je tu utworzył w momencie udostępnienia.
3. **Monitoruj aktywność udostępniania:**
   - W **Centrum administracyjnym SharePoint** sprawdź obszar **Raporty** (dokładna etykieta może się różnić w zależności od tenanta/fali aktualizacji — szukaj raportów udostępniania lub użycia) pod kątem aktywności udostępniania zewnętrznego.
   - Skrzyżuj to z **Centrum administracyjnym Microsoft 365** > **Raporty** > **Użycie**, gdzie również pojawiają się statystyki udostępniania zewnętrznego SharePoint.
4. **Odwołaj dostęp na dwa sposoby i porównaj:**
   - **Wąskie odwołanie:** wróć do pliku z Lab 2, kliknij **Zarządzaj dostępem** i usuń link/uprawnienie gościa Fabrikam. Potwierdź, że traci on dostęp *tylko do tego pliku*.
   - **Pełne odwołanie:** w Entra ID > **Użytkownicy** > **Użytkownicy-goście** usuń całkowicie konto gościa Fabrikam. Potwierdź, że to usuwa dostęp *wszędzie*, gdzie go miał — witryny, pliki, wszystko.

### Wyzwanie:
Skonfiguruj **listę dozwolonych domen** w ustawieniach współpracy zewnętrznej Entra ID, zezwalając tylko gościom z `fabrikam.com`, a następnie spróbuj udostępnić plik testowemu adresowi `gmail.com` i potwierdź, że jest to zablokowane. To właśnie konfiguruje większość prawdziwych projektów klienckich zamiast otwartego udostępniania gościom.

### Sprawdzenie wiedzy:
Twój zespół bezpieczeństwa pyta: "Jak zagwarantujemy, że nikt poza Fabrikam i naszą własną firmą nigdy nie dostanie się do witryny `Fabrikam Project`?" Wymień dwa ustawienia (jedno w SharePoint, jedno w Entra ID), które razem to wymuszają.

---

Te ćwiczenia dają praktyczne, warstwowe doświadczenie z udostępnianiem zewnętrznym — od sufitu zasad tenanta, aż po pojedynczy link do pliku, oraz konta gości Entra ID, które sprawiają, że to wszystko działa.
