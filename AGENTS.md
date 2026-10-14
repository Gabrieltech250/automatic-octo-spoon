# AGENTS.md — MUSORICHO Wines & Spirits POS

## What this is
A browser-only POS app (single-page React) with **no build step, no backend, no package.json**. It runs entirely client-side:
- `index.html` loads React 18, ReactDOM, Recharts, Babel standalone, and Firebase from CDNs (unpkg/gstatic)
- `app.js` is loaded as `<script type="text/babel">` — Babel transpiles JSX in-browser at runtime
- `style.css` holds the static page reset; component styles are generated at runtime by app.js
- Firebase (Firestore + Google Auth) provides optional cloud sync; **localStorage is the primary store** and the app works fully offline without Firebase

## Running in the sandbox
- Served by nginx (static files) via `docker-compose.base44.yml` on host port 3000
- No live-reload dev server exists for this project (no bundler/build step) — file changes are visible on page refresh; call `reload_preview` after edits
- nginx runs as `user root` because the sandbox repo directory has restrictive (700) permissions that the default nginx worker can't read
- Healthcheck uses `127.0.0.1` (not `localhost`) because Alpine resolves localhost to IPv6 `::1` and nginx listens on IPv4 only

## Secrets
- **None required.** Firebase config (apiKey, projectId, etc.) is hardcoded inline in `index.html`. The app degrades gracefully to localStorage-only when Firebase is unreachable or the user isn't signed in.

## Verification
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → 200
- Preview shows the login screen (Google sign-in) — the app's default state for unsigned-in users
