---
type: reference
tags: [ecosire, servers, production, aws, infrastructure]
created: 2026-03-02
---

# Ecosire — Production Servers

> All production server details, deployment commands, and architecture.

---

## Server Inventory

| # | Server | IP | Domain | Purpose | Instance | OS |
|---|--------|----|--------|---------|----------|----|
| 1 | ECOSIRE.COM | 13.223.116.181 | ecosire.com | Main SaaS Platform | EC2 t3.large | Ubuntu 24.04 |
| 2 | AI Content Engine | 3.28.95.67 | ecosire.org | AI Content Automation | EC2 t3.large | Ubuntu 24.04 |
| 3 | Sahara Properties | 158.252.2.128 | saharaproperties.ecosire.com | Client Odoo 19 | Bitnami | Bitnami Odoo 19 |

---

## Server 1: ECOSIRE.COM (13.223.116.181)

### Connection
```bash
# SSH (use PowerShell wrapper from Git Bash)
powershell.exe ssh -i "C:\path\to\ecosire.pem" ubuntu@13.223.116.181
```

### Architecture: Hybrid (Docker + PM2 + Nginx)

**Docker Services:**

| Container | Port | Purpose |
|-----------|------|---------|
| PostgreSQL 17 | 5432 | Primary database |
| Redis 7 | 6379 | Caching |
| Authentik Server | 9000 | OIDC Authentication |
| Authentik Worker | — | Background auth tasks |
| Uptime Kuma | 3005 | Status monitoring |
| Glitchtip | — | Error tracking |

**PM2 Processes:**

| App | Port | Framework |
|-----|------|-----------|
| ecosire-web | 3000 | Next.js 16 |
| ecosire-api | 3001 | NestJS 11 |
| ecosire-docs | 3002 | Docusaurus |
| muhammadamir-web | 3020 | Next.js 16 |
| odovation-web | 3010 | Next.js 16 |

**Domain → Backend Mapping:**

| Domain | Backend Port |
|--------|-------------|
| ecosire.com | 3000 |
| api.ecosire.com | 3001 |
| docs.ecosire.com | 3002 |
| odovation.com | 3010 |
| muhammadamir.com | 3020 |
| auth.ecosire.com | 9000 |
| status.ecosire.com | 3005 |

**Key Paths:**
- Deploy: `/opt/ecosire/app/`
- Infrastructure: `/opt/ecosire/infrastructure/`
- Docker env: `/opt/ecosire/infrastructure/.env`
- App env: `/opt/ecosire/app/.env.local`
- SSL: `/etc/ssl/cloudflare/ecosire.com.{pem,key}`
- Nginx: `/etc/nginx/sites-available/ecosire.conf`

**Deployment (Quick):**
```bash
powershell.exe ssh -i ecosire.pem ubuntu@13.223.116.181 "cd /opt/ecosire/app && git pull && pnpm install && pnpm build && pm2 restart all --update-env"
```

**Important Notes:**
- `NEXT_PUBLIC_*` vars inlined at build time — must rebuild on change
- `pm2 restart all --update-env` if env vars changed
- www redirect via Cloudflare (never Nginx)
- SSH user is `ubuntu@` (NOT `ecosire@`)

---

## Server 2: AI Content Engine (3.28.95.67)

### Connection
```bash
ssh -i "D:\Ecosire Customer Service\Active Clients\Ai Content Automation\aicontentgeneration.pem" ubuntu@3.28.95.67
```

### Architecture: Full Docker (11 Containers)

| Container | Image | Port |
|-----------|-------|------|
| ace-postgres | postgres:16-alpine | 5432 |
| ace-redis | redis:7-alpine | 6379 |
| ace-api | custom (FastAPI) | 8000 |
| ace-celery-worker | custom | — |
| ace-celery-beat | custom | — |
| ace-dashboard | custom (Next.js) | 3000 |
| ace-nginx | nginx:1.27-alpine | 80, 443 |
| ace-authentik | authentik:2024.12.3 | 9000 |
| ace-authentik-worker | authentik:2024.12.3 | — |
| ace-authentik-db | postgres:16-alpine | 5432 |
| ace-authentik-redis | redis:7-alpine | 6379 |

**Key Paths:**
- Project: `/opt/ace/ai-content-engine/`
- Docker dir: `/opt/ace/ai-content-engine/docker/`
- Env file: `/opt/ace/ai-content-engine/.env`
- SSL certs: `docker/nginx/ssl/` (Cloudflare Origin Certificate, valid until 2041)

**Deployment:**
```bash
ssh -i aicontentgeneration.pem ubuntu@3.28.95.67 "cd /opt/ace/ai-content-engine/docker && docker compose --env-file ../.env build && docker compose --env-file ../.env up -d"
```

**Known Gotchas:**
- Docker compose always needs `--env-file ../.env` flag
- Dashboard healthcheck: `http://127.0.0.1:3000/` (NOT localhost — IPv6 fails in Alpine)
- Cloudflare blocks default Python User-Agent with 403
- API and Authentik on different Docker networks — cannot reach each other via Docker DNS
- After container recreation: always `docker exec ace-nginx nginx -s reload`
- Celery worker concurrency: 2

---

## Server 3: Sahara Properties (158.252.2.128)

### Connection
```bash
ssh -i "sharaproperties.pem" bitnami@158.252.2.128
```

### Architecture: Bitnami Odoo 19

| Field | Value |
|-------|-------|
| Stack | Bitnami Odoo 19 |
| RAM | 8 GB |
| Disk | 60 GB |
| SSL | Let's Encrypt (expires May 29, 2026) |
| Enterprise Modules | 1,423 |

**Key Paths:**
- Custom addons: `/bitnami/odoo/addons`
- Core addons: `/opt/bitnami/odoo/lib/odoo/addons`
- Config: `/opt/bitnami/odoo/conf/odoo.conf`
- Logs: `/opt/bitnami/odoo/log/odoo-server.log`
- Venv pip: `/opt/bitnami/odoo/venv/bin/pip`

**Service Management:**
```bash
sudo /opt/bitnami/ctlscript.sh status
sudo /opt/bitnami/ctlscript.sh restart odoo
sudo /opt/bitnami/ctlscript.sh restart postgresql
```

**Important:** Always use `/opt/bitnami/odoo/venv/bin/pip` — NEVER system pip.

---

## Authentik Auth Servers

| Server | URL | Admin | Used By |
|--------|-----|-------|---------|
| ECOSIRE.COM | https://auth.ecosire.com | akadmin | Main platform |
| AI Content | https://auth.ecosire.org | akadmin | AI Content Engine |

---

## SSL Certificates

| Server | Provider | Type | Expiry |
|--------|----------|------|--------|
| ECOSIRE.COM | Cloudflare | Origin Certificate | Long-lived |
| AI Content | Cloudflare | Origin Certificate | 2041 |
| Sahara Properties | Let's Encrypt | Auto-renew | May 29, 2026 |

---

## Related

- [[ECOSIRE.COM Platform]] — Main platform details
- [[PRJ - AI Content Automation Engine]] — AI Content project
- [[PRJ - Sahara Properties]] — Sahara Properties project
- [[Server Access Credentials]] — All server passwords and keys
- [[MOC - Credentials & Access]] — Credentials index
