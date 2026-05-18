# PZ Backyard Ultra / pzbackyard.com

Static one-page site for the **PZ Backyard Ultra** in Pérez Zeledón, Costa Rica.

- **2026 (Second Edition):** 24 hours. Ended Last Man Standing before the full 24h ran out.
- **2027 (Third Edition):** 48 hours. First bell Fri Jan 29 at 18:00 CR. Cutoff Sun Jan 31 at 18:00 CR.

Live: https://pzbackyard.com/  (hosted on Cloudflare)

## Stack

- Single `index.html`, no build step.
- Tailwind via CDN, Google Fonts (Teko + Inter + JetBrains Mono).
- Vanilla JS for countdown, language toggle (ES/EN), and image carousel.
- Carousel auto-loads `images/1.jpg` ... `images/N.jpg` until a 404. Drop a new `images/18.jpg` in and it appears, no code change.

## Local preview

```powershell
python -m http.server 8000
# http://localhost:8000
```

## Editing content

All copy is bilingual using `lang-es` / `lang-en` spans. When adding text, always add both versions and keep `lang-en` with the `hidden` class so ES is the default.

### Countdown timer
`index.html` script block, `raceDate`:

```js
// Friday Jan 29, 2027 at 18:00 Costa Rica (UTC-6, no DST)
const raceDate = new Date('2027-01-30T00:00:00Z').getTime();
```

CR has no daylight saving so a fixed UTC offset is safe.

### Voice rules

Two hard rules for any copy added to the site:

1. **No em dashes anywhere.** Not in body copy, not in titles, not in alt text. Use periods, line breaks, or slashes.
2. **No AI cadence.** No "Inspired by", no triple-construct ("Twice the X. Two Ys. One Z."), no filler adjectives like "psychologically devastating". Write like a person who has actually run an ultra wrote it.

## Deploy

Already wired to Cloudflare and pulling from this repo. Push to `main` triggers a redeploy.

## TODO / open questions for 2027

- [ ] **OG/share image.** Original `share.jpg` (clintonwasylishen.com) is 404. Currently falls back to `images/1.jpg`. Replace with a proper 1200×630 social card.
- [ ] **Registration price.** Kept at ₡60.000 (~$115 USD) carried over from 2026. Confirm for 2027.
- [ ] **Package pickup.** Set to Thu Jan 28, 16:00 to 19:00 at Monte General. Confirm date, time, venue.
- [ ] **Favicon.** Currently using `images/1.jpg`. Add a proper square favicon when available.
- [ ] **2026 recap block.** Once we have the data (winner, longest yard, total laps) add a recap.
