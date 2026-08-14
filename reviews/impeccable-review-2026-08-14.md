# pzbackyard.com — Impeccable review

Date: 2026-08-14 · Surface: race landing page (Persuade) · Live site = repo, no drift.
Evidence: full-page screenshots (intake/snapshots/), source read, live HTTP checks,
DNS/Worker inspection. Read-only review; nothing was changed or deployed.

## Verdict: 28 / 40

| Lens | /5 | Notes |
|---|---|---|
| Art direction & hierarchy | 4.5 | Distinct brutalist race-bib world: Teko + JetBrains Mono, yellow/black hazard stripes, film grain, glitch H1, ticker. Nothing template about it. |
| Typography | 4 | Confident scale (10rem H1), good mono kickers. Some gray-500-on-black labels run faint. |
| Layout & rhythm | 4 | Bordered grids, timeline, ticker all cohere. Mobile hero fits 390px cleanly; countdown stays 4-across. |
| Copy & voice | 4.5 | Spanish is genuine tico voseo ("Llegás tarde, estás afuera", "sigue sin vos"). Sharp, dry, no AI smell. Blemish: section kickers are English-only ("The Format", "Rulebook") even in ES mode. |
| Conversion flow | 3.5 | One honest funnel: WhatsApp direct to Javier, pre-filled message, floating button + hero CTA + register card. Right-sized for a tico event. But www.pzbackyard.com is dead (522), there is no fallback contact, and "CUPOS LIMITADOS" never says how many. |
| Event info completeness | 3 | Format, rules, dates, venue, price, schedule, splits: all present and correct. Missing: course map/elevation, what the fee includes, crew/pacer/camping logistics, aid station detail, refund policy, results/photos from 2026, organizer identity. |
| Performance | 2 | Tailwind via CDN script in production. Carousel loader probes images/1..40.jpg (23 guaranteed 404s) and eagerly downloads all 17 full-res JPGs — ~30 MB on every visit, hostile to runners on mobile data. Favicon is a 141 KB photo JPG. |
| Accessibility & i18n/SEO | 2.5 | Lang toggle never updates `<html lang>`, doesn't persist, EN content is display:none (invisible to crawlers; no hreflang, no canonical, no sitemap, no Event JSON-LD). No prefers-reduced-motion for ticker/glitch/pulse. Carousel images carry no alt/aria. |

## Claim honesty — clean
- 2027 dates consistent everywhere: Jan 29 2027 is a Friday; Fri 18:00 → Sun 18:00 = 48 h; countdown target 2027-01-30T00:00:00Z = Fri 18:00 UTC-6 (Costa Rica, no DST). Correct.
- Math holds: 48 × 6.706 km = 321.9 km ≈ 322 km ≈ 200 mi; 6.706 km = 4.167 mi; splits at H+6/12/24/36 all check out. ₡60.000 ≈ $115 is right at current rates.
- "El formato original de Lazarus Lake" — fair attribution, not an affiliation claim.
- Only soft spot: "CUPOS LIMITADOS" with no number, and 2026 recap claims ("nadie llegó a las 24 horas") rest on organizer knowledge — presumably Clinton's own, fine.

## Findings, ranked

1. **[Critical, infra] www.pzbackyard.com returns 522.** CNAME www → apex exists, but the Worker custom domain covers only the apex. Anyone typing www gets a Cloudflare error page. Fix: add www as a Worker custom domain or a redirect rule. Zero visual risk.
2. **[High, perf] ~30 MB eager image load + 23 junk 404 requests.** The carousel probes 1..40.jpg and fully downloads every hit before the page settles. Compress/resize (ShortPixel per playbook, target ~150–250 KB each), hardcode the count of 17, lazy-load beyond the first slide.
3. **[High, platform] Tailwind CDN (`cdn.tailwindcss.com`) in production.** Explicitly unsupported for production; render-blocking JS that compiles CSS at runtime. Replace with a compiled stylesheet (site is one file; a small build or pre-generated CSS both work).
4. **[High, SEO/i18n] English half of the site is invisible and unfindable.** EN strings are display:none in an `es` document: crawlers index only Spanish; no Event JSON-LD (races get rich results), no canonical, no sitemap, robots.txt has no Sitemap line, favicon is a photo JPG. The toggle also forgets choice on reload and never flips `<html lang>`.
5. **[Medium, content/conversion] Info gaps a committing runner will WhatsApp about anyway:** course map + surface/elevation of the 6.706 km loop, what ₡60.000 includes, crew/tent/camping rules, aid provisions, refund/transfer policy, 2026 results or winner, who organizes it. Each gap is friction before a $115 commitment. Plus: translate the English kickers in ES mode, name the actual spot count, add one non-WhatsApp contact.
6. [Low] No prefers-reduced-motion guards (ticker, glitch, pulse); carousel divs lack alt text; some gray-on-black labels near contrast floor; robots.txt is Cloudflare boilerplate only; "6.706" decimal-point style is ambiguous in Spanish (CR convention is comma) — consistent enough to keep, worth a conscious choice.

## What is genuinely good
The site has a real point of view — race-bib brutalism that fits a last-man-standing event — the Spanish reads like a tico wrote it, the format explanation is the clearest part of the page (rules, splits timeline, 24→48 story), and every number on the page is true. This is a polish-and-harden job, not a redesign.

## Phased fix plan (no surprises — nothing runs without Clinton's go)

- **Phase 1 — Stop the bleeding (infra only, no page changes).** Add www custom domain/redirect on the Worker; verify www + apex + 404 live. ~15 min, zero visual risk.
- **Phase 2 — Weight.** ShortPixel/resize all 17 JPGs, fix the carousel loader (known count, lazy load), compiled Tailwind CSS replacing the CDN script, real favicon/apple-touch-icon. Byte-identical layout; verify with before/after screenshots. Target: first load under 1.5 MB.
- **Phase 3 — Findability & bilingual honesty.** Event JSON-LD, canonical, sitemap.xml + robots Sitemap line, translate kickers, persist language + set `<html lang>`, decide hreflang strategy (likely `?lang=en` or `/en/` if EN SEO matters; otherwise document the choice).
- **Phase 4 — Content that closes.** With Clinton's input (he owns the facts): course map, fee inclusions, crew/camping rules, refunds, 2026 results, spot count, secondary contact. Copy pass in the same voseo voice; claims check on the new facts.
- **Phase 5 — Gate.** Reduced-motion guards, contrast touch-ups, Lighthouse 95+ on all four scores, independent post-deploy inspection at both viewports before "done".

Phases 1–3 need no new information from Clinton. Phase 4 is blocked on his facts.
