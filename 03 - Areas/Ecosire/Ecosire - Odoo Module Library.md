---
type: reference
category: ecosire, odoo
tags: [odoo, modules, ecosire, connectors, marketplace]
updated: 2026-03-02
---

# Ecosire — Odoo Module Library

> Complete inventory of 42 Odoo modules developed by [[Ecosire Private Limited]].
> Source: `D:\Development\Ecosire Solutions\`

---

## Marketplace Connector Modules (35 Modules)

### Tier 1 — $99 (16 Platforms)

| # | Module | Platform | Versions |
|---|--------|----------|----------|
| 1 | AliExpress Store Management | AliExpress | 17, 18, 19 |
| 2 | Catch Store Management | Catch.com.au | 17, 18, 19 |
| 3 | Cdiscount Store Management | Cdiscount | 17, 18, 19 |
| 4 | Coupang Store Management | Coupang | 17, 18, 19 |
| 5 | Daraz Store Management | Daraz | 17, 18, 19 |
| 6 | Flipkart Store Management | Flipkart | 17, 18, 19 |
| 7 | Fruugo Store Management | Fruugo | 17, 18, 19 |
| 8 | Kaufland Store Management | Kaufland | 17, 18, 19 |
| 9 | Lazada Store Management | Lazada | 17, 18, 19 |
| 10 | MercadoLibre Store Management | MercadoLibre | 17, 18, 19 |
| 11 | Noon Store Management | Noon | 17, 18, 19 |
| 12 | Ozon Store Management | Ozon | 17, 18, 19 |
| 13 | Rakuten Store Management | Rakuten | 17, 18, 19 |
| 14 | SHEIN Store Management | SHEIN | 17, 18, 19 |
| 15 | Shopee Store Management | Shopee | 17, 18, 19 |
| 16 | Tokopedia Store Management | Tokopedia | 17, 18, 19 |

### Tier 2 — $149 (10 Platforms)

| # | Module | Platform | Versions |
|---|--------|----------|----------|
| 17 | Allegro Store Management | Allegro | 17, 18, 19 |
| 18 | BigCommerce Store Management | BigCommerce | 17, 18, 19 |
| 19 | Bol.com Store Management | Bol.com | 17, 18, 19 |
| 20 | Etsy Store Management | Etsy | 17, 18, 19 |
| 21 | OpenCart Store Management | OpenCart | 17, 18, 19 |
| 22 | PrestaShop Store Management | PrestaShop | 17, 18, 19 |
| 23 | Temu Store Management | Temu | 17, 18, 19 |
| 24 | TikTok Store Management | TikTok Shop | 17, 18, 19 |
| 25 | Walmart Store Management | Walmart | 17, 18, 19 |
| 26 | Wix Store Management | Wix | 17, 18, 19 |

### Tier 3 — $199 (4 Platforms)

| # | Module | Platform | Versions |
|---|--------|----------|----------|
| 27 | eBay Store Management | eBay | 17, 18, 19 |
| 28 | Magento Store Management | Magento/Adobe Commerce | 17, 18, 19 |
| 29 | Shopify Store Management | Shopify | 17, 18, 19 |
| 30 | WooCommerce Store Management | WooCommerce | 17, 18, 19 |

### Tier 4 — $399 (2 Platforms)

| # | Module | Platform | Versions |
|---|--------|----------|----------|
| 31 | Otto Store Management | Otto | 17, 18, 19 |
| 32 | Zalando Store Management | Zalando | 17, 18, 19 |

### Tier 5 — $499 (1 Platform)

| # | Module | Platform | Versions |
|---|--------|----------|----------|
| 33 | Amazon Store Management | Amazon (SP-API) | 17, 18, 19 |

---

## Internal / Infrastructure Modules (7 Modules)

| # | Module | Price | Purpose |
|---|--------|-------|---------|
| 34 | Ecosire License Client | FREE | License verification (required dependency) |
| 35 | Branding Management | Internal | License enforcement UI |
| 36 | Private Enterprise | Internal | Enterprise subscription override protection |
| 37 | SaaS Business Management | $499 | Multi-tenant SaaS management |
| 38 | Property Management System | Custom | Real estate business app |
| 39 | Chrome Extensions | Utility | Browser support tools |
| 40 | OpenClaw Integration | Internal | AI framework connector |

---

## Module Architecture

Every marketplace connector follows a standard architecture:

```
module_name/
├── __manifest__.py              # Metadata, pricing, LGPL-3 license
├── models/
│   ├── configuration.py         # API auth (SP-API / OAuth / REST)
│   ├── sync_service.py          # Bidirectional sync engine
│   └── ...
├── controllers/main.py          # Dashboard data endpoint
├── tools.py                     # REST/OAuth client implementation
├── wizard/                      # Setup wizards (connect, import, map)
├── views/                       # XML views (config, logs, dashboard)
├── security/                    # Access control (XML + CSV)
├── data/cron_data.xml           # Automated sync crons
├── tests/                       # 14 test files per module
├── reports/                     # Sync performance reports
└── static/
    ├── description/index.html   # App Store listing page
    ├── src/js/dashboard.js      # OWL dashboard (~467 lines)
    ├── src/xml/dashboard.xml    # Dashboard template (~322 lines)
    └── src/css/module.css       # Design system (~1265 lines)
```

### Dashboard Features (All Modules)
- KPI Cards: Revenue, Orders, Customers, Products, AOV, Sync Health
- Charts: Line (Sales Trend) + Doughnut (Order Status) via Chart.js
- Filters: Today, 7D, 30D, 90D, Year, Custom date range
- Activity feed, quick actions, 60s auto-refresh, dark mode, responsive

---

## Sync Capabilities

| Feature | Details |
|---------|---------|
| Products | Import/export, variants, images, pricing, inventory |
| Orders | Real-time import, line items, shipping, taxes, discounts |
| Customers | Profile sync, address management |
| Inventory | Bidirectional stock updates (<5s latency for Shopify) |
| Fulfillment | FBA, FBM, Hybrid (Amazon); platform-native for others |
| Webhooks | Real-time event-driven sync where supported |
| Crons | Scheduled automated sync for all data types |

---

## Development Workflow

### 7-Step Process
```
1. Develop in Odoo 19 (primary version, port 8072)
2. Test thoroughly on Odoo 19
3. Backport to v18 and v17 (automated scripts)
4. Test on all 3 versions (ports 8069, 8070)
5. Copy to Ecosire Solutions publishing folder
6. Push to GitHub (per-module repos)
7. Publish to Odoo App Store
```

### Git Conventions
- Each module has its own repo: `github.com/ecosire/{module_name}`
- Version branches: `19.0`, `18.0`, `17.0`
- `main` branch MUST always mirror latest version branch (currently `19.0`)
- License: LGPL-3 on all modules

### Version Management
- **Primary**: Odoo 19 (developed first)
- **Backporting**: Automated scripts convert v19 → v18 → v17
- Scripts: `D:\Development\scripts\backporting\`
- See [[Odoo Development Reference]] for full version differences and backporting checklists

### Publishing
- Automated publishing to GitHub repos per module
- Script: `D:\Development\scripts\publishing\publish_all_modules_v3.py`
- All 33 connector modules published across 19.0, 18.0, 17.0, and main branches

### Testing
- 14 test files per module (auto-generated)
- Validation: Python, XML, CSV, manifest checks
- Script: `D:\Development\scripts\testing\validate_store_modules.py`
- **9 modules lack test suites**: bigcommerce, daraz, magento, noon, opencart, prestashop, shopee, tiktok, wix

### Utility Scripts (40+)
Located at `D:\Development\scripts\`:
- `scripts/backporting/` — Automated version conversion
- `scripts/publishing/` — GitHub push across all branches
- `scripts/generation/` — Module boilerplate and test generation
- `scripts/fixes/` — Batch fix scripts for common issues
- `scripts/testing/` — Validation across all modules

### Licensing Enforcement
- All modules use active license enforcement via `ecosire_license_client`
- 2026-02-28: Major licensing fix — 237 files modified across 3 Odoo versions
- All 32 marketplace modules (+ Shopify) validated with OWL dashboards (Chart.js)

---

## Reference Modules (External)

Location: `D:\Odoo Modules\` (organized by version 12-19)

60+ reference ZIPs including: ks_dashboard_ninja, amazon_ept, shopify_ept, ebay_ept, magento2_ept, pos extensions, CRM integrations, accounting tools

---

## Google Drive Organization

| Folder | Description |
|--------|-------------|
| Odoo Modules | Curated modules by version (16-19) |
| Odoo Enterprise | Enterprise edition resources |
| Private Modules | Proprietary collection |
| Odoo Industry Data | R&D and marketing research |

---

## Related

- [[Ecosire Private Limited]] — Parent company
- [[Ecosire - Technical Stack]] — Full infrastructure
- [[Odoo Development Reference]] — Version differences, backporting, conventions
- [[PRJ - Odoo-Shopify Connector]] — Shopify connector project
- [[Marketplace Strategy]] — Expansion roadmap
- [[MOC - Odoo Expertise]] — Technical knowledge
