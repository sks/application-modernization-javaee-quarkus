# AGENTS.md - Project Guidance

## Project Purpose
This project is a sample modernization journey from a Java EE monolith (WebSphere) to cloud-native microservices (Quarkus/Open Liberty) and micro-frontends (Single-SPA/Vue.js).

## Setup & Test Commands
- Build: mvn clean install
- Test: mvn test
- Docker: docker-compose up -d

## Bounded Contexts
- **Customer Orders**: Core monolith logic for order management.
- **Service Catalog**: Strangled context for item management.
- **Frontend**: Micro-frontend shell and widgets.
- **Infrastructure**: CI/CD (Tekton), GitOps (ArgoCD).

## Target Layout
- /services/service-catalog: Quarkus reactive service.
- /services/customer-orders: Open Liberty/Quarkus monolith.
- /frontend/shell: Single-SPA host.
- /frontend/widgets: Vue.js widgets.

## Conventions
- Use Kafka for asynchronous communication.
- Use Postgres for microservices; Db2 remains for legacy.

## Migration Phases
1. Strangled Catalog Service
2. Frontend Decomposition
3. Monolith Isolate & Wrap
4. Database Migration
5. Full GitOps/ArgoCD integration