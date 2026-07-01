# Asystent AI do Automatyzacji Procesów Biznesowych

Zaprojektowałam i wdrożyłam rozbudowany system automatyzacji procesów biznesowych dla firmy zajmującej się sprzedażą i montażem okien, drzwi, parapetów, rolet oraz bram. Celem projektu było stworzenie inteligentnego asystenta AI opartego na modelu OpenAI w ChatGPT, który pozwala właścicielowi firmy zarządzać codzienną pracą za pomocą prostych komend w naturalnym języku.

---

## Zakres prac

System składa się z wielu połączonych workflow w n8n. Każdy workflow odpowiada za konkretny proces biznesowy i komunikuje się z asystentem AI przez własne endpointy API.

Zakres obejmował:

- tworzenie i aktualizację wycen
- przenoszenie wycen do zamówień
- tworzenie zamówień bez wcześniejszej wyceny
- generowanie dokumentów dla klienta
- tworzenie faktur zaliczkowych, końcowych i zwykłych
- zapis faktur i dokumentów w Google Drive
- automatyczną organizację folderów klienta
- analizę i zapis danych z plików WEB / kosztorysów producentów
- porównywanie danych z zamówienia z plikami producenta
- obsługę reklamacji
- tworzenie maili roboczych i wysyłanie wiadomości
- wysyłkę zleceń do ekip montażowych
- przypomnienia Todo
- podsumowania dnia
- integrację z Google Calendar
- integrację SMS
- obsługę statusów zamówień i dostaw
- zarządzanie kontrahentami i producentami
- bezpieczne wyszukiwanie rekordów po ID, telefonie, nazwisku, e-mailu oraz produkcie

---

## Technologie

| Narzędzie | Zastosowanie |
|---|---|
| **n8n** | Silnik automatyzacji — workflow, logika, integracje |
| **OpenAI / ChatGPT** | Interfejs asystenta AI, rozpoznawanie intencji |
| **JavaScript** | Kod logiki biznesowej w węzłach n8n |
| **REST API / OpenAPI** | Komunikacja między asystentem a workflow |
| **Google Sheets** | Baza danych wycen, zamówień, kontrahentów |
| **Google Drive** | Przechowywanie dokumentów i folderów klientów |
| **Gmail** | Komunikacja mailowa |
| **Google Calendar** | Planowanie montażów i przypomnień |
| **Fakturownia API** | Wystawianie i pobieranie faktur |
| **SMS API** | Powiadomienia dla klientów |
| **JSON** | Format wymiany danych między węzłami |

Byłam odpowiedzialna za analizę procesu biznesowego, projekt architektury automatyzacji, przygotowanie logiki workflow, konfigurację integracji, tworzenie endpointów API, pisanie kodu JavaScript w n8n, testowanie scenariuszy oraz poprawianie działania systemu na podstawie feedbacku właściciela firmy.

---

## Najważniejsze funkcje systemu

### 1. Asystent AI sterowany komendami właściciela

System pozwala właścicielowi firmy wykonywać działania przez zwykły czat w ChatGPT. Użytkownik nie musi znać nazw workflow, API ani struktury arkuszy — wpisuje polecenie naturalnym językiem, a asystent rozpoznaje intencję i uruchamia odpowiedni proces.

**Przykładowe komendy:**

```
"Stwórz wycenę dla klienta X"
"Przenieś klienta X do zamówień"
"Wystaw fakturę zaliczkową dla klienta X"
"Przygotuj umowę dla klienta X"
"Wyślij mail do producenta"
"Pokaż wszystkie reklamacje"
"Usuń wycenę klienta X"
"Dodaj przypomnienie do Todo"
"Sprawdź, czy przyszło zamówienie"
```

---

### 2. Automatyzacja wycen i zamówień

System tworzy nowe wyceny i zamówienia w Google Sheets, zapisuje dane klienta, produkty, ceny, statusy, terminy, informacje o montażu oraz dokumenty powiązane z klientem.

Dodana logika wyszukiwania istniejących rekordów zapobiega tworzeniu duplikatów. Rekordy są wyszukiwane po:

- ID wyceny / ID zamówienia
- numerze telefonu
- nazwisku, e-mailu
- produkcie, modelu, wymiarach, kolorze

Jeżeli system znajdzie kilka podobnych rekordów, nie wykonuje akcji automatycznie — zwraca listę dopasowań i prosi właściciela o wybór właściwego rekordu.

---

### 3. Dokumenty klienta

System generuje dokumenty na podstawie danych zapisanych w arkuszach i folderach klienta. Obsługiwane typy dokumentów:

- umowy
- protokoły
- zlecenia dla ekipy montażowej
- dokumenty do zamówień
- dokumenty powiązane z montażem

Wszystkie dokumenty są automatycznie zapisywane w odpowiednich folderach Google Drive.

---

### 4. Integracja z Fakturownią

Wdrożona integracja z Fakturownią umożliwia tworzenie różnych typów faktur bezpośrednio z poziomu asystenta AI:

- faktury zaliczkowe
- faktury końcowe
- faktury zwykłe

System zapisuje numery i linki do faktur w arkuszu, pobiera dokumenty PDF i umieszcza je w Google Drive, a następnie zwraca właścicielowi gotową odpowiedź z numerem faktury i linkiem do pliku.

---

### 5. Automatyczna organizacja Google Drive

System tworzy i aktualizuje foldery klientów w Google Drive. Dokumenty, faktury, pliki WEB, kosztorysy, umowy i inne załączniki są zapisywane w odpowiednim miejscu. Dzięki temu wszystkie dane klienta są połączone z rekordem w Google Sheets i dostępne w jednym miejscu.

---

### 6. Obsługa plików WEB i kosztorysów producentów

System analizuje pliki WEB / kosztorysy producentów, wyciąga z nich kluczowe dane i zapisuje je w arkuszach. Rozpoznaje:

- modele produktów, wymiary, kolory, ilości
- ceny netto, sumy netto
- usługi i montaż
- pozycje produktowe
- dane producenta i numer WEB / kosztorysu

Dane mogą być następnie porównywane z zamówieniem klienta w celu wykrycia rozbieżności między zamówieniem a specyfikacją producenta.

---

### 7. Komunikacja mailowa

System obsługuje komunikację mailową przez Gmail. W zależności od komendy właściciela może:

- utworzyć mail roboczy (draft) lub wysłać wiadomość bezpośrednio
- odpowiedzieć klientowi lub kontrahentowi
- przygotować wiadomość do producenta
- obsłużyć reklamacje
- rozpoznać typ wiadomości i przypisać ją do odpowiedniego procesu
- zapisać załączniki

---

### 8. Reklamacje

System wspiera obsługę reklamacji — zapisuje zgłoszenia, przypisuje je do klienta, analizuje wiadomości, pracuje z załącznikami i zdjęciami oraz tworzy zadania do dalszej obsługi. Właściciel może w każdej chwili sprawdzić aktywne reklamacje i status ich rozwiązania.

---

### 9. Todo, przypomnienia i podsumowania dnia

System automatycznie tworzy przypomnienia i podsumowania dnia, uwzględniając statusy zamówień, faktury, reklamacje, zadania, zamówienia u producentów i płatności.

Automatyzacja jest dopasowana do realnego trybu pracy firmy:

- brak wysyłki podsumowań w niedzielę
- osobny harmonogram dla soboty
- przypomnienia o płatnościach raz w tygodniu
- przypomnienia o zamówieniach ze statusem „do zamówienia"
- przypomnienia o montażach
- zadania dla faktur kosztowych i płatności ręcznych

---

### 10. Integracja SMS i kalendarza

- **SMS API** — powiadomienia przy zmianie statusu zamówienia, dostawie, gotowości produktu lub informacji dla klienta
- **Google Calendar** — planowanie montażów, przypomnień oraz zadań powiązanych z terminami

---

## Skala projektu

| Obszar | Liczba |
|---|---|
| Główne obszary automatyzacji | ponad 8 |
| Scenariusze biznesowe | ponad 20 |
| Endpointy API | kilkanaście |
| Zintegrowane narzędzia | 7 |
| Typy obsługiwanych dokumentów | min. 4 |
| Typy faktur | min. 3 |

Projekt obejmuje ponadto: kilka arkuszy operacyjnych w Google Sheets, automatyczne foldery klientów w Google Drive, logikę wyszukiwania rekordów po wielu polach, zabezpieczenia przed duplikatami, obsługę przypadków z wieloma podobnymi rekordami oraz automatyczne odpowiedzi dla właściciela po każdej zakończonej akcji.

---

## Efekt biznesowy

System znacząco skraca czas wykonywania powtarzalnych działań w firmie. Właściciel może zarządzać procesami z poziomu jednego czatu, bez ręcznego przełączania się między Google Sheets, Google Drive, Gmail, Fakturownią i kalendarzem.

**Najważniejsze efekty:**

- mniej ręcznego przepisywania danych
- mniejsze ryzyko pomyłek w zamówieniach i fakturach
- szybsze tworzenie dokumentów
- lepsza organizacja plików klienta
- prostsza obsługa reklamacji
- większa kontrola nad statusami zamówień
- automatyczne przypomnienia o ważnych zadaniach
- centralizacja danych w jednym systemie
- możliwość zarządzania firmą przez naturalne komendy w czacie

---

## Podsumowanie

W efekcie powstał centralny system operacyjny wspierający codzienną pracę właściciela firmy. Rozwiązanie znacząco ograniczyło liczbę ręcznych operacji administracyjnych, skróciło czas obsługi procesów biznesowych oraz umożliwiło zarządzanie firmą za pomocą inteligentnego asystenta AI.
