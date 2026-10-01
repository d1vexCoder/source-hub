# SourceHub — Render + Neon

## Render Environment

Set these variables:

- `TELEGRAM_BOT_TOKEN` — token from BotFather
- `TELEGRAM_BOT_USERNAME` — `sourcereg_bot`
- `SOURCEHUB_SECRET` — long random secret
- `SOURCEHUB_ADMIN_CODE` — your private 6-digit administrator code
- `DATABASE_URL` — Neon PostgreSQL connection string
- `PUBLIC_BASE_URL` — `https://python3-server-py.onrender.com`
- `CORS_ORIGINS` — `https://s0urse.netlify.app`

## Admin login

The reserved nickname is `bogMurphy` and the administrator UID is fixed to `b4174ad4-6dd0-47ff-8e38-2d0a7986a3e0`.

Two administrator paths are supported:
1. Enter `SOURCEHUB_ADMIN_CODE` with `@bogMurphy`.
2. Use the permanent 6-digit Telegram code returned by `@sourcereg_bot` with `@bogMurphy`.

The second path binds the Telegram account to the fixed administrator UID.

## Persistent sources

When `DATABASE_URL` is set, users, sessions, publications, ZIP files, covers and avatars are stored in Neon PostgreSQL. ZIP files are stored as database binary data, so they do not disappear when the Render service restarts.

Without `DATABASE_URL`, the backend falls back to local SQLite/filesystem for Pydroid/local development; Render's ephemeral filesystem should not be used for production persistence.

## Deploy

Build: `pip install -r requirements.txt`
Start: `python3 server.py`
