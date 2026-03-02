# Ecosire AI — SEO Knowledge Base (2026)

> **Last updated**: 2026-02-25
>
> This document consolidates current best practices for search engine optimization,
> answer engine optimization, and generative/LLM optimization. It serves as the
> canonical reference for all content produced by the Ecosire AI Content Engine.
>
> **Sources**: Google Search Essentials (2026), Bing Webmaster Guidelines, OpenAI
> bot documentation, Anthropic bot documentation, Perplexity citation guidelines,
> Web Almanac 2025, Core Web Vitals documentation, Schema.org specifications,
> Google Search Quality Evaluator Guidelines (2025 edition).
[[seo instructions by grok]][[seo instructions by perplexity]][[seo Instructions by deepseek]][[]]
---

## Table of Contents

1. [Core Search Eligibility](#1-core-search-eligibility)
2. [Technical SEO](#2-technical-seo)
3. [Content Quality](#3-content-quality)
4. [On-Page Optimization](#4-on-page-optimization)
5. [Structured Data](#5-structured-data)
6. [AI Bot Governance](#6-ai-bot-governance)
7. [Answer Engine Optimization (AEO)](#7-answer-engine-optimization-aeo)
8. [Generative Engine Optimization (GEO)](#8-generative-engine-optimization-geo)
9. [LLM Optimization](#9-llm-optimization)
10. [Anti-Spam & Compliance](#10-anti-spam--compliance)
11. [Monitoring & Metrics](#11-monitoring--metrics)

---

## 1. Core Search Eligibility

A page must satisfy every item in this section before any ranking signal matters.
If a search engine cannot crawl, render, and index a page, nothing else applies.

### 1.1 Crawlability

- **robots.txt** must be present at the domain root (`/robots.txt`). It must not
  block Googlebot or Bingbot from critical content paths. Use `Allow` directives
  for key sections and `Disallow` only for admin, staging, and duplicate paths.
- **Crawl budget**: For sites with more than 10,000 pages, submit an XML sitemap
  and avoid soft 404s, redirect chains (3+ hops), and parameter-heavy URLs that
  waste crawl budget.
- **Internal link graph**: Every indexable page must be reachable from the homepage
  within 3 clicks. Orphan pages (no inbound internal links) will not be
  discovered reliably.

### 1.2 Indexation

- Use `<meta name="robots" content="index, follow">` on pages that should appear
  in search results. Use `noindex` only for thin, duplicate, or utility pages.
- **Canonical tags**: Every page must include `<link rel="canonical" href="...">`.
  Self-referencing canonicals are required even on unique pages. Canonical URLs
  must be absolute, use HTTPS, and match the preferred URL version (with or
  without trailing slash — be consistent).
- **Hreflang**: For multilingual or multi-region sites, include `<link
  rel="alternate" hreflang="xx-YY" href="...">` tags in the `<head>` or XML
  sitemap. Every hreflang set must include a self-referencing entry and an
  `x-default` fallback.
- **HTTP status codes**: Indexable pages return `200`. Permanently moved pages
  return `301`. Temporarily unavailable pages return `503` with a `Retry-After`
  header. Never serve soft 404s (200 status on empty/error pages).

### 1.3 XML Sitemaps

- Maximum **50,000 URLs** per sitemap file.
- Maximum **50 MB** uncompressed per sitemap file.
- Use a sitemap index file when multiple sitemaps are needed.
- Include `<lastmod>` with accurate ISO 8601 dates (not the current date on
  every build).
- Submit sitemaps via Google Search Console and Bing Webmaster Tools.
- Reference the sitemap in `robots.txt`: `Sitemap: https://example.com/sitemap.xml`.

### 1.4 URL Structure

| Rule                   | Guideline                                                    |
|------------------------|--------------------------------------------------------------|
| Separator              | Use hyphens (`-`), never underscores (`_`)                   |
| Length                  | Under **2,048 characters** total (under 75 recommended)      |
| Case                   | All lowercase                                                |
| Characters             | ASCII alphanumeric + hyphens only; no spaces, special chars  |
| Depth                  | 3 directory levels max (`/section/category/page`)            |
| Keywords               | Include primary keyword in the URL slug                      |
| Parameters             | Avoid query parameters for indexable content                 |
| Trailing slash         | Be consistent site-wide (pick one convention and enforce it) |

### 1.5 HTTPS

HTTPS is **mandatory** for all indexable pages. Google confirms HTTPS as a
ranking signal. Requirements:

- Valid TLS 1.2+ certificate (TLS 1.3 preferred).
- No mixed content (all sub-resources loaded over HTTPS).
- HSTS header enabled: `Strict-Transport-Security: max-age=31536000; includeSubDomains`.
- 301 redirect from HTTP to HTTPS.

### 1.6 Rendering

- **Server-Side Rendering (SSR)** is preferred for SEO-critical pages. Search
  engines can execute JavaScript, but SSR removes the dependency on JS execution
  and reduces time-to-index.
- **Client-Side Rendering (CSR)** risks delayed or incomplete indexation.
  Critical content must not depend solely on client-side JavaScript.
- **Dynamic rendering** (serving pre-rendered HTML to bots) is an acceptable
  fallback but not a long-term solution.
- Use the **URL Inspection Tool** in Google Search Console to verify what
  Googlebot sees after rendering.

---

## 2. Technical SEO

### 2.1 Core Web Vitals

Core Web Vitals are confirmed ranking signals. All pages must meet the "Good"
threshold for each metric.

| Metric | Full Name                    | Good      | Needs Improvement | Poor     |
|--------|------------------------------|-----------|-------------------|----------|
| LCP    | Largest Contentful Paint     | ≤ 2.5 s   | 2.5 – 4.0 s       | > 4.0 s  |
| INP    | Interaction to Next Paint    | ≤ 200 ms  | 200 – 500 ms      | > 500 ms |
| CLS    | Cumulative Layout Shift      | ≤ 0.1     | 0.1 – 0.25        | > 0.25   |
| FCP    | First Contentful Paint       | ≤ 1.8 s   | 1.8 – 3.0 s       | > 3.0 s  |
| TTFB   | Time to First Byte           | ≤ 800 ms  | 800 – 1800 ms     | > 1800 ms|

**LCP optimization**: Preload hero images, use `fetchpriority="high"` on the LCP
element, serve images in WebP/AVIF, implement a CDN, minimize render-blocking CSS.

**INP optimization**: Break long tasks (>50 ms) into smaller chunks, use
`requestIdleCallback` for non-critical work, minimize main-thread JavaScript,
use `content-visibility: auto` for off-screen content.

**CLS optimization**: Set explicit `width` and `height` on images/videos, reserve
space for ads and embeds, avoid injecting content above the fold after load,
use CSS `contain` for dynamic elements.

### 2.2 Mobile-First Indexing

Google indexes and ranks based on the mobile version of a page. Requirements:

- **Viewport meta tag**: `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- **Touch targets**: Minimum **48 x 48 CSS pixels** with at least 8px spacing
  between adjacent targets.
- **Minimum font size**: **16px** base font; no text below 12px.
- **No horizontal scroll**: Content must fit within the viewport without
  horizontal scrolling.
- **Same content on mobile and desktop**: Do not hide critical content behind
  tabs, accordions, or "read more" toggles on mobile if it is visible on desktop.
- **Responsive images**: Use `srcset` and `sizes` attributes; serve appropriately
  sized images for each breakpoint.

### 2.3 Server & Protocol

- **HTTP/2** minimum; **HTTP/3** (QUIC) preferred for performance.
- **Content Security Policy (CSP)** header to prevent XSS.
- **No mixed content**: All resources (images, scripts, fonts, iframes) must load
  over HTTPS.
- **Compression**: Enable Brotli (preferred) or Gzip for text resources.
- **Caching**: Use `Cache-Control` headers with appropriate `max-age` values.
  Static assets should have long cache lifetimes with content-hashed filenames.
- **CDN**: Serve static assets from a CDN to reduce TTFB globally.

### 2.4 On-Page HTML Elements

| Element            | Requirement                                              |
|--------------------|----------------------------------------------------------|
| `<title>`          | 50–60 characters; primary keyword near the front         |
| `<meta name="description">` | 120–160 characters; include CTA and keyword   |
| `<h1>`             | Exactly **1** per page; matches the page topic           |
| Heading hierarchy  | H1 → H2 → H3 (no skipping levels)                       |
| Image `alt` text   | Under **125 characters**; descriptive, not keyword-stuffed|
| Internal links     | **3–100** per page; use descriptive anchor text          |
| External links     | Link to authoritative sources; use `rel="nofollow"` for  paid/untrusted links |
| Language           | `<html lang="en">` (or appropriate language code)        |

---

## 3. Content Quality

### 3.1 E-E-A-T Framework

E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) is Google's
quality framework from the Search Quality Evaluator Guidelines. It is not a
direct ranking factor but informs the algorithms that determine content quality.

#### 3.1.1 Experience

Demonstrate first-hand, lived experience with the topic.

- Include **first-person accounts** ("In our testing...", "When we implemented...").
- Provide **case studies** with specific metrics, timelines, and outcomes.
- Share **original research**: proprietary data, surveys, experiments, or audits
  that cannot be found elsewhere.
- Include **screenshots, photos, or videos** from actual experience as evidence.
- Date-stamp experiential content so readers know when the experience occurred.

#### 3.1.2 Expertise

Prove the author possesses deep knowledge in the subject area.

- Every content piece must have an **author byline** linked to an author bio page.
- Author bio pages must include **credentials**: degrees, certifications, years
  of experience, employer/affiliation, and links to published work.
- Use **Person schema** on author bio pages with `sameAs` linking to LinkedIn,
  professional profiles, and other authoritative presences.
- For technical topics, the author should hold relevant certifications or
  demonstrated professional experience.
- Expert review: YMYL content should be reviewed by a qualified professional
  (e.g., a licensed financial advisor for financial content).

#### 3.1.3 Authoritativeness

Establish the site and its authors as recognized authorities.

- Build a portfolio of **high-quality backlinks** from topically relevant,
  authoritative domains.
- Earn **brand mentions** in industry publications, news outlets, and
  professional forums.
- Seek **PR coverage**: press releases, interviews, expert commentary in
  third-party articles.
- Maintain consistent **entity presence** across the web: Wikipedia (if notable),
  Crunchbase, industry directories, social profiles.
- Publish on the same topic cluster consistently to build topical authority.

#### 3.1.4 Trustworthiness

Trustworthiness is the most important E-E-A-T dimension.

- **HTTPS** on all pages (see Section 1.5).
- **Privacy policy** accessible from every page (footer link).
- **Contact information**: physical address, phone number, and/or email clearly
  visible (About or Contact page linked from main navigation).
- **Editorial policy**: disclose content creation process, review standards, and
  correction procedures.
- **Transparency**: label sponsored content, affiliate links, and AI-assisted
  content. Disclose conflicts of interest.
- **Accurate citations**: link claims to primary sources; do not cite outdated or
  retracted sources.

### 3.2 YMYL (Your Money or Your Life)

Pages that could impact a person's health, financial stability, safety, or
well-being are classified as YMYL. These pages require the **highest level of
E-E-A-T**:

- Content must be written or reviewed by qualified professionals.
- All claims must be sourced to authoritative references.
- The site must demonstrate a strong reputation in the topic area.
- Factual accuracy must be verifiable.

YMYL categories include: medical/health, financial advice, legal information,
news/current events, civic information, and safety-related content.

### 3.3 Content Freshness

- **Quarterly updates**: Review and update all evergreen content at least once
  every 90 days.
- **"Last updated" date**: Display a visible "Last updated: YYYY-MM-DD" on every
  content page. Use `dateModified` in Article schema.
- **Staleness threshold**: Content not updated within **90 days** should be
  flagged for review. Content not updated within 180 days should be refreshed,
  consolidated, or removed.
- **Freshness signals**: Update statistics, examples, screenshots, and external
  links during each review cycle.
- **Historical content**: If content is intentionally archival, label it clearly
  ("Originally published: YYYY-MM-DD") and note that information may be outdated.

### 3.4 Content Depth & Quality

- **Minimum word count**: 800 words for informational/educational content. There
  is no maximum, but every word must add value.
- **No AI filler**: Content generated by AI must be reviewed, edited, and enriched
  by humans. Pure AI-generated content without editorial oversight is considered
  low quality by Google and is a spam policy violation if published at scale to
  manipulate rankings.
- **Comprehensive coverage**: Address the full scope of the topic, including
  related questions, edge cases, and counterarguments.
- **Unique value**: Every page must offer something not available on competing
  pages — original data, unique perspective, better organization, or deeper
  analysis.
- **Readability**: Target a Flesch-Kincaid grade level appropriate to the
  audience (8th–10th grade for general audiences).

---

## 4. On-Page Optimization

### 4.1 Keyword Placement

| Location              | Requirement                                                |
|-----------------------|------------------------------------------------------------|
| Page title (`<title>`)| Primary keyword within the first 60 characters             |
| H1 heading            | Primary keyword included naturally                         |
| First paragraph       | Primary keyword within the first 100 words                 |
| Meta description      | Primary keyword included with a compelling CTA             |
| URL slug              | Primary keyword in the URL path                            |
| Image alt text        | Primary or semantic keyword where contextually appropriate |

- **Keyword density**: Target **1–2%** for the primary keyword. Avoid
  over-optimization; write for humans first.
- **Semantic variations**: Use synonyms, related terms, and natural language
  variations throughout the content. Search engines understand semantic
  relationships.
- **Long-tail keywords**: Target phrases of **15–23 words** for conversational
  and voice search queries. These often have lower competition and higher
  conversion intent.
- **Cannibalization prevention**: Each keyword target should map to exactly one
  URL. If multiple pages target the same keyword, consolidate them or
  differentiate their intent (informational vs. transactional vs. navigational).

### 4.2 Internal Linking

#### Hub-and-Spoke Model

Organize content into topic clusters:

- **Hub page** (pillar): Comprehensive overview of a broad topic.
- **Spoke pages**: Deep dives into specific subtopics within the cluster.
- Each spoke links back to the hub, and the hub links to all spokes.
- Spokes link to related spokes within the same cluster.

#### Internal Linking Rules

| Rule                          | Guideline                                         |
|-------------------------------|---------------------------------------------------|
| Click depth from homepage     | Maximum **3 clicks** to any indexable page         |
| Minimum links per page        | At least **3** internal links per content page     |
| Maximum links per page        | Approximately **100** (including navigation)       |
| Exact-match anchor text       | Less than **30%** of internal anchors              |
| Branded + natural anchor text | Greater than **50%** of internal anchors           |
| Link placement                | In-body contextual links carry more weight than    navigation/footer links |
| Broken links                  | Zero tolerance — check monthly                     |

#### Anchor Text Distribution

- **Exact match** (target keyword as anchor): < 30%
- **Partial match** (keyword + other words): 20–30%
- **Branded** (brand name or domain): 20–30%
- **Natural/generic** ("click here", "read more", "this article"): 10–20%
- **Naked URL** (https://example.com/page): < 5%

### 4.3 Content Structure

- **Inverted pyramid**: Lead with the most important information (answer the
  query immediately), then provide supporting details, then background context.
- **Lists and tables**: Use bulleted/numbered lists and comparison tables to
  improve scannability and increase chances of featured snippet selection.
- **Short paragraphs**: 2–4 sentences per paragraph. Walls of text reduce
  engagement and increase bounce rate.
- **Table of Contents (TOC)**: Include a linked TOC for content exceeding
  **1,500 words**. Use anchor links to each H2 section.
- **Question headings**: At least **30%** of H2/H3 headings should be phrased as
  questions (matching how users search). Example: "How does X work?" rather
  than "X functionality".
- **Visual breaks**: Include an image, chart, table, or callout box at least
  every 300–500 words to maintain engagement.

---

## 5. Structured Data

### 5.1 Priority Tiers

Structured data helps search engines understand page content and enables rich
results. Use **JSON-LD** format exclusively (not Microdata or RDFa).

| Tier          | Schema Types                                                | When to Use                                |
|---------------|-------------------------------------------------------------|--------------------------------------------|
| **Critical**  | Article, FAQPage, HowTo, BreadcrumbList                    | Every applicable content page              |
| **Important** | Organization, Person, Product, LocalBusiness                | Site-wide (Org), author pages, product/service pages, local pages |
| **Recommended** | WebPage, WebSite, ImageObject, VideoObject                | Homepage (WebSite), all pages (WebPage), media-heavy pages |
| **Specialized** | Recipe, Event, Course, SoftwareApplication                | Only when the content type exactly matches |

### 5.2 Implementation Rules

1. **Every page** must include at least **1 schema type** in JSON-LD format.
2. **Content pages** (blog posts, articles, guides) must include **Article**
   schema with: `headline`, `author` (Person with `url`), `datePublished`,
   `dateModified`, `image`, `publisher` (Organization).
3. **BreadcrumbList** must appear on every page **except** the homepage.
4. **FAQPage** schema must be added to any page containing a FAQ section
   (question-and-answer pairs). Do not use FAQPage on pages that are not
   genuinely FAQ content.
5. **HowTo** schema must be added to any content structured as step-by-step
   instructions, including: `name`, `step` (array of HowToStep with `text`),
   `totalTime`, `estimatedCost` (if applicable).
6. **Speakable** schema should be added to news articles and key content pages
   to indicate sections suitable for voice assistant read-aloud. Mark the
   introduction and key summary paragraphs as speakable using CSS selectors.
7. **Validation**: Every page with structured data must pass the
   [Rich Results Test](https://search.google.com/test/rich-results) with zero
   errors and zero warnings before publication.
8. **No misleading schema**: Schema must accurately represent visible page
   content. Adding schema for content not present on the page is a spam
   violation.

### 5.3 Schema Templates

#### Article (Blog Post)

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Page Title (max 110 chars)",
  "author": {
    "@type": "Person",
    "name": "Author Name",
    "url": "https://example.com/about/author-name"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Organization Name",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png"
    }
  },
  "datePublished": "2026-01-15T08:00:00+00:00",
  "dateModified": "2026-02-20T10:30:00+00:00",
  "image": "https://example.com/images/hero.jpg",
  "description": "Meta description text here."
}
```

#### BreadcrumbList

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://example.com/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Category",
      "item": "https://example.com/category/"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Page Title"
    }
  ]
}
```

#### FAQPage

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is the question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "The answer to the question."
      }
    }
  ]
}
```

---

## 6. AI Bot Governance

### 6.1 Bot Classification

The AI ecosystem now includes both **training bots** (that ingest content to
improve models) and **citation bots** (that retrieve content to answer user
queries with attribution). The recommended strategy is to block training bots
while allowing citation bots.

| Bot Name             | Operator     | Purpose           | Recommendation | robots.txt Directive         |
|----------------------|-------------|-------------------|----------------|-------------------------------|
| GPTBot               | OpenAI      | Model training    | **BLOCK**      | `User-agent: GPTBot`          |
| ClaudeBot            | Anthropic   | Model training    | **BLOCK**      | `User-agent: ClaudeBot`       |
| Google-Extended      | Google      | AI training       | **BLOCK**      | `User-agent: Google-Extended` |
| CCBot                | Common Crawl| Dataset building  | **BLOCK**      | `User-agent: CCBot`           |
| Bytespider           | ByteDance   | Model training    | **BLOCK**      | `User-agent: Bytespider`      |
| anthropic-ai         | Anthropic   | Model training    | **BLOCK**      | `User-agent: anthropic-ai`    |
| OAI-SearchBot        | OpenAI      | Search citations  | **ALLOW**      | `User-agent: OAI-SearchBot`   |
| ChatGPT-User         | OpenAI      | Live browsing     | **ALLOW**      | `User-agent: ChatGPT-User`    |
| PerplexityBot        | Perplexity  | Search citations  | **ALLOW**      | `User-agent: PerplexityBot`   |
| Applebot-Extended    | Apple       | Siri/AI features  | **ALLOW**      | `User-agent: Applebot-Extended`|
| Googlebot            | Google      | Search indexing   | **ALLOW**      | `User-agent: Googlebot`       |
| Bingbot              | Microsoft   | Search indexing   | **ALLOW**      | `User-agent: Bingbot`         |

### 6.2 robots.txt Template

```
# Search engine crawlers — ALLOW
User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

# AI citation/search bots — ALLOW
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Applebot-Extended
Allow: /

# AI training bots — BLOCK
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: anthropic-ai
Disallow: /

Sitemap: https://example.com/sitemap.xml
```

### 6.3 Meta Tag Controls

For granular page-level control, use these meta tags in the `<head>`:

| Meta Tag                                        | Effect                                      |
|-------------------------------------------------|---------------------------------------------|
| `<meta name="robots" content="noai">`           | Block all AI training on this page          |
| `<meta name="robots" content="noimageai">`      | Block AI training on images on this page    |
| `<meta name="robots" content="max-snippet:-1">` | Allow unlimited snippet length (for citation bots) |
| `<meta name="robots" content="max-image-preview:large">` | Allow large image previews         |

### 6.4 /llms.txt

Place an `/llms.txt` file at the domain root to provide structured information
for LLMs about the site. This is an emerging standard for machine-readable site
descriptions.

```
# Site Name
> Brief description of the site and its purpose.

## Topics covered
- Topic 1
- Topic 2

## Key pages
- [Page Title](https://example.com/page): Description
- [Page Title](https://example.com/other-page): Description

## Contact
- Email: contact@example.com
- Website: https://example.com
```

---

## 7. Answer Engine Optimization (AEO)

AEO focuses on winning **featured snippets**, **People Also Ask** boxes, and
**voice search** results — positions where search engines extract and display
a direct answer from your content.

### 7.1 Featured Snippet Targeting

- Provide a **direct answer** of **40–60 words** within the first two scrolls of
  the page (above the fold on desktop, within first ~600px on mobile).
- Structure the answer as: **Question as H2 or H3 heading**, followed immediately
  by a concise paragraph answer.
- The answer paragraph should be self-contained — it must make sense when
  extracted without surrounding context.
- Include the target question **verbatim** in the heading (match user search
  query phrasing).

### 7.2 Snippet Format Types

| Format       | Structure                                         | Best For                          |
|--------------|---------------------------------------------------|-----------------------------------|
| Definition   | 40–60 word paragraph directly answering "What is" | Definitions, concepts, overviews  |
| List         | Ordered or unordered list with **3–8 items**       | Steps, tips, types, reasons       |
| Table        | HTML `<table>` with header row and 3+ data rows   | Comparisons, specifications, data |
| Steps        | Numbered list with HowTo schema markup            | How-to procedures, tutorials      |

**Definition snippet pattern**:

```html
<h2>What is [Topic]?</h2>
<p>[Topic] is [40-60 word definition that directly answers the question,
includes the key term in the first sentence, and provides enough context
to be a standalone answer.]</p>
```

**List snippet pattern**:

```html
<h2>What are the best [things]?</h2>
<ol>
  <li><strong>Item 1</strong> — Brief description</li>
  <li><strong>Item 2</strong> — Brief description</li>
  <!-- 3-8 items total -->
</ol>
```

### 7.3 Voice Search Optimization

Voice search queries are typically longer and more conversational than typed
queries.

- Write answers in a **conversational tone** that sounds natural when read aloud.
- Target **question phrases**: who, what, where, when, why, how.
- Keep spoken answers under **30 words** (voice assistants prefer concise
  responses).
- Optimize for **local queries**: "near me", "[service] in [city]",
  "[business type] open now".
- Use **Speakable** schema (Section 5.2) to identify content suitable for
  text-to-speech.
- Ensure the page loads fast (voice search devices prioritize fast-loading pages).

### 7.4 FAQPage Schema for AEO

- Add **FAQPage** schema to every page containing 2 or more Q&A pairs.
- Each question in the schema must appear verbatim in the visible page content.
- Answers in the schema must match the visible answer (no hidden content).
- Limit to **5–10 questions** per FAQPage schema to maintain quality signals.
- Questions should reflect actual user search queries (use People Also Ask
  data, Google Search Console queries, and keyword research tools).

---

## 8. Generative Engine Optimization (GEO)

GEO optimizes content for inclusion in AI-generated responses (Google AI
Overviews, ChatGPT search, Perplexity answers, Bing Copilot). When an AI
system generates an answer, it selects and synthesizes information from
multiple sources — GEO ensures your content is selected and cited.

### 8.1 Five Principles of GEO

1. **SEO fundamentals first**: GEO does not replace SEO. Pages must rank in
   traditional search before AI systems will consider them for citations.
2. **Entity clarity**: Make it unambiguous what your content is about. Use
   precise entity names, define terms, and link to authoritative references.
3. **Content extractability**: Structure content so AI systems can extract
   discrete facts, statistics, and claims without needing full-page context.
4. **Cross-platform presence**: AI systems cross-reference multiple sources.
   Maintain consistent information about your brand/topic across your site,
   social media, Wikipedia (if notable), and industry directories.
5. **AI metrics tracking**: Monitor new metrics like citation rate, Share of
   Model, and AI Overview inclusion alongside traditional SEO metrics.

### 8.2 Content Methods and Impact

| Method                    | Estimated Visibility Boost | Implementation                                  |
|---------------------------|---------------------------|--------------------------------------------------|
| Statistics & data         | **+30–40%**               | Min 2 statistics per section, cite sources        |
| Citations & references    | **+40%**                  | Min 1 citation per factual claim, link to source  |
| Authoritative language    | **+15–20%**               | Confident tone, definitive statements, expert phrasing |
| Fluency & readability     | **+10–15%**               | Clear prose, logical flow, no jargon without definition |
| Technical terminology     | **+5–10%**                | Use precise industry terms alongside plain-language explanations |
| Keyword stuffing          | **NEGATIVE**              | Degrades AI citation probability; triggers spam filters |

### 8.3 GEO Content Rules

- Include at least **2 statistics or data points per major section**, each with a
  source citation (author, publication, year).
- Include at least **1 citation per factual claim** — link to the primary source,
  not a secondary aggregator.
- Write **self-contained paragraphs**: each paragraph should express a complete
  idea that can be extracted and cited independently by an AI system.
- Use **entity-first sentences**: begin key paragraphs with the subject entity
  ("React Server Components allow..." rather than "When building modern web
  applications, one approach is...").
- Avoid filler phrases, hedging language, and unnecessary qualifiers.
  AI systems prefer confident, direct statements backed by evidence.

### 8.4 Share of Model Metric

**Share of Model** measures how often your brand or content is cited in AI-generated
responses relative to competitors. Track this by:

- Querying AI systems (ChatGPT, Perplexity, Google AI Overviews) with your
  target keywords and recording citation frequency.
- Comparing citation counts against competitor domains.
- Monitoring changes over time as you implement GEO strategies.

---

## 9. LLM Optimization

LLM Optimization ensures that large language models accurately represent your
brand, content, and expertise in their outputs — whether in training data,
retrieval-augmented generation, or real-time search.

### 9.1 Entity Consistency

- Use the **exact same name, description, and key attributes** for your brand,
  products, and people across every platform: website, social media, business
  listings, Wikipedia, press coverage.
- Maintain a canonical entity reference (e.g., your About page or organization
  schema) that all other mentions can be traced back to.
- Inconsistent entity information (different names, conflicting descriptions)
  reduces model confidence and citation likelihood.

### 9.2 Semantic Footprint

- Build content around **topics and entities**, not just keywords. LLMs
  understand semantic relationships and topic coverage.
- Create comprehensive **topic clusters** that demonstrate authority across a
  subject area (see Section 4.2 hub-and-spoke model).
- Cover the topic from multiple angles: definitions, comparisons, tutorials,
  case studies, opinion/analysis, data, and FAQs.
- The goal is to be the most comprehensive and authoritative source on your
  core topics so that LLMs naturally reference your content.

### 9.3 /llms.txt File

Maintain an `/llms.txt` file at the domain root (see Section 6.4) that provides:

- A structured overview of the site's purpose and key topics.
- Links to the most important pages with brief descriptions.
- Information about the organization and its expertise.
- This file helps LLMs understand site structure and content hierarchy during
  retrieval-augmented generation.

### 9.4 Comparison Content

LLMs frequently generate comparison-based answers. Create content that:

- Includes **comparison tables** (Feature A vs. Feature B) with clear,
  factual data.
- Covers "X vs. Y" queries for your product/service category.
- Provides balanced, accurate comparisons (not just promotional content).
- Uses table format for easy extraction by AI systems.

### 9.5 Long-Tail Query Targeting

- Target long-tail queries of **15–23 words** that match natural language
  question patterns.
- These queries align with how users interact with AI assistants and voice search.
- Structure content to directly answer these multi-word queries within the
  first paragraph after the relevant heading.

### 9.6 Citation Readiness

Make your content easy for AI systems to cite:

| Principle                     | Implementation                                           |
|-------------------------------|----------------------------------------------------------|
| Lists and tables              | Use structured formats that AI can extract cleanly        |
| Short paragraphs              | 2–4 sentences; each paragraph = one citable idea          |
| Original data                 | Proprietary statistics, survey results, benchmarks        |
| Source trust                  | Cite authoritative sources; be cited by authoritative sources |
| Lead-with-answer format       | Put the answer in the first sentence of each section      |
| Definitive statements         | "X is Y" rather than "X might be Y in some cases"        |

---

## 10. Anti-Spam & Compliance

### 10.1 Google Spam Policies

Violating any of these policies can result in manual actions or algorithmic
demotion. The following practices are **prohibited**:

| Violation              | Description                                                  |
|------------------------|--------------------------------------------------------------|
| Cloaking               | Showing different content to search engines than to users    |
| Doorway pages          | Low-quality pages created solely to rank for specific queries|
| Link schemes           | Buying/selling links, excessive link exchanges, automated link building |
| Hidden text/links      | Text or links invisible to users but visible to crawlers     |
| Pure AI content        | Scaled AI-generated content published without human editorial oversight, created primarily to manipulate rankings |
| Scraped content        | Copying content from other sites without adding value        |
| Expired domain abuse   | Purchasing expired domains to exploit their existing authority|
| Parasitic SEO          | Publishing low-quality content on a high-authority domain to exploit its ranking power (also called "site reputation abuse") |
| Keyword stuffing       | Unnaturally repeating keywords in content, meta tags, or alt text |
| Sneaky redirects       | Redirecting users to different content than what search engines see |

### 10.2 Legal Compliance

| Regulation    | Requirement                                                        |
|---------------|--------------------------------------------------------------------|
| **FTC**       | Disclose affiliate links, sponsored content, and material connections. Use clear language ("Ad", "Sponsored", "Affiliate link"). |
| **GDPR**      | Cookie consent banner (opt-in, not opt-out) for EU visitors. Privacy policy detailing data collection, processing, and retention. Right to erasure. Data processing agreements with third parties. |
| **CCPA**      | "Do Not Sell My Personal Information" link for California residents. Privacy policy disclosing categories of personal information collected. |
| **ADA/WCAG**  | Meet **WCAG 2.2 AA** accessibility standards: alt text on images, keyboard navigation, color contrast (4.5:1 minimum for normal text), focus indicators, screen reader compatibility, captions on video. |
| **EU AI Act** | If content is AI-generated, disclose this to users. Label AI-generated images and media. Maintain human oversight of AI content production. |

### 10.3 Content Integrity

- **Verifiable claims**: Every factual claim should be traceable to a primary
  source. Do not make claims that cannot be verified.
- **Professional review for YMYL**: Health, financial, legal, and safety content
  must be reviewed by a qualified professional before publication.
- **Editorial vs. sponsored**: Clearly distinguish editorial content from
  sponsored/paid content using visual labels and `rel="sponsored"` on paid links.
- **AI-assisted content disclosure**: If content is significantly AI-generated,
  disclose this transparently. "Written with AI assistance" or similar language.
- **Correction policy**: Publish corrections promptly and transparently when
  errors are identified. Do not silently edit published content.

---

## 11. Monitoring & Metrics

### 11.1 Traditional SEO Metrics

| Metric          | Target / Benchmark                                          |
|-----------------|-------------------------------------------------------------|
| Organic traffic | Month-over-month growth; track by landing page and query    |
| Keyword rankings| Track top 20 target keywords; aim for page 1 positions      |
| Click-through rate (CTR) | Above average for position (pos 1: ~27%, pos 5: ~5%) |
| Bounce rate     | Below 70% for informational content; below 50% for commercial |
| Core Web Vitals | All pages in "Good" threshold (see Section 2.1)             |
| Crawl errors    | Zero 4xx/5xx errors on indexable pages                      |
| Backlink profile| Growing referring domains; declining spam score              |
| Index coverage  | All target pages indexed; no unexpected noindex/excluded pages |

### 11.2 AI-Era Metrics

| Metric                   | Description                                                | How to Measure                           |
|--------------------------|------------------------------------------------------------|------------------------------------------|
| Citation Rate            | How often your content is cited in AI-generated answers    | Query target keywords in ChatGPT, Perplexity, Gemini; count citations |
| Share of Voice           | Your citation frequency vs. competitors in AI responses    | Track citations for branded and unbranded queries across AI platforms |
| AI Overview Inclusion    | Whether your content appears in Google AI Overviews        | Monitor Google Search Console for AI Overview impressions |
| Bot Crawl Frequency      | How often AI bots crawl your site                          | Analyze server logs for GPTBot, ClaudeBot, PerplexityBot user agents |
| Sentiment                | How AI systems characterize your brand/content             | Query AI systems about your brand; analyze tone and accuracy |
| Zero-Click Impressions   | Searches where your content is shown but not clicked       | Google Search Console: impressions without clicks; growing ratio indicates AI extraction |

### 11.3 Monitoring Cadence

| Frequency     | Tasks                                                               |
|---------------|---------------------------------------------------------------------|
| **Daily**     | Core Web Vitals (CrUX or RUM), server errors (5xx), uptime monitoring, crawl error alerts |
| **Weekly**    | Keyword rankings, organic traffic trends, bot crawl logs, Search Console messages, new backlinks |
| **Monthly**   | Content freshness audit (flag pages >90 days stale), structured data validation (Rich Results Test on modified pages), internal/external link health check, competitor ranking comparison |
| **Quarterly** | Full technical SEO audit (crawl, indexation, schema, CWV, mobile), competitor analysis (content gaps, backlink gaps, SERP feature comparison), AI citation check (query top keywords in all major AI platforms), content consolidation review (merge thin/duplicate pages) |

### 11.4 Recommended Tools

- **Google Search Console**: Indexation, impressions, clicks, CWV, crawl stats.
- **Bing Webmaster Tools**: Bing-specific indexation and crawl data.
- **Google PageSpeed Insights**: Per-page CWV analysis with lab + field data.
- **Rich Results Test**: Structured data validation.
- **Screaming Frog / Sitebulb**: Full-site crawl audits.
- **Ahrefs / Semrush**: Keyword tracking, backlink analysis, competitor research.
- **Server logs**: Direct analysis of bot crawl patterns (essential for AI bot
  monitoring).

---

## Appendix: Quick-Reference Checklist

Use this checklist before publishing any content page:

- [ ] HTTPS with valid TLS certificate
- [ ] Canonical tag present and correct
- [ ] Title tag: 50–60 chars, keyword near front
- [ ] Meta description: 120–160 chars, includes CTA
- [ ] Exactly 1 H1 tag matching page topic
- [ ] Heading hierarchy: H1 → H2 → H3 (no skipped levels)
- [ ] Keyword in title, H1, first paragraph, URL, meta description
- [ ] Minimum 800 words (informational content)
- [ ] At least 3 internal links with descriptive anchor text
- [ ] All images have alt text under 125 chars
- [ ] Article schema (JSON-LD) with author, dates, image
- [ ] BreadcrumbList schema (non-homepage pages)
- [ ] FAQPage schema (if FAQ section exists)
- [ ] HowTo schema (if step-by-step content exists)
- [ ] "Last updated" date visible on page
- [ ] Author byline with link to bio page
- [ ] Core Web Vitals in "Good" range
- [ ] Mobile-friendly (viewport meta, touch targets, no horizontal scroll)
- [ ] Featured snippet answer (40–60 words, self-contained)
- [ ] At least 2 statistics per major section with sources
- [ ] Rich Results Test passes with zero errors
- [ ] No spam policy violations
- [ ] Accessibility: alt text, contrast, keyboard nav, focus indicators

---

*This document is maintained by the Ecosire AI Content Engine team and should be
reviewed quarterly alongside the content it governs.*
