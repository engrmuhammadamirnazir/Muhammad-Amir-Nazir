---
type: reference
tags: [odoo, development, backporting, versions, reference]
created: 2026-03-02
---

# Odoo Development Reference

> Comprehensive development reference for Odoo 17, 18, and 19 — version differences, backporting, workflows, and conventions.
> Source: `D:\Development\CLAUDE.md` and Claude memory files

---

## Development Environment

| Version | Path | Port | Database |
|---------|------|------|----------|
| Odoo 19 (Primary) | `D:\Development\odoo19\server\` | 8072 | odoo19 |
| Odoo 18 | `D:\Development\odoo18\server\` | 8069 | (db manager) |
| Odoo 17 | `D:\Development\odoo17\server\` | 8070 | (db manager) |

**PostgreSQL:** localhost:5432, user `openpg`

**Addons path pattern:** `D:\Development\odoo{VER}\server\addons\{module_name}\`
**Publishing path:** `D:\Development\Ecosire Solutions\{Display Name}\{technical_name}\`

---

## Development Workflow (7 Steps)

```
1. Develop in Odoo 19 (primary version)
2. Test thoroughly on Odoo 19
3. Backport to v18 and v17 (automated scripts)
4. Test on all 3 versions
5. Copy to Ecosire Solutions publishing folder
6. Push to GitHub (per-module repos)
7. Publish to Odoo App Store
```

---

## Git Conventions

- Each module has its own repo: `github.com/ecosire/{module_name}`
- Branches per version: `19.0`, `18.0`, `17.0`
- `main` branch MUST always mirror the latest version branch (currently `19.0`)
- License: LGPL-3 on all modules

---

## Version Differences (Odoo 19 vs 18 vs 17)

### 1. SQL Constraints

| Version | Syntax |
|---------|--------|
| **19** | `models.Constraint(...)` |
| **18** | `_sql_constraints = [...]` |
| **17** | `_sql_constraints = [...]` |

### 2. View Tags

| Feature | 19 | 18 | 17 |
|---------|----|----|-----|
| List view tag | `<list>` | `<list>` | `<tree>` |
| view_mode value | `list` | `list` | `tree` |
| Search view | `<search>` (no attrs) | `<search>` (no attrs) | `<search>` (no attrs) |

### 3. Controller Route Types

| Version | JSON-RPC | HTTP |
|---------|----------|------|
| **19** | `type='jsonrpc'` | `type='http'` |
| **18** | `type='json'` | `type='http'` |
| **17** | `type='json'` | `type='http'` |

### 4. `_inherit` Format

| Version | Rule |
|---------|------|
| **19** | MUST be a list: `_inherit = ['res.partner']` |
| **18** | String OK: `_inherit = 'res.partner'` |
| **17** | String OK: `_inherit = 'res.partner'` |

### 5. Field `group_operator` → `aggregator`

| Version | Attribute Name |
|---------|---------------|
| **19** | `aggregator` |
| **18** | `aggregator` |
| **17** | `group_operator` |

### 6. `name_get()` Deprecation

| Version | Method |
|---------|--------|
| **19** | REMOVED — use `_compute_display_name()` |
| **18** | Deprecated — use `_compute_display_name()` |
| **17** | Still works |

### 7. `read_group()` Deprecation

| Version | Method |
|---------|--------|
| **19** | Deprecated — use `_read_group()` or `formatted_read_group()` |
| **18** | Deprecated — use `_read_group()` or `formatted_read_group()` |
| **17** | `read_group()` standard |

### 8. Kanban Card Template

| Version | Template |
|---------|----------|
| **19** | `<card>` |
| **18** | `<card>` |
| **17** | `<t t-name="kanban-box">` |

### 9. Hook Signatures

All versions: `def post_init_hook(env):` (env parameter, not cr/registry)

### 10. Import Statement Changes (Odoo 19)

```python
# Odoo 19 uses:
from odoo.tools.safe_eval import safe_eval  # Never exec()/eval()
```

### 11. Field Renames in Core Models (Odoo 19)

| Old Name | New Name |
|----------|----------|
| `tax_id` | `tax_ids` |
| `product_uom` | `product_uom_id` |
| `groups_id` | `group_ids` |
| `factor_inv` | `relative_factor` |

### 12. Security Groups Architecture (Odoo 19)

| Old | New |
|-----|-----|
| `category_id` | `privilege_id` |

### 13. Removed Models/Fields (Odoo 19)

- `res.partner.title` — removed
- `res.partner.mobile` — removed
- `_apply_ir_rules()` — removed

### 14. States Attribute

| Version | Status |
|---------|--------|
| **19** | Removed from Python AND XML |
| **18** | Removed from Python |
| **17** | Removed from XML only |

---

## Backporting Checklist: 19 → 18

```
- [ ] Change models.Constraint() → _sql_constraints = [...]
- [ ] Change type='jsonrpc' → type='json' in controllers
- [ ] Change _inherit = ['model'] → _inherit = 'model' (if single)
- [ ] Revert any Odoo 19 import changes
- [ ] Check for renamed fields (tax_ids → tax_id, etc.)
- [ ] Check privilege_id → category_id in security groups
- [ ] Test all views and controllers
```

## Backporting Checklist: 18 → 17

```
- [ ] Change <list> → <tree> in XML views
- [ ] Change view_mode='list' → view_mode='tree'
- [ ] Change aggregator → group_operator on fields
- [ ] Change <card> → <t t-name="kanban-box"> in kanban views
- [ ] Restore name_get() if needed
- [ ] Change _read_group() → read_group()
- [ ] Test all views and controllers
```

---

## Utility Scripts (40+)

Located at `D:\Development\scripts\`

### Backporting (`scripts/backporting/`)
Automated scripts to convert modules from v19 → v18 → v17

### Publishing (`scripts/publishing/`)
- `publish_all_modules_v3.py` — Push all modules to GitHub repos across version branches

### Generation (`scripts/generation/`)
Auto-generate module boilerplate, tests, and App Store listings

### Fixes (`scripts/fixes/`)
Batch fix scripts for common issues across all modules

### Testing (`scripts/testing/`)
- `validate_store_modules.py` — Validate Python, XML, CSV, manifests across all modules

---

## Odoo App Store HTML Rules

When creating `static/description/index.html`:

```
- <head>, <style>, <script> tags are STRIPPED
- Use inline styles only
- No external fonts (Google Fonts, etc.)
- No CSS variables (var(--x))
- No animations or transitions
- No @import or @font-face
- Images must use relative paths
```

---

## Module Structure Template

```
module_name/
├── __init__.py
├── __manifest__.py
├── models/
│   ├── __init__.py
│   └── model_name.py
├── views/
│   └── model_name_views.xml
├── security/
│   ├── ir.model.access.csv
│   └── security.xml
├── data/
│   └── cron_data.xml
├── wizard/
│   ├── __init__.py
│   └── wizard_name.py
├── controllers/
│   ├── __init__.py
│   └── main.py
├── tests/
│   ├── __init__.py
│   └── test_model.py
├── reports/
├── static/
│   ├── description/
│   │   ├── index.html
│   │   └── icon.png
│   └── src/
│       ├── js/
│       ├── xml/
│       └── css/
└── i18n/
```

---

## Common Server Commands

### Start Odoo Server
```bash
# Odoo 19
cd D:\Development\odoo19\server
python odoo-bin -c odoo.conf

# With specific module update
python odoo-bin -c odoo.conf -u module_name -d odoo19

# With module install
python odoo-bin -c odoo.conf -i module_name -d odoo19
```

### Run Tests
```bash
python odoo-bin -c odoo.conf -d odoo19 --test-enable --stop-after-init -i module_name
```

---

## ECOSIRE Module Branding Standard

All modules must use:
```python
{
    'author': 'ECOSIRE (PRIVATE) LIMITED',
    'company': 'ECOSIRE (PRIVATE) LIMITED',
    'maintainer': 'ECOSIRE (PRIVATE) LIMITED',
    'website': 'https://www.ecosire.com/',
    'license': 'LGPL-3',
}
```

---

## Related

- [[Ecosire - Odoo Module Library]] — Complete module inventory
- [[MOC - Odoo Expertise]] — Technical knowledge map
- [[Odoo Development Commands]] — CLI command reference
- [[Bitnami & Odoo Server Commands]] — Server management
- [[MOC - Command Cheatsheets]] — All command references
