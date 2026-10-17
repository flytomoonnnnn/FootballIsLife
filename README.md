# Football Platform

Бэкенд футбольной платформы на Java: бронирование полей, сбор состава на матч, складчина на оплату и live-счёт матчей.

## Сервисы

| Сервис | Что делает | Статус |
|---|---|---|
| `booking-service` | Поля, слоты, бронирования, составы, оплата | в работе |
| `match-service` | Симулятор матча и live-счёт через Kafka | план (март–апрель 2027) |
| `notification-service` | Уведомления о матчах и оплате | план |

## Стек

Java 21, Spring Boot 4, PostgreSQL 16, Flyway, Docker Compose, JUnit 5, Testcontainers.
Дальше по плану: Redis, Kafka, RabbitMQ, Spring Security (JWT), Micrometer + Prometheus + Grafana, ELK.

## Запуск

```bash
docker compose up -d                 # PostgreSQL
cd booking-service && ./mvnw spring-boot:run
curl http://localhost:8080/actuator/health   # {"status":"UP", ...}
```

## Решения

Почему выбрано так, а не иначе. Это готовые ответы на вопрос «а почему не…».

- **Flyway + `ddl-auto: validate`.** Схемой управляют версионированные миграции, а Hibernate только проверяет, что сущности с ней совпадают. `update` в проде непредсказуем и не умеет откатываться.
- **`open-in-view: false`.** Сессия Hibernate не живёт до конца HTTP-запроса, поэтому lazy-загрузки вне транзакции падают сразу, а не превращаются в скрытые N+1 в контроллере.
- **Spring Boot 4.** start.spring.io больше не генерирует проекты на Boot 3, а к 2027 году новые проекты стартуют на 4.x.
