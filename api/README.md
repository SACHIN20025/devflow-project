# devflow API

A Node.js + Express REST API for users, projects, and tasks, backed by a real relational database via Sequelize, with JWT authentication and an AI-assisted task generation / summarization feature.

## Features

**REST API**
- User management endpoints (register, login, profile, update, delete)
- Project creation, retrieval, update, delete
- Task creation, update, deletion, and dedicated status-transition endpoint
- Centralized error handling with consistent JSON error shapes
- Input validation on every write operation (`express-validator`)
- Correct, meaningful HTTP status codes (200/201/204/400/401/403/404/409/500)
- Environment variables for all configuration and secrets

**Persistent Data Layer**
- Real database via Sequelize (SQLite by default, MySQL/Postgres by changing one env var)
- Full CRUD across Users, Projects, and Tasks
- Schema-level validation (required fields, email format, enums, string lengths)
- Relationships: User → Projects (owner), Project → Tasks, User → Tasks (assignee), with cascading deletes
- Secure configuration — no hard-coded credentials, everything reads from `.env`

**AI feature**
- `POST /api/ai/generate-tasks` — turns a plain-English project description into a starter task list
- `GET /api/ai/projects/:id/summary` — generates a short status summary from a project's current tasks
- Uses Google's Gemini API (genuinely free tier — get a key at [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey), no card required) when `GEMINI_API_KEY` is set; falls back to a local heuristic otherwise, so the feature still works in a demo without a key configured

## Tech Stack

Node.js, Express, Sequelize (SQLite by default; MySQL/PostgreSQL supported via env var), JSON Web Tokens, bcryptjs, express-validator

## Project Structure

```
src/
├── app.js                    # Express app, route mounting, error handlers
├── server.js                  # DB connection + server bootstrap
├── config/database.js         # Sequelize config (dialect from env)
├── models/                     # User, Project, Task + associations
├── middleware/                 # auth, validation, error handling
├── controllers/                 # auth, users, projects, tasks, ai
├── routes/                       # one file per resource, mounted under /api/*
└── utils/seed.js                 # Populates demo data
```

## Installation

```bash
npm install
cp .env.example .env   # at minimum, set JWT_SECRET to a random string
npm run seed              # optional: creates a demo user + sample data
npm run dev                # with auto-reload
```

The API runs at `http://localhost:5000` by default. Health check: `GET /api/health`.

## Switching Databases

By default this uses a local SQLite file (`./data/devflow.sqlite`) — zero setup required. To point it at real MySQL or PostgreSQL instead, edit `.env`:

```bash
DB_DIALECT=postgres        # or "mysql"
DB_HOST=localhost
DB_PORT=5432
DB_NAME=devflow
DB_USER=postgres
DB_PASSWORD=your-password
```

No code changes needed.

## API Reference

All responses are JSON, shaped as `{ success, data }` or `{ success: false, message, errors? }`. Protected routes require `Authorization: Bearer <token>`.

### Auth

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/register` | – | Create an account. Body: `name, email, password` |
| POST | `/api/auth/login` | – | Log in. Body: `email, password`. Returns `{ user, token }` |
| GET | `/api/auth/me` | ✅ | Get the current authenticated user |

### Users

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| GET | `/api/users` | ✅ | List all users |
| GET | `/api/users/:id` | ✅ | Get one user |
| PATCH | `/api/users/:id` | ✅ | Update name (self or admin only) |
| DELETE | `/api/users/:id` | ✅ | Delete a user (self or admin only) |

### Projects

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| GET | `/api/projects?status=&search=` | ✅ | List projects, with per-project progress % |
| GET | `/api/projects/:id` | ✅ | Get one project with its tasks |
| POST | `/api/projects` | ✅ | Create a project |
| PATCH | `/api/projects/:id` | ✅ | Update a project (owner or admin only) |
| DELETE | `/api/projects/:id` | ✅ | Delete a project and its tasks (owner or admin only) |

### Tasks

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| GET | `/api/tasks?status=&priority=&projectId=&search=` | ✅ | List/filter/search tasks |
| GET | `/api/tasks/:id` | ✅ | Get one task |
| POST | `/api/tasks` | ✅ | Create a task |
| PATCH | `/api/tasks/:id` | ✅ | Update task fields |
| PATCH | `/api/tasks/:id/status` | ✅ | Update just the status |
| DELETE | `/api/tasks/:id` | ✅ | Delete a task |

### AI

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/ai/generate-tasks` | ✅ | Body: `{ description, projectId? }` → array of suggested `{ title, priority }` |
| GET | `/api/ai/projects/:projectId/summary` | ✅ | Returns `{ summary }` |

## Notes for Evaluation

- Every write route validates input and returns 400 with field-level errors on failure
- `npm run seed` resets and repopulates the database so live CRUD is visible immediately
