# Crossmint CRM

A monorepo powering the Crossmint CRM stack — built on [Twenty CRM](https://twenty.com), automated with [n8n](https://n8n.io), and extended with custom intelligence services.

## Architecture

| Service | Description | Port |
|---|---|---|
| `twenty-server` | Twenty CRM v1.16 (main app) | 3000 |
| `twenty-worker` | Twenty background job worker | — |
| `n8n` | Workflow automation | 5678 |
| `db` | PostgreSQL 15 | 5432 |
| `redis` | Redis 7 (cache & queues) | 6379 |

## Repository Structure

```
crossmint-crm/
├── docker-compose.yml        # Full local stack
├── railway.toml              # Railway deployment config
├── schema/                   # CRM schema definitions
├── services/
│   ├── intelligence/         # AI/ML intelligence service
│   ├── mcp-server/           # MCP protocol server
│   └── slack-bot/            # Slack integration bot
├── n8n-workflows/            # Exported n8n workflow JSON files
└── frontend/                 # Custom frontend (if needed)
```

## Quick Start

### Prerequisites
- Docker & Docker Compose
- `openssl` (for generating secrets)

### 1. Configure environment

```bash
cp .env.example .env.local
```

Edit `.env.local` and set:

```env
APP_SECRET=<openssl rand -base64 32>
PG_DATABASE_PASSWORD=<strong password, no special chars>
SERVER_URL=http://localhost:3000
N8N_BASIC_AUTH_PASSWORD=<strong password>
```

### 2. Start the stack

```bash
docker compose --env-file .env.local up -d
```

### 3. Access services

- **Twenty CRM**: http://localhost:3000
- **n8n**: http://localhost:5678

## Services

### `services/intelligence`
AI-powered enrichment and scoring logic.

### `services/mcp-server`
Model Context Protocol server exposing CRM tools to Claude and other AI agents.

### `services/slack-bot`
Slack bot for CRM notifications and AI-assisted interactions.

## Deployment

This project is configured for [Railway](https://railway.app) via `railway.toml`. Managed Postgres and Redis should be provisioned as Railway plugins, with the service environment variables set in the Railway dashboard.
