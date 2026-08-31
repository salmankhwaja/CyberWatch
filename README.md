# 🛡 CyberWatch v3

**Real-time cybersecurity intelligence dashboard powered by [NewsData.io](https://newsdata.io)**

A fully self-contained, single-file HTML application. No backend. No build step. No dependencies. Open in any browser.

---

## Live Demo

> Deploy to Vercel in 30 seconds — see [Deployment](#deployment) below.
> 
> One can view the deployed app @ https://cyberwatch-dashboard-tawny.vercel.app

---

## Features

### Two Fetch Modes
| Mode | How |
|------|-----|
| **AUTO** | Build query visually using sidebar controls — keywords, domains, countries, categories, timeframe |
| **MANUAL** | Edit the raw API URL directly in the Live Query box before fetching |

### Sidebar Controls
- **Quick Fill** — one-click presets: Cybersecurity · GRC · PCI DSS · Breaches · Threat Intel
- **Keywords** — comma-separated, max 5 terms · Full Article or Title Only toggle
- **Recent Searches** — cookie-persisted, click to re-run
- **Live Query (Editable)** — see the exact URL being sent · edit it freely · save for later
- **Saved Queries** — up to 8 saved URLs persisted in `localStorage` · load with one click
- **Domains** — INC / EXC / IGN per source · 18 pre-loaded cybersec/GRC sources · add custom
- **Countries** — 15 presets (US, UK, UAE, Pakistan, India, AU, SG…) + custom ISO codes
- **Categories** — all 18 NewsData native categories · multi-select
- **Timeframe** — 12h / 24h / 48h / All presets
- **Priority Domain** — All / Top Only / Medium+
- **Results per Fetch** — 5 / 10 / 20 / 30 / 50

### Results & Export
- Auto-categorised tabs: CYBER · THREAT · GRC · PCI · BREACH
- Keyword highlighting in titles and descriptions
- Sentiment badges · source country · time-ago
- **HTML export** — full styled report
- **CSV export** — open in Excel
- **LinkedIn post** — smart hashtags based on active keyword · copy to clipboard
- **Email** — opens default mail client with formatted 10-article digest

### Developer Tools
- **Debug Console** — live coloured log: HTTP status · response time · raw preview · error hints
- Rate limit / auth error detection with remediation hints
- CORS / `file://` detection with fix instructions

### Themes
| Theme | Description |
|-------|-------------|
| Obsidian | Dark blue-grey (default) |
| Crimson | SOC terminal red |
| Midnight | Deep cyan |
| Arctic | Light / print-friendly |

All settings (theme, filters, recent searches) persisted 90 days via cookies.

---

## Quick Start

```bash
# Option 1 — just open the file
open index.html   # macOS
xdg-open index.html   # Linux

# Option 2 — local server (avoids any file:// CORS issues)
python3 -m http.server 8080
# then open http://localhost:8080
```

1. Enter your [NewsData.io API key](https://newsdata.io) in the top bar
2. Pick keywords or a Quick Fill preset
3. Click **Fetch**

---

## Deployment

### Vercel (recommended — free, HTTPS, custom domain)

```bash
# One-time setup
npm i -g vercel

# From this repo folder
vercel --prod
```

That's it. Vercel serves `index.html` as a static site. The NewsData.io API allows browser-direct calls — no proxy needed.

### GitHub Pages

1. Go to repo **Settings → Pages**
2. Source: **Deploy from a branch** → `main` → `/ (root)`
3. Your dashboard will be live at `https://salmankhwaja.github.io/CyberWatch`

### Netlify drag-and-drop

Drag the repo folder onto [app.netlify.com](https://app.netlify.com) — done in 10 seconds.

---

## API Key Security

The API key is entered by the user in the browser and never leaves the browser. It is not stored server-side anywhere.

If you want to **hardcode the key** for a private/internal deployment, replace this line in `index.html`:

```html
<input type="password" id="apiKey" placeholder="pub_xxxxxxxxxxxxxxxx" .../>
```

with:

```html
<input type="password" id="apiKey" value="pub_YOUR_KEY_HERE" style="display:none"/>
```

> ⚠ Never commit a real API key to a public repo.

---

## Changelog

### v3.3.0 — 2026-08
- **Fix:** Saved queries now persist correctly — switched from cookies to `localStorage` (cookie 4KB limit was causing silent failures)
- **Fix:** `saveQuery()` now shows correct toast: saved / duplicate / missing key — previously always showed "saved" even when nothing was stored
- **Fix:** `addSavedQ()` now returns `true` / `'dup'` / `false` so callers can respond correctly
- **Fix:** API key masking regex now uses `/g` flag — previously only masked first `apikey=` occurrence
- **Fix:** Removed duplicate `file://` CORS warning in debug console (was showing twice)
- **Fix:** Domain INC filter now uses correct API parameter `domain` (was incorrectly using `domainurl`, causing domains to be silently ignored by the API)
- **Improvement:** Countries panel now shows amber warning explaining that the `country` filter is by *source publisher location*, not article topic — explains why PK/IN/AE may return sparse results
- **Improvement:** Auto-save to query history on successful manual-mode fetch, with debug log confirmation

### v3.2.0 — 2026-08
- **Feature:** Live Query box is now fully editable — switch between AUTO (sidebar-driven) and MANUAL (free-edit) mode
- **Feature:** Saved Queries panel — save, load, and delete up to 8 queries, persisted in `localStorage`
- **Feature:** AUTO / MANUAL badge with pulsing animation in MANUAL mode
- **Feature:** Reset button returns to AUTO mode and regenerates from sidebar
- **Feature:** Copy URL button copies the current query URL to clipboard
- **Fix:** `country` filter correctly limited to 5 entries (API free-tier limit)

### v3.1.0 — 2026-08
- **Fix:** `title` and `description` null-coerced before hashing — fixed crash when NewsData returns null fields
- **Fix:** Consistent 4-space indentation throughout — no more `TabError`
- **Fix:** Stray `/` character removed from CVE card template HTML

### v3.0.0 — 2026-08
- Initial release: single-file dashboard, 4 themes, debug console, export (HTML/CSV/LinkedIn/Email), domain INC/EXC/IGN, country/category/timeframe filters, cookie state persistence

---

## Known Limitations (NewsData.io Free Tier)

| Limit | Value |
|-------|-------|
| Credits per day | 200 |
| Articles per request | 10 |
| Data freshness delay | Up to 12 hours |
| Max domains (INC or EXC) | 5 |
| Max countries | 5 |
| Historical data | Not available |

---

## File Structure

```
CyberWatch/
├── index.html      ← entire application (self-contained)
├── README.md
├── .gitignore
└── COMMIT_MSG.txt  ← ready-to-use git commit message
```

---

## License

MIT — free for personal and commercial use.

---

*Built with vanilla HTML · CSS · JavaScript · NewsData.io API*
*No frameworks. No build tools. No node_modules.*
