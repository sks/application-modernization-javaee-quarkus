# Monorepo Split Analysis

## BOUNDARY SCAN SUMMARY
- Primary language: Java
- Build system: Maven multi-module
- Top-level dirs: frontend-dojo, frontend-react, micro-frontend-{account,catalog,messaging,navigator,order,shell}, monolith-open-liberty, monolith-open-liberty-cloud-native, monolith-quarkus-synch, monolith-websphere-{855,90,liberty}, service-catalog-quarkus-{reactive,synch}, transformation-advisor, mono2micro, tekton, argocd, scripts
- Java package: org.pwte.example (shared across all monolith variants)
- Already strangled: service-catalog-quarkus-reactive, service-catalog-quarkus-synch
- Event bus: Kafka
- Databases: DB2 (legacy), PostgreSQL (catalog)
- Frontend: Dojo (legacy) → React → single-spa micro-frontends (Vue.js + RxJS)
- CI/CD: Tekton + ArgoCD
- mono2micro artifacts present
- test_confidence_score: 0.35
- test_frameworks: junit4, jaxrs-integration-tests
- packages_without_tests: monolith-open-liberty, monolith-open-liberty-cloud-native, monolith-quarkus-synch, service-catalog-quarkus-reactive, service-catalog-quarkus-synch, all micro-frontends, frontend-react

## COUPLING SIGNALS
- CP-01 HIGH: EJB Dependency Chain — CustomerOrderServices EJB → CustomerOrderServicesApp → CustomerOrderServicesWeb. Replicated across 5 monolith variants.
- CP-02 HIGH: Shared Package — org.pwte.example spans all monolith variants. Phase-0 sub-package refactor required.
- CP-03 HIGH: Shared DB2 Database — Customer/Order/Product tables co-exist with FK constraints.
- CP-04 LOW (positive): Kafka Seam — cleanest existing seam, reuse pattern.
- CP-05 LOW (positive): Dual DB Boundary — DB2 vs PostgreSQL already proven.
- CP-06 MEDIUM: Frontend Coupling — micro-frontend-shell is runtime dependency of all micro-frontends.
- CP-07 INFO: mono2micro artifacts present — validate before finalizing cuts.
