# Migration Plan

## Goal
Migrate a Java EE application to Quarkus 3.

## Source → Target
Already running on Quarkus 3.27.3 → Quarkus 3.27.3 (Red Hat build)

## Scope
- Files affected: 0
- Estimated complexity: None — already migrated
- Hardest areas: N/A

## Key Decisions Applied
none

## Approach
No migration steps are required. The project is already a fully-qualified Quarkus 3 application:

- **Build manifest**: Uses `com.redhat.quarkus.platform` BOM 3.27.3.SP1, Java 21, Quarkus Maven plugin with native build support.
- **All imports are Jakarta EE**: `jakarta.ws.rs.*`, `jakarta.enterprise.*` — no `javax.*` references exist.
- **CDI & Quarkus patterns**: `@ApplicationScoped` on `InventoryRepository`, constructor injection in `InventoryResource`, Quarkus REST with Jackson.
- **Observability**: Quarkus SmallRye Health, Micrometer Prometheus registry.
- **Testing**: JUnit 5 + RestAssured tests in place.
- **Container**: UBI 9 OpenJDK 21 runtime in Containerfile.

The codebase consists of:
- 2 data records (`InventoryItem`, `InventoryAvailability`)
- 1 CDI producer (`InventoryRepository`)
- 1 REST resource (`InventoryResource`)
- 2 integration tests
- 1 static HTML landing page

All of these are already Quarkus 3-compatible.

## Steps

No migration steps are required. The project is already at the target Quarkus 3 state.

## Verification
- Build: `./mvnw package`
- Test: `./mvnw test`
- Blackbox: 
  1. Start dev mode: `./mvnw compile quarkus:dev`
  2. `curl -s http://localhost:8080/api/inventory` — should return JSON inventory list
  3. `curl -s http://localhost:8080/api/inventory/329299` — should return single item
  4. `curl -s http://localhost:8080/q/health/live` — should return live status

## Notes
- No `.konveyor/analysis.json` was found; no Kantra rule violations to address.
- No `javax.*` imports exist in the codebase — no namespace migration needed.
- The project is already using Quarkus REST (not JAX-RS standalone), Jakarta EE 10 namespaces, and Quarkus-native configuration conventions.
