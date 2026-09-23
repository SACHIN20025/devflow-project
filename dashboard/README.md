# devflow — Dashboard

A responsive developer productivity dashboard built with React, Vite, and Tailwind CSS. Tracks projects, tasks, and daily completion activity in one clean, reusable component system. Runs standalone on mock data — no backend required.

## Features

- Dashboard home view with greeting, live stats, and an activity summary
- Collapsible sidebar navigation with an accessible mobile menu
- Project cards with tech tags, due dates, task counts, and progress bars
- A "commit pulse" activity heatmap — a GitHub-contribution-style graph of daily task completions
- Search across tasks/projects, plus a status filter dropdown
- Loading skeleton states and an empty state when a search/filter returns nothing
- Fully responsive, down to mobile

## Tech Stack

React 18, Vite 5, Tailwind CSS 3, lucide-react

## Project Structure

```
src/
├── App.jsx                 # Page composition, data fetching, filtering logic
├── theme.js                 # Color tokens + status/priority metadata
├── data.js                  # Mock data + fake async fetch
└── components/
    ├── Sidebar.jsx           # Nav + profile section
    ├── Topbar.jsx             # Search bar, new task button, notifications
    ├── ProjectCard.jsx        # Project card with progress + tech tags
    ├── TaskList.jsx           # Filterable task list + task row
    ├── PulseGraph.jsx         # Commit-pulse activity heatmap
    └── Primitives.jsx         # StatCard, ProgressBar, SkeletonCard, EmptyState
```

## Installation

```bash
npm install
npm run dev
```

Runs at `http://localhost:5173`.

### Build for production

```bash
npm run build
npm run preview
```

## Screenshots

> Add screenshots of the dashboard (desktop + mobile) here.

## Demo

> Add your demo video link here.
