# SL Financial Planning — Project Guide

## Overview
Static bilingual (ES/EN) website for SL Financial Planning. Built with vanilla HTML/CSS/JS — no framework, no build step.

- **Live site:** https://webslfinancialplanningclaudecode.vercel.app
- **GitHub:** https://github.com/sebastianlarocca-ops/sl-financial-planning
- **Vercel project:** `web.slfinancialplanning.claudecode` (scope: `sebastianlarocca-4081s-projects`)

---

## File Structure

```
index.html      ← Production site (Concierge v2 design)
favicon.svg     ← SL monogram, dark background, terracotta L
vault.html      ← Vault direction (reference only, not linked)
.gitignore
CLAUDE.md       ← This file
```

---

## Design System

| Token | Value | Usage |
|-------|-------|-------|
| `--bone` | `#F4EFE6` | Page background |
| `--ink` | `#1A1612` | Primary text |
| `--ink-2` | `#3A3128` | Secondary text |
| `--accent` | `oklch(0.58 0.12 45)` | Terracotta — CTAs, highlights |
| `--hair` | `rgba(26,22,18,0.12)` | Borders, dividers |
| `--mute` | `rgba(26,22,18,0.55)` | Muted text |

**Fonts:** Fraunces (display/serif) · Inter Tight (body) — loaded from Google Fonts.

---

## i18n

All user-facing text lives in the `i18n` object inside `index.html`:

```js
const i18n = { en: { ... }, es: { ... } };
```

- Default language: `es` (stored in `localStorage` key `sl-lang`)
- To update copy: edit the matching key in both `en` and `es` blocks
- The `render()` function re-renders all text; call it after any i18n change

---

## Site Sections (in order)

1. **Hero** — headline, lede, plan card, 4 stats
2. **Approach** (`#practice`) — 4-area model: Current situation / Protection / Growth / Evolution
3. **Process** (`#method`) — CSS-animated plan assembly scene
4. **Cadence** (`#cadence`) — 5 steps; last step (Annual review) has the orbit animation
5. **Principles** (`#principles`) — 5 commitments
6. **Advisor** (`#advisor`) — portrait placeholder + bio
7. **Clients** (`#clients`) — 3 testimonials (roles only)
8. **FAQ** (`#faq`) — 4 accordion items
9. **Contact** (`#begin`) — dark section with form

---

## Deploying Changes

### Full workflow (commit + push + deploy)

```bash
# 1. Stage files
git add index.html favicon.svg   # or: git add -A

# 2. Commit
git commit -m "describe what changed"

# 3. Push — Vercel auto-deploys from main
git push

# 4. (Optional) Force an immediate prod deploy without waiting for CI
~/.npm-global/bin/vercel --prod --yes --scope sebastianlarocca-4081s-projects
```

Vercel is connected to the `main` branch on GitHub — every `git push` triggers a production deployment automatically.

### Quick deploy only (skip git)

```bash
~/.npm-global/bin/vercel --prod --yes --scope sebastianlarocca-4081s-projects
```

---

## Common Tasks

### Update text / copy
Edit the `i18n` object in `index.html`. Both `en` and `es` keys must be updated.

### Add a new section
1. Add HTML element with an `id` in `index.html`
2. Add nav entry in `i18n.en.nav` / `i18n.es.nav` (with matching `id`)
3. Add i18n strings for the section in both language objects
4. Add a render block inside the `render()` function
5. Style with CSS custom properties from the design system above

### Scroll-reveal on a new element
Add `data-reveal` attribute to the element — the `IntersectionObserver` in `observeReveal()` handles the rest. Elements animate in with `opacity + translateY` when they enter the viewport.

### Orbit animation (Annual review)
The last Cadence step uses `.step-orbit` class + injected `.orbit-months` / `.orbit-mark` elements. The animation triggers when `.is-in` is added by the scroll observer. See the `/* ── ORBIT ── */` CSS block and the steps render in the JS.
