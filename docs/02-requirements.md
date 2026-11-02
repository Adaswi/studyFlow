# Study flow - wymagania projektu

## 1. Główne procesy użytkownika

### Proces 1 - Rejestracja i konfiguracja konta

1. Użytkownik zakłada konto.
2. Użytkownik rejestruje się do aplikacji.
3. Użytkownik uzupełnia dane profilu.
4. System tworzy przestrzeń użytkownika.
5. Użytkownik otrzymuje dostęp do wszystkich funkcji aplikacji.

**Rezultat:** użytkownik może rozpocząć planowanie swojej nauki.

---

### Proces 2 - Tworzenie celu edukacyjnego

1. Użytkownik tworzy nowy cel edukacyjny.
2. Określa nazwę, opis oraz termin realizacji.
3. System zapisuje cel.
4. Cel pojawia się na liście aktywnych celów.
5. Użytkownik może przypisywać do niego zadania i materiały.

**Przykład:** „Przygotowanie do egzaminu z Programowania Niskopoziomowego”.

**Rezultat:** użytkownik definiuje konkretny kierunek nauki.

---

### Proces 3 - Planowanie i realizacja zadań

1. Użytkownik dodaje zadania związane z celem.
2. Przypisuje termin realizacji.
3. Określa priorytet zadania.
4. System uwzględnia zadanie w harmonogramie.
5. Użytkownik oznacza zadania jako wykonane.
6. System aktualizuje postęp celu.

**Rezultat:** użytkownik może systematycznie realizować większe cele edukacyjne.

---

### Proces 4 - Zarządzanie materiałami edukacyjnymi

1. Użytkownik wybiera cel edukacyjny, z którym chce powiązać materiał.
2. Opcjonalnie przypisuje materiał do konkretnego zadania.
3. System zapisuje pliki i metadane.
4. Materiały są dostępne bezpośrednio z poziomu celu, zadania lub z listy.

**Obsługiwane typy plików:**

- PDF
- DOCX
- PPTX
- PNG
- JPG
- JPEG

**Rezultat:** wszystkie materiały związane z nauką znajdują się w jednym miejscu.

---

### Proces 5 - Organizacja harmonogramu nauki

1. Użytkownik przegląda wszystkie aktywne zadania.
2. Filtruje zadania według terminu, statusu lub priorytetu.
3. Sprawdza zadania zaplanowane na najbliższe dni.
4. System wyróżnia zadania zaległe i zbliżające się do terminu realizacji.

**Rezultat:** użytkownik wie, czym powinien zająć się w pierwszej kolejności.

---

### Proces 6 - Monitorowanie postępów

1. Użytkownik przechodzi do dashboardu.
2. System pobiera dane dotyczące celów i zadań.
3. Wyświetlane są statystyki i wykresy.
4. Użytkownik analizuje własne postępy.
5. Na podstawie danych aktualizuje plan nauki.

**Rezultat:** użytkownik może kontrolować efektywność realizacji swoich planów edukacyjnych.

---

## 2. Wymagania funkcjonalne

### Zarządzanie użytkownikiem

- Rejestracja konta.
- Logowanie.
- Wylogowanie.
- Edycja danych profilu.
- Ochrona wybranych widoków dla zalogowanych użytkowników.

### Zarządzanie celami edukacyjnymi

- Tworzenie celu edukacyjnego.
- Wyświetlanie listy celów.
- Podgląd szczegółów celu.
- Edycja celu.
- Usuwanie celu.
- Oznaczanie celu jako ukończonego.

### Zarządzanie zadaniami

- Tworzenie zadania.
- Przypisanie zadania do celu.
- Edycja zadania.
- Usuwanie zadania.
- Oznaczanie zadania jako wykonane.
- Wyświetlanie historii wykonanych zadań.

Statusy zadań: TODO (oczekujące), IN_PROGRESS (w trakcie), DONE (wykonane).

### Priorytety i klasyfikacja zadań

Każde zadanie posiada jeden z poziomów ważności:

- Krytyczne
- Wysoki priorytet
- Standardowe
- Opcjonalne

### Harmonogram

- Wyświetlanie nadchodzących terminów.
- Wyświetlanie zadań zaległych.
- Lista najbliższych aktywności.

### Materiały edukacyjne

- Dodawanie plików.
- Usuwanie plików.
- Pobieranie plików.
- Powiązanie materiałów z celem lub zadaniem.

### Wyszukiwanie i filtrowanie

- Filtrowanie po statusie.
- Filtrowanie po celu.
- Filtrowanie po priorytecie.
- Filtrowanie po terminie.
- Wyszukiwanie tekstowe.

### Dashboard

- Liczba aktywnych celów.
- Liczba ukończonych celów.
- Liczba zadań oczekujących.
- Liczba zadań ukończonych.
- Procent realizacji celów.
- Liczba przechowywanych materiałów.
- Sekcja nadchodzących terminów.
- Wizualizacja danych przy pomocy wykresów.

#### Integracja z API

- REST API.
- Tworzenie danych (POST).
- Odczyt danych (GET).
- Aktualizacja danych (PUT/PATCH).
- Usuwanie danych (DELETE).
- Obsługa błędów HTTP.
- Obsługa stanów loading/error/empty.

---

## Funkcjonalności dodatkowe

### Powiadomienia

- Powiadomienie o zadaniach z terminem realizacji w ciągu najbliższych dni.
- Wyróżnienie zadań zaległych.
- Sekcja „Nadchodzące terminy”.

### Rozszerzone statystyki

- Aktywność użytkownika w czasie.
- Historia realizacji celów.
- Statystyki ukończonych zadań w podziale na miesiące.

### Personalizacja

- Tryb jasny.
- Tryb ciemny.

### Zaawansowane filtrowanie

- Łączenie wielu filtrów jednocześnie.
- Sortowanie wyników według różnych kryteriów.

---

## 3. Role i uprawnienia

### Role systemowe

1. Gość
2. Użytkownik

### Macierz uprawnień

| Funkcjonalność | Gość | Użytkownik |
|--------------|------|-----------|
| Rejestracja | ✓ | ✗ |
| Logowanie | ✓ | ✗ |
| Podgląd strony głównej | ✓ | ✓ |
| Podgląd własnego profilu | ✗ | ✓ |
| Edycja własnego profilu | ✗ | ✓ |
| Tworzenie celów | ✗ | ✓ |
| Edycja własnych celów | ✗ | ✓ |
| Usuwanie własnych celów | ✗ | ✓ |
| Tworzenie zadań | ✗ | ✓ |
| Edycja własnych zadań | ✗ | ✓ |
| Usuwanie własnych zadań | ✗ | ✓ |
| Oznaczanie zadań jako wykonane | ✗ | ✓ |
| Dodawanie materiałów | ✗ | ✓ |
| Usuwanie materiałów | ✗ | ✓ |
| Pobieranie materiałów | ✗ | ✓ |
| Korzystanie z filtrów | ✗ | ✓ |
| Dostęp do dashboardu | ✗ | ✓ |
| Dostęp do statystyk | ✗ | ✓ |