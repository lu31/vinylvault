# VinylVault

<p align="center">
  <img src="hero.png" alt="VinylVault" width="100%"/>
</p>

## The Story

I have been collecting vinyl records for many years. Like most collectors, my system for tracking everything was somewhere between a memory and a messy CSV file. I knew what I had, mostly, but not always what I paid, whether something was a special edition, or if it was signed. My original Pink Floyd *The Wall*, signed by Roger Waters himself, deserved better than a spreadsheet row.

So I built VinylVault, not by sitting down with a code editor, but through a long and iterative conversation with Claude Code, across seventeen versions.

What I brought to it was twenty-plus years of product design experience. I knew what the app needed to feel like, how the flow had to work, where friction would kill the experience, and when something just looked done versus when it was actually right. Those instincts don't come from a tutorial. They come from years of shipping real products, working with real users, and learning to tell the difference between a good decision and a comfortable one.

The AI handled the implementation. I handled the product thinking. That combination, a designer who knows what to build and why, working with an AI that knows how to build it, is the real story behind VinylVault. The barrier to shipping something real is no longer writing code. It's knowing what to build.

**Live app:** [lu31.github.io/vinylvault](https://lu31.github.io/vinylvault/)

---

## Updates

### v6.0 — May 2026

Every record in your vault now has a Videos tab.

Open any album and you'll see it next to Info and Tracklist. It searches YouTube for official videos related to that album — music videos, live performances, whatever exists — and shows them as a thumbnail grid. Click one and it plays right there inside the overlay. No new tab, no leaving the app.

The one thing it needs is a YouTube Data API v3 key, which you add once in Settings. The key is free, takes about five minutes to set up through Google Cloud Console, and stays in your browser — it never leaves your device. If you haven't set one up yet, the tab tells you exactly how to get one.

Some videos can't be embedded due to restrictions set by the rights holders. For those, a Watch on YouTube link appears below the player so you're never stuck.

### v5.0 — May 2026

The background image you see when VinylVault loads has always been the same one I chose when I built the thing. That made sense when it was just mine. It makes less sense now that other people are using it.

So in this version you can change it. Open Settings, scroll to the bottom, and you'll find a new section for the hero cover image. You can upload something from your computer, or paste a URL from the web. Either way, you get a preview before anything is saved. If you paste a URL and it ever breaks, the app quietly falls back to the default so nothing looks broken. Accepted formats are JPG, PNG, and WebP — no animated GIFs, they're too heavy. The recommended size is 1440 × 900 px or larger, and there is a 2 MB cap to keep things from choking the browser.

Your vault should feel like yours.

### v4.1 — May 2026

VinylVault is now licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/). Free to use, share, and build on for personal purposes. Not for commercial use. The license notice is visible in the app footer so there is no ambiguity about how it can be used.


### v4.0 — May 2026

The record detail view got a meaningful upgrade. What was a single panel showing cover art, value, and metadata is now a tabbed interface with two views.

The first tab, Info, is exactly what it was before — nothing changed there.

The second tab, Tracklist, is new. Opening it fetches the full track list from Discogs, then checks the iTunes catalogue in parallel to see which tracks have audio previews available. The ones that do get a small icon next to them. Tapping a track expands the row: the album cover appears as a spinning vinyl record, the preview plays automatically, and a close button collapses it again when you're done. The whole thing works without a login, an account, or an extra API key.

The detail modal also got a layout fix along the way. The header and footer are now always visible regardless of how long the content is, with only the content area scrolling. A small change, but it was overdue.


---

## What it does

Here's what came out of that process. VinylVault is a single-page app that connects to the Discogs database and gives you a clean, fast way to manage your vinyl collection — from your browser, with no install required.

- **Dark UI** — designed to look like something worth opening every day
- **Barcode scanning** — point your phone camera at a sleeve, it pulls the metadata automatically
- **Discogs search** — search by artist or album, filter by format (Vinyl, CD, Cassette), sort by year
- **Market value tracking** — pulls current Discogs marketplace data for each record
- **Cover art** — auto-fetched from Discogs, or upload your own
- **Custom hero image** — upload a photo from your computer or paste a URL to personalise the background; falls back to the default automatically if the link breaks
- **YouTube Videos tab** — browse official videos for any album, play them inline; bring your own free YouTube API key
- **Local storage** — your collection lives in your browser, no account needed
- **Export & Import** — download your collection as JSON or CSV; import it back on any device
- **Merge collections** — combine collections from multiple browsers or devices without losing existing records
- **Recent search history** — remembers your last searches

---

## Screenshots

| Collection | Search |
|:---:|:---:|
| ![Collection](screenshot-collection.png) | ![Search](screenshot-search.png) |

| Analytics | Settings |
|:---:|:---:|
| ![Analytics](screenshot-analytics.png) | ![Settings](screenshot-settings.png) |

<p align="center">
  <img src="screenshot-scan.png" alt="Barcode Scanner" width="50%"/>
  <br><em>Barcode scanner — point your camera at the sleeve</em>
</p>

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
- iTunes Search API — 30-second track previews, no account required
- Built conversationally with [Claude Code](https://claude.ai/code) by Anthropic
- Deployed via GitHub Pages

---

## About

If you collect vinyl, give it a try. It is completely free. If you build things with AI and want to compare notes, find me at [lu31.com](https://lu31.com).

Long live the vinyl.

— Lu Tapuch, Product Designer | Vibe Coder -

---

*Licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) — free for personal use, not for commercial use.*
