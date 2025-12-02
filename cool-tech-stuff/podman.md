# Podman

Podman is a modern, daemonless container engine for building, running, and managing containers with a strong focus on security and composability. Unlike Docker’s single privileged daemon, Podman runs containers directly and supports rootless operation, which reduces attack surface and simplifies permissions. For multi-service web applications this matters: Podman’s concept of a "pod" groups multiple containers into a single shared network and IPC namespace so services can reach each other over localhost and shared ports. That means a frontend, API, and sidecar can be developed and run together without the extra bridge-network configuration often needed with separate Docker containers.

## Why Podman can be simpler than Docker for webapps

- **Pods:** Group containers into one network namespace so services communicate over `localhost` without extra network wiring.
- **Daemonless & rootless:** No long‑running privileged daemon; runs without root and minimizes operational complexity and security risk.
- **Docker compatibility:** Use existing Docker images and a very similar CLI, making migration straightforward.
- **Unified lifecycle:** Manage a pod as a single unit — creating, stopping, or removing the pod controls all member containers at once.
- **Composed tooling:** Build (Buildah), image management (Skopeo), and runtime (Podman) are decoupled, enabling clearer, composable workflows.

### Quick example

1. Create a pod and publish port 8080:

```bash
podman pod create --name webpod -p 8080:80
```

2. Run an API and frontend container inside the pod:

```bash
podman run -d --pod webpod --name api my-api-image
podman run -d --pod webpod --name frontend my-frontend-image
```

Inside the pod, the `frontend` can reach the `api` over `http://localhost:<internal-port>` without additional network configuration.

## Example: Pod with Frontend, Backend (Prisma) and Postgres

This example shows minimal `Containerfile`s for a static frontend and a Node backend using Prisma, then how to create a Podman pod, run Postgres, and run the services inside the same pod so they can talk over `localhost`.

### Containerfiles

- `frontend/Containerfile` (builds static assets and serves with nginx):

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- `backend/Containerfile` (Node + Prisma):

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
RUN npx prisma generate

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma
ENV NODE_ENV=production
ENV PORT=4000
CMD ["node", "dist/index.js"]
```

Notes:

- The backend expects a `DATABASE_URL` env var (e.g. `postgresql://app:secret@localhost:5432/appdb`).
- It's common to run `npx prisma migrate deploy` as part of the start-up script to apply migrations.

### Build images

From the repo root (adjust paths as needed):

```bash
podman build -t my-frontend-image -f frontend/Containerfile frontend/
podman build -t my-backend-image -f backend/Containerfile backend/
```

### Create pod, volumes, and run services

- Create a persistent volume for Postgres data and a pod that publishes frontend port 8080 -> 80:

```bash
podman volume create pgdata
podman pod create --name app-pod -p 8080:80
```

- Start Postgres in the pod (data persisted to `pgdata`):

```bash
podman run -d --pod app-pod --name db \
  -e POSTGRES_USER=app -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=appdb \
  -v pgdata:/var/lib/postgresql/data \
  postgres:15
```

- Start the backend in the same pod. Note the `DATABASE_URL` points at `localhost` (because pod shares network namespace):

```bash
podman run -d --pod app-pod --name backend \
  -e DATABASE_URL="postgresql://app:secret@localhost:5432/appdb" \
  -e PORT=4000 \
  my-backend-image
```

- Start the frontend in the pod (frontend served on internal port 80, published to host port 8080 by the pod):

```bash
podman run -d --pod app-pod --name frontend my-frontend-image
```

### Verify and access

- Open `http://localhost:8080` in your browser to see the frontend.
- The frontend can call the backend at `http://localhost:4000` (internal pod address). The backend connects to Postgres at `postgresql://localhost:5432` from inside the pod.
- View logs: `podman logs -f frontend`, `podman logs -f backend`, `podman logs -f db`.

### Tips

- If your frontend needs to call the backend via a different host/path when running in development vs production, use environment variables at build or runtime to swap the API base URL.
- To run Prisma migrations on startup, add a small entrypoint script in `backend` that runs `npx prisma migrate deploy` before launching the server.
- If you want the backend reachable from the host (for testing API directly), publish an additional port for the backend when creating the pod (e.g., `podman pod create --name app-pod -p 8080:80 -p 4000:4000`).

Would you like me to add an example `docker-compose.yml` → `podman play kube` conversion snippet or an entrypoint script for running Prisma migrations automatically?
