# Network Engineering Study Tracker — PWA (Pre-Supabase)

This is the pre-Supabase version of the 30-day Network Engineering study tracker.

## Included
- `index.html` — complete mobile-first study tracker
- `manifest.json` — Progressive Web App metadata
- `service-worker.js` — offline app-shell caching
- `icons/icon-192.png` and `icons/icon-512.png` — Android/PWA icons

## Current storage
Progress is stored in the browser using `localStorage`.

That means:
- Progress survives closing/reopening the site on the same browser/device.
- Phone and laptop do NOT share progress yet.
- Supabase authentication + cloud progress synchronization will replace this storage later.

## GitHub Pages
Upload all files while preserving the folder structure. The repository root should contain:

```
index.html
manifest.json
service-worker.js
icons/
  icon-192.png
  icon-512.png
```

Then enable GitHub Pages from the repository's Settings → Pages → Deploy from a branch → `main` → `/ (root)`.

## Android installation
Open the published HTTPS GitHub Pages URL in a supported browser such as Chrome. Use the browser's `Install app` / `Add to Home screen` option. The page is configured as a standalone PWA.

The service worker requires HTTPS in normal browser use. GitHub Pages provides HTTPS.

## Important security note
There is no login in this version. Do not add passwords or other secrets to this version. The real authentication system will be added with Supabase in the next stage.
