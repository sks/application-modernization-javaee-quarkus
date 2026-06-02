# Split Plan

## BOUNDED CONTEXTS
- BC-01 Catalog: ALREADY EXTRACTED — service-catalog-quarkus-reactive/synch. Promote to standalone repo.
- BC-02 Order: IN MONOLITH — monolith-quarkus-synch. HIGH coupling. Strangler Fig + Kafka.
- BC-03 Customer/Account: IN MONOLITH — monolith-quarkus-synch. MEDIUM coupling. After Order extraction.
- BC-04 Product Search: ABSORB INTO CATALOG — clean cut, no separate service.
- BC-05 Messaging: FRONTEND ONLY — micro-frontend-messaging. LOW coupling. AsyncAPI spec needed.
- BC-06 Frontend Shell: ALREADY MODULAR — micro-frontend-shell. Promote with import-map contract.

## 12 PROPOSED REPOS (3 Phases)
Phase 1 (P0 — Low Risk):
1. service-catalog — promote strangled service (source: service-catalog-quarkus-reactive/synch)
2. service-product — absorb into service-catalog
3. infra-cicd — git subtree split (source: tekton, argocd, scripts)
4. legacy-archive — git subtree split + archive (source: monolith-websphere-855/90/liberty, frontend-dojo)

Phase 2 (P1 — Medium Risk, after Phase 0 prerequisites):
5. frontend-shell — micro-frontend-shell (add Playwright E2E first)
6. frontend-catalog — micro-frontend-catalog (after service-catalog standalone)
7. frontend-messaging — micro-frontend-messaging (AsyncAPI spec first)
8. frontend-navigator — micro-frontend-navigator (alongside shell)

Phase 3 (P2 — High Risk, all prerequisites required):
9. service-order — monolith-quarkus-synch Order classes (strangler fig)
10. service-customer — monolith-quarkus-synch Customer classes (after service-order)
11. frontend-account — micro-frontend-account (after service-customer)
12. frontend-order — micro-frontend-order (after service-order)

## PHASE 0 PREREQUISITES (must complete before any extraction)
1. Write characterization tests against monolith-quarkus-synch (Golden Master pattern, target 60% coverage)
2. Introduce org.pwte.example.order / .customer / .catalog sub-packages
3. Review mono2micro artifacts to validate proposed boundaries
4. Establish Pact broker infrastructure
