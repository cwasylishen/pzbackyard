# PZ Backyard Ultra — pzbackyard.com

Static one-page site for the **PZ Backyard Ultra** in Pérez Zeledón, Costa Rica.

- **2026 (Second Edition):** 24 hours · ran until Last Man Standing (did not go the full 24h)
- **2027 (Third Edition):** 48 hours · Fri Jan 29 18:00 → Sun Jan 31 18:00 CR (or until LMS)

## Stack

- Single `index.html`, no build step.
- Tailwind via CDN (`cdn.tailwindcss.com`), Google Fonts (Teko + Inter).
- Vanilla JS for countdown, language toggle (ES/EN), and image carousel.
- Carousel auto-loads `images/1.jpg` ... `images/N.jpg` until a 404 — drop in `images/18.jpg` and it appears, no code change.

## Local preview

Just open `index.html` in a browser, or serve the folder:

```powershell
# Python
python -m http.server 8000
# then http://localhost:8000
```

## Editing content

All copy is bilingual using `lang-es` / `lang-en` spans. When adding text, always add **both** versions and keep `lang-en` with the `hidden` class so ES is the default.

### Countdown timer
`index.html` script block, `raceDate`:

```js
// Friday Jan 29, 2027 · 6:00 PM Costa Rica (UTC-6, no DST)
const raceDate = new Date('2027-01-30T00:00:00Z').getTime();
```

CR has no daylight saving, so a fixed UTC offset is safe.

## Deploy to Cloudflare

The site is hosted on Cloudflare. Two options to wire this repo to it:

### Option A — Cloudflare Pages (recommended, git-driven)
1. Push this repo to GitHub.
2. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
3. Build settings: **Framework = None**, **Build command = (empty)**, **Build output dir = `/`**.
4. Add a custom domain → `pzbackyard.com`. Every push to `main` redeploys.

### Option B — Wrangler CLI (manual)
```powershell
npm i -g wrangler
wrangler login
wrangler pages deploy . --project-name pzbackyard
```

## TODO / open questions for 2027

- [ ] **OG/share image** — original `share.jpg` (clintonwasylishen.com) is 404. Currently falls back to `images/1.jpg`. Replace with a proper 1200×630 social card.
- [ ] **Registration price** — kept at ₡60.000 (~$115 USD) as 2026. Confirm for 2027.
- [ ] **Package pickup** — set to Thu Jan 28 4–7 PM at Monte General. Confirm date/time/venue.
- [ ] **Favicon** — currently using `images/1.jpg`. Add a proper square favicon when available.
- [ ] **Past results** — consider a "2026 recap" block (winner / longest yard / total laps) once we have the data.
