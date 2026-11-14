# mini-shop

Kleines E-Commerce-Beispiel als Microservices-Monorepo. Java 21, Spring Boot 4.1, Maven Multi-Module.

## Module

| Module | Port | Base Package |
|---|---|---|
| `product-service` | 8081 | `com.sltr.product` |
| `order-service` | 8082 | `com.sltr.order` |
| `notification-service` | 8083 | `com.sltr.notification` |

Lokale Infrastruktur über `docker-compose.yml`: PostgreSQL (5432), RabbitMQ (5672, Management UI 15672).

## Architekturregeln

- Jeder Service ist ein eigenständiges Spring-Boot-Deployable. Keine Maven-Dependency von einem Service auf einen anderen.
- Services kommunizieren nur über REST (synchron) oder RabbitMQ (asynchron), nie über gemeinsame Klassen oder eine gemeinsame Datenbank.
- Jeder Service besitzt seine Daten allein. Kein Zugriff auf Tabellen eines anderen Service.
- Versionen werden über den Spring Boot Parent im Root-`pom.xml` verwaltet. In den Service-POMs keine `<version>` für von Spring Boot verwaltete Dependencies.
- Neue Infrastruktur-Dependencies (JPA, AMQP usw.) erst hinzufügen, wenn eine Story sie braucht. Sonst scheitert der Start ohne laufende Infrastruktur.

## Coding Conventions

- Java 21: `record` für DTOs, kein Lombok.
- Constructor Injection, keine Field Injection mit `@Autowired`.
- Package-Struktur je Service unter `com.sltr.<service>`; Application-Klasse `<Service>ServiceApplication` im Base Package.
- Spring Boot 4 Starter-Namen verwenden (`spring-boot-starter-webmvc`, nicht `spring-boot-starter-web`) und zu jedem Starter den passenden `-test` Starter.
- Konfiguration in `application.yaml`, nicht `.properties`.
- Tests: Unit Tests ohne Spring Context, wo möglich. `@SpringBootTest` nur für Integrationstests. Test-Klassen enden auf `Tests`, Methodennamen beschreiben das erwartete Verhalten.
- Jeder Service behält den Test auf `GET /actuator/health`.
- Code, Bezeichner und Commit Messages auf Englisch; Stories auf Deutsch.

## Build-Befehle

```bash
./mvnw clean verify                              # alle Module bauen und testen
./mvnw -pl product-service -am verify            # ein Modul bauen und testen
./mvnw -pl product-service spring-boot:run       # einen Service starten
docker compose up -d                             # PostgreSQL und RabbitMQ starten
docker compose down                              # Infrastruktur stoppen (-v löscht auch die Daten)
curl http://localhost:8081/actuator/health       # Health Check
```

Unter Windows `mvnw.cmd` statt `./mvnw`.

## Story-Workflow

- Stories liegen unter `docs/stories/NNN-name.md`, Vorlage: `docs/stories/_template.md`.
- Vor der Umsetzung die Story lesen und einen Plan vorlegen. Nur umsetzen, was in den Acceptance Criteria steht; Out of Scope respektieren.
- Eine Story ist fertig, wenn `./mvnw clean verify` lokal grün ist und die GitHub Actions Pipeline (`.github/workflows/ci.yml`) durchläuft.
