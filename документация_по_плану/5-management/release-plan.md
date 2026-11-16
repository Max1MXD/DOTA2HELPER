# Release Plan — План випуску

## Версіонування (SemVer)

| Версія | Зміст | Дата |
|---|---|---|
| v0.1.0 | Початкова структура документації | 2026-11-16 |
| v1.0.0 | MVP: vision, SRS, архітектура, API, прототип, тест-план, план, ризики, супровід | 2027-02-08 |

## Склад v1.0.0

- `0-product/vision.md` — ідея, проблеми, аудиторія, цінність, метрики.
- `1-lifecycle/` — process, srs, mvp, traceability, sprint-plan, changes.
- `2-architecture/` — use-cases, class-diagram, architecture, modules, api, integration-plan, adr/, prototype/.
- `3-implementation/` — data-model, validation-rules.
- `4-quality/` — test-plan, test-cases, test-report, review-report, acceptance.feature, usability-test.
- `5-management/` — plan, risks, release-plan, user-guide, support, roadmap, privacy, retro.
- `README.md`, `CHANGELOG.md`, `LICENSE`, `.github/workflows/docs.yml`.

## Стратегія випуску

1. Усі документи теми лежать у `main`.
2. У `CHANGELOG.md` запис нової версії.
3. Тег `v1.0.0` на комміті.
4. GitHub Release з описом та посиланням на CHANGELOG.

## План відкату

- Якщо після релізу знайдено критичний дефект у документації:
  1. Зафіксувати дефект у GitHub Issues.
  2. Створити гілку `hotfix/<id>`.
  3. Застосувати виправлення, пройти рев'ю.
  4. Злити в `main`, підняти версію (patch: `v1.0.1`).
  5. Повторний тег + Release.

## Оточення

- GitHub репозиторій, один на команду, викладач як collaborator.
- CI: markdownlint + перевірка посилань на кожен push.
- Версія даних: 7.37d (навчальна черновик).