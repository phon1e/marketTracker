# marketTracker
v1

# Market Tracker

A single-page US market dashboard — live quotes, Fear & Greed gauge, custom watchlist, and top headlines. No build step, no API key, no external libraries. Open `index.html` directly or serve it from GitHub Pages.

**Live demo:** `https://<your-username>.github.io/<repo-name>/`

---

## Features

| Feature | Details |
|---|---|
| **Index cards** | S&P 500, Nasdaq 100, Dow Jones — price, $ change, % change, intraday sparkline |
| **Pre / after-hours** | ETF proxies (SPY · QQQ · DIA) show pre-market and after-hours prices & % |
| **Market status pill** | Live / Pre-market / After-hours / Closed with animated dot |
| **Fear & Greed Index** | SVG semicircle gauge (CNN data) — score, label, Prev Close / 1W / 1M history |
| **Watchlist** | Add up to 10 arbitrary tickers; prices (pre/regular/post), mini sparkline; persists in `localStorage` |
| **Top market news** | 5 headlines from CNBC Markets RSS with description and relative time |
| **Auto-refresh** | Quotes & watchlist every 60 s; news & Fear & Greed every 5 min |
| **Manual refresh** | Button top-right; spins during fetch |
| **Responsive** | Single-column layout on mobile (≤ 760 px) |

---

## Data sources

| Data | Source | Notes |
|---|---|---|
| Index & ticker quotes | Yahoo Finance v8 chart API | Routed through CORS proxies (see below) |
| Pre / post-market | Same API with `includePrePost=true` on ETF symbols | Indices report `hasPrePostMarketData: false`; ETF proxies used instead |
| Fear & Greed Index | `production.dataviz.cnn.io` | Same proxy chain |
| News headlines | CNBC Markets RSS via `rss2json.com` | Free tier, no key needed |

### CORS proxy chain

Yahoo Finance sends no CORS header and cannot be called directly from a browser. The app tries three read-only proxies in order (falling through on failure):

1. **r.jina.ai** — primary; most reliable, sends `Access-Control-Allow-Origin: *`
2. **allorigins.win/raw** — passthrough fallback
3. **allorigins.win/get** — JSON-wrapped fallback (unwraps `.contents`)

Quotes are **15-min delayed** per Yahoo Finance's free tier.

---

## Stack

- Vanilla HTML / CSS / JS — zero dependencies, zero build step
- All CSS inside `<style>`, all JS inside `<script>`
- `localStorage` for watchlist persistence

---

## Deploy to GitHub Pages

1. Create a new GitHub repository.
2. Push (or upload) this folder so `index.html` is at the repo root.
3. Go to **Settings → Pages → Source** and select `main` branch, root `/`.
4. GitHub Pages will publish your site at `https://<username>.github.io/<repo>/`.

That's it — the file is self-contained and requires no server-side logic.

---

## Local usage

Just open `index.html` in any modern browser. No localhost server is needed.

> **Note:** Some browsers block outbound `fetch()` from `file://` pages. If quotes fail to load locally, open via a local server (`npx serve .`) or deploy to GitHub Pages.

---

## Known limitations

- **Delayed data** — Yahoo Finance free-tier quotes are ~15 minutes delayed.
- **Proxy availability** — The CORS proxies are free public services and may occasionally rate-limit or go offline. The app retries automatically and shows an error banner if all proxies fail.
- **Index pre/post** — The S&P 500, Nasdaq, and Dow Jones indices do not publish pre/post-market prints; the shown pre/after-hours values are the corresponding ETF (SPY/QQQ/DIA) at a different price scale.
- **News** — `rss2json.com` free tier limits the item count; the app slices to 5 client-side.

---

## License

For informational use only. Not financial advice.
