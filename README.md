# Traefik Hello World

A simple Nginx service with support for standalone, Traefik reverse proxy, or Swarm + Traefik reverse proxy deployments.

## Quick Start

1. Copy environment template:
```bash
cp .env.example .env
```

2. Edit `.env` and configure your domain:
```bash
NGINX_HOST=hello.example.com
NGINX_PORT=80
```

## Deployment Options

### Standalone Deployment
Direct access via host ports:
```bash
docker compose up -d
```
Access at: http://localhost:80 (or configured NGINX_PORT)

### Traefik Integration
Deploy with reverse proxy integration on a single host:
```bash
docker compose -f docker-compose.yml -f docker-compose.traefik.yml up -d
```
Access at: https://your-domain (with automatic TLS)

### Swarm + Traefik Reverse Proxy
The Swarm-mode equivalent of Traefik Integration above - not a separate,
Traefik-less option, and not layered on top of `docker-compose.traefik.yml`.
Use this when the target is a Docker Swarm cluster instead of a single host:
```bash
docker stack deploy -c docker-compose.yml -c docker-compose.swarm.yml <stack-name>
```
Access at: https://your-domain (with automatic TLS)

The two overlay files exist separately because Swarm mode deploys via
Traefik's Swarm Provider, which only reads routing labels from the service
spec (`deploy.labels`), not container labels - that's the only meaningful
difference between the two.

## Portainer Deployment

When deploying from GitHub in Portainer, the additional compose path to use
depends on the target environment:

**Standalone Docker environment** (Traefik reverse proxy):
1. **Compose path**: `docker-compose.yml`
2. **Additional paths**: Click "Add file" and enter `docker-compose.traefik.yml`

**Swarm environment** (Traefik reverse proxy):
1. **Compose path**: `docker-compose.yml`
2. **Additional paths**: Click "Add file" and enter `docker-compose.swarm.yml`

![Portainer Configuration](docs/portainer.png)

This configures Portainer to merge both files, equivalent to running the
matching `docker compose` or `docker stack deploy` command above.

## Prerequisites

### Standalone
- Docker & Docker Compose

### Traefik Integration  
- Docker & Docker Compose
- External `traefik` network
- Traefik instance with Let's Encrypt configured

### Swarm + Traefik Reverse Proxy
- Docker Swarm mode initialized
- External `traefik` overlay network already created (e.g. by a
  Traefik/Portainer stack deployed first)
- Traefik instance running with the Swarm Provider and Let's Encrypt
  configured
- A DNS record for `NGINX_HOST` pointing at the Swarm cluster

## Configuration

See `.env.example` for available environment variables.

## Access

Service will be available at the configured `NGINX_HOST` domain with automatic TLS via Let's Encrypt.