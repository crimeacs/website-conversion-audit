# Website Conversion Audit

**Find why your website gets traffic but no leads.**

A Claude Code skill that crawls your live site and runs automated conversion diagnostics. It finds the revenue-killing problems that SEO tools miss: broken CTA buttons, dead forms, missing tracking pixels, copy-pasted meta tags, and mobile UX gaps.

## The problem this solves

SEO tools tell you about meta tags, Core Web Vitals, and schema markup. But they don't tell you:

- That 9 out of 13 pages have CTA buttons pointing to non-existent elements (so clicking "Book now" does nothing)
- That your Florida tour page has Rhode Island's description because it was copy-pasted during page duplication
- That you have zero tracking pixels installed (so every visitor is lost forever)
- That the only contact form is buried at 55% of the page and your audience is 87% mobile from Instagram
- That your English homepage shows Russian OG tags when shared on social media

This skill finds all of that.

## Install

### Via Claude Code plugin marketplace

```bash
/plugin marketplace add crimeacs/website-conversion-audit
/plugin install website-conversion-audit
```

### Manual install

```bash
mkdir -p ~/.claude/skills/website-conversion-audit
curl -o ~/.claude/skills/website-conversion-audit/SKILL.md \
  https://raw.githubusercontent.com/crimeacs/website-conversion-audit/main/skills/website-conversion-audit/SKILL.md
```

## Usage

```
/website-conversion-audit https://yoursite.com
```

Or just describe what you need:

```
Audit my website for conversion problems: https://yoursite.com
```

## What it checks

| Module | What it finds |
|--------|--------------|
| **CTA & Funnel Integrity** | Broken anchor links, dead buttons, CTA positions, form field count, submit button copy |
| **Mobile Contact Flow** | Missing floating WhatsApp/phone buttons, contact options only in footer, inconsistent messenger handles |
| **Tracking & Analytics** | Missing Meta Pixel, GA4, GTM, TikTok Pixel; no conversion events; no UTM parameters on social links |
| **Social Sharing (OG Tags)** | Copy-pasted descriptions from wrong pages, markdown artifacts in titles, language mismatches, missing image dimensions |
| **Meta & Content Quality** | Duplicate titles/descriptions across pages, stale dates, wrong-language meta, template duplication artifacts |
| **Site Structure** | Dead pages in sitemap, redirect chains, orphan pages |
| **Search Context** | Indexing gap, branded search presence, Search Console setup |

## What makes this different from SEO audit skills

| Feature | SEO skills | This skill |
|---------|-----------|------------|
| Meta tags & schema | Yes | Yes (baseline) |
| Core Web Vitals | Yes | Basic |
| **Broken CTA anchor detection** | No | **Yes** |
| **Form position & field analysis** | No | **Yes** |
| **Cross-page OG copy-paste detection** | No | **Yes** |
| **Tracking pixel verification** | No | **Yes** |
| **Mobile contact flow audit** | No | **Yes** |
| **UTM discipline check** | No | **Yes** |
| **Social scraper simulation** | No | **Yes** |
| **Template duplication artifact detection** | No | **Yes** |

## Output

The audit produces:
- Executive summary with the #1 revenue killer and a quick win
- Detailed findings with severity (CRITICAL/HIGH/MEDIUM/LOW), evidence, revenue impact, and fix steps
- Priority matrix (effort vs impact)
- Scorecard with grades per module

## Requirements

- Claude Code (CLI, desktop app, or IDE extension)
- No API keys needed — works with any public website using only `curl`, `WebFetch`, and `WebSearch`

## Origin

Built from a real audit of a tour company website that had 452 monthly sessions but only 1 lead (0.22% conversion). The audit discovered 9 of 13 tour pages had broken CTA buttons from template duplication, zero tracking pixels, and copy-pasted meta descriptions from wrong pages. The conversion methodology was then generalized into this reusable skill.

## License

Apache 2.0
