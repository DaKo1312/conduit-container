# Conduit Container

Containerized version of Conduit, a blogging platform with a Django REST API (`backend/`) and an Angular client (`frontend/`). Each part has a multi-stage Dockerfile, and `docker-compose.yaml` runs frontend, backend and a PostgreSQL database together, so the whole application starts with one command.

## Table of Contents

- [Quickstart](#quickstart)
- [Usage](#usage)

## Quickstart

Prerequisites: Docker with the Compose plugin.

```bash
git clone https://github.com/your-github-username/conduit-container.git
cd conduit-container
cp .env.example .env    # fill in the empty values
docker compose up -d
```

The frontend is then available at `http://<server-ip>:8282`.

## Usage

**Configuration** — all values are set in `.env`:

| Variable | Default | Purpose |
|----------|---------|---------|
| `DJANGO_SECRET_KEY` | – | Django secret key (required) |
| `POSTGRES_PASSWORD` | – | Database password (required) |
| `DJANGO_SUPERUSER_PASSWORD` | – | Password of the admin account |
| `DJANGO_ALLOWED_HOSTS` | `localhost` | Server IP or hostname, comma-separated |
| `FRONTEND_HOST_PORT` | `8282` | Host port of the frontend |
| `BACKEND_HOST_PORT` | `8000` | Host port of the backend |

Non-critical settings such as database name, user and internal ports are set in `docker-compose.yaml` under `environment`. After a change, apply it with `docker compose up -d`; changes to the Dockerfiles need `docker compose up -d --build`.

**Admin panel** — create the admin account once and log in at `http://<server-ip>:8282/admin/` with the email and `DJANGO_SUPERUSER_PASSWORD`:

```bash
docker compose exec backend python manage.py createsuperuser --noinput --username admin --email admin@example.com
```

**Data** — the database is stored in the named volume `postgres-data` and survives restarts and rebuilds. `docker compose down -v` deletes it.

**Logs** — view or save the logs of a container:

```bash
docker compose logs -f backend
docker logs conduit-backend-1 > conduit-backend-logs.txt
```
