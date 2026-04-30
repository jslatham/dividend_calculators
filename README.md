# DRIP Calculator — iPhone PWA

A browser-based dividend reinvestment calculator that mirrors `Advanced.ipynb`. Designed
to be installed to the iPhone home screen via Safari → Share → "Add to Home Screen".

## What it does

- Enter a ticker; pulls the current price and full dividend history from Yahoo Finance.
- Auto-detects the payment cadence (weekly / monthly / quarterly / yearly) from the
  spacing of recent dividends — same heuristic as the notebook.
- Annualizes the most recent dividend (× detected frequency) to compute the yield.
- Simulates DRIP compounding over your chosen horizon and shows summary stats, a
  stacked area chart (initial / dividends / appreciation), and a per-period table.

## How it fetches Yahoo

Yahoo's `query1.finance.yahoo.com` endpoints don't send CORS headers, so static pages
can't hit them directly. The app routes the request through a public CORS proxy
(`corsproxy.io`, with `allorigins.win` as fallback). If both proxies fail or are
rate-limited, the app shows an error — try again or use a different ticker.

## Deploying to GitHub Pages

1. Copy this `iphone-app/` folder into your `dividend_calculators` repo.
2. In the repo's **Settings → Pages**, set the source to your default branch and the
   folder to `/` (root) or `/docs`, depending on where you placed it.
3. After Pages publishes, the app will live at
   `https://<your-username>.github.io/dividend_calculators/iphone-app/` (or the path
   you chose).

If you'd rather have it at the repo root, move the contents of `iphone-app/` to the
top level and the URL becomes `https://<your-username>.github.io/dividend_calculators/`.

## Installing on iPhone

1. Open the published URL in **Safari** (Chrome on iOS won't add a real PWA — only
   Safari supports "Add to Home Screen" with full PWA chrome).
2. Tap the Share button → **Add to Home Screen** → Add.
3. Launch from the home screen — it opens in standalone mode (no Safari UI).

## Files

- `index.html` — single-file app (HTML + CSS + JS, with Chart.js from CDN).
- `manifest.json` — PWA manifest (name, icons, theme color).
- `icon.svg` — source icon (a green chart-line tile with "DRIP" wordmark).
- `apple-touch-icon.png` (180×180) — used by iOS for the home-screen icon.
- `icon-192.png`, `icon-512.png` — referenced from the manifest for Android and
  desktop PWA installs.

## Notes / caveats

- Yahoo's unofficial endpoints can change shape or rate-limit. If the app stops
  fetching, check the browser console; you may need to swap to a different proxy
  or paid data source.
- The simulation makes the same simplifying assumptions as the notebook: constant
  yield, constant price growth, dividends reinvested at the prevailing share price,
  no taxes or fees.
