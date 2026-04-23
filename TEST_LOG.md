# VinylVault — TEST LOG

## Version History Summary

| Version | Date | Status | Summary |
|---------|------|--------|---------|
| v1.0 | 2026-04-19 | Built | Initial release — Discogs API, barcode scan, localStorage, JSON/CSV export, Uncodixfy dark UI |
| v1.1 | 2026-04-22 | Fixed | Scanner bug: camera not rendering in modal. Fixed timing + stopScanner async cleanup |
| v1.2 | 2026-04-22 | Fixed | Scanner: infinite recursion (flash) + multi-fire callback. Fixed circular call + scannerBusy guard |
| v2.0 | 2026-04-22 | Built | Major redesign — new toolbar, Discogs search dropdown, analytics overlay, itshover icons, upload image |
| v2.1 | 2026-04-22 | Fixed | Vinyl icon reset on mouseout; Filter wrap + Z-A/Value↑ options; search placeholder; Scan=primary btn; 1340px cap + vinyl tile bg |
| v2.2 | 2026-04-22 | Built | Sticky toolbar (desktop always visible, mobile hide-on-scroll-down/show-on-scroll-up); back to top button |
| v2.3 | 2026-04-22 | Built | Import JSON feature in Settings; renamed Downloads → Download your collection |
| v2.4 | 2026-04-22 | Built | Token field: type=password by default + eye icon to reveal/hide |
| v2.5 | 2026-04-22 | Built | Full-viewport hero image + centered logo; back-to-top lands at navs (not page top); Save Changes button |
| v2.6 | 2026-04-22 | Fixed | Hero: icon and title side by side in one row; subtitle colour changed to white |
| v2.7 | 2026-04-22 | Fixed | Hero subtitle alignment: align-self:stretch + text-align:center so it sits centred under the logo row |
| v2.8 | 2026-04-22 | Fixed | Mobile horizontal overflow: overflow-x:clip on html+body (safe with position:sticky, unlike overflow:hidden) |
| v2.9 | 2026-04-23 | Built | Search = Discogs only (no collection filtering); artist= API param for cleaner results; search icon fixed (flex wrapper); Delete in detail modal; click artist name to filter vault |
| v3.0 | 2026-04-23 | Fixed | Dropdown max-height 340→520px; results 10→25 per page; label shows result count + scroll hint |
| v3.1 | 2026-04-23 | Fixed | Removed results cap entirely — shows all results Discogs returns; per_page=100 (API max) |
| v3.2 | 2026-04-23 | Fixed | Double border fixed (outline:none on search-inner); API back to q= + format=Vinyl filter; placeholder updated |
| v3.3 | 2026-04-23 | Fixed/Built | Double border fully killed (-webkit-appearance+box-shadow on input); dropdown sort toggle ↑↓ (newest/oldest first, no extra API call) |
| v3.4 | 2026-04-23 | Fixed | Double border: color-scheme:dark on input (stops Chrome injecting its own dark-mode style); Dropdown stays open on sort click: e.stopPropagation() on ↑↓ buttons so DOM rebuild doesn't trigger document close listener |
| v3.5 | 2026-04-23 | Fixed/Built | Double border: :focus-visible { outline:0 } kills Chrome's macOS focus ring; Sort header sticky in dropdown (position:sticky top:0); Search history: stores up to 10 terms in localStorage, shows on focus/empty, autocompletes on partial match, × to remove |
| v3.6 | 2026-04-23 | Fixed | Hero image path updated from Image/ subfolder to root — required for GitHub Pages deployment |
| v3.7 | 2026-04-23 | Fixed | Deduplicate search results by master_id — collapses multiple pressings of same album into one entry; label now shows album count not pressing count |
| v3.8 | 2026-04-23 | Fixed | Dedup fallback was using title+year — different pressings have different years so they slipped through. Fallback now uses title only |
| v3.9 | 2026-04-23 | Fixed | Dedup still broken — master_id=0 is falsy so releases without a master fell back to title; titles varied by pressing (e.g. "(Remaster)" suffix). Added normalizeTitle(): strips "Artist - " prefix, strips parentheticals, lowercases. Key is now master_id>0 ? master : normalized title |

---

## Session 1 — 2026-04-19

### Version: v1.0
**File:** `VinylVault_v1.0.html` / working copy: `index.html`

### What was built
- Single-file HTML vinyl record collection manager
- Discogs API integration (search by barcode, search by text, full release lookup, market stats)
- Barcode scanning via html5-qrcode CDN
- localStorage persistence (`vinylvault_collection`, `vinylvault_token`)
- Add / Edit / Delete records with full form (artist, title, label, year, pressing year, country, catalog #, barcode, format, condition, market value, notes, cover image)
- Discogs auto-fill: search inside add form populates all fields from Discogs release
- Detail view: cover art, market value highlight, all metadata, refresh market value button
- Export: JSON + CSV download
- Sort: recently added, artist A–Z, title A–Z, year oldest/newest, value high
- Search: filters by artist, title, label, barcode
- Empty state with CTA
- Settings modal for Discogs token (persisted to localStorage)
- First-run: Settings modal opens automatically if no token set
- Dark Uncodixfy theme: `#0d0d0d` bg, `#c8a25a` accent, system fonts
- All DOM manipulation uses safe DOM API (no innerHTML on user content)

---

## Session 2 — 2026-04-22

### Version: v1.1
**File:** `VinylVault_v1.1.html` / working copy: `index.html`

### Bug fixed
**Scanner camera not rendering** — modal opened but camera feed never appeared.

**Root cause:** `startScanner()` was called synchronously right after `openModal()`. The modal goes from `display:none` → `display:flex` via a CSS class change, but the browser hasn't painted the new layout yet. html5-qrcode reads the width of `#scanner-region` at init time — it was reporting 0, so the scanner failed silently.

**Fix 1 — `openScannerModal`:** Added `setTimeout(..., 150)` between `openModal()` and `startScanner()` so the browser has a full frame to render the modal before the scanner measures the container.

**Fix 2 — `stopScanner`:** Rewrote cleanup to wait for `stop()` to resolve before calling `clear()`. Previously `clear()` ran synchronously after a fire-and-forget `stop()`, which could leave the scanner in a broken state on re-open.

---

## Session 3 — 2026-04-22

### Version: v1.2
**File:** `VinylVault_v1.2.html` / working copy: `index.html`

### Bugs fixed

**Bug 1 — Screen flashes, nothing registers (infinite recursion):**
`stopScanner()` called `closeModal('modal-scanner')`, which called `stopScanner()` again. The `scannerInst = null` guard prevented the camera from double-stopping, but the `closeModal ↔ stopScanner` loop kept cycling, causing the visible flash.

Fix: `stopScanner()` now closes the modal directly (`classList.remove('open')`) instead of going through `closeModal()`. `closeModal()` no longer calls `stopScanner()`. Backdrop click for scanner modal now calls `stopScanner()` directly.

**Bug 2 — Success callback fires multiple times:**
html5-qrcode calls the success callback on every video frame where a barcode is visible (~10x/sec at fps:10). Multiple async calls were racing before the scanner stopped.

Fix: added `scannerBusy` boolean flag. Set to `true` on first successful detection — all subsequent frames return immediately.

**Also fixed:** `startScanner()` now returns early if `scannerInst` already exists instead of calling `stopScanner()` (which would trigger the circular call).

**Storage note (no change needed):** localStorage persists across browser sessions. Records saved in one session are still there on next open. JSON export is the manual backup.

### Open issues / next steps (carried to v2.0)
- [ ] Test barcode scanning on a real record sleeve
- [ ] Test Discogs search results with real token
- [ ] Consider: import JSON (restore from backup)
- [x] Collection stats panel → implemented in v2.0 as Analytics overlay
- [ ] Consider: filter by condition or label
- [ ] Consider: "Want list" separate from collection

---

## Session 4 — 2026-04-22

### Version: v2.0
**File:** `VinylVault_v2.0.html` / working copy: `index.html`

### What was built (major redesign)

**Header:**
- Vinyl icon (animated spin on hover) + title + record count badge
- Analytics button (chart-bar icon) → opens analytics overlay
- Settings gear icon (animated rotate on hover) → opens settings modal

**Toolbar:**
- Compact 82px sort dropdown with short labels (Recent / A–Z / Title / Oldest / Newest / Value)
- Discogs search: single input that (a) filters collection in real-time + (b) queries Discogs API after 500ms debounce → live dropdown of results. Click a result → opens pre-filled Add Record modal
- "Scan barcode" button (scan icon, hover pulse animation) → opens scanner modal
- "+ Add record" button (solid accent) → opens blank add form

**Settings modal — 2 sections:**
- API Configuration: Discogs token input + test token button
- Downloads: Export JSON + Export CSV buttons (moved from header)

**Add Record modal:**
- No embedded Discogs search (removed — toolbar search handles it)
- Cover image area: URL input OR upload local file (FileReader → base64 data URL stored in localStorage)
- Upload triggers file picker, converts to base64, shows preview immediately

**Analytics overlay:**
- Total records, total collection value, average value (3 stat cards)
- Top artists bar chart (horizontal, up to 12 artists, normalized to max count)
- Decade timeline (vertical bars, normalized to max count)

**Icons — itshover SVGs with CSS animations:**
- Vinyl: disc spins on hover (`animation-play-state: paused → running`)
- Gear: rotates 360° on hover (one-shot `animation:` on hover)
- Scan: scan-line pulses on hover (`animation-play-state: paused → running`)
- Magnifier: slight scale on hover (CSS transition)
- X close: rotates 90° on hover
- Chart-bar: bars grow on hover
- Upload arrow: translates up on hover
- Download arrow: translates down on hover

**Technical notes:**
- Camera permission reminder: serve via `python3 -m http.server 8080` from VinylVault folder; access at `http://localhost:8080` — Chrome remembers camera permission permanently for localhost
- All SVG icons are static hardcoded markup (no user data via innerHTML)
- Image storage: base64 data URLs in localStorage; practical limit ~5–10MB for reasonable collections

### Open issues / next steps
- [ ] Test barcode scanning on a real record sleeve at localhost
- [ ] Test Discogs search dropdown with real token
- [ ] Consider: import JSON (restore from backup)
- [ ] Consider: filter by condition or label
- [ ] Consider: "Want list" separate from collection
