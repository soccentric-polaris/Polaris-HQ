# Polaris-HQ — specs

Remote: `git@github.com:soccentric-polaris/Polaris-HQ.git`
Stack: FastAPI + Postgres + web UI. Design docs: `PLAN.md` (roadmap, phases, decisions), `ARCHITECTURE.md` (system design, data model, protocols, security).

Multi-tenant cloud service for managing paired Polaris devices: provisioning, updates,
backup/restore, disable, factory reset, location tracking, telemetry, faults and alarms.

## Layout

* `api/` — the service. Routers under `api/app/routers/`.
* `web/` — browser UI.
* `agent-gateway/` — the device-facing endpoint.
* alembic migrations — **single head enforced by the gate**.

## How it relates to the fleet

* **Device side lives in Polaris-Configuration.** HQ pairs with the on-device WebConfig daemon, which already aggregates system status and logs and owns the RAUC update path. The device-side HQ section (address/pairing config, token storage, outbound agent) is Polaris-Configuration's work, scoped from here.
* **Reuses the Client-to-Server connectivity *pattern*, not the protos.** Device dials out, persistent connection, presence equals connection liveness. Its own protocol, its own auth (device pairing token), not `polaris.stream`.
* **No build coupling.** No `add_subdirectory`, no proto reuse, not in the on-device image.
* **OTA artifact storage is new infra HQ owns.** Polaris-Configuration's existing `UpdateBackend`/RAUC flow will point at HQ's artifact repo URL.
