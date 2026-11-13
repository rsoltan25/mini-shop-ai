Kontext

Bevor Fachlogik entsteht, braucht das Projekt ein Repo, in dem der Agent in jeder Session denselben Kontext vorfindet und jede Änderung automatisch gebaut und getestet wird.

Acceptance Criteria

Monorepo auf GitHub mit den Modulen product-service, order-service, notification-service

CLAUDE.md im Repo-Root mit Architekturregeln, Coding Conventions und Build-Befehlen

Story-Template unter docs/stories/_template.md (Kontext, Acceptance Criteria, Out of Scope)

docker compose up startet PostgreSQL und RabbitMQ lokal

GitHub Actions Pipeline führt bei jedem Push Build und Tests aus und ist grün

Jeder Service startet und liefert GET /actuator/health mit Status UP

Out of Scope

Fachliche Endpoints

Deployment
