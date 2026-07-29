# Ćwiczenia laboratoryjne SharePoint Online - LAB-04

> **Scenariusz przewodni:** Witryna `Fabrikam Project` Contoso Consulting (z LAB-03) zapełnia się dokumentami, a ludzie nie mogą niczego znaleźć. Twoim zadaniem w tym labie jest naprawienie tego za pomocą metadanych — najpierw kolumn tekstu dowolnego, a potem zarządzanej, ogólnotenantowej taksonomii poprzez Magazyn terminów.

## Lab 1: Czym są metadane i dlaczego mają znaczenie?
### Cel:
Zrozumieć, czym są metadane i dlaczego są lepsze od folderów przy organizowaniu treści na dużą skalę.

### Kroki:
1. **Przyjrzyj się metadanym, które już masz:**
   - Otwórz bibliotekę dokumentów `Fabrikam Project`.
   - Przełącz się na widok **Wszystkie dokumenty** i zwróć uwagę na wbudowane kolumny metadanych: **Nazwa**, **Zmodyfikowano**, **Zmodyfikowane przez**, **Rozmiar pliku**. Istnieją one automatycznie w każdym pliku — metadane to *dane o danych*, a nie coś, co trzeba wymyślać od zera.
2. **Poczuj ból, jaki powodują foldery:**
   - Utwórz dwa foldery: `Propozycje` i `Raporty`.
   - Prześlij ten sam przykładowy dokument do obu, raz jako `Propozycja-Wersja robocza.docx` i raz jako `Raport-Wersja robocza.docx`.
   - Teraz spróbuj odpowiedzieć: "pokaż mi każdy dokument w wersji roboczej, niezależnie od typu." Same foldery na to nie pozwalają — musiałbyś/łabyś otworzyć oba foldery i przeglądać wzrokiem.
3. **Zaplanuj rozwiązanie:**
   - Zamiast kolejnych folderów, wymień 3 pola metadanych, które pozwolą filtrować/wyszukiwać w całej bibliotece naraz: np. **Typ dokumentu**, **Status**, **Klient**.

### Sprawdzenie wiedzy:
Podaj jedno konkretne wyszukiwanie lub filtrowanie, które możesz zrobić za pomocą kolumn metadanych, a którego zasadniczo nie da się zrobić za pomocą samych folderów.

---

## Lab 2: Konfigurowanie kolumn metadanych
### Cel:
Zamienić plan z Lab 1 w prawdziwe, działające kolumny w bibliotece `Fabrikam Project`.

### Kroki:
1. **Utwórz kolumny:**
   - Przejdź do biblioteki dokumentów `Fabrikam Project` > **Ustawienia biblioteki** (lub przycisk **+ Dodaj kolumnę** w widoku listy) > **Utwórz kolumnę**.
   - Utwórz:
     - **Typ dokumentu** (Wybór: Propozycja, Raport, Faktura, Umowa)
     - **Status** (Wybór: Wersja robocza, W recenzji, Ostateczny)
     - **Klient** (Jeden wiersz tekstu — naprawisz to w Lab 3)
2. **Otaguj istniejące dokumenty:**
   - Wróć do `Propozycja-Wersja robocza.docx` i `Raport-Wersja robocza.docx`, edytuj ich właściwości i wypełnij nowe kolumny.
3. **Udowodnij wartość:**
   - Utwórz nowy widok (lub użyj **Grupuj według**), który grupuje dokumenty według **Status**. Teraz "pokaż mi wszystko, co jest jeszcze w wersji roboczej" to jedno kliknięcie, w każdym folderze i każdym typie dokumentu.
4. **Dodaj wartość domyślną:**
   - Ustaw wartość domyślną **Status** na `Wersja robocza`, żeby każdy nowy przesłany plik zaczynał poprawnie otagowany, bez potrzeby pamiętania o tym przez nikogo.

### Wyzwanie:
Usuń całkowicie oba foldery z Lab 1 i polegaj wyłącznie na widokach opartych na metadanych (grupowanych/filtrowanych według Typu dokumentu, Statusu), żeby przeglądać bibliotekę. Zauważ, że to jest właśnie prawdziwa rekomendacja Microsoft dla nowoczesnych bibliotek — płaska struktura, widoki oparte na metadanych.

### Sprawdzenie wiedzy:
Dlaczego **wartość domyślna** w kolumnie typu Wybór to małe zwycięstwo governance, a nie tylko wygoda?

---

## Lab 3: Przegląd i użycie Magazynu terminów
### Cel:
Poznać, dlaczego kolumny tekstu dowolnego, jak `Klient` (Lab 2), są pułapką, oraz jak Magazyn terminów rozwiązuje to za pomocą zarządzanej, wielokrotnego użytku taksonomii.

### Kroki:
1. **Zobacz najpierw pułapkę:**
   - Wróć do wartości kolumny `Klient` wpisanych w Lab 2. Jeśli dwie osoby wpisały odpowiednio `Fabrikam`, `Fabrikam Inc` i `Fabrikam Inc.`, masz teraz trzech "różnych" klientów w wyszukiwaniu i filtrach. To dokładnie problem, który rozwiązuje Zarządzane metadane.
2. **Otwórz Magazyn terminów:**
   - Przejdź do **Centrum administracyjnego SharePoint** > **Usługi zawartości** > **Magazyn terminów**.
3. **Utwórz grupę terminów:**
   - Utwórz nową grupę terminów o nazwie `Contoso Metadata` i dodaj krótki opis.
4. **Utwórz zestawy terminów:**
   - W ramach `Contoso Metadata` utwórz zestaw terminów o nazwie `Clients` z terminami: `Fabrikam Inc.`, `Northwind Traders`.
   - Utwórz drugi zestaw terminów o nazwie `Departments` z terminami: `HR`, `IT`, `Marketing`, `Finance`.
5. **Zastąp kolumnę tekstu dowolnego:**
   - W `Fabrikam Project` utwórz nową kolumnę typu **Zarządzane metadane** o nazwie `Client (Managed)`, powiązaną z zestawem terminów `Clients`.
   - Zmigruj istniejące dokumenty biblioteki, aby używały jej zamiast starej kolumny tekstu dowolnego `Klient`, a następnie usuń starą kolumnę.
6. **Otaguj i przetestuj:**
   - Prześlij nowy dokument i otaguj go za pomocą selektora zarządzanych metadanych (zauważ, że teraz wymusza on spójną pisownię — koniec z ręcznym wpisywaniem).
   - Wyszukaj `Fabrikam` w polu wyszukiwania witryny i potwierdź, że dokument otagowany zarządzanymi metadanymi pojawia się niezawodnie.

### Wyzwanie:
Dodaj synonim do terminu `Fabrikam Inc.` (np. "Fabrikam") w Magazynie terminów i potwierdź, że wyszukiwanie synonimu nadal znajduje dokumenty otagowane kanonicznym terminem. To dokładnie ten problem, który rozwiązują synonimy, a którego kolumny tekstu dowolnego nigdy nie mogły rozwiązać.

### Sprawdzenie wiedzy:
Witryny `HR`, `IT` i `Marketing` z LAB-02 mogłyby wszystkie ponownie wykorzystać ten sam zestaw terminów `Departments`. Co poszłoby nie tak, gdyby każda witryna zamiast tego utworzyła własną, lokalną kolumnę Wybór `Departments`?

---

Te ćwiczenia budują prawdziwy osąd co do tego, *kiedy* zwykła kolumna wystarczy, a *kiedy* rzeczywiście potrzebujesz Magazynu terminów — decyzję, którą każdy administrator SharePoint podejmuje bez przerwy.
