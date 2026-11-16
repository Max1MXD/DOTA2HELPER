# Class Diagram — Діаграма класів

```mermaid
classDiagram
    class MatchContext {
        +String hero
        +Role role
        +List<String> enemyHeroes
        +GameStage gameStage
        +String patchVersion
        +validate(): ValidationResult
    }

    class Recommendation {
        +String id
        +RecommendationType type
        +String itemOrAbilityId
        +Priority priority
        +String reason
        +String ruleSource
        +String dataVersion
    }

    class RecommendationEngine {
        +RecommendationContext context
        +List<Recommendation> generate(MatchContext): List<Recommendation>
    }

    class DataStore {
        +Map<String, Hero> heroes
        +Map<String, Item> items
        +Map<String, Ability> abilities
        +String dataVersion
        +Date updateDate
        +String source
        +refresh(): void
    }

    class Validator {
        +ValidationResult validate(MatchContext): ValidationResult
    }

    class ExplanationGenerator {
        +String generate(Recommendation, MatchContext): String
    }

    class APIService {
        +RecommendationResponse handleRequest(MatchContext): RecommendationResponse
    }

    MatchContext --> Validator : validates
    MatchContext --> RecommendationEngine : uses
    RecommendationEngine --> DataStore : reads
    RecommendationEngine --> ExplanationGenerator : uses
    RecommendationEngine --> Recommendation : produces
    APIService --> Validator : validates
    APIService --> RecommendationEngine : delegates
    APIService --> MatchContext : receives
```

## Головні класи предметної області

| Клас | Атрибути | Методи | Призначення |
|---|---|---|---|
| `MatchContext` | hero, role, enemyHeroes, gameStage, patchVersion | validate() | Контекст запиту користувача |
| `Recommendation` | id, type, itemOrAbilityId, priority, reason, ruleSource, dataVersion | — | Результат рекомендації |
| `RecommendationEngine` | context | generate(MatchContext) | Основна логіка пошуку правил |
| `DataStore` | heroes, items, abilities, dataVersion, updateDate, source | refresh() | Версіоноване сховище ігрових даних |
| `Validator` | — | validate(MatchContext) | Перевірка вхідних даних |
| `ExplanationGenerator` | — | generate(Recommendation, MatchContext) | Формування текстових пояснень |
| `APIService` | — | handleRequest(MatchContext) | Точка входу для інтерфейсу |

## Звʼязки

- `MatchContext` — вхідні дані, не залежить від інших класів.
- `RecommendationEngine` читає `DataStore`, використовує `ExplanationGenerator`, створює `Recommendation`.
- `Validator` перевіряє `MatchContext` до передачі в `RecommendationEngine`.
- `APIService` — фасад, що зв'язує інтерфейс із внутрішніми модулями.