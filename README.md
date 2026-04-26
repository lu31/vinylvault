# VinylVault

<p align="center">
  <img src="hero.png" alt="VinylVault" width="100%"/>
</p>

I have been collecting vinyl records for many years. Like most collectors, my system for tracking everything was somewhere between a memory and a messy CSV file. I knew what I had — mostly — but not always what I paid, whether something was a special edition, or if it was signed. My original Pink Floyd *The Wall*, signed by Roger Waters himself, deserved better than a spreadsheet row.

So I built VinylVault.

**Live app:** [lu31.github.io/vinylvault](https://lu31.github.io/vinylvault/)

---

## What it does

VinylVault is a single-page app that connects to the Discogs database and gives you a clean, fast way to manage your vinyl collection — from your browser, with no install required.

- **Barcode scanning** — point your phone camera at a sleeve, it pulls the metadata automatically
- **Discogs search** — search by artist or album, filter by format (Vinyl, CD, Cassette), sort by year
- **Market value tracking** — pulls current Discogs marketplace data for each record
- **Cover art** — auto-fetched from Discogs, or upload your own
- **Local storage** — your collection lives in your browser, no account needed
- **Export & Import** — download your collection as JSON or CSV; import it back on any device
- **Merge collections** — combine collections from multiple browsers or devices without losing existing records
- **Recent search history** — remembers your last searches
- **Dark UI** — designed to look like something worth opening every day

---

## How it works

<p align="center">
  <img src="flow.svg" alt="VinylVault user flow" width="100%"/>
</p>

## Getting started

1. Open the [live app](https://lu31.github.io/vinylvault/) or download `index.html` and open it in any browser
2. Go to **Settings** and enter your free Discogs API token
   - Get one at [discogs.com/settings/developers](https://www.discogs.com/settings/developers) → Generate new token
3. Search for a record or scan a barcode to add your first album

That's it.

---

## Built with

- Vanilla HTML, CSS, JavaScript — no frameworks, no build step
- [Discogs API](https://www.discogs.com/developers/)
- Deployed via GitHub Pages

---

## About

My background is in product design and UX strategy — twenty-plus years of thinking about products and experiences, but not writing apps from scratch. VinylVault went through 17 versions. Not because it was broken, but because every iteration revealed something worth fixing. That process taught me more than I expected.

If you collect vinyl, give it a try. It is completely free. If you like it — or don't — I would love to hear from you.

**Long live the vinyl.**

---

*An AI solution by [Lu31](https://lu31.com)*
