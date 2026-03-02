---
type: area
tags: [ecosire, odoo, modules, proprietary]
created: 2026-03-02
updated: 2026-03-02
---

# Ecosire — Private Modules

> Proprietary Odoo modules not published to the App Store.

---

## Module Inventory

| Module | Technical Name | Purpose | Versions | Price |
|--------|---------------|---------|----------|-------|
| Private Enterprise | `private_enterprise` | Blocks Odoo phone-home (3-layer HTTP interception) | 17, 18, 19 | Internal |
| Property Management | `property_management_system` | Real estate management | 19 | $500 |
| Simplify Access Management | `simplify_access_management` | Simplified user access controls | 19 | Internal |
| SaaS Business Management | `saas_business_management` | Multi-tenant SaaS management | 19 | $499 |
| POS Kiosk Limiter | — | Limits POS kiosk attributes | 17 | Internal |
| Ecosire License Client | `ecosire_license_client` | License verification (required dependency for all modules) | 17, 18, 19 | Free |
| Branding Management | — | License enforcement UI | 17, 18, 19 | Internal |

---

## Private Enterprise — Details

Three-layer HTTP interception to block Odoo's phone-home:

1. `requests.Session.request()` monkey-patch — blocks Python HTTP calls to 40+ Odoo domains
2. `publisher_warranty.contract` AbstractModel override — intercepts warranty calls
3. `iap_tools.iap_jsonrpc` monkey-patch — intercepts IAP service calls

Deployed at: [[PRJ - Sahara Properties]] (and other client instances)

---

## Related

- [[Ecosire - Odoo Module Library]] — Public marketplace modules (42)
- [[Odoo Development Reference]] — Development workflow and conventions
- [[MOC - Odoo Expertise]] — Technical knowledge map
