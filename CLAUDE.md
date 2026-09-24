# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendly is a Flask-b  Many modules contain placeholder comments like `# Students will write this file in Step 1` or routes that literally return `"Logout — coming in Step 3"`. When asked to implement a "Step", check `app.py` and `database/db.py` for these markers to see what's expected — don't assume unimplemented areas are bugs.

## Commands

Activate the virtualenv before running anything (Windows):

```
venv\Scripts\activate
```

Run the app locally (serves on port 5001, debug mode on):

```
python app.py
```

Install/update dependencies:

```
pip install -r requirements.txt
```

Run tests (pytest + pytest-flask are declared in `requirements.txt`, but no test files exist yet — add them under a `tests/` directory):

```
pytest
pytest tests/test_file.py::test_name   # single test
```

## Architecture

- **Single-file Flask app** (`app.py`) — all routes are defined directly on the `app` object, no blueprints. Implemented routes: `/` (landing), `/register`, `/login`, `/terms`, `/privacy`. Placeholder routes awaiting implementation: `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`.
- **Auth forms have no backend yet**: `register.html` and `login.html` POST to `/register` and `/login`, but those routes are only registered for `GET` — submitting either form currently 405s until POST handling and `database/db.py` are implemented.
- **Database layer is unimplemented**: `database/db.py` is a stub describing three functions to build — `get_db()` (SQLite connection, row_factory + foreign keys), `init_db()` (CREATE TABLE IF NOT EXISTS), `seed_db()` (sample data). `database/__init__.py` is empty.
- **Templates use Jinja2 inheritance from `templates/base.html`**, which owns the `<head>`, navbar, footer, and the single global stylesheet/script includes. Page templates only fill `{% block title %}`, `{% block content %}`, and optionally `{% block head %}` / `{% block scripts %}`. Route-to-template links (nav, footer, CTAs) use `url_for('<endpoint>')`, not hardcoded paths — new routes must keep this convention so templates don't break.
- **One global stylesheet**: `static/css/style.css` (not split per page). It's theme-driven via CSS custom properties defined in `:root` (`--ink`, `--paper`, `--accent`, radii, fonts, etc.) — reuse these variables rather than hardcoding new colors/spacing. Fonts are Google Fonts `DM Serif Display` (headings) and `DM Sans` (body), loaded in `base.html`.
- **`static/js/main.js` is currently an empty placeholder** for global JS; page-specific JS (e.g. the landing page's video modal) lives inline in that template's `{% block scripts %}` instead.
- Legal pages (`terms.html`, `privacy.html`) use inline styles rather than stylesheet classes, matching each other's structure exactly — follow that pattern if adding another static content page rather than introducing new CSS classes.
