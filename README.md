# LinkedIn Connections Graph

A single-file, browser-only visualizer for your LinkedIn connections. Upload your `Connections.csv` and see your network as an Obsidian-style force-directed graph — bubbles of people clustered by company, role, or year — with **You** at the center.

Everything runs locally in the browser. Your data is never uploaded anywhere.

[**Live demo →**](https://kosuvorov.github.io/linkedin-connections-graph/)

## Features

- **One-click upload** of LinkedIn's `Connections.csv` — handles the export's leading "Notes:" preamble robustly (so it doesn't break like some other tools when the preamble shape varies).
- **Group by**:
  - Company
  - Role keyword (extracts Engineer / Founder / Sales / Director / etc. from messy position strings)
  - Position (exact string)
  - Connected — year
  - Connected — month / year
- **Bubble clusters** — every pair of people in the same group is connected, which under D3's force layout pulls them into tight clusters, the same way the original [linkedin-network-visualization](https://github.com/Thanh-To/linkedin-network-visualization) tool does it.
- **Min group size** — hide groups smaller than N to keep the graph legible (default 3).
- **You node** — central anchor node connected to every visible person.
- **Hover** for tooltip with name, position, company, and connection date.
- **Click** a person to open their LinkedIn profile.
- **Search** highlights matching nodes across all fields.
- **Drag**, **scroll-zoom**, and **pan** on the graph.

## How to use

1. Export your connections from LinkedIn:
   - Settings → Data Privacy → **Get a copy of your data** → check **Connections** → request archive.
   - LinkedIn emails you a `Connections.csv` within a few minutes.
2. Open the [live demo](https://kosuvorov.github.io/linkedin-connections-graph/) (or this repo's `index.html` locally).
3. Click **Upload Connections.csv** (or drop the file onto the page).
4. Switch the **Group by** dropdown to slice your network differently.

## Privacy

The app is a single static HTML file. The CSV is parsed in your browser using the [`FileReader`](https://developer.mozilla.org/en-US/docs/Web/API/FileReader) API and never leaves your machine. There is no backend, no analytics, no tracking. You can verify by opening DevTools → Network while uploading.

## Running locally

Just open `index.html` in a browser. No build step. The only external dependency is D3 v7, loaded from a CDN — for fully offline use, download `d3.v7.min.js` and replace the `<script src="…">` tag.

## Why the bubbles look the way they do

There's no community-detection algorithm here. The bubble effect comes from:

1. Every pair of people in the same group is connected by a direct edge (a clique). For very large groups (>18 members) it falls back to a sparse ring + chords pattern to keep edge counts manageable.
2. D3's `forceManyBody` has a capped `distanceMax`, so node repulsion is local — distant clusters don't push each other apart.
3. No `forceCollide`, so members of a group can pack tightly together.

## Credits

Inspired by [Thanh-To/linkedin-network-visualization](https://github.com/Thanh-To/linkedin-network-visualization), which used the same clique-of-edges-per-group technique to produce the bubble layout.

## License

[MIT](LICENSE)
