---
type: resource
tags: [odoo, technical, python, postgresql, owl]
created: 2026-03-02
updated: 2026-03-02
---

# Odoo Technical Stack

> Core technology stack powering Odoo ERP.

---

## Backend

| Component | Technology |
|-----------|-----------|
| Language | Python 3.10+ |
| ORM | Odoo ORM (custom, built on psycopg2) |
| Database | PostgreSQL 14+ |
| WSGI | Werkzeug (HTTP server) |
| RPC | JSON-RPC 2.0 (`type='jsonrpc'` in v19, `type='json'` in v17/18) |
| Reports | wkhtmltopdf (PDF), QWeb templates (HTML) |
| Cron | Built-in `ir.cron` scheduler |

---

## Frontend

| Component | Technology |
|-----------|-----------|
| Framework | OWL 2.x (Odoo Web Library) — custom reactive JS framework |
| Templates | QWeb (XML-based) compiled to JS |
| Views | XML view definitions (form, list, kanban, search, pivot, graph) |
| Assets | Asset bundles via `__manifest__.py` |

---

## Key Version Differences

| Feature | Odoo 19 | Odoo 18 | Odoo 17 |
|---------|---------|---------|---------|
| Constraints | `models.Constraint()` | `_sql_constraints` | `_sql_constraints` |
| List view | `<list>` | `<list>` | `<tree>` |
| Route type | `type='jsonrpc'` | `type='json'` | `type='json'` |
| `_inherit` | Must be list | String OK | String OK |
| `name_get()` | Removed | Deprecated | Works |
| Kanban | `<card>` | `<card>` | `<t t-name="kanban-box">` |

See [[Odoo Development Reference]] for complete version differences and backporting checklists.

---

## Related

- [[Odoo Development Reference]] — Full dev environment, workflows, and scripts
- [[Odoo ERP Overview]] — What is Odoo
- [[Odoo Community vs Enterprise]] — Feature comparison
- [[MOC - Odoo Expertise]] — Odoo knowledge map
