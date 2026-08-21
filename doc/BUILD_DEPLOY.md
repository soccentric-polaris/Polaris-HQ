# Polaris-HQ Build and Deployment

## Purpose and scope

Polaris-HQ is a local multi-service development stack: Python API, Python agent gateway, Vue web frontend, and Docker Compose services for PostgreSQL, MinIO, Redis, and Keycloak.

## 1. Prerequisites

- Docker and `docker compose`, Python 3 with `venv`, Node/npm, curl, and GNU Make.
- Network access for pip/npm dependencies on first install.
- Ports 5173, 8000, 8081, 50051, 5432, 9000 and Redis/Compose ports available as configured by `infra/docker-compose.yml`.

## 2. Setup

```bash
make doctor
make install                # Python editable installs and web npm install
make db-up                  # Compose services, databases, bucket, Keycloak
make migrate
make seed EMAIL=admin@example.com
```

`make install` creates `.venv`, installs `./api[dev]` and `./agent-gateway[dev]`, and runs `npm install` in `web`. `db-up` persists data in Docker volumes and waits for service readiness.

## 3. Build and compile

```bash
make build                  # cd web && npm run build
```

This builds the Vue frontend only. The Python services are installed and run from the virtual environment; no separate Python compile target is declared.

## 4. Test and lint

```bash
make test                   # API, gateway, and web suites
make test-api
make test-gateway
make test-web
make lint                   # ruff plus web ESLint/oxlint and vue-tsc
```

Tests use `TEST_DATABASE_URL` and `S3_ENDPOINT_URL`, which may require `make db-up` first. Override `DATABASE_URL`, `TEST_DATABASE_URL`, or `S3_ENDPOINT_URL` for a different environment.

## 5. Packaging and artifacts

No package archive or image build target is declared. `web/dist` is the frontend build output; `.run/*.log` and `.run/*.pid` are local runtime state. Compose service images and volumes are managed by `infra/docker-compose.yml`.

## 6. Deployment, install, and run

```bash
make run                    # start db, API, gateway, and Vite in background
make logs
make stop
make db-down               # stop Compose services and retain volumes
```

The Makefile prints LAN URLs for web/API/Keycloak and development credentials (`admin/admin`, `admin@example.com` / `devpassword`). These are development-only values. `make run` binds services broadly and uses the machine LAN IP; do not expose it to an untrusted network. No production/cloud deployment or credential provisioning is defined.

## 7. Cleanup

```bash
make stop
make db-down
make clean                  # venv, run state, node_modules, dist, caches
```

`db-down` deliberately leaves Docker volumes and database data intact; remove volumes through the Compose project only when a full data reset is intended.

## 8. Verification caveats

No hardware dependency is declared. Manually verify service health, LAN-origin login redirects, database migrations, object storage bucket access, Keycloak configuration, and frontend/API integration. Cloud deployment requires external secrets, TLS, persistence, ingress, and process supervision.