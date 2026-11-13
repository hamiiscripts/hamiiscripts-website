# HamiiScripts Website — Developer README

The main website for **Hamii Scripts** (https://hamiiscripts.com) — the
API platform and tools store built by Hamid Ali.

> **Note for the developer picking this up:** this README was written
> from the v3 upgrade notes. Before you trust any path below, check it
> against the actual file tree in this repo and fix it if it moved.

## What it is

- **API platform** — 17 live APIs (Free Fire friend management, wallet,
  profile/banner lookups, Dynamic Duo state and more), key-based
  subscriptions, customer + admin panels sharing one design system.
- **Tools & Software** (v3) — public `/tools/` list and detail pages
  with a counted download gateway, and an admin CRUD at
  `/admin/tools.php` (title, type, category, CDN link or file upload,
  cover + up to 6 screenshots, version/platform/size/license, free/paid
  + price, features/requirements/changelog).
- **Subscriptions** — 1 API Rs 200/month, 2 Rs 400, 5 Rs 1000,
  10 Rs 2000, all 17 Rs 4000. APIs are admin-assigned only; new keys
  start at zero balance; no API without an active subscription.
- **Top-up / payments** — JazzCash, Easypaisa, Meezan Bank, Binance,
  Sadapay, Nayapay. Verification and activation are manual in the
  admin panel — there is no automatic payment gateway.
- **Auth** — Google login required for customers; owner key is
  headers-only (owner headers are baked in as code defaults; admin
  login needs no env vars; wrong headers return 401). Customer keys
  need no header and may go via query.

## Stack

- **PHP 8.3** + **MariaDB** (developed and live-tested locally on
  MariaDB + PHP 8.3)
- Plain PHP pages + shared design system (light, clean theme — see the
  `virtuallearning.online` reference in the site's design notes)
- Hosted on Hostinger (live)

## Local setup

1. Clone the repo and serve it with PHP 8.3 (built-in server is fine
   for development):
   ```bash
   php -S localhost:8000
   ```
2. Create a MariaDB database and import the schema (see the SQL dump
   in the repo).
3. Copy `config.php.example` to `config.php` if one exists, otherwise
   create `config.php` and fill in: database credentials, Google OAuth
   credentials, mail credentials. **Never commit a `config.php` with
   real secrets** — see "Security" below.
4. Open http://localhost:8000 — the public site, `/tools/` list and
   detail pages, and `/admin/` panel should all load.

## Project layout (verify against the tree)

- `/` — public pages (home, tools list/detail, auth)
- `/tools/` — Tools & Software public pages + download gateway
  (download count increments on each download)
- `/admin/` — admin panel (`tools.php` = Tools & Software CRUD)
- `config.php` — secrets + environment config (NOT committed with
  real values — see below)
- Uploads: tool covers/screenshots and direct file uploads live in
  the uploads directory. Direct upload is capped at ~10 MB by
  Hostinger — use a CDN link for bigger software.

## Security rules (do not break these)

1. **Rotate every secret in `config.php`** (database, Google, mail
   passwords) before sharing or deploying a copy — real secrets were
   once present in a shared archive.
2. **Delete debug files from live** (`diagnose`, `debug-login`,
   `debug-otp`, `db-check`, `test-insert`, `update-admin`,
   `cleanup.php` and any `test-*`/`debug-*` file). `cleanup.php`
   once contained a hardcoded key that deleted all customers/orders.
3. Uploads must be checked by real MIME type (`finfo`) — SVG and PHP
   uploads are rejected. Directories holding uploads are blocked by
   `.htaccess`.
4. No automatic payment gateway: top-ups are verified manually in
   the admin panel.

## API quick reference

- Live API base: `https://api.hamiiscripts.com/`
- Manager (game-account control center):
  `https://manager.hamiiscripts.com/` — players log in with their
  Free Fire access token; token tool:
  `https://api.hamiiscripts.com/tool/get-access-token`
- Game API (Node.js, port 3150 in development): friend requests/list/
  decline/confirm/remove, Dynamic Duo state, wallet, profile card.

## Contributing

1. Work on a branch, keep changes limited to what was asked.
2. Test locally (PHP 8.3 + MariaDB) before pushing — list/detail
   pages must return 200 and the download gateway must redirect
   and count.
3. Never commit secrets, debug files, or database dumps with real
   customer data.

## Contact

- Developer/owner: Hamid Ali (Hamii Scripts)
- Email: hamidali@hamiiscripts.com
- Website: https://hamiiscripts.com
