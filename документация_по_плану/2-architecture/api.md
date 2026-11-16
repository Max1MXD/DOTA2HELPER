# API — Контракт API

> OpenAPI 3.0 (YAML)

```yaml
openapi: 3.0.3
info:
  title: Dota 2 AI Assistant API
  version: 1.0.0
  description: API для отримання рекомендацій по предметах і здібностям

servers:
  - url: http://localhost:3000/api/v1
    description: Development server

paths:
  /recommendations:
    post:
      summary: Отримати рекомендації
      operationId: getRecommendations
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MatchContext'
      responses:
        '200':
          description: Успішна відповідь
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RecommendationResponse'
        '400':
          description: Помилка валідації
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '404':
          description: Немає даних для поєднання
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '500':
          description: Внутрішня помилка сервера
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

components:
  schemas:
    MatchContext:
      type: object
      required:
        - hero
      properties:
        hero:
          type: string
          description: Ідентифікатор героя з довідника
          example: "antimage"
        role:
          type: string
          enum: [carry, mid, offlane, support, hard_support]
          description: Роль гравця
          example: "carry"
        enemyHeroes:
          type: array
          items:
            type: string
          maxItems: 5
          uniqueItems: true
          description: Список ідентифікаторів супротивників
          example: ["axe", "legion_commander", "crystal_maiden"]
        gameStage:
          type: string
          enum: [early, mid, late]
          description: Стадія матчу
          example: "early"
        patch:
          type: string
          description: Версія даних або спеціальне значення "unverified"
          example: "7.37d"

    RecommendationResponse:
      type: object
      properties:
        recommendations:
          type: array
          items:
            $ref: '#/components/schemas/Recommendation'
        dataVersion:
          type: string
          description: Версія набору даних
          example: "7.37d"
        warnings:
          type: array
          items:
            type: string
          description: Попередження (застарілі дані, нестача правил)

    Recommendation:
      type: object
      properties:
        id:
          type: string
          description: Унікальний ідентифікатор рекомендації
        type:
          type: string
          enum: [item, ability]
          description: Тип рекомендації
        itemOrAbilityId:
          type: string
          description: Ідентифікатор предмета або здібності
          example: "black_king_bar"
        priority:
          type: integer
          minimum: 1
          maximum: 5
          description: Пріоритет (1 — найвищий)
        reason:
          type: string
          description: Коротке пояснення причини
          example: "BKB дає імунітет до магічного урону, що критично проти Axe + Legion"
        ruleSource:
          type: string
          description: Джерело правила (id правила або категорія)
        dataVersion:
          type: string
          description: Версія даних, на якій базується

    ErrorResponse:
      type: object
      properties:
        error:
          type: string
          example: "VALIDATION_ERROR"
        message:
          type: string
          example: "Поле 'hero' є обов'язковим"
        details:
          type: object
          description: Деталі помилки для полів
```

## Операції (підсумок)

| Операція | Метод | Шлях | Опис |
|---|---|---|---|
| Отримати рекомендації | POST | `/recommendations` | Основний endpoint для генерації рекомендацій |

## Версіонування

- Контракт версіонується через URL (`/api/v1/`) та поле `dataVersion` в відповіді.
- Зміни в схемі запиту/відповіді — новий endpoint `/api/v2/` або сумісне розширення.