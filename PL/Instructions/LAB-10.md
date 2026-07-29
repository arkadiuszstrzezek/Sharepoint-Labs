# Ćwiczenia laboratoryjne SharePoint Online - LAB-10

> **Scenariusz przewodni:** Zbudowałeś/aś już i zarządzasz governance większości środowiska SharePoint Contoso Consulting w LAB-01 do LAB-09. Ostatnia umiejętność to nie funkcja — to nadążanie za tempem wydawania zmian przez samego Microsoft, żebyś nigdy nie był/a ostatnią osobą, która dowiaduje się o awarii lub zmianie łamiącej coś (jak wycofanie klasycznych witryn, które udokumentowałeś/aś w LAB-02).

## Lab 1: Odkrywanie Centrum kondycji Microsoft 365
### Cel:
Nauczyć się monitorować kondycję usług w czasie rzeczywistym oraz odróżniać "problem Microsoftu, poczekaj" od "problem mojego tenanta, idź to napraw".

### Kroki:
1. **Otwórz Kondycję usługi:**
   - Przejdź do **Centrum administracyjnego Microsoft 365** (https://admin.microsoft.com) > **Kondycja** > **Kondycja usługi**.
2. **Przejrzyj obecny status:**
   - Sprawdź obecny status SharePoint Online oraz wszelkie aktywne incydenty lub komunikaty w całym tenancie.
3. **Zbadaj incydent szczegółowo (użyj przeszłego/rozwiązanego, jeśli żaden nie jest aktywny):**
   - Otwórz incydent i zapisz: ID incydentu, dotknięte usługi, obecny status i szacowany czas rozwiązania.
   - Zwróć uwagę, które pola mówią Ci, czy otworzyć zgłoszenie do wsparcia (problem *specyficzny dla Twojego tenanta*), czy po prostu poczekać (incydent *dotyczący całego Microsoftu*, nad którym już się pracuje).
4. **Skonfiguruj powiadomienia:**
   - Przejdź do **Kondycja** > **Centrum wiadomości** i skonfiguruj powiadomienia e-mail, żeby administratorzy Contoso byli informowani o zmianach kondycji usługi bez konieczności ręcznego sprawdzania panelu.
5. **Przeprowadź mini-ćwiczenie incydentu:**
   - Wyobraź sobie, że użytkownicy zgłaszają, iż `Fabrikam Project` (LAB-03) jest właśnie "całkowicie niedostępny". Korzystając tylko z Kondycji usługi, przejdź przez to, jak potwierdziłbyś/łabyś w ciągu 2 minut, czy to awaria po stronie Microsoftu, czy coś lokalnego w Twoim tenancie/witrynie — zapisz dokładną sekwencję kliknięć.

### Sprawdzenie wiedzy:
Użytkownik mówi "SharePoint nie działa". Kondycja usługi pokazuje wszystko na zielono. Jakie dwie specyficzne dla tenanta rzeczy sprawdziłbyś/łabyś dalej, biorąc pod uwagę wszystko, co skonfigurowałeś/aś w LAB-01 do LAB-09 (podpowiedź: pomyśl o tym, co skonfigurowałeś/aś w LAB-06 i LAB-08a)?

---

## Lab 2: Zrozumienie mapy drogowej SharePoint (Roadmap)
### Cel:
Nauczyć się śledzić nadchodzące zmiany w SharePoint, zanim trafią na ekrany użytkowników — i zanim popsują coś, co zbudowałeś/aś wcześniej w tym kursie.

### Kroki:
1. **Otwórz mapę drogową:**
   - Przejdź do **Microsoft 365 Roadmap** (https://www.microsoft.com/microsoft-365/roadmap) i filtruj po **SharePoint**.
2. **Zbadaj nadchodzące funkcje:**
   - Przejrzyj pozycje oznaczone jako **W trakcie rozwoju** lub **Wdrażane**. Wybierz jedną, która faktycznie miałaby znaczenie dla Contoso (np. cokolwiek dotyczące witryn hubowych, udostępniania lub Advanced Management) i podsumuj ją w jednym zdaniu.
3. **Sprawdź się retrospektywnie:**
   - Wyszukaj na mapie drogowej wycofanie klasycznych witryn oraz rebranding SharePoint Premium, które udokumentowałeś/aś wcześniej w LAB-02 i LAB-08. Potwierdź, że wpis na mapie drogowej istniał *zanim* zostałbyś/łabyś tym zaskoczony/a w produkcji — to cała wartość tego nawyku.
4. **Śledź uruchomione funkcje:**
   - Odfiltruj do **Uruchomione** i znajdź jedną niedawną funkcję SharePoint, której jeszcze nie użyłeś/aś w tym kursie. Wypróbuj ją raz, krótko, na dowolnej witrynie testowej.
5. **Zasubskrybuj aktualizacje:**
   - Dodaj do zakładek przefiltrowany widok mapy drogowej (lub skonfiguruj powiadomienie RSS/Centrum wiadomości), żeby stało się to powtarzającym się, 10-minutowym cotygodniowym nawykiem, a nie jednorazowym ćwiczeniem laboratoryjnym.

### Sprawdzenie wiedzy:
Wymień jedną zmianę, którą udokumentowałeś/aś wcześniej w tym kursie (wycofanie klasycznych witryn z LAB-02 lub rebranding Syntex→Premium z LAB-08), którą administrator śledzący mapę drogową zobaczyłby nadchodzącą z wyprzedzeniem wielu miesięcy. Jaki jest konkretny koszt dowiedzenia się o tym na trudny sposób zamiast tego?

---

## Lab 3: Zamiana monitorowania w planowanie administracyjne
### Cel:
Przekształcić świadomość Centrum kondycji i mapy drogowej w faktyczny nawyk operacyjny dla Contoso, a nie tylko coś, na co patrzysz, gdy coś się psuje.

### Kroki:
1. **Monitoruj proaktywnie, nie reaktywnie:**
   - Korzystając z panelu Kondycji usługi, zidentyfikuj dowolny komunikat (nawet drobny) i naszkicuj dwuzdaniową wiadomość ostrzegawczą, którą wysłałbyś/łabyś do właścicieli witryn Contoso *zanim* ich to dotknie.
2. **Zbuduj plan adopcji funkcji:**
   - Weź funkcję z mapy drogowej, którą oznaczyłeś/aś jako istotną w Lab 2, i naszkicuj plan wdrożenia: kto potrzebuje szkolenia, kto musi to zatwierdzić i jak łączy się to z czymś, co już objąłeś/aś governance (np. jeśli to funkcja związana z udostępnianiem, powiąż ją z powrotem z Twoją zasadą udostępniania z LAB-03).
3. **Przygotuj raport dla interesariuszy:**
   - Połącz podsumowanie Kondycji usługi i podsumowanie mapy drogowej w jedną jednostronicową aktualizację, napisaną dla kierownictwa Contoso — nie dla innego administratora. Bez żargonu, bez surowych ID incydentów, po prostu "oto co się stało, oto co nadchodzi, oto co z tym robimy".

### Wyzwanie:
Ustal powtarzający się 15-minutowy cotygodniowy slot (dosłownie dodaj go do kalendarza) na "sprawdzenie Kondycji usługi + Mapy drogowej" i zdefiniuj na piśmie, co spowodowałoby eskalację czegoś z tego sprawdzenia do faktycznego wniosku o zmianę dla Contoso — to jest różnica między administratorami, którzy dają się zaskoczyć, a tymi, którzy się nie dają.

### Sprawdzenie wiedzy:
Dlaczego raport dla interesariuszy mówiący "SharePoint miał 2 incydenty w tym miesiącu, oba rozwiązane, i 3 istotne funkcje wdrażają się w przyszłym kwartale" jest bardziej wartościowy dla kierownictwa niż surowy dostęp do samego panelu Kondycji usługi?

---

Te ćwiczenia dają praktyczne doświadczenie w monitorowaniu kondycji usługi Microsoft 365 oraz korzystaniu z mapy drogowej SharePoint do planowania administracyjnego — zamykając pętlę kursu, który zaczął się od "czym zajmuje się administrator SharePoint" a kończy na "jak robić to dobrze przez cały czas".
