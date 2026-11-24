# Dual Origin Group — landing page

One-screen landing page for **dualorigingroup.com**, served by GitHub Pages from `main` (root). Dual Origin Group LLC ("DOG LLC") is a two-partner consulting company (Sam & Gio, equal co-founders) building process-optimization and cost-analysis tools for general contractors and manufacturers.

## Files
- `index.html` — the whole page: HTML, CSS, inline SVG logo, animations. No build step.
- `images/cup.webp` (1236w), `cup-900.webp`, `cup-640.webp` — what browsers actually load (responsive `<picture>`, preloaded, `fetchpriority=high`). `images/cup.png` is the fallback. A ~1KB blurred copy is inlined in CSS as the `.cupbox` background so the cup outline appears instantly; JS removes it on load.
- `images/cup.png` — the to-go cup contact card as ONE flattened image (lid, sleeve, stamp, cream "Sam & Gio" tape with both portraits above the names, a small copper espresso-cup email icon at each portrait's bottom-right, phone, corgi "Head of Security" badge). 1236×1404. The email addresses are NOT shown in the image; the espresso icons are the only way to email.
- `images/wordmark-light.png` — "DUAL ORIGIN GROUP" wordmark (Lora, cream, D/O/G slightly brighter).
- `images/corgi.png` — corgi cutout (the badge itself is now baked into `cup.png`).
- `images/steam-1..3.png` — steam textures for the rising-steam animation.
- `CNAME` — custom domain. **Never delete.** `.nojekyll` — serve files as-is.

## Hard rules
1. **Fits on one screen, no scrolling**, at common sizes: 375×667, 390×664, 390×844, 430×932, 768×1024, 1280×720, 1366×768, 1440×900, 1920×1080. Check `document.documentElement.scrollHeight === innerHeight` after every change (Playwright/headless Chrome).
2. **No readable contact info in the code.** The phone is shown only inside the cup image; emails are reachable only through the espresso-cup icons (anti-scraping). Never put a plain-text email, phone number, or literal `mailto:<address>` in HTML, alt text, metadata or JS. The ONE exception (approved by the owners): the two espresso-cup email icons on the portraits are `<button class="mailbtn">` hotspots whose script holds each address XOR-scrambled and reversed, assembles it only on click, copies it to the clipboard, and opens `mailto:`. The phone number has no such exception. This repo is public.
3. **Dark mode only.** No light theme or toggle.
4. Sam and Gio are **equal partners**: always side by side / shared, never one above the other.
5. Animations stay subtle and respect `prefers-reduced-motion`.

## Design system
- Colors: bg `#1A1310`, ink `#F3E7DA`, muted `#BFAE9F`, copper accent `#C8692A`, line `#3A2C24`, card `#241A15`. Stamp ink `#8E3A14`.
- Fonts: Lora (headline, wordmark), Inter (body/UI), via Google Fonts.
- Layout top→bottom: two-bean logo (per-bean jiggle) + wordmark + "CONSULTANTS" → h1 "Bottlenecks brewing?" → 3-line blurb (fixed line breaks via `.tag .l` spans, `white-space:nowrap`) → cup image with beans funneling in and steam behind → footer.
- Blurb lines (keep these breaks): "We build custom solutions that surface the" / "right information and keep work flowing," / "so your team can make better calls, faster."
- Cup bottom (inside `cup.png`): phone centered under the tape, then the corgi mini ID badge — light cream card, "HEAD OF / SECURITY" then "Guards / the beans" (four short lines beside the photo, keeps the card narrow) (dog never "drinks" coffee — caffeine is toxic to dogs). Name tape and badge share the cream `#EADFCF`/`#F1E8DB` family. No "CO-FOUNDERS" label.
- Footer: "Brewed in MA" + muted US flag, then smaller "© 2026 DOG LLC".
- Stamp on cup: "DUAL ORIGIN GROUP" top arc, "EST. 2026" bottom arc, "LET'S TALK" center, slightly rough ink, overlapping the top of the name tape (not the portraits).

## CSS note
`index.html` has many appended override blocks at the end of `<style>` (with `!important`) from iterative design. Later rules win. When changing sizing, prefer adding/adjusting the media-query blocks near the end rather than editing early rules. Spacing rhythm is set by the LAST block ("consistent vertical rhythm"): `main/.wrap/.copy` are `display:contents`, three `<i class="sp">` spacers (logo→h1, blurb→lid, cup→footer) flex in body's column, and the cup width `--cw` grows with screen height up to `--cap`. Tune per breakpoint via `--k` (height reserved for everything except the cup), `--g` (base gap) and `--glogo` (logo gap, gets 2 shares of leftover). h1→blurb stays a fixed 10px. If a change makes the header/text taller, raise `--k` for that band or the page will scroll.

## Regenerating the cup image
`cup.png` is rendered from a private HTML source (not in this repo because it contains the contact text). The source kit lives in the **private** repo `dualorigingroup/website-assets` (`cup-source.html`, `tools/cupshot.js`, README with steps); attach it to the session to edit the cup. Never copy its contents into this public repo. Process: open the source HTML in headless Chrome at 1440×900, deviceScaleFactor 4, hide everything except the cup (lid, cup body, sleeve, stamp, names), disable animations and any glow/pseudo-elements, screenshot the bounding box of `.lid,.cta,.kraft,.names,.tape,.cb` with 24px transparent padding (`node tools/cupshot.js` in `website-assets` does all of this) and `omit_background`, resize to 1236px wide, save as `images/cup.png`, then update the `height` attribute on `<img class="cupimg">`, regenerate `cup.webp`/`cup-900.webp`/`cup-640.webp` (quality 82, alpha 90) and the inlined blurred placeholder (32px wide, WebP data URI in the `.cupbox` rule). `cupshot.js` prints the email icons' centers as % of the image: copy them into the `left`/`top` styles of the two `.mailbtn` buttons. If the cup's height/width ratio changes, update the `/ 1.27` divisor in the `--cw` formula (visible cup height ÷ `.mugwrap` width) and re-tune `--k`. The image must have fully transparent edges so the page's glow fades smoothly.

## Workflow
- Develop on the session's working branch; never push to `main` directly.
- After each change, republish the private preview artifact (https://claude.ai/artifact/KuAvVPuuUiJg3CMSHYxLaC) from a copy of `index.html` with the doctype/html/head/body wrapper tags removed, plus the `images/` files. Pass that URL to update it from a new session.
- Merge to `main` (via pull request) **only when the owner says "merge"**. Merging to `main` publishes the live site.
