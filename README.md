# Docker & Docker Compose — Multi-Container Application

## Implementation Summary

This project containerizes a three-tier application using Docker and Docker Compose, built as part of Week 4 of the Davine Technologies DevOps Internship.

**Frontend**: A custom Docker image (`lightosita/my-custom-app:1.0`), built from `nginx:1.27-alpine` via a Dockerfile that copies a static `index.html` into the image. Built, run, tagged, and pushed to Docker Hub.

**Backend**: A lightweight Node.js HTTP service (`node:20-alpine`), started via a custom Compose `command` rather than a separate Dockerfile.

**Database**: MongoDB (`mongo:7`), with its data directory mounted to a named Docker volume (`db-data`) so data persists independently of the container's lifecycle.

**Networking**: All three services run on a custom bridge network (`app-net`), created automatically by Docker Compose. This gives each service DNS resolution by service name — verified directly by execing into the `backend` container and resolving `database` to its internal IP using `getent hosts database`, with no manual IP configuration required.

**Verification steps performed:**
- `docker compose up -d` — brought up all three services together
- `docker compose ps` — confirmed all containers reached `Up` status
- Exec'd into `backend` and confirmed name resolution to `database` over the custom network
- Confirmed `frontend` reachable in browser at `localhost:8090`, serving the custom image's content
- Confirmed a named volume (`app-data`, tested separately) persists data across full container deletion and recreation

## Usage

\`\`\`bash
docker compose up -d
docker compose ps
\`\`\`

Frontend available at `http://localhost:8090`.

## Files

- `Dockerfile` — custom nginx-alpine image definition
- `docker-compose.yml` — three-service orchestration (frontend, backend, database)
- `index.html` — static content served by the frontend
- `week4_docker_report.pdf` — written explanation of Docker concepts (images, containers, volumes, networks, Compose, Docker Hub)
