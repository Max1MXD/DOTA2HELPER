# Modules — Модулі системи

## Таблиця модулів

| Модуль | Відповідальність | Операції (інтерфейс) | Готові компоненти / бібліотеки | Залежності |
|---|---|---|---|---|
| UI | Введення параметрів, відображення результату, стани екрану | `renderForm()`, `renderResults()`, `renderError()`, `renderLoading()` | React / Vue / Svelte (на вибір) | API Service |
| API Service | Маршрутизація запитів, версіонування, обробка помилок | `POST /recommendations` | Express / Fastify / Hono | Validator, Engine |
| Validator | Перевірка полів, нормалізація, політика патча | `validate(ctx)`, `normalize(ctx)` | Zod / Valibot / custom | — |
| Recommendation Engine | Пошук правил, пріоритизація, формування варіантів | `generate(ctx)` | Custom rule engine | Data Store, Explanation Generator |
| Data Store | Завантаження, кешування, версіонування ігрових даних | `load()`, `refresh()`, `getHero()`, `getItem()`, `getAbilities()` | SQLite / JSON / Redis (на вибір) | — |
| Explanation Generator | Текстові пояснення на основі правил | `generate(rec, ctx)` | i18n / template engine | — |

## Взаємодія модулів

```
UI → API Service → Validator → Recommendation Engine → Data Store
                                              ↓
                                        Explanation Generator
                                              ↓
                                            API Service → UI
```

## Готові компоненти (перевикористовуємо)

| Компонент | Версія | Призначення |
|---|---|---|
| React / Vue | latest | UI фреймворк |
| Zod / Valibot | latest | Валідація схем |
| Express / Fastify / Hono | latest | API сервер |
| Mermaid.js | latest | Діаграми в документації |
| GitHub Actions | — | CI/CD |

## Версіонування

- Data Store: версія набору даних (напр. `7.37d`), дата оновлення, джерело.
- API: SemVer (`v1.0.0`), зміни в контракті — новий endpoint або версія в URL.
- Правила: версія правила, прив'язана до версії даних.