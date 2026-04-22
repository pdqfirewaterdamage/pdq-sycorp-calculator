# CLAUDE.md — PDQ Sycorp Drying Calculator

## Project

Restoration drying calculator for PDQ Restoration — psychrometric / dehumidifier / air-mover sizing based on Sycorp methodology. Single-page web app.

- **Repo:** https://github.com/pdqfirewaterdamage/pdq-sycorp-calculator
- **Live deploy:** https://pdq-restoration-calc.tj-rozansky.workers.dev
- **Deploy mechanism:** **Cloudflare Worker with Assets**, NOT Pages. There is **no git-push auto-deploy**. Deploy is manual via wrangler:
  ```bash
  npm run deploy   # source .env && wrangler deploy
  ```
  Pushing to `master` on GitHub does NOT ship the site. You must run the deploy script locally (or via CI) for changes to go live.

## Stack

- **Frontend:** vanilla HTML/JS — `public/index.html` is the served file
- **Runtime:** Cloudflare Worker via `assets.directory = "public"` in [wrangler.jsonc](wrangler.jsonc), `not_found_handling = "single-page-application"`
- **No server functions** — static assets only, served by the Worker's Assets runtime
- **PWA:** has `manifest.json` + `icon-192.png` / `icon-512.png` / `apple-touch-icon.png`

## WHERE TO EDIT THE FRONTEND

**The Worker serves from `public/`. Edit `public/index.html`.**

There is also a `index.html` at the repo root (currently identical to `public/index.html`). That root copy is **not served** — `wrangler.jsonc` points assets at `public/`. If you edit the root copy without syncing, deploys will not reflect your change.

Safest pattern: edit `public/index.html` directly. If you touch root, run `cp index.html public/index.html` before `npm run deploy`.

## Local dev

```bash
npm run dev   # wrangler dev (serves from public/)
```

## Do not

- Do not assume pushes to master deploy automatically. This is a Worker, not a Pages project — deploys are manual (`npm run deploy`).
- Do not confuse this with `pdq-dashboard`, `pdq-dashboard-crm`, or `pdq-facebook-group-crm` — separate projects, separate Worker.
- Do not commit `.env` (contains the Cloudflare account/token used by `wrangler deploy`).
