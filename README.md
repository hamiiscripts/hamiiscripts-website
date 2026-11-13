# HamiiScripts Website — Developer README

The main website for **Hamii Scripts** (https://hamiiscripts.com) — the API platform and tools store built by Hamid Ali.

> **Checked against the actual file tree (2026-11-13):** this repo contains only the static front-end — `index.html` plus `css/`, `js/`, `images/`, `fonts/` and `favicons/`. The PHP / MariaDB backend (API platform, `/tools/` and `/admin/` pages, `config.php`, SQL schema, uploads) is the live site's code and is **not** in this repo.

## What it is

- **API platform** — 17 live APIs (Free Fire friend management, wallet, profile/banner lookups, Dynamic Duo state and more), key-based subscriptions, customer + admin panels sharing one design system.
- **Tools & Software** (v3) — public `/tools/` list and detail pages with a counted download gateway, and an admin CRUD at `/admin/tools.php` (title, type, category, CDN link or file upload, cover + up to 6 screenshots, version/platform/size/license, free/paid + price, features/requirements/changelog). *(Live site only — the `/tools/` and `/admin/` PHP pages are not in this repo.)*
- **Subscriptions** — 1 API Rs 200/month, 2 Rs 400, 5 Rs 1000, 10 Rs 2000, all 17 Rs 4000. APIs are admin-assigned only; new keys start at zero balance; no API without an active subscription.
- **Top-up / payments** — JazzCash, Easypaisa, Meezan Bank, Binance, Sadapay, Nayapay. Verification and activation are manual in the admin panel — there is no automatic payment gateway.
- **Auth** — Google login required for customers; owner key is headers-only (owner headers are baked in as code defaults; admin login needs no env vars; wrong headers return 401). Customer keys need no header and may go via query.

## Stack

- **PHP 8.3** + **MariaDB** (developed and live-tested locally on MariaDB + PHP 8.3)
- Plain PHP pages + shared design system (light, clean theme — see the `virtuallearning.online` reference in the site's design notes)
- Hosted on Hostinger (live)
- **This repo:** static front-end only — `index.html`, `css/`, `js/`, `images/`, `fonts/`, `favicons/`. No PHP files are in this repo.

## Local setup

1. Clone the repo and serve the static front-end (PHP's built-in server works for the static files, or use any static server):
   ```bash
   php -S localhost:8000
   ```
2. This repo has no backend: there is no SQL dump, no `config.php` / `config.php.example`, and no `/tools/` or `/admin/` pages here. Do not create a `config.php` in this repo — secrets belong to the live backend only and must never be committed.
3. Open http://localhost:8000 — the static home page (`index.html`) should load. The `/tools/` list/detail pages and `/admin/` panel are part of the live PHP backend and are not in this repo.

## Project layout (checked against the tree)

- `index.html` — the site's home page (static)
- `css/` — stylesheets
- `js/` — front-end scripts
- `images/` — site images
- `fonts/` — web fonts
- `favicons/` — favicons
- `README.md` — this file
- Not in this repo: `/tools/` and `/admin/` PHP pages, `config.php`, SQL schema/dump, and the uploads directory — those belong to the live PHP + MariaDB backend.

## Security rules (do not break these)

1. **Rotate every secret in the live backend's `config.php`** (database, Google, mail passwords) before sharing or deploying a copy — real secrets were once present in a shared archive. `config.php` is not in this repo and must never be committed with real values.
2. **Delete debug files from live** (`diagnose`, `debug-login`, `debug-otp`, `db-check`, `test-insert`, `update-admin`, `cleanup.php` and any `test-*`/`debug-*` file). `cleanup.php` once contained a hardcoded key that deleted all customers/orders. (None of these files are in this repo.)
3. Uploads must be checked by real MIME type (`finfo`) — SVG and PHP uploads are rejected. Directories holding uploads are blocked by `.htaccess`. (Live backend rule.)
4. No automatic payment gateway: top-ups are verified manually in the admin panel.

## API quick reference

- Live API base: `https://api.hamiiscripts.com/`
- Manager (game-account control center): `https://manager.hamiiscripts.com/` — players log in with their Free Fire access token; token tool: `https://api.hamiiscripts.com/tool/get-access-token`
- Game API (Node.js, port 3150 in development): friend requests/list/decline/confirm/remove, Dynamic Duo state, wallet, profile card.

## Contributing

1. Work on a branch, keep changes limited to what was asked.
2. Test locally before pushing — the static home page must return 200. (The PHP + MariaDB list/detail and download-gateway checks apply to the live backend, not this repo.)
3. Never commit secrets, debug files, or database dumps with real customer data.

## Contact

- Developer/owner: Hamid Ali (Hamii Scripts)
- Email: hamidali@hamiiscripts.com
- Website: https://hamiiscripts.com
