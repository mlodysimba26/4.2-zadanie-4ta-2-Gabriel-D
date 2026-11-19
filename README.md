# Notatnik-v2 — Konstytucja, decyzje i taksonomia reguł projektu

Projekt repozytorium `notatnik-v2` utworzony w ramach modułu poświęconego specyfikacjom, inwariantom architektonicznym i dokumentowaniu decyzji projektowych (Spec-Driven Development).

---

## 1. Rozsortowanie dwunastu zdań (Taksonomia ustaleń projektowych)

Poniżej znajduje się dwanaście zdań rozdzielonych do czterech właściwych miejsc w architekturze dokumentacji projektu:

### A. Plik reguł (`AGENTS.md` / reguły dla agenta programistycznego i dewelopera)
*Plik reguł definiuje zasady procesu wytwarzania kodu, higienę pracy w repozytorium oraz wytyczne warsztatowe dla programisty i modelu AI.*

1. **„Metoda nie może być dłuższa niż dwadzieścia linii.”**  
   *Uzasadnienie:* Jest to typowa reguła czystego kodu i formatowania źródeł (standard deweloperski). Mówi o tym, jak ma być zorganizowany kod, a nie co oferuje produkt użytkownikowi.
2. **„Nie zmieniaj plików, o które nie było prośby.”**  
   *Uzasadnienie:* Jest to dyrektywa procesowa / guardrail dla agenta programistycznego, ograniczająca zakres nieautoryzowanych modyfikacji w drzewie plików repozytorium.

---

### B. Specyfikacja (`spec.md` / specyfikacja wymagań funkcjonalnych i interfejsu)
*Specyfikacja opisuje konkretne zachowania systemu, stany kontrolek interfejsu graficznego oraz kryteria akceptacji poszczególnych funkcji.*

3. **„Kiedy tytuł notatki jest pusty, przycisk zapisu jest nieaktywny.”**  
   *Uzasadnienie:* Precyzyjny opis zachowania elementu interfejsu (logiki walidacji kontrolki przycisku w formularzu dodawania notatki).
4. **„Program uruchamia się w mniej niż dwie sekundy.”**  
   *Uzasadnienie:* Niefunkcjonalne kryterium akceptacji wydajności (performance requirement / SLO) sprawdzane przy odbiorze konkretnego wydania aplikacji.

---

### C. Konstytucja (`konstytucja.md` / niezmienniki architektoniczne i produktowe)
*Konstytucja formułuje nienaruszalne prawdy i inwarianty całego systemu, które muszą pozostać prawdziwe w każdej wersji programu, niezależnie od dodawanych funkcji.*

5. **„Program nie wymaga połączenia z internetem.”**  
   *Uzasadnienie:* Fundamentalny inwariant architektoniczny systemu (tryb 100% offline, brak zależności od sieci i chmury).
6. **„Program jest dla jednej osoby na jednym komputerze.”**  
   *Uzasadnienie:* Niezmienne założenie modelu użytkownika (architektura jednoosobowa, local-first, brak kont, logowania i ról).
7. **„Dane nigdy nie opuszczają komputera użytkownika.”**  
   *Uzasadnienie:* Twarda zasada prywatności i bezpieczeństwa danych, obowiązująca każdą istniejącą i przyszłą funkcję.

---

### D. Zapis decyzji (ADR w katalogu `decyzje/`)
*Zapis decyzji architektonicznej (Architectural Decision Record) dokumentuje świadomy wybór techniczny lub zakresowy spośród rozważanych alternatyw, wraz z kontekstem, uzasadnieniem i analizą skutków.*

8. **„Wybraliśmy Markdown zamiast JSON, bo plik ma być czytelny w edytorze.”**  
   *Uzasadnienie:* Klasyczny zapis decyzji technicznej (ADR): wskazuje dokonany wybór formatu magazynu danych, odrzuconą alternatywę (JSON) oraz konkretne uzasadnienie wyboru.
9. **„Zrezygnowaliśmy z wyszukiwania w tej wersji, bo wydłużyłoby tydzień o drugi.”**  
   *Uzasadnienie:* Świadoma decyzja o cięciu zakresu (scope cut) z udokumentowanym kompromisem czasowym (trade-off) pomiędzy zakresem funkcji a terminem realizacji.

---

## 2. Zdania celowo dwuznaczne i analiza kontekstu

Trzy spośród zdań mogą należeć do różnych dokumentów w zależności od intencji i kontekstu technicznego:

### Dwuznaczność 1: „Wszystkie pliki w UTF-8.”
- **Od czego zależy odpowiedź:** Zależy od tego, jakich plików dotyczy to wymaganie:
  - **Plik reguł:** Jeśli dotyczy *plików źródłowych projektu* (`.cs`, `.axaml`, `.csproj`, `.md`) tworzonych i edytowanych przez dewelopera lub agenta AI podczas programowania (instrukcja warsztatowa zapobiegająca problemom z kodowaniem znaków w repozytorium).
  - **Konstytucja / Specyfikacja:** Jeśli dotyczy *plików magazynu danych użytkownika* (`notatki.md`) tworzonych przez uruchomiony program — wówczas jest to inwariant trwałości danych w Konstytucji (użytkownik zawsze ma pliki w uniwersalnym standardzie UTF-8) lub techniczne kryterium formatu w Specyfikacji.

### Dwuznaczność 2: „Komunikaty o błędach są po polsku.”
- **Od czego zależy odpowiedź:** Zależy od grupy odbiorców komunikatów i warstwy systemu:
  - **Specyfikacja:** Jeśli dotyczy *komunikatów prezentowanych użytkownikowi końcowemu w oknie programu* (np. powiadomień w kontrolkach UI o braku tytułu czy niepowodzeniu zapisu na dysku) — jest to wymóg lokalizacji interfejsu użytkownika.
  - **Plik reguł:** Jeśli dotyczy *instrukcji dla agenta AI, wpisów w konsoli deweloperskiej lub komentarzy w kodzie* generowanych podczas pracy programistycznej.
  - **Konstytucja:** Jeśli traktujemy to jako nadrzędny inwariant tożsamości produktu (zasada mówiąca, że program w każdym punkcie styku z człowiekiem posługuje się wyłącznie językiem polskim).

### Dwuznaczność 3: „Notatki sortujemy od najnowszej.”
- **Od czego zależy odpowiedź:** Zależy od tego, czy chodzi o logikę prezentacji w UI, czy fizyczny układ danych w pliku:
  - **Specyfikacja:** Jeśli dotyczy *sposobu wyświetlania elementów w kontrolce widoku listy* (np. kontrolka `ListBox` sortuje elementy malejąco po dacie).
  - **Zapis decyzji (ADR) / Konstytucja:** Jeśli dotyczy *fizycznej struktury zapisu w pliku `notatki.md`* (decyzja o odwróconej chronologii i wstawianiu wpisów na początku pliku zamiast dopisywania na końcu, z uzasadnieniem i analizą wad takiego podejścia).

---

## 3. Zasada z szablonu konstytucji Spec Kit

- **Szablon źródłowy:** [Spec Kit Constitution Template (`templates/constitution-template.md`)](https://raw.githubusercontent.com/github/spec-kit/main/templates/constitution-template.md) w oficjalnym repozytorium GitHub Spec Kit (`github/spec-kit`).
- **Data odczytania źródła:** 19 listopada 2026 r.
- **Wybrana zasada z szablonu:**
  > **Principle I: Library-First**  
  > *„Every feature starts as a standalone library; Libraries must be self-contained, independently testable, documented; Clear purpose required - no organizational-only libraries.”*  
  > *(Każda funkcja zaczyna się jako samodzielna biblioteka; biblioteki muszą być samowystarczalne, niezależnie testowalne i udokumentowane; wymagany jasny cel — brak bibliotek wyłącznie organizacyjnych).*

### Dlaczego ta zasada dla notatnika byłaby bez sensu:
W przypadku małego, jednoosobowego notatnika desktopowego rozbijanie każdej drobnej funkcjonalności na osobną, autonomiczną bibliotekę (*Library-First*) wprowadziłoby ogromny, zbędny narzut architektoniczny i ceremonialny (wiele projektów `.csproj`, skomplikowane zarządzanie zależnościami i wersjonowaniem), paraliżując prostotę i szybkość rozwoju programu, który ma być prostą aplikacją okienkową bez nadmiarowych warstw abstrakcji.

---

## 4. Spełnienie warunków zadania

1. **Warunek 1:** Pierwszy commit (`8ec80bc`) zawiera dokładnie dwa pliki (`konstytucja.md` oraz `decyzje/0001-format-pliku-notatek.md`), zero linijek kodu programistycznego i opis `konstytucja i pierwsza decyzja`.
2. **Warunek 2:** Plik `konstytucja.md` zawiera 6 nienaruszalnych punktów (mieszczących się w przedziale 5–8) definiujących inwarianty produktu bez wchodzenia w szczegóły implementacji kodu ani zachowania pojedynczych kontrolek.
3. **Warunek 3:** Plik `decyzje/0001-format-pliku-notatek.md` posiada cztery wymagane sekcje (*Kontekst*, *Decyzja*, *Odrzucone*, *Skutki*), wiernie odzwierciedla format z Budowy 2 (`notatki.md` z nagłówkiem `## YYYY-MM-DD HH:mm — Tytuł` i odwróconą chronologią), a w sekcji *Skutki* szczegółowo omawia wady i trudności wynikające z tej decyzji (złożoność $O(N)$, brak atomowości zapisu, kruchość parsowania, trudność edycji pojedynczych wpisów).
4. **Warunek 4:** Plik `README.md` zawiera pełną taksonomię 12 zdań z podziałem na 4 kategorie, wyczerpującą analizę zdań dwuznacznych, odwołanie do szablonu konstytucji Spec Kit z datą odczytania oraz wyjaśnienie bezsensowności zasady *Library-First* dla notatnika.
