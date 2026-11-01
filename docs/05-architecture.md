# Study flow - Architektura projektu

## 1. Wykorzystywane technologie

Aplikacja StudyFlow będzie tworzona z wykorzystaniem następujących technologii:

- **Angular 22.2.2** – główny framework frontendowy, odpowiedzialny za budowę interfejsu użytkownika i obsługę logiki aplikacji.
- **TypeScript 6.0.3** – język programowania wykorzystywany w projekcie Angular.
- **HTML i SCSS** – technologie wykorzystywane do tworzenia struktury i wyglądu interfejsu.
- **Supabase** – rozwiązanie backendowe zapewniające uwierzytelnianie użytkowników (Auth), relacyjną bazę danych PostgreSQL, przechowywanie plików (Storage) oraz REST API.

## 2. Architektura aplikacji

Aplikacja będzie opierać się na architekturze klient-serwer. Angular będzie odpowiadać za interfejs użytkownika i obsługę poszczególnych funkcjonalności, natomiast Supabase za uwierzytelnianie, przechowywanie danych i plików.

Kod frontendowy zostanie podzielony na trzy główne elementy:

- **Components** – odpowiadają za wyświetlanie widoków, obsługę formularzy i interakcje użytkownika.
- **Services** – odpowiadają za logikę aplikacji oraz komunikację z Supabase.
- **Models** – określają strukturę danych wykorzystywanych w aplikacji, np. celów, zadań i materiałów.

Komunikacja pomiędzy komponentami a backendem będzie odbywać się za pośrednictwem serwisów Angulara.

## 3. Struktura projektu

Kod aplikacji zostanie podzielony na katalogi według funkcjonalności. Dzięki temu poszczególne obszary aplikacji będą od siebie oddzielone, a ich rozwijanie i utrzymywanie będzie łatwiejsze.

```text
src/
└── app/
    ├── components/
    │   ├── dashboard/
    │   ├── goals/
    │   ├── tasks/
    │   ├── materials/
    │   ├── schedule/
    │   ├── statistics/
    │   └── profile/
    │
    ├── services/
    │   ├── auth.service.ts
    │   ├── goal.service.ts
    │   ├── task.service.ts
    │   └── material.service.ts
    │
    ├── models/
    │   ├── goal.model.ts
    │   ├── task.model.ts
    │   ├── material.model.ts
    │   └── user.model.ts
    │
    ├── app.routes.ts
    └── app.component.ts
```

- **components/** – komponenty odpowiedzialne za poszczególne widoki i elementy interfejsu.
- **services/** – serwisy obsługujące operacje na danych, uwierzytelnianie i komunikację z Supabase.
- **models/** – modele opisujące strukturę danych wykorzystywanych w aplikacji.
- **app.routes.ts** – konfiguracja tras i nawigacji pomiędzy widokami.
- **app.component.ts** – główny komponent aplikacji.

## 4. Dobór bibliotek i narzędzi

| Potrzeba | Rozwiązanie | Uzasadnienie |
|---|---|---|
| Budowa interfejsu użytkownika | Angular | Umożliwia tworzenie aplikacji SPA z wykorzystaniem komponentów i gotowych mechanizmów frameworka. |
| Komunikacja z backendem | Supabase | Zapewnia dostęp do bazy danych, uwierzytelniania i przechowywania plików. |
| Obsługa formularzy | Angular Reactive Forms | Ułatwia tworzenie formularzy i walidację danych wprowadzanych przez użytkownika. |
| Zarządzanie stanem | Angular Signals | Umożliwia reaktywne przechowywanie i aktualizowanie danych wykorzystywanych przez komponenty. |
| Komponenty interfejsu | Angular Material i Angular CDK | Dostarczają gotowe elementy interfejsu i narzędzia ułatwiające tworzenie komponentów. |
| Formatowanie kodu | Prettier | Zapewnia spójne formatowanie kodu w całym projekcie. |
| Kontrola jakości kodu | ESLint | Pomaga wykrywać potencjalne błędy i nieprawidłowe wzorce w kodzie. |
| Obsługa operacji asynchronicznych | RxJS | Umożliwia obsługę operacji asynchronicznych i strumieni danych. |

## 5. Zarządzanie stanem i komunikacja

Zarządzanie stanem aplikacji będzie realizowane za pomocą **Angular Signals**. Będą one wykorzystywane do przechowywania i aktualizowania danych potrzebnych w komponentach, takich jak lista celów, lista zadań czy informacje o zalogowanym użytkowniku.

Komponenty będą odpowiadać za prezentowanie danych i obsługę interakcji użytkownika, natomiast serwisy będą odpowiedzialne za operacje na danych oraz komunikację z Supabase.

Przykładowy przepływ danych podczas pobierania zadań:

1. Użytkownik otwiera widok listy zadań.
2. Komponent wywołuje odpowiednią metodę serwisu.
3. Serwis pobiera dane z Supabase.
4. Pobrane dane zostają zapisane w stanie zarządzanym za pomocą Signals.
5. Komponent reaguje na zmianę stanu i aktualizuje wyświetlaną listę.

Komunikacja z backendem będzie obejmować dodawanie, pobieranie, edytowanie i usuwanie celów, zadań oraz materiałów. Operacje na plikach będą realizowane za pomocą Supabase Storage, natomiast ich podstawowe informacje będą przechowywane w bazie danych.

Aplikacja będzie również obsługiwać stany ładowania, błędy oraz sytuacje, w których użytkownik nie posiada jeszcze żadnych danych.