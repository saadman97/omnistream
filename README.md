# Vault — Open Directory Video Browser

A Manifest V3 Chrome extension that turns a set of HTTP open directories (FTP mirrors, index pages) into a fast, searchable local library. It crawls every server concurrently, stores the file list in IndexedDB, and lets you search, filter, and play videos straight in the browser.

No build step, no `node_modules`, no frameworks — vanilla JavaScript and hand-written CSS.

## Features

- **Fast concurrent crawler** — per-host round-robin scheduling keeps every server saturated at Chrome's 6-socket limit instead of hammering one host; request timeouts, retries, depth limits, batched IndexedDB writes.
- **Stop & resume** — progress is checkpointed; an interrupted crawl can be resumed from where it stopped.
- **Incremental updates** — the old index stays visible while a new crawl runs; stale entries are removed when it completes.
- **Instant search** — the whole library lives in memory; filter with prefixes: `ext:mkv`, `is:series`, `is:movie`, `server:ftp4`.
- **Series grouping** — episodes collapse into one folder card; open it to see every episode.
- **Lazy thumbnails** — frames are captured from the video as you scroll (throttled, black-frame retry) and cached in IndexedDB.
- **Built-in player** — `player.html` with a folder playlist, auto-next, resume position and keyboard shortcuts.
- **Settings page** — add or remove servers (with on-demand host permission), tune crawler limits, thumbnails and library options, export/import the server list, clear data.

## Install

1. Open `chrome://extensions/` and enable **Developer mode**.
2. Click **Load unpacked** and choose this folder.
3. Click the Vault icon (or press `Alt+Shift+V`) to open the library.

## Use

1. Click **Update index**. The strip under the header shows live progress; the server rack in the sidebar lights up per server.
2. Search with `/`, pick a category, or click a server to filter. `←` `→` page through results.
3. Click a card to play (videos) or open (everything else). Hover a card for copy-link, download and open-folder actions.
4. Open **Settings** (gear icon) to add your own servers. Enter the **http://** address of the directory — Chrome no longer supports `ftp://`.

## Files

| File | Purpose |
| --- | --- |
| `manifest.json` | MV3 manifest. Host permissions for the default servers plus optional `http://*/*` / `https://*/*` requested when you add a server. |
| `background.js` | Service worker: crawler, scheduler, checkpoints, dynamic CORS rules. |
| `shared.js` | Constants, settings/servers storage helpers, IndexedDB helpers. Loaded by the worker (`importScripts`) and every page. |
| `browser.html/js` | Library UI. |
| `settings.html/js` | Settings page (also the extension's options page). |
| `player.html/js` | Video player with playlist. |
| `browser.css` | The design system shared by all pages. |
| `rules.json` | Static declarativeNetRequest rules that add CORS headers for the default hosts so thumbnail capture works. |
