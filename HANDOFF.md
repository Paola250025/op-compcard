# O&P CompCard — Handoff / Status

_Read this first when picking the project back up._

## Status: LIVE ✅ and in daily use

- **Live at:** https://card.homesbuyolaf.com (HTTPS working; installed to Paola's iPhone home screen as an app).
- **Repo:** github.com/Paola250025/op-compcard (public, GitHub Pages, `main` / root).
- **Password:** `Flecha25` (only the SHA-256 hash is in the code).
- **RentCast:** free Developer plan. API key entered once per device (⚙ Settings), stored in
  localStorage + a long-lived cookie (survives cache clears); the app also requests persistent
  storage. It should not keep re-asking for the key. (Note: http:// and https:// are separate
  origins — the key must be entered under the https app, which is the installed one.)
- **Verified working** end-to-end with real addresses.

## Features (all built & live)

- Address search → RentCast value + rent + property, with `lookupSubjectAttributes=true` for accuracy.
- **Comp-based price range** (median $/sqft, min/max) + RentCast AVM shown as reference. Range uses "to", not a dash.
- **Editable beds/baths/sqft** with an **Update** button that re-queries RentCast for a closer estimate.
- **Rental estimate** in its own section; the rent number is **editable** (tap to correct). Feeds the share image.
- **Comps** show a **Sold / Active / Pending** status chip each.
- **Competition (market view):** on-demand button "See active & pending within 1 mile" → RentCast
  `/listings/sale` by lat/long + 1 mile radius → summary (X active · Y pending) + list with DOM.
  On-demand to save free-tier lookups.
- **Costs & flags:** HOA, taxes, assessment, non-warrantable (auto when available, manual otherwise). HOA-vs-comps chip.
- **Fully editable payment calculator** (price, down, rate, taxes, HOA, insurance).
- **Save homes** for ~2 days (localStorage; warns if a Private window blocks saving).
- **Share = real PNG image** (canvas) with branding, the home, range, rent, HOA/taxes/payment,
  the **Olaf & Paola contact block** (phones/emails/website), and an "estimates only" disclaimer.
  Native share sheet when available, otherwise downloads. Contact is ONLY on the client image (not in the app UI).
- **PWA:** manifest + chess-piece app icon (rook/queen/knight) so it installs like an app.
- Brand: Homes Buy Olaf & Paola wordmark + chess motif + slate palette + Fraunces serif.

## Contact info used (on the client share image)
- Olaf 562-233-6523 · oek@homesbuyolaf.com
- Paola 929-245-2908 · paola@homesbuyolaf.com
- homesbuyolaf.com

## Open items / to verify next session

1. **Comp & listing status accuracy** — the Sold/Active/Pending labels and the competition list are
   read from RentCast fields and normalized in `compStatus()` / `loadMarket()`. NOT verified against a
   live RentCast response (this environment can't reach RentCast). Paola should check: do comps show the
   right status? does "See active & pending" return listings and split them correctly? If a status is wrong
   or Pending never shows, adjust the field mapping in `compStatus()` (RentCast may name Pending differently)
   and the `/listings/sale` query in `loadMarket()`.
2. **"Sold in 90 days"** — Paola asked for solds closed within 90 days. Current comps come from the AVM
   (recent comparable sales) with status chips, but they are NOT explicitly filtered to a 90-day window
   (the AVM comp date field wasn't confirmed). If needed, filter comps by their sale/removed date ≤ 90 days.
3. **Free-tier budget** — each search uses ~3 lookups (value + rent + properties); the market button adds 1.
   50/month free. If she needs more volume, RentCast paid is $74/mo — but then move the key server-side
   (e.g. Cloudflare) since a public GitHub repo can't hide it.
4. **Olaf's phone** — have Olaf open the https app, enter Flecha25 + the RentCast key once, add to home screen.
5. Optional polish: real chess logo file if Paola ever sends the PNG/SVG (currently recreated in code).

## Verifying logic locally
`node --check` the extracted `<script>`; a small harness in /tmp tested mapProperty + calc numbers
(comp range, rent, monthly payment) and they were correct. The RentCast calls themselves can only be
tested in Paola's browser.

## Files
- `index.html` — the whole app. `manifest.json`, `apple-touch-icon.png`, `assets/icon-*.png` — PWA.
- `CNAME` — card.homesbuyolaf.com. `README.md`, this `HANDOFF.md`.
- `docs/roadmap.md`, `docs/spec-fase-1.md`, `docs/plan-fase-1.md` — planning.

## Other project in flight
The **Archery From Zero** sales page (Paola Adventurer brand) is built and committed on branch
`claude/archery-website-requirements-8yk46i` of the **Bear-Card** repo, at `archery/index.html`
(would serve at paolaadventurer.com/archery). Waiting on the Payhip checkout link + photos before launch.
