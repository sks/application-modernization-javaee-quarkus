# Architectural Decomposition Proposal

## Overview

This document summarizes the analysis of the `application-modernization-javaee-quarkus` monorepo and provides recommendations for decomposing it into smaller, domain-aligned repositories following modern cloud-native and microservices architectural guidance.

---

## Repository Analysis

The repository currently contains a large monorepo covering a full Java EE → Quarkus modernization journey, including multiple UI implementations, backend services, infrastructure-as-code, and migration tooling.

### Identified Bounded Contexts / Domains

| Domain | Description |
|---|---|
| **Core Backend Services** | Quarkus-based microservices (customer, order, catalog, etc.) migrated from WebSphere/Liberty |
| **Legacy UI (Dojo)** | Original Dojo-based frontend — legacy, kept for reference |
| **React Monolith UI** | React-based single-page application frontend |
| **Micro-Frontend Shell** | Single-SPA orchestration layer with Vue.js micro-frontends |
| **Database Migrations** | DB2 and PostgreSQL schema scripts and migration assets |
| **Messaging / Events** | Kafka topic configurations and event-driven integration assets |
| **CI/CD Pipelines** | Tekton pipelines, ArgoCD configurations, and Kubernetes manifests |
| **Infrastructure / IaC** | Docker Compose, Helm charts, and deployment automation |
| **Migration Tooling** | IBM Transformation Advisor and Mono2Micro analysis bundles |
| **Documentation & Media** | Architecture diagrams, modernization journey docs, and media assets |

---

## Recommended Repository Split

Following the **Strangler Fig** pattern and **Domain-Driven Design (DDD)** bounded context principles:

### 1. `app-mod-backend-services`
- Quarkus microservices (customer, order, catalog, inventory)
- Shared domain models and APIs
- OpenAPI specs

### 2. `app-mod-ui-legacy`
- Dojo-based legacy frontend (archived/reference only)
- Kept separate to avoid polluting active development

### 3. `app-mod-ui-react`
- React monolith frontend
- Component library and shared UI utilities

### 4. `app-mod-ui-microfrontends`
- Single-SPA shell application
- Vue.js micro-frontend modules
- Module federation configuration

### 5. `app-mod-database`
- DB2 DDL and migration scripts
- PostgreSQL migration scripts
- Flyway / Liquibase configurations

### 6. `app-mod-messaging`
- Kafka topic definitions and schemas
- Event contracts (Avro/JSON Schema)
- Integration test harnesses

### 7. `app-mod-gitops`
- Tekton pipeline definitions
- ArgoCD application manifests
- Kubernetes base and overlay manifests (Kustomize)

### 8. `app-mod-infrastructure`
- Docker Compose files for local development
- Helm charts
- Terraform / IaC assets

### 9. `app-mod-migration-assets`
- IBM Transformation Advisor reports
- Mono2Micro analysis outputs
- Migration runbooks and decision logs

### 10. `app-mod-docs`
- Architecture Decision Records (ADRs)
- Modernization journey documentation
- Diagrams and media

---

## Migration Strategy

1. **Phase 1 — Extract & Stabilize**: Extract each domain into its own repo while keeping the monorepo as the source of truth. Use `git subtree` or `git filter-repo` to preserve history.
2. **Phase 2 — Independent CI/CD**: Establish independent CI/CD pipelines per extracted repo. Wire up ArgoCD app-of-apps pattern.
3. **Phase 3 — Decommission Monorepo**: Once all domains are independently deployable and tested, archive the monorepo with a pointer to the successor repos.
4. **Phase 4 — Governance**: Enforce bounded context ownership via GitHub CODEOWNERS, branch protection rules, and team-level access controls.

---

## Key Architectural Principles Applied

- **Strangler Fig Pattern**: Incrementally replace monorepo slices without a big-bang rewrite.
- **Domain-Driven Design (DDD)**: Each repo maps to a single bounded context with clear ownership.
- **GitOps**: Infrastructure and deployment state managed declaratively via `app-mod-gitops`.
- **Trunk-Based Development**: Short-lived feature branches, frequent integration to main, automated gates.
- **Inner Source**: Each domain repo exposes a clear API contract, enabling contribution across teams without tight coupling.

---

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| History loss during extraction | Use `git filter-repo` to rewrite history per subdirectory |
| Cross-domain dependency drift | Define and version shared contracts (OpenAPI, Avro schemas) |
| CI/CD duplication | Centralize reusable pipeline templates in `app-mod-gitops` |
| Team coordination overhead | Adopt inner-source contribution model with clear RFC process |
| Broken local dev experience | Maintain a top-level `docker-compose` in `app-mod-infrastructure` for local orchestration |

---

## References

- [Strangler Fig Application (Martin Fowler)](https://martinfowler.com/bliki/StranglerFigApplication.html)
- [Domain-Driven Design — Eric Evans](https://domainlanguage.com/ddd/)
- [Git filter-repo](https://github.com/newren/git-filter-repo)
- [ArgoCD App of Apps Pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/)
- [Single-SPA Micro-Frontend Architecture](https://single-spa.js.org/)
- [IBM Mono2Micro](https://www.ibm.com/cloud/mono2micro)
