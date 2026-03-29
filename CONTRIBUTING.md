# Contributing

This skill improves by finding real conversion problems on real websites. The best contributions come from running the audit against sites and discovering patterns it should catch.

## How to contribute

### Report a missed finding
If you ran the audit on a site and it missed something that cost conversions, open an issue with:
- The type of problem missed (broken CTA, bad OG tag, missing pixel, etc.)
- How you discovered it
- What the skill should have checked

### Add a new check
If you have a conversion pattern that should be in the audit:
1. Fork the repo
2. Add the check to the relevant module in `skills/website-conversion-audit/SKILL.md`
3. Include evidence from a real site where this pattern caused conversion loss
4. Open a PR

### Improve existing modules
The skill's quality depends on specificity. Vague checks like "is the CTA good?" are useless. Specific checks like "does `href='#rec123'` point to an existing element on the page?" are valuable. PRs that make checks more specific and actionable are welcome.

## What makes a good check

- **Automated:** Claude can verify it by crawling the live site
- **Evidence-based:** You've seen this pattern kill conversions on a real site
- **Specific:** Tells Claude exactly what to look for (selectors, patterns, thresholds)
- **Actionable:** The fix is clear from the finding

## What we don't need

- Generic SEO checklist items (use claude-seo for that)
- Checks that require paid APIs or browser automation
- Subjective design opinions ("the hero image should be bigger")
