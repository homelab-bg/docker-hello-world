# Traefik Hello World

A simple Nginx service configured for Traefik reverse proxy integration.

## Quick Start

1. Copy environment template:
```bash
cp .env.example .env
```

2. Edit `.env` and set your domain:
```bash
NGINX_HOST=hello.example.com
```

3. Deploy with Docker Compose:
```bash
docker-compose up -d
```

## Prerequisites

- Docker & Docker Compose
- External `traefik` network (for Traefik integration)

## Configuration

See `.env.example` for available environment variables.

## Access

Service will be available at the configured `NGINX_HOST` domain with automatic TLS via Let's Encrypt.