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
- ARCHIVED

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

| Pole | Typ | Opis |
|--------|--------|--------|
| id | UUID | Identyfikator użytkownika |
| email | String | Adres e-mail |
| createdAt | DateTime | Data utworzenia konta |

---

# Planowane zasoby REST API

## Goals

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /goals | Pobranie listy celów |
| GET | /goals/{id} | Pobranie szczegółów celu |
| POST | /goals | Utworzenie celu |
| PATCH | /goals/{id} | Aktualizacja celu |
| DELETE | /goals/{id} | Usunięcie celu |

---

## Tasks

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /tasks | Pobranie listy zadań |
| GET | /tasks/{id} | Pobranie szczegółów zadania |
| POST | /tasks | Utworzenie zadania |
| PATCH | /tasks/{id} | Aktualizacja zadania |
| DELETE | /tasks/{id} | Usunięcie zadania |

---

## Materials

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /materials | Pobranie listy materiałów |
| POST | /materials | Dodanie materiału |
| DELETE | /materials/{id} | Usunięcie materiału |

---

## Profiles

| Metoda | Endpoint | Opis |
|----------|----------|----------|
| GET | /profiles/{id} | Pobranie danych użytkownika |
| PATCH | /profiles/{id} | Aktualizacja danych użytkownika |

---

# Filtrowanie i wyszukiwanie

Planowane jest wykorzystanie parametrów zapytań REST API do filtrowania danych.

Przykłady:

```http
GET /tasks?status=TODO
```

Pobranie wszystkich niewykonanych zadań.

```http
GET /tasks?priority=HIGH
```

Pobranie zadań o wysokim priorytecie.

```http
GET /tasks?goalId={id}
```

Pobranie zadań przypisanych do konkretnego celu edukacyjnego.

```http
GET /goals?status=ACTIVE
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

Dostęp do zasobów aplikacji będzie ograniczony wyłącznie do danych należących do aktualnie zalogowanego użytkownika.

---

# Przechowywanie plików

Materiały edukacyjne będą przechowywane przy wykorzystaniu Supabase Storage.

Rozwiązanie pozwala na:

- przesyłanie plików,
- pobieranie plików,
- usuwanie plików,
- przypisywanie materiałów do wybranych celów edukacyjnych.

Każdy użytkownik będzie miał dostęp wyłącznie do własnych materiałów.

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