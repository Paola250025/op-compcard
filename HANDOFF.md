# O&P CompCard — Handoff / Status

_Last updated: 2026-07-16. Read this first when picking the project back up._

## Status: LIVE ✅ and working end-to-end

- **Live at:** https://card.homesbuyolaf.com (HTTPS cert still finishing the first
  time — `http://card.homesbuyolaf.com` works now; enable "Enforce HTTPS" once
  available).
- **Repo:** github.com/Paola250025/op-compcard (public, GitHub Pages, `main` / root).
- **Custom domain:** `card.homesbuyolaf.com` — CNAME record lives in the
  **homesbuyolaf.com** GoDaddy DNS (`card` → `paola250025.github.io`). DNS check
  passed. (Note: a `card` record was also mistakenly added to paolaadventurer.com
  earlier — harmless, can be ignored or deleted.)
- **Password:** `Flecha25` (stored only as a SHA-256 hash in the code).
- **RentCast:** free Developer plan active. API key is entered once per device via
  the ⚙ Settings screen and saved in that browser only — never in the repo.
- **Verified:** tested with a real address (30 Cascada, Rancho Santa Margarita, CA)
  — real estimate, comps, HOA, taxes, and calculator all worked.

## Done (Fase 1)

- Address search via RentCast
- Comps sheet (subject + up to 4 comps, $/sqft)
- **Comp-based price range** (median $/sqft for center, min/max for the range) with
  RentCast's AVM shown as a secondary reference — tighter and more defensible
- Money flags: HOA, taxes, assessment, non-warrantable (auto when available, manual
  field when not)
- HOA vs comps comparison
- Payment calculator (down payment + rate → loan + taxes + HOA + insurance)
- Save homes for ~2 days (auto-delete), recent list on home screen
- Share button (see pending #3)
- Homes Buy Olaf & Paola branding + password gate + per-device API key

## Pending for tomorrow

1. **Enforce HTTPS** — once GitHub issues the cert, check "Enforce HTTPS" in
   Settings → Pages so `https://` works and `http` redirects.
2. **Real logo** — the chess-piece Homes Buy Olaf logo needs to be attached as a
   file (PNG transparent or SVG). Right now only the text wordmark shows. Place it
   above the wordmark and on the share card.
3. **Share = actual image** — the Share button currently uses the phone's share
   sheet with text / copies text. To produce a real PNG image to send on WhatsApp,
   add image capture (inline html2canvas or draw to Canvas). Decide + build.
4. **Olaf's phone** — enter the password once and paste the RentCast key once on
   Olaf's device.
5. **Field-mapping check** — try several more addresses and confirm HOA / taxes /
   assessment populate. If any are blank when they shouldn't be, adjust
   `mapProperty()` in index.html to RentCast's real field names. Non-warrantable and
   special assessments are manual-only (no API has them).

## Ground rules / decisions

- Stay on the **RentCast free plan** (50 lookups/month). The key sits in the
  browser of a public-repo app, so free-only means an exposed key can never cause
  charges. If you ever need more volume, move hosting to something that hides the
  key server-side (e.g. Cloudflare Pages) before upgrading.
- Data is RentCast only (which is built from public records + listings). No second
  source planned.
- Clients never see this URL — they only receive the shared card/image. That's why
  the internal domain doesn't matter to them.

## Files in this repo

- `index.html` — the whole app (HTML + CSS + JS, no dependencies).
- `CNAME` — custom domain config (`card.homesbuyolaf.com`).
- `README.md` — setup steps.
- `HANDOFF.md` — this file.
- `docs/roadmap.md`, `docs/spec-fase-1.md`, `docs/plan-fase-1.md` — planning.

## Fase 2 (later)

Sync saved homes between Olaf & Paola (needs a server), HOA-by-city table that grows
with use, automatic daily interest rate, full CRM. See `docs/roadmap.md`.
