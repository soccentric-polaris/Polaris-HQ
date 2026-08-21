# Polaris-HQ

## Project Summary

- Polaris-HQ is the fleet-management control plane for Polaris devices. It is
  organized into a Python API, a Python agent gateway, and a TypeScript web
  application. The API package under `api/` contains application modules,
  models, routers, database setup, and tests for devices, telemetry,
  alarms, pairing tokens, OIDC, RBAC, tenant isolation, and phase-three
  behavior. The `agent-gateway/` package handles device-facing pairing and
  framed communication, with tests for pairing and stale-session sweeping.
  The `web/` package supplies the operator interface and includes device
  fault/alarm components, authentication state, and shared utilities.

- Supporting files describe Alembic migrations, PostgreSQL-oriented
  infrastructure, Keycloak configuration, protobuf definitions under
  `proto/`, and Docker Compose deployment. The root Makefile declares
  doctor, installation, database up/down, migration, seed, run, logs, test,
  lint, build, and clean workflows. The API and gateway test directories
  provide concrete automated evidence; the web package has Vitest-style
  unit/spec files. Existing project text called this work “planning,”
  but the current code and tests demonstrate implemented slices, so status
  is recorded per feature rather than inherited from that label.

- Coverage remains uneven. Device and security behaviors have focused tests,
  while full API-to-gateway-to-web integration, production identity
  configuration, deployment, and real device connectivity require manual
  validation. The migration files and infrastructure are implementation
  inputs, not evidence that a database has been provisioned successfully in
  this checkout. Use the commands in the table and the Makefile as the
  starting points for reproducible checks.

## Feature Inventory

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
|---:|---|---|---|---|
| 1 | Python API application and routing | Implemented | API tests under `api/tests/` | [ ] Manual verification required |
| 2 | Device inventory and management | Implemented | `test_devices.py` | [ ] Manual verification required |
| 3 | Telemetry ingestion/routes | Implemented | `test_telemetry_router.py` | [ ] Manual verification required |
| 4 | Faults and alarms | Implemented | `test_alarms_router.py`, web panel spec | [ ] Manual verification required |
| 5 | Pairing tokens and device pairing | Implemented | `test_pairing_tokens.py`, gateway pairing tests | [ ] Manual verification required |
| 6 | RBAC and tenant isolation | Implemented | `test_rbac.py`, `test_tenant_isolation.py` | [ ] Manual verification required |
| 7 | OIDC authentication integration | Partial | `test_oidc.py` | [ ] Manual verification required |
| 8 | Agent gateway framed transport | Implemented | `test_frames.py` | [ ] Manual verification required |
| 9 | Stale device/session sweep | Implemented | `test_stale_sweep.py` | [ ] Manual verification required |
| 10 | TypeScript web authentication and device UI | Implemented | Web auth, utility, and device specs | [ ] Manual verification required |
| 11 | PostgreSQL migrations and seed workflow | Implemented | Migration/API fixtures; live DB unverified | [ ] Manual verification required |
| 12 | Keycloak and Docker Compose infrastructure | Partial | No deployment integration test found | [ ] Manual verification required |
| 13 | Fleet operations such as OTA, backup, and factory reset | Partial | `test_phase3.py`; completeness not established | [ ] Manual verification required |

## Validation

```bash
make test
make lint
make migrate
make seed
```

Run API, gateway, and web checks separately when diagnosing a failure. Identity providers, database provisioning, browser workflows, and real device sessions remain manual.
