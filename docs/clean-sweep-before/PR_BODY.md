## Summary

Visual/structural phase of the clean sweep. Token-driven CSS layer, Level 2 card elevation, warm-white section alternation, icon migration off Linearicons to Font Awesome, vendor stylesheet pruning. No copy/content rewrites (deferred to a later content session).

17 commits on `clean-sweep` branched from `avielrodriguez-patch-3`. Rollback anchor: tag `pre-clean-sweep`.

## What shipped

### CSS architecture
- New `css/styles.css` (216 lines): `:root` tokens for color, spacing, typography, elevation, radius — sourced from `DESIGN.md`.
- Inter webfont wired into `<head>` of `index.html`, `about.html`, `portfolio.html`, ahead of local CSS.
- `lang="en"` on the three active pages (was `zxx`).
- Base typography, focus-visible + Safari fallback, `prefers-reduced-motion`, warm-bg link contrast.
- Cache-bust query on `styles.css?v=2026-04-12`.

### Layout
- Warm White (`#f6f5f4`) alternation on every `<section>` across the three pages.
- 96px desktop / 48px mobile section padding rhythm.
- Legacy `.section-gap` padding neutralized with `!important` (it wins against both old and new rules because the classes coexist).

### Components
- Level 2 whisper shadow + 12px radius + 1px warm border + 24px padding on `.card`, `.single-portfolio`, `.single-blog`, `.single-recent-blog`, `.single-services`, `.single-price`, `.feature-item`, `.single-team`, `.service-item`.
- `@media (hover: hover)` guard prevents shadow sticking on touch.
- Notion Blue (`#0075de`) primary buttons with `#005bab` hover; pill badge component.
- Stack / pad / gap spacing utilities.
- Global `img { max-width: 100%; height: auto }` — fixes a 545px overflow img that broke mobile.
- Skill-bar CLS guard (`min-height` on bar + label).

### Icon + vendor CSS prune
- All `lnr-*` glyphs swapped to Font Awesome across `index.html` (service cards), `about.html`, `portfolio.html`, `archive.html`, and `js/main.js` (mobile hamburger, carousel nav, chevrons).
- Deleted 7 unused vendor stylesheets: `linearicons.css`, `magnific-popup.css`, `jquery-ui.css`, `jquery-ui.min.css`, `nice-select.css`, `animate.min.css`, `owl.carousel.css`.
- Pruned 194 lines of dead rules from `main.css` (owl/magnific/nice-select/jquery-ui/linearicon selectors).

### Meta/title/nav
- `<meta name="author">` fixed on `portfolio.html` (Harrison Jansma → Aviel Rodriguez).
- `<title>` fixed on all three pages (`Aviel Rodriguez` / `About — Aviel Rodriguez` / `Work — Aviel Rodriguez`).
- 5 nav links `href="archive"` → `portfolio.html`.
- Deleted template demo page `elements.html`.

## Verification (Playwright Chromium)

| Check | Result |
|---|---|
| V1 Inter renders, warm alternation, Level 2 cards | ✅ 1440 + 375 |
| V3 `lang="en"` on 3 active pages | ✅ |
| V3 titles | ✅ |
| V4 no horizontal scroll | ✅ 1440, 960, 375 |
| V5 CSS payload = `font-awesome + bootstrap + main + styles.css?v=…` + Inter | ✅ |
| lnr-* eradication | ✅ zero in all surviving HTML/JS/CSS |
| V7 contrast | not measured (no contrast checker) |
| V6 voice | skipped — no copy changes this phase |

## Deferred to the content session (not in this PR)

- `<h3>H. Jansma</h3>` header/footer on `portfolio.html`
- `HarrisonJansmaResume.pdf` resume link on `portfolio.html` (404)
- `harrisonjansma` GitHub / Medium / LinkedIn URLs in colophon prose on all pages
- `archive.html` still has `lang="zxx"`, old `<title>A. Rodriguez>`, no `styles.css` (kept URL-reachable but visually legacy — decision to keep/redirect/replace deferred)
- Hero copy, Areas of Interest blurbs, Skills preamble, footer "Let's be social", about-page framing + FAQ voice pass (Tasks 10 + 11)
- Deeper `main.css` prune to `<200-line legacy-shim.css` (Task 13 was partial — dead vendor rules only)

## Test plan

- [ ] Open `index.html`, `about.html`, `portfolio.html` at 1440, 1200, 768, 375 — confirm Inter, warm alternation, card shadows, no horizontal scroll.
- [ ] Tap a card on mobile (DevTools device mode) — shadow should NOT stick.
- [ ] Toggle mobile hamburger — icon should flip between `fa-bars` and `fa-times`.
- [ ] DevTools Network → filter CSS → only 4 local CSS files + Inter webfont.
- [ ] Click every nav "Portfolio" link — lands on `portfolio.html`, never `archive`.
- [ ] Lighthouse accessibility pass ≥ 95.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
