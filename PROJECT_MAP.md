# Daily Trend Radar

## [TECH_STACK]
- **Framework**: React 19 + TypeScript 6
- **Desktop Runtime**: Electron 42
- **UI Libraries**: Vanilla CSS (Glassmorphism)
- **Storage Method**: `electron-store` (JSON file storage for settings and cache)
- **Data-Fetching Approach**: `axios` with primary APIs and reliable fallbacks

## [SYSTEM_FLOW]
1. App starts, Electron creates a transparent, frameless desktop window.
2. App reads cache and settings via `electron-store`.
3. Cached data immediately sent to React UI for instant rendering.
4. App fetches fresh data from public APIs/RSS feeds in the background.
5. If successful, cache updates and React UI re-renders with LIVE badge.
6. If failed, previous cache is kept and UI shows ERR or STALE badges.
7. Settings panel allows user to reorder categories, toggle visibility, and configure UI opacity/autostart.
8. Background node-cron scheduler automatically repeats step 4 every X minutes.

## [ARCHITECTURE]
- `electron/` — Main process (Node.js)
  - `main.ts` — Window setup, Tray menu, IPC event listeners
  - `preload.ts` — Safe bridge between Main and Renderer
  - `store.ts` — Persistent JSON settings and cache
  - `scheduler.ts` — Cron job runner
  - `logger.ts` — Async file-based logging
  - `sources/` — Abstraction layer for fetching external data securely
- `src/` — Renderer process (React)
  - `hooks/` — Custom hooks wrapping `window.electronAPI`
  - `components/` — UI components like Dashboard, TrendCard, Settings
  - `styles/` — Vanilla CSS modules for layout and glassmorphism styling
- `public/` — Static assets (icons)

## [DATA_SOURCES]
1. **Currency**: `https://api.exchangerate-api.com/v4/latest/EGP`
   - Purpose: USD/EUR/SAR to EGP rates.
   - Fallback: `https://open.er-api.com/v6/latest/EGP`
2. **Gold**: `https://api.metalpriceapi.com`
   - Purpose: Egyptian gold prices (24k, 21k, 18k).
   - Reliability: High if API key is provided, falls back to `api.metals.live` approximation.
3. **AI Tools**: `https://hn.algolia.com/api/v1/search`
   - Purpose: Hacker News trends for AI Tools.
   - Reliability: High (Public free API).
4. **Dev Tools**: `https://dev.to/api/articles`
   - Purpose: Dev.to trending productivity articles.
   - Reliability: High (Public free API).
5. **Education**: `https://api.openalex.org/works`
   - Purpose: Recent academic papers on TESOL/Applied Linguistics.
   - Reliability: High (Open Academic Graph API).

## [VERIFIABLE_GOALS]
- [x] Create project scaffold
- [x] Configure Electron and Vite build pipeline
- [x] Implement main process with transparent window
- [x] Implement electron-store for settings and cache
- [x] Implement background cron scheduler
- [x] Abstract 5 distinct data sources with fallback protection
- [x] Implement React UI with Dashboard and glassmorphism
- [x] Implement Settings panel with priority sorting and category toggles

## [ORPHANS & PENDING]
- **API Keys**: User must register at `metalpriceapi.com` and set `VITE_METAL_PRICE_API_KEY` in `.env` to get exact reliable gold prices instead of approximated fallbacks.
