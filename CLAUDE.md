# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a Flask expense tracker (₹/INR-focused) built as a step-by-step teaching project. Much of the app is intentionally unimplemented: placeholder routes in `app.py` return strings like `"coming in Step 7"`, and `database/db.py` / `static/js/main.js` are stubs describing what a later step should add. When implementing a step, replace the matching placeholder rather than adding parallel routes.

## Commands

A virtualenv lives in `venv/` (Windows layout: `venv/Scripts/`).

```bash
# Setup
python -m venv venv
venv/Scripts/pip install -r requirements.txt   # or: .\venv\Scripts\Activate.ps1 then pip install ...

# Run dev server (debug mode, http://localhost:5001)
venv/Scripts/python app.py

# Tests (pytest + pytest-flask are installed; no tests exist yet)
venv/Scripts/pytest
venv/Scripts/pytest path/to/test_file.py::test_name
```

There is no linter or build step configured.

## Architecture

- **`app.py`** — the single Flask app module; all routes are defined here directly on `app` (no blueprints, no app factory). Currently only page-rendering routes exist (`/`, `/login`, `/register`, `/terms`, `/privacy`); auth, profile, and expense CRUD (`/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`) are placeholders.
- **`database/db.py`** — planned SQLite layer (Step 1): `get_db()` returning a connection with `row_factory` and foreign keys enabled, `init_db()` using `CREATE TABLE IF NOT EXISTS`, and `seed_db()` for sample data. The DB file `expense_tracker.db` is gitignored.
- **Templates** — every page extends `templates/base.html`, which provides the navbar, footer, and blocks `title`, `head` (page-specific CSS), `content`, and `scripts` (page-specific JS). `login.html`/`register.html` already POST to `/login`/`/register` and render an optional `error` variable, but the routes currently only handle GET.
- **Styling** — `static/css/style.css` is the global stylesheet and defines design tokens as CSS variables in `:root` (ink/paper palette, `--accent` green, `--accent-2` amber, DM Serif Display + DM Sans fonts, radii). Use these variables rather than hard-coded values. Page-specific CSS (e.g. `landing.css`) is loaded via the `head` block.
- Page-specific JavaScript is written inline in the template's `scripts` block (see the video modal in `landing.html`); `main.js` is for shared JS.
