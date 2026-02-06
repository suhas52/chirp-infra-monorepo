# chirp-infra-monorepo
# Chirp

This repository contains a full-stack web application designed for production-style deployment. It demonstrates a modern frontend–backend architecture using Docker, reverse proxying with Nginx, a PostgreSQL database, and optional Cloudflare Tunnel exposure.

The project is structured as a portfolio-ready monorepo with the frontend and backend managed as Git submodules.

---

## Architecture Overview

- **Client**: Single-page application built with Vite and TypeScript
- **Server**: Node.js API with Prisma ORM
- **Database**: PostgreSQL 15
- **Reverse Proxy**: Nginx
- **Deployment**: Docker Compose
- **Optional Edge Access**: Cloudflare Tunnel (cloudflared)

All services are orchestrated through Docker Compose and communicate over an internal Docker network.

---

## Repository Structure

````
.
├── client/ # Frontend (submodule)
├── server/ # Backend API (submodule)
├── nginx/
│ ├── Dockerfile
│ └── nginx.conf
├── compose.yaml
````
---

## Services

### API (`api`)
- Built from the `server` submodule
- Runs on port `3000`
- Exposed internally to Nginx
- Intended to serve all `/api/*` routes

### Database (`db`)
- PostgreSQL 15
- Persistent storage via Docker volume
- Credentials provided through environment variables

### Nginx (`nginx`)
- Serves the compiled frontend
- Proxies API requests to the backend
- Acts as the single public entry point

### Cloudflared (`cloudflared`)
- Optional service
- Exposes the application securely via Cloudflare Tunnel
- Requires a valid `TUNNEL_TOKEN`

---

## Nginx Routing Behavior

- `/`  
  Serves the frontend SPA and falls back to `index.html` for client-side routing.

- `/api/*`  
  Proxied to the backend service running at `api:3000`.

---

## Environment Variables

Create a `.env` file in the root directory:

```env
POSTGRES_PASSWORD=your_database_password
TUNNEL_TOKEN=your_cloudflare_tunnel_token
```

## Running the Project
# Prerequisites

 - Docker
 - Docker Compose
 - Git
# Setup
````
git submodule update --init --recursive
docker compose up --build
````

The application will be available on:

```http://localhost```
