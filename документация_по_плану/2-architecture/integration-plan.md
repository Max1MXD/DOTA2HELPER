# Integration Plan — План інтеграції

## Порядок інтеграції модулів

1. **Data Store** — незалежний, завантажується першим.
2. **Validator** — залежить від схем, не залежить від Data Store.
3. **Explanation Generator** — залежить від формату рекомендацій.
4. **Recommendation Engine** — інтегрує Validator, Data Store, Explanation Generator.
5. **API Service** — обгортає Engine, Validator.
6. **UI** — інтегрується з API Service.

## Стіки модулів (Sequence Diagram)

### Основний сценарій: отримати рекомендації

```mermaid
sequenceDiagram
    participant User as Гравець
    participant UI as Інтерфейс
    participant API as API Service
    participant Val as Validator
    participant Engine as Recommendation Engine
    participant Store as Data Store
    participant Expl as Explanation Generator

    User->>UI: Заповнює форму, натискає "Показати варіанти"
    UI->>API: POST /recommendations {MatchContext}
    API->>Val: validate(MatchContext)
    Val-->>API: ValidationResult
    alt Валідація не пройдена
        API-->>UI: 400 ErrorResponse
        UI-->>User: Показати помилки поруч з полями
    else Валідація пройдена
        API->>Engine: generate(MatchContext)
        Engine->>Store: getHero, getItems, getRules
        Store-->>Engine: Data
        Engine->>Expl: generate(rec, ctx)
        Expl-->>Engine: Reason string
        Engine-->>API: List<Recommendation>
        API-->>UI: 200 RecommendationResponse
        UI-->>User: Показати картки рекомендацій
    end
```

### Сценарій: оновлення даних після патчу

```mermaid
sequenceDiagram
    participant Scheduler as Планувальник / Ручний запуск
    participant Store as Data Store
    participant Engine as Recommendation Engine
    participant Tests as Регресивні тести
    participant Changelog as CHANGELOG

    Scheduler->>Store: refresh()
    Store-->>Scheduler: Нова версія даних
    Scheduler->>Engine: Перевірити правила на нових даних
    Scheduler->>Tests: Запустити регресивні тести
    Tests-->>Scheduler: Результати
    Scheduler->>Changelog: Записати версію, дату, зміни
```

## Можливі проблеми на стиках

| Стик | Проблема | Мінімізація |
|---|---|---|
| API → Validator | Новий обовʼязковий поле | Версіонування API, дефолтні значення |
| Engine → Store | Відсутні дані для героя/предмету | Fallback на попередню версію, попередження |
| Engine → Expl | Правило без шаблону пояснення | Дефолтний шаблон "Рекомендовано на основі правил" |
| UI → API | Зміна контракту | Версіонування в URL, сумісність назад |
| Store → External source | Джерело недоступне | Кеш останньої версії, retry з backoff |

## Тестування інтеграції

- Інтеграційні тести для кожного стоку (див. `4-quality/test-cases.md`).
- Контрактні тести API (Pact або схожі).
- Smoke-тест повного шляху UI → API → Engine → Store.