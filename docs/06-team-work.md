# Study flow - Organizacja pracy

## 1. Podział odpowiedzialności

Projekt StudyFlow jest realizowany indywidualnie. Wszystkie prace związane z projektowaniem, implementacją, testowaniem i dokumentowaniem aplikacji wykonuje jedna osoba. Z tego względu nie jest wymagany formalny podział obowiązków pomiędzy członków zespołu.

## 2. Zasady pracy z branchami

W projekcie przyjęto następującą strukturę gałęzi:

- **main** – główna gałąź zawierająca stabilną, sprawdzoną wersję aplikacji.
- **development** – gałąź przeznaczona do bieżącego rozwoju i łączenia ukończonych funkcjonalności.
- **feature_*** – gałęzie tworzone na podstawie `development`, przeznaczone do implementacji konkretnych funkcjonalności.

Zasady tworzenia branchy:

1. Każda większa funkcjonalność powinna być implementowana na osobnej gałęzi.
2. Nazwa gałęzi powinna rozpoczynać się od prefiksu `feature_` i określać realizowaną funkcjonalność, np. `feature_login`.
3. Po zakończeniu implementacji i sprawdzeniu zmian gałąź funkcjonalna jest łączona z `development`.
4. Po zakończeniu i sprawdzeniu większego etapu prac zmiany z `development` są łączone z `main`.

## 3. Zasady Pull Request i Code Review

Do łączenia gałęzi funkcjonalnych z `development` oraz `development` z `main` będą wykorzystywane Pull Requesty (PR) na GitHubie.

Każdy Pull Request powinien:

- posiadać tytuł opisujący wprowadzane zmiany,
- zawierać krótki opis zrealizowanej funkcjonalności lub poprawki,
- umożliwiać sprawdzenie zakresu wprowadzonych zmian przed ich połączeniem.

Ze względu na indywidualny charakter projektu formalne Code Review przez inną osobę nie będzie przeprowadzane. Autor projektu będzie samodzielnie sprawdzać zmiany przed zaakceptowaniem i scaleniem Pull Requesta.

Przed scaleniem należy upewnić się, że zmiany nie powodują błędów kompilacji ani problemów z działaniem aplikacji.