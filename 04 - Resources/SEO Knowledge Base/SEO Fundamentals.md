---
type: resource
category: seo
tags: seo, technical-seo, resource
---

# SEO Fundamentals

> Core search engine optimization principles — the non-negotiable technical bedrock for all digital properties.

---

## Core Search Eligibility

A page must satisfy every item here before any ranking signal matters:

### Crawlability
- `robots.txt` at domain root — don't block Googlebot or Bingbot
- Crawl budget: XML sitemap for 10,000+ page sites
- Internal link graph: every page reachable within 3 clicks from homepage

### Indexation
- `<meta name="robots" content="index, follow">` on target pages
- Self-referencing canonical tags on every page
- Proper HTTP status codes (200, 301, 404/410, 503)

### URL Structure
- Hyphens as separators, all lowercase, under 75 characters recommended
- 3 directory levels max
- Primary keyword in URL slug

---

## Technical SEO

### Core Web Vitals (Ranking Signals)

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP | ≤ 2.5s | 2.5–4.0s | > 4.0s |
| INP | ≤ 200ms | 200–500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1–0.25 | > 0.25 |

### Mobile-First Indexing
- Viewport meta tag required
- Touch targets: min 48x48px
- Base font: 16px minimum
- No horizontal scroll

### Server & Protocol
- HTTP/2 minimum (HTTP/3 preferred)
- Brotli compression
- CDN for static assets
- Long cache lifetimes with content hashing

---

## On-Page HTML Elements

| Element | Requirement |
|---------|------------|
| `<title>` | 50–60 chars, keyword near front |
| Meta description | 120–160 chars with CTA |
| H1 | Exactly 1 per page |
| Heading hierarchy | H1 → H2 → H3 (no skipping) |
| Image alt text | Under 125 chars, descriptive |
| Internal links | 3–100 per page |

---

## Content Quality: E-E-A-T

- **Experience** — First-hand accounts, case studies, original research
- **Expertise** — Author bylines with credentials, certifications
- **Authoritativeness** — Quality backlinks, brand mentions, PR coverage
- **Trustworthiness** — HTTPS, privacy policy, contact info, accurate citations

---

## Related

- [[MOC - SEO & Digital Marketing]] — Full SEO knowledge map
- [[Answer Engine Optimization (AEO)]] — Featured snippets and voice search
- [[Generative Engine Optimization (GEO)]] — AI response optimization
- [[SEO_KNOWLEDGE_BASE]] — Canonical reference document
