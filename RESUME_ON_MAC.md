# Resume Prompt — paste this into a new Claude Code session on Mac

Copy everything between the lines below into a fresh `claude` session
after you `cd` into the local clone of the `jake` repo.

---

I'm resuming a project that was started in Claude Code on the web. Here's the full context — please load the repo state and stand by for next steps.

## Repo
- GitHub: `jakerob1582/jake`
- Active feature branch: `claude/nyc-sunlight-patios-map-9eRhw`
- Open PR: https://github.com/jakerob1582/jake/pull/1 (base `main`)
- Files of interest:
  - `sunlight.html` — the new app (single-file, ~1450 lines)
  - `index.html` — the existing portfolio; one link added to `sunlight.html`
  - `profile-photo.jpeg` — unchanged
  - `README.md` — unchanged

To pick up where I left off:
```
git fetch origin
git checkout claude/nyc-sunlight-patios-map-9eRhw
open sunlight.html        # preview in default browser
```

## What `sunlight.html` is
A vibe-coded, dark-mode, 3D MapLibre map of NYC anchored at **Windsor Court (155 E 31st St, lng -73.97935, lat 40.74548)** that ranks ~30 curated rooftops, patios, gardens, and waterfront bars by a live **sun score** for any date between **May 1 – Nov 1, 2026** and any time of day.

### Tech
- **MapLibre GL JS 4.7.1** (CDN) with OpenFreeMap `positron` style, recolored on load to a dark navy palette.
- **SunCalc 1.9.0** (CDN) for real solar position (azimuth + altitude).
- **3D buildings**: a `fill-extrusion` layer added on top of the vector building source. Heights pulled from `render_height` / `height` / `levels` properties with fallbacks.
- **Directional shading**: `map.setLight({ anchor: 'map', color, intensity, position: [radial, azimuth, polar] })` driven by SunCalc. Color shifts cool → warm → orange → twilight purple → night blue with altitude.
- **Sun-ray overlays**: a GeoJSON line layer drawn from the sun's direction toward the highest-scoring patios.
- **No build step, no API keys**. Single static HTML file, runs from `file://`, `python3 -m http.server`, or GitHub Pages.

### Sun scoring (in `sunScore(patio, when)`)
Each patio has `windows: [[azMin, azMax], ...]` in degrees from N — the unobstructed sky directions the patio opens to. Plus a `season` array (months it's open) and a `floor` height.
- altitude < -3° → 0
- altitude < 0° → twilight glow (0–22 pts, halved if sun not in any window)
- altitude > 45° → 70 + rooftop bonus
- altitude 12–45° → 60 ± 18 based on window match
- altitude 0–12° (golden hour) → 50 + 32 if in window, with extra boost for rooftops / water-facing spots
- out of season → 0

Scores 0–100 drive: marker color + glow + size, sidebar score ring, popup stat.

### UI
- **Top left**: brand + "Home Base · Windsor Court · 155 E 31st St"
- **Top right**: pills showing live sun alt/az + phase (Dawn / Golden Hour / Daylight / Midday / Sunset / Dusk / Night)
- **Left sidebar (340px)**: filter chips (All / Rooftop / Patio / Garden / Waterfront) and a live-ranked patio list with score rings, walking time, and vibe blurbs
- **Bottom console**: date picker (May 1 – Nov 1, 2026), 24-hr time slider with rise/set/golden-hour marker ticks and a glowing "now" line, "Golden Hour" jump button, ▶ play button (auto-advances 4 min every 90 ms)
- **Top right map UI**: ⌂ recenter on Windsor Court, "3D" pitch toggle, ☀ sun-ray overlay toggle
- **Bottom right**: sun-score legend
- Mobile: sidebar/legend hide under 800px, console compresses

### Patio data (~30 spots, hard-coded in the `PATIOS` array)
Mix of Midtown / Murray Hill / Flatiron / NoMad spots near Windsor Court, plus Lower East Side, Hudson / West Side, Lower Manhattan / Seaport, Brooklyn waterfront (Wythe, William Vale), and one uptown (Met Cantor Roof Garden). Each entry has: `name, addr, lng, lat, type, floor, windows, vibe, season`.

## What's done
- App built, committed, pushed.
- PR #1 open from `claude/nyc-sunlight-patios-map-9eRhw` → `main`.
- Small "NYC Patio Sun ☀" link added to the portfolio header in `index.html`.

## Possible next moves (any of these are fair game — wait for me to pick)
1. **Merge PR #1** into `main` and enable **GitHub Pages** so `https://jakerob1582.github.io/jake/sunlight.html` goes live.
2. **Mobile redesign**: bottom-sheet patio list, larger touch targets, swipe-to-scrub time, gesture-friendly markers.
3. **True shadow occlusion**: raycast from each patio toward the sun against extruded building heights instead of the current open-window heuristic.
4. **More spots**: I have ~30 right now — could push to 60–80, especially Brooklyn (Greenpoint, DUMBO), Long Island City, and Queens waterfront.
5. **Add directions**: clickable "walk from Windsor Court" button that opens Apple Maps / Google Maps with the route.
6. **Best-spot-tonight digest**: a small "tonight's top 3 at sunset" card that auto-picks the highest-scoring patios at today's golden hour.
7. **Reservation links**: OpenTable / Resy deep links per patio.
8. **Weather overlay**: hook a free weather API and dim scores when it's forecast to be overcast.

Please confirm you can see `sunlight.html` and tell me what to do next.

---
