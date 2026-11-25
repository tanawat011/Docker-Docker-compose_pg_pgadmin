# Docker-Docker-compose_pg_pgadmin

A Docker Compose file for a local PostgreSQL with two web tools to work
with it: Adminer and pgAdmin.

| Service | Image | On this machine |
|---|---|---|
| `db` | `postgres:18.7` | `localhost:9601` |
| `adminer` | `adminer:6.3.1` | http://localhost:9602 |
| `pgadmin` | `dpage/pgadmin4:9.19.0` | http://localhost:9603 |

Originally written in 2018 while learning Docker Compose; refreshed in
Nov 2026: pinned images, passwords out of the compose file, healthchecks,
ports on 127.0.0.1 only, pgAdmin on the right port with the database
already listed, and CI.

## Quick start

Needs Docker with Compose v2.

```sh
# Clone Project
git clone https://github.com/tanawat011/Docker-Docker-compose_pg_pgadmin.git
cd Docker-Docker-compose_pg_pgadmin

# Settings: dev-only placeholder passwords; change them if you like
cp .env.example .env

# Download images and start everything (waits until all are healthy)
docker compose up -d --wait

# Stop; add -v to also delete the data
docker compose down
```

- **psql**: `docker compose exec db psql -U app -d app`, or from this
  machine `psql -h localhost -p 9601 -U app app`
- **Adminer**: http://localhost:9602 — system PostgreSQL, server `db`,
  user / password / database from `.env`
- **pgAdmin**: http://localhost:9603 — sign in with `PGADMIN_EMAIL` /
  `PGADMIN_PASSWORD`; "Local PostgreSQL" is already in the list (it asks
  for the database password the first time)

The passwords in `.env` are for this local setup only. Every port is bound
to 127.0.0.1, so nothing is reachable from other machines.

CI starts the stack and checks each service answers.
