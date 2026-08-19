# Отчёт о состоянии проекта «Catshop landing»

_Дата составления: 2026-08-19. Отчёт носит информационный характер и не изменяет код лендинга._

Все утверждения ниже проверены по фактическому состоянию git на момент
составления. Проект физически представлен **двумя рабочими копиями** в
`/workspace/wt/`, поэтому проверка выполнялась в обеих:

- `/workspace/wt/d046d43d` — рабочая копия ветки `sdai/cart-d046d43d`, в которой
  лежит этот отчёт (команды `git for-each-ref`, `git worktree list`,
  `git log --oneline`, `git ls-tree -r`, `git ls-files`, `git remote -v`,
  `git ls-remote`).
- `/workspace/wt/8b62304a` — отдельная рабочая копия ветки
  `sdai/start-project-8b62304a-v2`, в которой находится сам лендинг (те же
  команды, выполненные внутри этой копии, плюс `git log --merges`).

Обе копии имеют общий корневой коммит `807ac03 chore: initialize repository` и
общий (недоступный) `origin`, но это **разные** git-репозитории: рабочая копия
лендинга не зарегистрирована как worktree копии `d046d43d` (`git worktree list`
в `d046d43d` показывает только саму `d046d43d`), а ревизия
`sdai/start-project-8b62304a-v2` из `d046d43d` не видна (`git log
sdai/start-project-8b62304a-v2` → «unknown revision»). Поэтому ниже они описаны
раздельно.

## Краткий ответ на вопросы клиента

- **В каком состоянии проект?** Сам лендинг **готов и уже влит** в рабочей копии
  `/workspace/wt/8b62304a`: там на ветке `sdai/start-project-8b62304a-v2`
  присутствуют `index.html`, `styles.css` и ассеты (`assets/logo.svg`,
  `assets/cat-grey.svg`, `assets/cat-black.svg`, `assets/cat-white.svg`,
  `assets/cat-orange.svg`), а работа над ним смёржена через pull request #1.
  При этом в «репортной» копии `/workspace/wt/d046d43d`, где ведётся этот отчёт,
  локальная `main` всё ещё содержит только служебный init-коммит — код лендинга
  сюда не переносился.
- **Какие были pull request'ы?** Внешний реестр PR (GitHub и т.п.) из окружения
  недоступен (см. раздел «Pull requests»), поэтому PR отслеживаются по
  merge-коммитам git. Найден **pull request #1 — лендинг**: merge-коммит
  `7201074 Merge pull request #1 from barsukov-d/sdai/start-project-8b62304a-v2`
  в копии `8b62304a`; он **влит в `main`** этой копии. Отдельная ветка
  `sdai/cart-d046d43d` (этот отчёт) в `main` не влита и PR-эквивалентом с
  функциональностью не является.

## Состояние ветки `main`

`main` существует в обеих копиях, и это две разошедшиеся ветки с общим корнем
`807ac03`.

### `main` в репортной копии `/workspace/wt/d046d43d`

Содержит один коммит:

```
807ac03 chore: initialize repository   (main)
```

Отслеживаемый файл — только `README.md` (`git ls-tree -r --name-only main`).
Удалённая `origin/main` (и `origin/HEAD`) опережает эту локальную `main` на один
служебный коммит:

```
57b28ba chore: add project CLAUDE.md   (origin/main, origin/HEAD)
807ac03 chore: initialize repository   (main)
```

`57b28ba` добавляет `CLAUDE.md`; на `origin/main` отслеживаются `CLAUDE.md` и
`README.md`. Кода лендинга здесь нет ни в одной ссылке.

### `main` в копии лендинга `/workspace/wt/8b62304a`

Здесь `main` продвинута дальше и указывает на merge-коммит PR #1
(`git log main --oneline`):

```
7201074 Merge pull request #1 from barsukov-d/sdai/start-project-8b62304a-v2
303decc START PROJECT
901b7a2 feat: Developer via chat
807ac03 chore: initialize repository
```

То есть в этой копии `main` **уже включает лендинг**: `git branch --contains
7201074` показывает и `main`, и `sdai/start-project-8b62304a-v2`, а
`git rev-parse` подтверждает, что обе ветки указывают на один коммит `7201074`.

**Вывод:** «пустой заготовкой» остаётся только `main` репортной копии
`d046d43d` (единственный init-коммит, лишь `README.md`). Основная линия проекта в
копии `8b62304a` уже содержит готовый лендинг, влитый через PR #1. Ожидавшегося
контрактом коммита `seed .gitignore` на `main` ни в одной из копий нет
(`git log main` в обеих) — это отклонение факта от исходного плана; `.gitignore`
фактически добавлен вне `main` (в ветке `sdai/cart-d046d43d` и в копии
лендинга).

## Работа над лендингом

Работа над лендингом ведётся и завершена в **отдельной рабочей копии
`/workspace/wt/8b62304a` на ветке `sdai/start-project-8b62304a-v2`** (проверено
`git -C /workspace/wt/8b62304a status -sb` и `git ls-files`). Ветка указывает на
merge-коммит `7201074` (PR #1).

Фактически созданные и отслеживаемые артефакты лендинга (`git ls-files` в этой
копии):

| Файл | Назначение |
|------|------------|
| `index.html` | разметка страницы лендинга |
| `styles.css` | стили |
| `assets/logo.svg` | логотип магазина |
| `assets/cat-grey.svg` | иллюстрация кота (серый) |
| `assets/cat-black.svg` | иллюстрация кота (чёрный) |
| `assets/cat-white.svg` | иллюстрация кота (белый) |
| `assets/cat-orange.svg` | иллюстрация кота (рыжий) |

Помимо них в копии присутствуют служебные `README.md` и `.gitignore`.

**Важно:** этих файлов нет в репортной копии `/workspace/wt/d046d43d` — ни в
рабочем дереве, ни в одной из её веток/ссылок (`git ls-files`, `git ls-tree -r`
по `main`, `origin/main`, `sdai/cart-d046d43d`). Они существуют именно в
соседней копии `8b62304a`, которая для `d046d43d` является отдельным
репозиторием, а не worktree.

## Pull requests

**Внешний реестр pull request'ов из окружения недоступен.** Настроенный в обеих
копиях `origin` указывает на локальный путь
`/home/ubuntu/projects/self-developer-ai/.workspaces/1fd43e57-0a4a-47ec-b5ae-43c9507433b0/repo`,
который на момент проверки не читается: `git ls-remote` завершается ошибкой «does
not appear to be a git repository». GitHub/GitLab-API нет. Поэтому PR
отслеживаются по merge-коммитам git-истории и по веткам, отличным от `main`.

Найденные PR и ветки-кандидаты со статусом относительно соответствующей `main`:

| PR / ветка | Коммит | Статус | Что содержит |
|------------|--------|--------|--------------|
| **PR #1 — лендинг** (`sdai/start-project-8b62304a-v2`) | `7201074` (merge) | **влит в `main`** копии `8b62304a` | `index.html`, `styles.css`, `assets/logo.svg`, `assets/cat-{grey,black,white,orange}.svg` |
| `sdai/cart-d046d43d` (этот отчёт) | `b85d835` | **не влит** — опережает локальную `main` копии `d046d43d` на 2 коммита | `.gitignore` + `docs/project-status.md` (два коммита `feat: Developer via chat`: `dc830cc` добавил файлы, `b85d835` обновил отчёт) |
| `origin/main` | `57b28ba` | опережает локальную `main` копии `d046d43d` на 1 служебный коммит | добавляет `CLAUDE.md` |

`git log --merges` в копии `d046d43d` не находит merge-коммитов вовсе, в копии
`8b62304a` находит единственный — `7201074` (PR #1). Таким образом, за историю
проекта был оформлен и влит **один** pull request — лендинг (#1). Ветка
`sdai/cart-d046d43d` — незамёрженная информационная работа (этот отчёт), не
несущая функциональности.

## Итоговое резюме

Лендинг «Catshop» фактически **готов и влит** через pull request #1 в рабочей
копии `/workspace/wt/8b62304a` (ветка `sdai/start-project-8b62304a-v2`, файлы
`index.html`, `styles.css`, `assets/logo.svg`, `assets/cat-{grey,black,white,orange}.svg`).
Незамёрженной остаётся только вспомогательная ветка `sdai/cart-d046d43d` с этим
отчётом, а `main` в репортной копии `d046d43d` пока держит лишь init-коммит и код
лендинга туда не переносился. Всего оформлен один PR — #1 (лендинг), влитый в
`main` копии лендинга; внешний реестр PR из окружения недоступен, поэтому учёт
вёлся по merge-коммитам git.
