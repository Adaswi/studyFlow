# Study flow - API i zaplecze danych

## Wybrane rozwiązanie

W projekcie StudyFlow wykorzystane zostanie rozwiązanie Supabase.

### Uzasadnienie wyboru

Aplikacja wymaga:

- trwałego przechowywania danych użytkowników,
- zarządzania celami edukacyjnymi,
- zarządzania zadaniami,
- przechowywania materiałów edukacyjnych,
- obsługi uwierzytelniania,
- komunikacji z REST API,
- wykonywania operacji CRUD na danych.

Supabase dostarcza wszystkie wymagane funkcjonalności w ramach jednej platformy. Dzięki temu nie ma konieczności implementowania własnego backendu, a projekt może skupić się na części frontendowej, zgodnie z założeniami przedmiotu.

---

## Adresy i dokumentacja

**Strona główna:**

https://supabase.com

**Dokumentacja:**

https://supabase.com/docs

**REST API:**

Supabase automatycznie generuje REST API na podstawie utworzonych tabel w bazie danych PostgreSQL.

Przykładowy adres API:

```text
https://<project-id>.supabase.co/rest/v1/
```

Dokładny adres zostanie uzupełniony po utworzeniu projektu.

---

## Architektura komunikacji

```text
Angular
   │
   ▼
Supabase API
   │
   ├── Authentication
   ├── PostgreSQL Database
   ├── Storage
   └── REST API
```

---

# Model danych

## Goal (Cel edukacyjny)

| Pole | Typ | Opis |
|--------|--------|--------|
| id | UUID | Unikalny identyfikator celu |
| userId | UUID | Właściciel celu |
| title | String | Nazwa celu |
| description | String | Opis celu |
| deadline | Date | Termin realizacji |
| status | Enum | Status celu |
| progress | Number | Stopień realizacji celu |
| createdAt | DateTime | Data utworzenia |

### Statusy celu

- ACTIVE
- COMPLETED

---

## Task (Zadanie)

| Pole | Typ | Opis |
|--------|--------|--------|
| id | UUID | Unikalny identyfikator zadania |
| goalId | UUID | Powiązany cel |
| title | String | Nazwa zadania |
| description | String | Opis zadania |
| priority | Enum | Priorytet zadania |
| status | Enum | Status zadania |
| deadline | Date | Termin realizacji |
| createdAt | DateTime | Data utworzenia |

### Priorytety

- CRITICAL
- HIGH
- NORMAL
- OPTIONAL

### Statusy zadania

- TODO
- IN_PROGRESS
- DONE

---

## Material (Materiał edukacyjny)

| Pole | Typ | Opis |
|--------|--------|--------|
| id | UUID | Identyfikator materiału |
| goalId | UUID | Powiązany cel edukacyjny |
| taskId | UUID | Powiązane zadanie |
| fileName | String | Nazwa pliku |
| fileType | String | Typ pliku |
| fileUrl | String | Lokalizacja pliku |
| uploadedAt | DateTime | Data dodania |

### Obsługiwane formaty

- PDF
- DOCX
- PPTX
- PNG
- JPG
- JPEG

---

## User (Użytkownik)

Konto użytkownika jest obsługiwane przez Supabase Auth. Dane profilu przechowywane są w tabeli `profiles`.

| Pole | Typ | Opis |
|--------|--------|--------|
| id | UUID | Identyfikator użytkownika zgodny z Supabase Auth |
| email | String | Adres e-mail |
| fullName | String | Imię i nazwisko użytkownika |
| createdAt | DateTime | Data utworzenia konta |

Hasła są obsługiwane przez Supabase Auth i nie są przechowywane w tabeli `profiles`.

---

# Planowane zasoby REST API

Supabase REST API korzysta z nazw tabel w bazie danych. Filtrowanie rekordów odbywa się za pomocą składni PostgREST.

## Goals

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /goals | Pobranie listy celów |
| GET | /goals?id=eq.<UUID> | Pobranie szczegółów celu |
| POST | /goals | Utworzenie celu |
| PATCH | /goals?id=eq.<UUID> | Aktualizacja celu |
| DELETE | /goals?id=eq.<UUID> | Usunięcie celu |

---

## Tasks

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /tasks | Pobranie listy zadań |
| GET | /tasks?id=eq.<UUID> | Pobranie szczegółów zadania |
| POST | /tasks | Utworzenie zadania |
| PATCH | /tasks?id=eq.<UUID> | Aktualizacja zadania |
| DELETE | /tasks?id=eq.<UUID> | Usunięcie zadania |

---

## Materials

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /materials | Pobranie listy materiałów |
| POST | /materials | Dodanie metadanych materiału |
| DELETE | /materials?id=eq.<UUID> | Usunięcie metadanych materiału |

Pliki są przesyłane i usuwane osobno za pomocą Supabase Storage. Usunięcie materiału powinno obejmować zarówno plik w Storage, jak i jego metadane w tabeli `materials`.

---

## Profiles

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /profiles?id=eq.<UUID> | Pobranie danych użytkownika |
| PATCH | /profiles?id=eq.<UUID> | Aktualizacja danych użytkownika |

---

# Filtrowanie i wyszukiwanie

Planowane jest wykorzystanie parametrów zapytań REST API do filtrowania danych.

Przykłady:

```http
GET /tasks?status=eq.TODO
```

Pobranie wszystkich niewykonanych zadań.

```http
GET /tasks?priority=eq.HIGH
```

Pobranie zadań o wysokim priorytecie.

```http
GET /tasks?goal_id=eq.<UUID>
```

Pobranie zadań przypisanych do konkretnego celu edukacyjnego.

```http
GET /goals?status=eq.ACTIVE
```

Pobranie aktywnych celów.

Dodatkowo planowane jest wyszukiwanie pełnotekstowe po nazwach celów oraz zadań.

---

# Autoryzacja

Za proces uwierzytelniania użytkowników odpowiadać będzie Supabase Auth.

Planowana funkcjonalność obejmuje:

- rejestrację użytkowników,
- logowanie użytkowników,
- wylogowanie użytkowników,
- zarządzanie aktywną sesją,
- odzyskiwanie dostępu do konta.

Po poprawnym zalogowaniu użytkownik otrzymuje token dostępu wykorzystywany podczas komunikacji z API.

Dostęp do zasobów aplikacji będzie ograniczony wyłącznie do danych należących do aktualnie zalogowanego użytkownika. W tym celu zostaną skonfigurowane reguły Row Level Security (RLS) w bazie danych Supabase. Same zabezpieczenia po stronie Angulara nie są wystarczające do ochrony danych.

---

# Przechowywanie plików

Materiały edukacyjne będą przechowywane przy wykorzystaniu Supabase Storage.

Rozwiązanie pozwala na:

- przesyłanie plików,
- pobieranie plików,
- usuwanie plików,
- przypisywanie materiałów do wybranych celów edukacyjnych.

Każdy użytkownik będzie miał dostęp wyłącznie do własnych materiałów. Informacje o plikach, takie jak nazwa, typ i powiązany cel, będą przechowywane w tabeli `materials`, natomiast same pliki w Supabase Storage.

---

# Obsługa błędów

Aplikacja będzie obsługiwała trzy podstawowe stany danych wymagane w nowoczesnych aplikacjach frontendowych.

## Loading

Podczas pobierania lub zapisywania danych użytkownik otrzymuje informację o trwającej operacji.

Przykłady:

- logowanie,
- pobieranie zadań,
- przesyłanie plików.

## Empty State

W przypadku braku danych użytkownik otrzymuje odpowiedni komunikat.

Przykłady:

- brak celów edukacyjnych,
- brak zadań,
- brak materiałów.

## Error State

W przypadku wystąpienia błędu wyświetlany jest czytelny komunikat.

Przykłady:

- brak połączenia z Internetem,
- błąd autoryzacji,
- nieudany zapis danych,
- niedostępność usługi.

---

# Ograniczenia wybranego rozwiązania

Projekt jest zależny od dostępności zewnętrznej usługi Supabase. Ewentualna awaria platformy może uniemożliwić dostęp do danych lub wykonywanie operacji zapisu.

Darmowy plan Supabase posiada ograniczone zasoby bazy danych oraz przestrzeni przeznaczonej na pliki, jednak przewidywana skala projektu StudyFlow nie powinna przekroczyć tych limitów.

Aplikacja wymaga połączenia z Internetem, ponieważ wszystkie dane przechowywane są poza urządzeniem użytkownika.

---

# Bezpieczeństwo

Dane użytkowników będą chronione za pomocą mechanizmów dostarczanych przez Supabase Auth.

Projekt zakłada następujące zasady bezpieczeństwa:

- dostęp do aplikacji wymaga uwierzytelnienia,
- użytkownik ma dostęp wyłącznie do własnych danych,
- komunikacja odbywa się przez protokół HTTPS,
- hasła nie są przechowywane po stronie aplikacji frontendowej,
- klucze dostępu i dane konfiguracyjne nie będą umieszczane w repozytorium projektu.

---

# Rozwój dokumentacji

Dokument przedstawia wstępną koncepcję komunikacji aplikacji z zapleczem danych. W kolejnych etapach projektu będzie aktualizowany wraz z doprecyzowaniem modelu danych, konfiguracji Supabase oraz finalnego sposobu komunikacji pomiędzy frontendem i usługami backendowymi.