# devflow

A developer productivity platform: project and task management with an AI-assisted planning feature, built as three parts.

## Structure

| Folder | What it is |
|---|---|
| `dashboard/` | Standalone frontend dashboard — React + Vite + Tailwind, runs on mock data, no backend needed |
| `api/` | REST API for users, projects, and tasks — Express + Sequelize, SQLite by default (swappable to MySQL/Postgres) |
| `app/` | The full product — authentication, live project/task management, AI-assisted task generation and summarization, wired to `api/` |

Each folder is a fully independent project with its own `package.json`, `README.md`, and `.env.example`.

## Run Order

`app/` depends on `api/` being up. `dashboard/` is fully standalone.

```bash
# Terminal 1 — backend
cd api
npm install
cp .env.example .env        # set JWT_SECRET at minimum
npm run seed                  # optional demo data
npm run dev                   # http://localhost:5000

# Terminal 2 — the full app
cd app
npm install
cp .env.example .env        # VITE_API_BASE_URL should point at the backend above
npm run dev                   # http://localhost:5173

# The standalone dashboard, any time
cd dashboard
npm install
npm run dev                   # pick a different port if the app above is also running
```

Demo login for `app/` (after `npm run seed` on the backend): `riya@devflow.io` / `password123`.

## AI Feature

`api/` calls Google's Gemini API (free tier, no card required — get a key at [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)) when `GEMINI_API_KEY` is set in its `.env`. Without a key, it falls back to a local heuristic so the feature still works end-to-end in a demo.
