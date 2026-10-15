# Matrix360 Social Media Manager — consolidated Lakhotia pilot (v1.3 code base)

This is a **single deployable application**, not multiple nested upgrade folders. The consolidated Python modules were sourced from the existing v1.3 local test application. Historical creative records, SQLite DB, local PNGs and private credentials are **not packaged**.

## Local Windows run

1. Open this folder in Command Prompt.
2. `python -m pip install -r requirements.txt`
3. Copy `.env.example` to `.env` and edit **locally only**. `.env` is gitignored. Note: a plain Uvicorn command **does not automatically read `.env`**; use `python local_start.py` instead.
4. `python local_start.py` then visit `http://127.0.0.1:8001/`.

## GitHub

Create a **private** Matrix360repo repository, or commit these files to a clean branch of `Matrix360repo/socialmediamanager`. Upload **the contents of this folder to repository root** (app.py visible at root), not a ZIP or parent directory. Never commit `.env`, `data/pilot.sqlite3` or rendered PNGs. History already on a public repo may contain sensitive files; removing them in a later commit does not erase history.

## Render

1. Use **New > Blueprint** and choose the Matrix360 repository/branch containing `render.yaml` at root.
2. Review `starter` and persistent disk charges **before confirming**. Render persistent disks are paid; free ephemeral service would lose draft history on restart.
3. In Render Environment, set `OPENAI_API_KEY` (Secret; do not paste `.env` contents wholesale). Blueprint creates `MATRIX360_API_KEY` randomly. Set the same long secret as `X-Matrix360-Key` credential in n8n Cloud. `MATRIX_MODEL` defaults to `gpt-4.1-mini`.
4. Deploy, then GET `/health` (should be public) and test authenticated app request. Browser uses HTTP Basic prompt; username can be anything, password is `MATRIX360_API_KEY`.
5. Run one generation, generate a revised creative, approve and export. Restart deployment and verify draft survived.

**Never commit the real `.env` file.** Production config belongs in Render Environment, not in Git. A Render free service does not have a durable disk and is **not** acceptable for this SQLite-based version.

## Current limits

- Not multi-tenant; current content and approved brand apply only to Lakhotia.
- AI generates textual content and chooses one of predefined visual layouts; it does **not** make unrestricted AI imagery.
- AI provider calls have **not** been live-tested here against your key; API credits/model access are required.
- No live social-media publishing or advertising expenditure.
- The `data/brand_profile.json` on an existing Render disk is retained across redeploys and must be deliberately updated if approved brand facts change.
- Basic auth is adequate only for a tightly controlled internal pilot, not a production multi-user approval service. Add proper authentication, per-user audit and backups for production.
