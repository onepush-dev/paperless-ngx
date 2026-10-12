# Paperless-ngx — scan, OCR, index and archive all your documents. Deploy on Railway with one click.

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/new/template/TEMPLATE_CODE)

## Services
| Service | Source | Notes |
|---|---|---|
| paperless-ngx | `ghcr.io/paperless-ngx/paperless-ngx:3.3.0` | web (granian, port 8000) + Celery worker + scheduler + consumer in one container; volume `/usr/src/paperless/data` |
| Postgres | Railway Postgres | metadata DB |
| Redis | Railway Redis | Celery broker |

## Variables (paperless-ngx)
| Variable | Template value | Secret |
|---|---|---|
| PORT / PAPERLESS_PORT | `8000` | |
| PAPERLESS_URL | `https://${{RAILWAY_PUBLIC_DOMAIN}}` | |
| PAPERLESS_SECRET_KEY | `${{secret(50)}}` | yes |
| PAPERLESS_ADMIN_USER | `admin` | |
| PAPERLESS_ADMIN_PASSWORD | `${{secret(24)}}` | yes |
| PAPERLESS_ADMIN_MAIL | `admin@example.com` | |
| PAPERLESS_DBENGINE | `postgresql` | |
| PAPERLESS_DBHOST/DBPORT/DBNAME/DBUSER/DBPASS | `${{Postgres.PGHOST}}` / `PGPORT` / `PGDATABASE` / `PGUSER` / `PGPASSWORD` | DBPASS (reference) |
| PAPERLESS_REDIS | `${{Redis.REDIS_URL}}` | reference |
| PAPERLESS_DATA_DIR | `/usr/src/paperless/data` | |
| PAPERLESS_MEDIA_ROOT | `/usr/src/paperless/data/media` | |
| PAPERLESS_CONSUMPTION_DIR | `/usr/src/paperless/data/consume` | |
| PAPERLESS_TIME_ZONE | `UTC` | |
| PAPERLESS_OCR_LANGUAGE | `eng` | |
| PAPERLESS_TASK_WORKERS / THREADS_PER_WORKER / WEBSERVER_WORKERS | `1` / `1` / `1` | |
| USERMAP_UID / USERMAP_GID | `1000` / `1000` | |

## Notes
- Railway allows one volume per service, so media, data and consume dirs all live under `/usr/src/paperless/data`.
- Log in at `/accounts/login/` with `admin` and the generated `PAPERLESS_ADMIN_PASSWORD` (Variables tab). The superuser is created on first boot only.
- Healthcheck: `/accounts/login/`. First boot runs migrations (~1–2 min).
