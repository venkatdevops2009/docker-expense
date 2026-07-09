# Docker Expense

A small example full-stack expense tracker split into:

- frontend: static HTML/CSS/JS served by nginx
- backend: Node.js API server (Express)
- mysql: database initialization SQL

This repository demonstrates running a simple containerized frontend + API + database. A docker-compose.yml is included to run all services together.

## Contents

- `backend/` — Node.js backend (index.js, TransactionService.js, DbConfig.js, package.json, Dockerfile)
- `frontend/` — static site (static/index.html, static/js/app.js, static/css/style.css), nginx.conf, Dockerfile
- `mysql/` — Dockerfile and `backend.sql` used to initialize the database schema
- `docker-compose.yml` — convenience compose file that wires the three services together

## What this does

- The frontend (nginx) serves the single-page UI (frontend/static/index.html) which calls the backend API to list/add/delete transactions.
- The backend exposes REST endpoints on `/transaction` and connects to MySQL using the credentials and schema defined in `mysql/backend.sql`.
- The MySQL SQL file creates the `transactions` database, a `transactions` table and a DB user `expense` with the password `ExpenseApp@1`.

## Quick start — docker compose (recommended)

The repository includes `docker-compose.yml`. By default it references prebuilt images (see the file). To start everything with the images referenced in the compose file:

```bash
# start services in the background
docker compose up -d

# view logs
docker compose logs -f

# stop and remove services
docker compose down
```

If you prefer to build the images locally from the Dockerfiles in this repo and then run with compose, run:

```bash
# build local images and start
docker compose build
docker compose up -d
```

Compose maps ports as follows by default (see `docker-compose.yml`):
- frontend → host port 80
- backend  → host port 8080

Open http://localhost in your browser to use the frontend.

## Quick start — run manually (alternative)

1. Initialize the database using the SQL file in `mysql/backend.sql` or run the mysql service from the Dockerfile in `mysql/`.

2. Backend (development):

```bash
cd backend
npm install
# start the backend (defaults to port 8080)
# set DB_HOST to the running MySQL hostname (e.g. mysql or localhost) if needed
APP_PORT=8080 node index.js
```

3. Frontend: open `frontend/static/index.html` in a browser, or serve the `frontend/static` directory via a static server or nginx.

## Environment & defaults

- DB user/password/database are created by `mysql/backend.sql`:
  - DB user: `expense`
  - DB password: `ExpenseApp@1`
  - DB name: `transactions`

- The backend's DB host field is defined in `backend/DbConfig.js`. The file contains default values for the DB user, password and DB name. The DB host is left blank in that file, so provide the correct host via environment or a modified config when running locally or via compose.

- The backend listens on `process.env.APP_PORT` or `8080` by default (see `backend/index.js`).

## Notes & recommendations

- The compose file in this repository currently references prebuilt images `piridi/expense-mysql:v1`, `piridi/expense-backend:v1`, and `piridi/expense-frontend:v1`. If you want to run the code in this repo instead of those images, either `docker compose build` (to build local images from the Dockerfiles) or edit `docker-compose.yml` to use `build:` with the local paths.

- The SQL file creates a DB user with a cleartext password. For production use, change credentials and do not commit secrets to the repo.

- The backend enables CORS in `backend/index.js` which allows the frontend to call the API when served from a different origin.

## Troubleshooting

- If the backend cannot connect to MySQL: ensure the DB host is correct (when using compose the service name `mysql` is the hostname) and the database has been initialized. The MySQL SQL script provided in `mysql/backend.sql` will create the DB and the `expense` user.

- If ports are in use, either change the host port mapping in `docker-compose.yml` or stop the conflicting service.

## Contributing

PRs welcome. If you add features, please update this README with updated run instructions.

## License

(choose a license or add one)
