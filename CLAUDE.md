# Dual Origin Group — landing page

One-screen landing page for **dualorigingroup.com**, served by GitHub Pages from `main` (root). Dual Origin Group LLC ("DOG LLC") is a two-partner consulting company (Sam & Gio, equal co-founders) building process-optimization and cost-analysis tools for general contractors and manufacturers.

## Files
- `index.html` — the whole page: HTML, CSS, inline SVG logo, animations. No build step.
- `images/cup.png` — the to-go cup contact card as ONE flattened image (lid, sleeve, stamp, "Sam & Gio" tape, CO-FOUNDERS, both emails, phone). See "Contact info" below.
- `images/wordmark-light.png` — "DUAL ORIGIN GROUP" wordmark (Lora, cream, D/O/G slightly brighter).
- `images/corgi.png` — corgi cutout used in the footer badge.
- `images/steam-1..3.png` — steam textures for the rising-steam animation.
- `CNAME` — custom domain. **Never delete.** `.nojekyll` — serve files as-is.

## Hard rules
1. **Fits on one screen, no scrolling**, at common sizes: 375×667, 390×664, 390×844, 430×932, 768×1024, 1280×720, 1366×768, 1440×900, 1920×1080. Check `document.documentElement.scrollHeight === innerHeight` after every change (Playwright/headless Chrome).
2. **No contact info in the code.** Emails and phone exist only inside `images/cup.png` (anti-scraping). Do not add mailto links, text emails, or the phone number to HTML, alt text, or metadata. This repo is public.
3. **Dark mode only.** No light theme or toggle.
4. Sam and Gio are **equal partners**: always side by side / shared, never one above the other.
5. Animations stay subtle and respect `prefers-reduced-motion`.

## Design system
- Colors: bg `#1A1310`, ink `#F3E7DA`, muted `#BFAE9F`, copper accent `#C8692A`, line `#3A2C24`, card `#241A15`. Stamp ink `#8E3A14`.
- Fonts: Lora (headline, wordmark), Inter (body/UI), via Google Fonts.
- Layout top→bottom: two-bean logo (per-bean jiggle) + wordmark + "CONSULTANTS" → h1 "Bottlenecks brewing?" → 3-line blurb (fixed line breaks via `.tag .l` spans, `white-space:nowrap`) → cup image with beans funneling in and steam behind → corgi badge → footer.
- Blurb lines (keep these breaks): "We build custom solutions that surface the" / "right information and keep work flowing," / "so your team can make better calls, faster."
- Footer: corgi mini ID badge "HEAD OF SECURITY / Guards the beans" (muted body, full-color photo; dog never "drinks" coffee — caffeine is toxic to dogs), then "Brewed in MA" + muted US flag, then smaller "© 2026 DOG LLC".
- Stamp on cup: "DUAL ORIGIN GROUP" top arc, "EST. 2026" bottom arc, "LET'S TALK" center, slightly rough ink, overlapping the top of the name tape.

## CSS note
`index.html` has many appended override blocks at the end of `<style>` (with `!important`) from iterative design. Later rules win. When changing sizing, prefer adding/adjusting the media-query blocks near the end rather than editing early rules. Spacing rhythm: tight (h1→blurb ~10px), medium (blurb→lid, cup→badge, badge→footer ~30–40px), largest under the logo.

## Regenerating the cup image
`cup.png` is rendered from a private HTML source (not in this repo because it contains the contact text). The source kit lives in the **private** repo `dualorigingroup/website-assets` (`cup-source.html`, `tools/cupshot.js`, README with steps); attach it to the session to edit the cup. Never copy its contents into this public repo. Process: open the source HTML in headless Chrome at 1440×900, deviceScaleFactor 4, hide everything except the cup (lid, cup body, sleeve, stamp, names), disable animations and any glow/pseudo-elements, screenshot the cup's bounding box with 24px transparent padding and `omit_background`, resize to 1236px wide, save as `images/cup.png`, then update the `height` attribute on `<img class="cupimg">`. The image must have fully transparent edges so the page's glow fades smoothly.

## Workflow
- Develop on the session's working branch; never push to `main` directly.
- After each change, republish the private preview artifact (https://claude.ai/artifact/KuAvVPuuUiJg3CMSHYxLaC) from a copy of `index.html` with the doctype/html/head/body wrapper tags removed, plus the `images/` files. Pass that URL to update it from a new session.
- Merge to `main` (via pull request) **only when the owner says "merge"**. Merging to `main` publishes the live site.
