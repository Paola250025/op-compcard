# 🏠 O&P CompCard

Private comps & cost tool for **Homes Buy Olaf & Paola**. Open it on your phone
while showing a house: type an address and get a price range, nearby comps, HOA
/ tax / assessment flags, and a payment calculator — then share a clean card
with the buyer. Built as a single static page (no build step). Property data
comes from the RentCast API (free tier).

## How access & the API key work

- **Password:** the app is gated by a shared password. Only its SHA-256 hash is
  in this code — the plain password is never stored here.
- **API key:** the RentCast key is **never** in this repo. Each phone enters it
  once (Settings ⚙) and it's saved only in that device's browser storage.
- Keep RentCast on the **free Developer plan** so an exposed link can never cause
  charges. If you ever upgrade to a paid plan, move hosting somewhere that can
  hide the key server-side.

## Setup (one time)

1. **Enable GitHub Pages:** repo Settings → Pages → Source: `main` branch, root.
2. **Custom domain:** the `CNAME` file points to `card.homesbuyolaf.com`. In
   GoDaddy DNS add a **CNAME** record: host `card` → value `paola250025.github.io`.
   Leave the rest of homesbuyolaf.com (Showit) untouched.
3. Wait for DNS, then turn on **Enforce HTTPS** in Pages settings.
4. Open the site, enter the password, then paste the RentCast API key once on
   each phone (yours and Olaf's).

## Files

- `index.html` — the whole app (HTML + CSS + JS, no dependencies).
- `CNAME` — GitHub Pages custom-domain config.

## Notes

- Saved homes live on the phone and auto-delete after ~2 days.
- All figures are approximate and for reference only — not an appraisal or offer.
