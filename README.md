# mini-shop-ai

Kleines E-Commerce-Beispiel als Microservices-Monorepo (Java 21, Spring Boot 4.1), entwickelt mit AI-Coding-Tools nach einem Story-basierten Workflow.

## Services

| Service | Port | Health |
|---|---|---|
| product-service | 8081 | http://localhost:8081/actuator/health |
| order-service | 8082 | http://localhost:8082/actuator/health |
| notification-service | 8083 | http://localhost:8083/actuator/health |

## Quickstart

```bash
docker compose up -d                          # PostgreSQL + RabbitMQ
./mvnw clean verify                           # Build und Tests
./mvnw -pl product-service spring-boot:run    # einen Service starten
```

RabbitMQ Management UI: http://localhost:15672 (Default-Login `minishop` / `minishop`). Eigene Werte: `.env.example` nach `.env` kopieren.

## Projektstruktur

- `product-service/`, `order-service/`, `notification-service/`: die drei Spring-Boot-Services
- `docs/stories/`: Stories, Vorlage in `_template.md`
- `CLAUDE.md`: Architekturregeln, Coding Conventions und Build-Befehle für AI-Agenten
