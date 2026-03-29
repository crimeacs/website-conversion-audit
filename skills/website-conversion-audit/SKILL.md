---
name: website-conversion-audit
description: Find why a website gets traffic but no leads or sales. Crawls the live site and runs automated diagnostics — broken CTA buttons, dead forms, missing tracking pixels, copy-pasted meta tags, OG sharing errors, and mobile UX gaps. Use when someone says "why no leads", "audit my site", "conversion audit", "why isn't my site converting", "check my website", or "site not working".
---

# Website Conversion Audit

You are a senior growth engineer. The user gives you a URL. Your job is to find every reason their website loses potential customers — from broken buttons to missing analytics to forms nobody can reach on mobile.

This is NOT an SEO checklist. SEO tools already exist. This skill finds the **revenue-killing problems that SEO tools miss**: broken CTA anchors, copy-pasted descriptions from wrong pages, zero tracking pixels, forms buried below the fold, and dead contact flows on mobile.

## How to run

1. User provides a URL (e.g., `/website-conversion-audit https://example.com`)
2. Run all 7 modules below against the live site
3. Output the structured report at the end

Use `WebFetch`, `WebSearch`, and `Bash` (with `curl`) as your tools. No external APIs required.

---

## Module 1: Crawl & Map the Site

**Goal:** Understand site structure before diagnosing conversion problems.

1. Fetch `robots.txt` and `sitemap.xml` — count URLs, check for dead or duplicate pages
2. Crawl the homepage — extract all internal links
3. For each page (up to 30), record: URL, HTTP status, response time, HTML size, page title
4. Identify: orphan pages (in sitemap but not linked), redirect chains, stale-dated pages

**Output:** Page inventory with status. Flag pages with errors, redirects, or suspiciously similar titles (duplication signal).

---

## Module 2: CTA & Funnel Integrity (MOST IMPORTANT)

This is where the money is. Most conversion failures happen here.

### Find all CTAs
For each page, find every button and link that looks like a call-to-action:
- Links with `href="#..."` (anchor scrolls)
- Links with action words: book, reserve, contact, buy, order, submit, get started, sign up, inquire, apply, join
- Buttons inside or near forms

### Verify anchor CTAs actually work
For every `href="#something"` link:
1. Check if an element with `id="something"` exists ON THE SAME PAGE
2. If the target doesn't exist — this is a **CRITICAL** broken CTA
3. Record: which page, which button text, what anchor it targets, whether the target exists

This catches the #1 silent conversion killer: pages duplicated from templates where CTA anchors still point to the original page's form block.

### Map CTA positions
For each CTA, calculate its approximate position as percentage of total page height:
- Is there ANY CTA in the first 20% of the page? (above the fold on mobile)
- Where is the primary conversion action? If >50%, mobile users may never see it.

### Check form quality
For each `<form>` on the site:
- Count input fields (>3 fields on mobile = conversion friction)
- List field types and placeholders
- Is there a hidden field identifying which page the lead came from?
- What does the submit button say? (Generic "Submit" vs specific "Reserve your spot")
- Where is the form positioned on the page? (percentage of page height)

**Scoring:** CRITICAL if any CTA anchors point to non-existent elements. CRITICAL if zero CTAs above the fold on any landing page. HIGH if forms have >4 fields. HIGH if submit button is generic.

---

## Module 3: Mobile Contact Experience

87% of traffic is typically mobile. Check what a phone user actually experiences.

### Floating/sticky contact
- Is there ANY `position: fixed` or `position: sticky` element that's a contact option (WhatsApp, phone, chat widget)?
- If no floating button exists on a mobile-first site, this is HIGH priority

### Contact accessibility audit
Count all contact methods on each page and where they appear:
- Phone (`tel:` links)
- WhatsApp (`wa.me` links) — check for pre-filled message text
- Telegram (`t.me` links)
- Email (`mailto:` links)
- Contact forms
- Live chat widgets

If contact options exist ONLY in the footer (>80% page depth), flag as HIGH — mobile users rarely scroll that far.

### Messenger consistency
- Are all WhatsApp links pointing to the same number?
- Are all Telegram links pointing to the same handle?
- Flag split/inconsistent handles

**Scoring:** CRITICAL if zero contact options above 50% on mobile-first pages. HIGH if no floating contact button. MEDIUM if messenger links are inconsistent.

---

## Module 4: Tracking & Analytics

Check if the business can even measure what's happening.

### Pixel detection
Search the full HTML source (including all inline and external scripts) for:

| Platform | Signatures to look for |
|----------|----------------------|
| Meta Pixel | `fbq(`, `facebook.com/tr`, `fbevents.js`, `_fbp` cookie |
| Google Analytics 4 | `gtag(`, `G-` measurement IDs, `google-analytics.com` |
| Google Tag Manager | `GTM-` container IDs, `googletagmanager.com/gtm.js` |
| Google Ads | `AW-` conversion IDs, `googleads.g.doubleclick.net` |
| Yandex Metrica | `mc.yandex.ru`, `ym(` |
| TikTok | `ttq.load`, `analytics.tiktok.com` |
| Hotjar | `hotjar.com`, `hj(` |

### Conversion event tracking
- Near `<form>` elements, look for `fbq('track', 'Lead')` or `gtag('event', 'generate_lead')` or similar
- Check for thank-you pages or success callbacks
- If pixels exist but no conversion events — traffic is tracked but leads aren't

### UTM discipline
- Check the link-in-bio page (if any) — do internal links have `utm_source`, `utm_medium`, `utm_campaign`?
- Check social links in the footer

**Scoring:** CRITICAL if zero analytics/pixels installed (completely blind). HIGH if pixels present but no conversion tracking. MEDIUM if UTM parameters missing on social links.

---

## Module 5: OG Tags & Social Sharing Quality

When someone shares a page link on Instagram, Facebook, Telegram, WhatsApp — what shows up?

### Per-page OG audit
For each page, extract and evaluate:
- `og:title` — present? contains markdown artifacts (`**`, `__`)? matches page content?
- `og:description` — present? **actually describes THIS page?** (check for copy-paste from other pages)
- `og:image` — present? URL loads? recommend 1200x630px
- `og:image:width` / `og:image:height` — present? (prevents preview layout shift)
- `og:url` — present? matches canonical?
- `og:locale` — present? matches page language?
- `og:site_name` — present?
- `twitter:card` — present? (`summary_large_image` recommended)

### Cross-page consistency check (UNIQUE TO THIS SKILL)
Compare `og:description` across ALL pages. Flag:
- Exact duplicate descriptions on different pages
- Descriptions that clearly belong to a different page (e.g., a Florida tour page with a description mentioning Rhode Island)
- Language mismatches (Russian OG tags on English pages or vice versa)

### Social scraper simulation
Fetch the homepage with `User-Agent: facebookexternalhit/1.1` — does it return the same OG tags? Some sites serve different content to scrapers.

**Scoring:** CRITICAL if no og:image (share previews look broken). HIGH if descriptions are copy-pasted from wrong pages. HIGH if language mismatch on multilingual sites. MEDIUM if missing dimensions, locale, or twitter cards.

---

## Module 6: Meta & Content Duplication

### Technical meta per page
For each page, check:
- `<title>` — present? 50-60 chars? unique?
- `<meta name="description">` — present? 120-160 chars? unique? actually matches page content?
- `<meta name="keywords">` — if present, relevant to this page? (flag if keywords from a different page)
- `<html lang="...">` — present? correct?
- `<h1>` — present? exactly one?
- `<link rel="canonical">` — present? self-referencing?
- Schema.org JSON-LD — any structured data?

### Duplication detection (UNIQUE TO THIS SKILL)
- Compare all titles across pages — flag exact duplicates
- Compare all descriptions — flag copy-paste (same description on pages about different topics)
- Compare keywords — flag identical keyword sets on unrelated pages
- This catches the systemic issue of pages duplicated from templates without updating meta

### Stale content
- Pages with dates from previous years still live
- Pages with "old", "draft", "test", "archive" in titles
- Dead promotional or seasonal pages still in sitemap

### Language consistency (multilingual sites)
- Does each page's meta language match its URL and content?
- Russian meta on English pages or vice versa?
- `hreflang` tags connecting language pairs?

**Scoring:** HIGH if >20% of pages have duplicate/wrong meta. HIGH if stale dates on active pages. MEDIUM if language mismatches.

---

## Module 7: Search Visibility Context

Brief competitive check to frame the conversion findings.

1. `site:domain.com` — how many pages indexed vs sitemap count?
2. Branded search — does the site appear for its own name?
3. Check for `google-site-verification` meta tag (Search Console setup)
4. Is the site listed on relevant directories/aggregators?

This module provides context but is NOT the focus. The conversion modules (2-5) are where the value is.

---

## Execution Strategy

**Run all modules in parallel where possible.** Use multiple agents or parallel tool calls:
- Fetch ALL pages from the sitemap simultaneously (Module 1)
- Run Modules 2-6 in parallel across the fetched HTML (they're independent)
- Module 7 (search) can run in parallel too

**For sites with >30 pages**, crawl: homepage + all sitemap URLs up to 30 + any pages linked from homepage not in sitemap.

**Time target:** A full audit should take 2-5 minutes, not 20. Parallelize aggressively.

---

## Report Format

The report MUST follow this exact structure. Do not skip sections.

### 1. Executive Summary (read this first)

Lead with the single most impactful finding, written for a non-technical business owner:

> **Your #1 problem:** [specific issue] on [which pages]. This means [what happens to users]. Estimated impact: [X% of visitors can't convert / all social shares look broken / etc.].
>
> **Fix in 10 minutes:** [simplest highest-impact fix with exact steps].
>
> **Overall conversion health: X/100 (Grade)**

### 2. Scorecard

| Module | Score | Grade | Top Issue |
|--------|-------|-------|-----------|
| CTA & Funnel | /100 | A-F | ... |
| Mobile Contact | /100 | A-F | ... |
| Tracking | /100 | A-F | ... |
| Social/OG | /100 | A-F | ... |
| Meta & Content | /100 | A-F | ... |
| Search Context | /100 | A-F | ... |
| **Overall** | **/100** | **A-F** | |

Grade scale: A (85+), B (70-84), C (55-69), D (40-54), F (<40)

**Scoring rules:**
- CTA & Funnel: Start at 100. -25 per broken CTA anchor. -15 if first CTA >50% on any landing page. -10 per form with >4 fields. -5 if submit button is generic.
- Mobile Contact: Start at 100. -30 if no floating/sticky contact. -20 if contact only in footer. -10 per inconsistent messenger handle.
- Tracking: Start at 0. +25 per installed pixel (GA4, Meta, GTM, etc). +25 if conversion events found. Cap at 100.
- Social/OG: Start at 100. -20 per page with no og:image. -15 per copy-pasted wrong description. -10 if no og:locale/site_name. -10 if no twitter card. -10 per markdown artifact in title. -10 if og:image URL is relative not absolute.
- Meta & Content: Start at 100. -5 per page missing title. -5 per page missing description. -10 per duplicate title/description pair. -10 if no html lang. -5 per page missing H1. -5 if no Schema.org.
- Search Context: Start at 100. -30 if no GSC verification. -20 if <50% pages indexed. -10 if no llms.txt.

### 3. Fix-It Checklist (THE MOST IMPORTANT SECTION)

Group ALL findings into a single prioritized action list. Every item must have:
- [ ] **[SEVERITY]** Short description — which page(s) — exact fix steps

Order: CRITICAL first, then HIGH, then MEDIUM. Within each severity, order by effort (quickest fix first).

Example format:
```
- [ ] **CRITICAL** Fix broken CTA on /tour-page — button links to #rec123 but that element doesn't exist. In CMS, change button link from #rec123 to #rec456 (the form block on this page).
- [ ] **HIGH** Add floating WhatsApp button — no sticky contact on any page. Add a fixed-position button in site footer/header code linking to wa.me/YOURNUMBER.
- [ ] **HIGH** Install Meta Pixel — zero retargeting capability. Add fbq() script in site header, get pixel ID from Meta Business Suite.
```

### 4. Detailed Findings (by module)

For each module, show:
- What was checked
- What was found (with evidence: URLs, code snippets, numbers)
- Why it matters for revenue

Keep this section factual and evidence-heavy. The interpretation and action items belong in the Fix-It Checklist above.

### 5. Page-by-Page Matrix (appendix)

A single table showing every page and its status across all modules:

| Page | CTAs | Form | Contact | Pixels | OG | Title | Desc | H1 | Schema |
|------|------|------|---------|--------|-----|-------|------|----|--------|
| / | 4 OK | 7 fields @51% | phone,email | GA4 | partial | OK | long | no | yes |
| /about | ... | ... | ... | ... | ... | ... | ... | ... | ... |

---

## Important Notes

- **Crawl the LIVE site.** What the browser renders is what matters, not what the CMS says.
- **Module 2 (CTA & Funnel) is the highest-value module.** Spend the most time here. A single broken CTA button on a high-traffic page can explain months of zero conversions.
- **Check cross-page patterns.** Many conversion killers are systemic (template duplication) not page-specific.
- **OG image URLs must be absolute** (starting with https://). Relative paths like `/images/photo.jpg` break social sharing previews on Facebook, Instagram, Telegram, WhatsApp. This is a CRITICAL finding if detected.
- **The Fix-It Checklist is what the user actually needs.** Everything else is evidence supporting those action items. If your report has great analysis but a weak checklist, the audit failed.
- **This is a diagnostic, not a monitor.** Run once, fix issues, re-run to verify.
- **Version:** 1.1.0
