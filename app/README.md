# devflow — App

The full product: authentication, live project/task management, and an AI-assisted planning feature, all wired to the `api` backend in this same submission.

## Features

- **Authentication** — register, login, logout, JWT-protected routes
- **Dashboard** — live stats, project overview, and a task list, pulled from the real API
- **Project management** — list, create, view details, delete; each card shows live progress computed from its tasks
- **Task management** — create, update status inline, set priority and due dates, delete, plus search and status filtering
- **AI feature**
  - **AI-assisted task generation** — describe a project in plain English, get a proposed task list back, add some or all of it with one click
  - **Task summarization** — generates a short natural-language status update for a project from its current tasks
  - Both work with a real (free) Gemini API key on the backend, or fall back to a clearly-labeled local heuristic

## Tech Stack

React 18, Vite, Tailwind CSS, React Router v6

## Project Structure

```
src/
├── App.jsx                     # Routes: login, register, and a protected shell
├── api/client.js                # fetch wrapper — attaches JWT, normalizes errors
├── context/AuthContext.jsx      # login/register/logout + current user
├── pages/
│   ├── Login.jsx / Register.jsx
│   ├── Dashboard.jsx             # live stats + task list, pulled from the API
│   ├── Projects.jsx              # project grid + "New Project" modal
│   └── ProjectDetail.jsx         # task board, AI generate modal, AI summary card
├── components/                    # AppShell, ProtectedRoute, Sidebar, Topbar, etc.
└── theme.js                       # shared color tokens + status/priority metadata
```

## Running Locally

This app needs the `api` backend running first.

```bash
# 1. Start the backend
cd ../api
npm install
cp .env.example .env
npm run seed
npm run dev

# 2. In a separate terminal, start this app
cd app
npm install
cp .env.example .env    # point VITE_API_BASE_URL at the backend above
npm run dev
```

Open `http://localhost:5173`. If you ran `npm run seed` on the backend, log in with:

```
email:    riya@devflow.io
password: password123
```

Otherwise, click **Register** to create a fresh account.

## Enabling Real AI Output

Without any setup, the AI features work using a local fallback. To get real AI-generated task suggestions and summaries, add a free key to the **backend's** `.env`:

```
GEMINI_API_KEY=your-key-here
```

Get one at [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) — no credit card required.

## Build for Production

```bash
npm run build
npm run preview
```

Update `VITE_API_BASE_URL` to your deployed backend's URL before building for a real deployment.
