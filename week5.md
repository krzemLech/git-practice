## Lesson 5

> Canonical location: `git-lesson/week5.md` (kopię możesz mieć też w root jako `week5.md`).

### Praca zespołowa (bez review): PR, upstream/origin, fetch upstream, rebase (wstęp)

Cel: umieć przygotować Pull Request na GitHubie oraz utrzymywać gałąź/“fork” na bieżąco względem źródła.

#### Pojęcia (GitHub)

- **Branch**: osobna gałąź pracy (np. `feature/login-copy`), z której robisz PR.
- **Pull Request (PR)**: prośba o scalenie zmian z jednej gałęzi do drugiej (np. `feature/...` -> `main`).
- **Fork**: kopia cudzego repo na Twoim koncie (na potrzeby pracy bez uprawnień do upstream).
- **origin**: zdalne repo, z którego klonowałeś (zwykle “Twoje”).
- **upstream**: zdalne repo źródłowe (oryginału), z którego chcesz pobierać zmiany.

#### PR na własnym repo (najczęstsze, najprostsze)

Workflow:

1. utwórz branch
2. zrób małe commity
3. push brancha
4. otwórz PR do `main`
5. uaktualniaj branch, jeśli `main` uciekł (merge albo rebase)
6. merge PR

Komendy (minimalny zestaw):

- `git switch -c feature/week5-pr`
- (zmiany w plikach)
- `git status`
- `git add -A`
- `git commit -m "docs: add week 5 notes"`
- `git push -u origin feature/week5-pr`

#### `git fetch upstream` — po co?

`fetch` pobiera obiekty i gałęzie ze zdalnego repo, ale **nie miesza Ci historii lokalnych gałęzi**.

Typowo:

- `git fetch origin` – pobierasz z własnego zdalnego repo (fork/Twoje repo)
- `git fetch upstream` – pobierasz z repo źródłowego (oryginału)

#### `git rebase` (wstęp) — po co i jak bezpiecznie

Rebase “przepisuje” Twoje commity na nowszą podstawę, np. na aktualne `main`:

- plusy: czysta, liniowa historia w PR, łatwiej się śledzi zmiany
- minusy: **zmienia historię commitów** (inne SHA), więc po rebase zwykle robisz push z nadpisaniem

Bezpieczna zasada:

- rebase rób na własnych gałęziach roboczych (np. branch do PR), nie na wspólnych branchach zespołu.

Najczęstszy scenariusz (branch do PR ma być “na świeżym main”):

- `git fetch origin`
- `git switch feature/week5-pr`
- `git rebase origin/main`
- jeśli były konflikty:
  - popraw pliki
  - `git add -A`
  - `git rebase --continue`
- wypchnij zmiany (po rebase zwykle trzeba):
  - `git push --force-with-lease`

`--force-with-lease` jest bezpieczniejsze niż `--force` (nie nadpisze cudzych zmian, jeśli ktoś coś dopchnął).

---

## Ćwiczenia (GitHub + Twoje repo, bez code review)

### Ćwiczenie 1 — PR w obrębie jednego repo (branch -> PR -> merge)

1. W tym repo utwórz branch `feature/week5-pr`.
2. Zmień jeden plik (np. dopisz 5 linijek do `notes.txt` albo popraw literówkę).
3. Zrób 1–2 commity z sensowną wiadomością.
4. Push brancha na GitHuba i otwórz PR do `main`.
5. Zmerguj PR (zależnie od ustawień repo: “Merge”, “Squash”, albo “Rebase and merge”).

Checkpoint: potrafisz wytłumaczyć “co jest source, a co target” w PR (z jakiej gałęzi do jakiej).

### Ćwiczenie 2 — “main uciekł”: zaktualizuj branch przez rebase

Cel: zasymulować sytuację, w której ktoś dopchnął zmiany do `main`, a Twój PR jest starszy.

1. Zrób PR z brancha (jak w ćwiczeniu 1), ale **nie merguj** go jeszcze.
2. Przełącz się na `main`, dodaj małą zmianę i wypchnij ją na GitHuba.
3. Wróć na branch PR i zrób:
   - `git fetch origin`
   - `git rebase origin/main`
   - `git push --force-with-lease`

Checkpoint: PR dalej ma “Twoje” commity, ale jego baza jest aktualna.

### Ćwiczenie 3 — Symulacja forka + upstream (na Twoich repo)

Jeśli chcesz przećwiczyć realny schemat “fork -> upstream” bez cudzych repo:

1. Utwórz repo A (źródło): np. `week5-upstream`.
2. Utwórz repo B (kopia): np. `week5-fork` (albo fork repo A, jeśli GitHub pozwoli).
3. Sklonuj repo B lokalnie.
4. Dodaj `upstream` jako drugi remote (URL repo A):
   - `git remote add upstream <URL_repo_A>`
5. Sprawdź remotes:
   - `git remote -v`
6. W repo A zrób zmianę na `main` (np. w UI GitHuba) i zacommituj.
7. Lokalnie w repo B:
   - `git fetch upstream`
   - `git switch main`
   - `git rebase upstream/main` (albo `git merge upstream/main`)
   - `git push origin main`

Checkpoint: umiesz zsynchronizować “forka” z upstream.

### Ćwiczenie 4 — Konflikt podczas rebase (kontrolowany)

1. Na `main` dodaj linię w pliku (np. `notes.txt`) i wypchnij.
2. Na branchu (np. `feature/week5-pr`) zmień _tę samą linię inaczej_.
3. Zrób `git rebase origin/main` i rozwiąż konflikt:
   - popraw plik
   - `git add -A`
   - `git rebase --continue`

Checkpoint: potrafisz przejść konflikt w rebase bez paniki.

