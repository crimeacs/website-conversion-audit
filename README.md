# Website Conversion Audit

Find why your site gets traffic but no leads.

```
/website-conversion-audit https://yoursite.com
```

## Install

```bash
/plugin marketplace add crimeacs/website-conversion-audit
/plugin install website-conversion-audit
```

<details>
<summary>Or install manually (one command)</summary>

```bash
mkdir -p ~/.claude/skills/website-conversion-audit && curl -sL -o ~/.claude/skills/website-conversion-audit/SKILL.md https://raw.githubusercontent.com/crimeacs/website-conversion-audit/main/skills/website-conversion-audit/SKILL.md
```
</details>

## What it finds that SEO tools don't

This skill crawls your live site and checks the things that actually lose you money:

- **Broken CTA buttons** — "Book Now" links pointing to elements that don't exist (from template duplication)
- **Forms nobody reaches** — 7-field form buried at 60% of the page, 87% mobile audience
- **Zero tracking** — no Meta Pixel, no conversion events, every visitor lost forever
- **Copy-paste meta disasters** — Florida page showing Rhode Island's description in social shares
- **Dead mobile experience** — WhatsApp link only in the footer, no floating contact button

It also covers the basics (titles, OG tags, schema, sitemap, indexing) but the conversion layer is why this exists.

## Sample output

```
EXECUTIVE SUMMARY

  Your #1 problem: 9 of 13 tour pages have broken "Book Now" buttons.
  The buttons link to #rec1055702941 which doesn't exist on those pages.
  Users click, nothing happens. This explains 0.22% conversion on 452 sessions.

  Fix in 10 minutes: In your CMS, update the button link on each page
  to point to that page's own form block.

  Overall conversion health: 31/100 (F)

FIX-IT CHECKLIST

  - [ ] CRITICAL: Fix CTA anchors on 9 pages (broken #rec links)
  - [ ] HIGH: Add floating WhatsApp button (no sticky contact on any page)
  - [ ] HIGH: Install Meta Pixel (zero retargeting capability)
  - [ ] HIGH: Simplify forms from 7 fields to 2-3
  - [ ] MEDIUM: Fix OG descriptions (3 pages show wrong tour)
  - [ ] MEDIUM: Add og:locale, og:site_name, twitter:card to all pages
```

## How it works

7 modules run against every page in your sitemap:

| # | Module | Key checks |
|---|--------|-----------|
| 1 | Crawl & Map | Sitemap, dead pages, redirects, duplicate titles |
| 2 | **CTA & Funnel** | Broken anchor links, CTA position, form fields, submit copy |
| 3 | **Mobile Contact** | Floating buttons, contact in footer only, messenger consistency |
| 4 | **Tracking** | Meta Pixel, GA4, GTM, conversion events, UTM params |
| 5 | **Social Sharing** | OG tags, cross-page copy-paste, Facebook scraper test |
| 6 | Meta & Content | Duplicate titles/descriptions, stale dates, language mismatches |
| 7 | Search Context | Indexing gap, GSC verification, branded search |

No API keys needed. Works with any public website.

## Origin story

Built from a real audit where a tour company had 452 monthly sessions but only 1 lead. We discovered 9 of 13 pages had broken CTA buttons from template duplication, zero tracking pixels, and copy-pasted descriptions from the wrong pages. The methodology was generalized into this skill.

## Contributing

Found a conversion pattern this skill should catch? [Open an issue](../../issues/new/choose) or PR against `SKILL.md`. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache 2.0
