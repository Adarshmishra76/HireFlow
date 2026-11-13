# HireFlow – Frontend

Web client for the HireFlow recruitment management system, built with **React**, **Vite** and **Tailwind CSS**.

## Features

- Login and registration with JWT handling and automatic logout on `401`
- Role-based menus and protected routes
  (`CANDIDATE`, `HIRING_MANAGER`, `HR_PARTNER`, `INTERVIEWER`, `IT_OPS`, `SYSTEM_ADMIN`)
- Modules: dashboard and analytics, requisitions and approvals, candidates and application tracking,
  interviews (scheduling, calendar, panels, availability), evaluations and scorecards, offers,
  admin (users, roles, audit logs) and a candidate portal (jobs, applications, interviews, offers, profile)
- Reusable UI kit: data tables, KPI cards, status badges, charts

## Tech Stack

React 18 · Vite 5 · Tailwind CSS 3 · React Router 6 · Axios · Recharts · Lucide React

## Getting Started

**Prerequisites:** Node.js 18+

```bash
cd frontend
cp .env.example .env        # Windows PowerShell: copy .env.example .env
npm install
npm run dev
```

The app runs at **http://localhost:5173**

| Script | Purpose |
|---|---|
| `npm run dev` | Start the dev server |
| `npm run build` | Production build into `dist/` |
| `npm run preview` | Preview the production build |

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `VITE_API_URL` | `http://localhost:8000/api/v1` | Backend base URL |
| `VITE_USE_MOCK_DATA` | `true` | `true` uses built-in mock data, `false` calls the real backend |

To use the backend, set `VITE_USE_MOCK_DATA=false` in `.env` and restart the dev server.
The backend must be running and allow CORS for `http://localhost:5173`.

## Project Structure

```
src/
├── components/   # UI kit, DataTable, ResourceListPage, ApprovalsPanel, Sidebar, Topbar, analytics widgets
├── layouts/      # AppLayout (sidebar + topbar), AuthLayout
├── pages/        # auth, dashboard, requisitions, candidates, interviews,
│                 # evaluations, offers, admin, portal
├── services/     # api.js (Axios instance, JWT header, 401 handling), mockData.js
├── hooks/        # useFetch.js
├── context/      # AuthContext.jsx (login, register, logout, role checks)
├── App.jsx       # Routes
└── main.jsx      # Entry point
```

## API Contract

Base path `/api/v1`. Responses are expected as:

```json
{ "success": true, "data": {}, "meta": { "page": 1, "page_size": 20, "total": 0 } }
```

List endpoints accept `search`, `page`, `page_size`, `sort_by`, `sort_dir`, plus resource-specific filters.

| Area | Endpoints |
|---|---|
| Auth | `POST /auth/login`, `POST /auth/register` |
| Meta | `GET /meta/departments`, `/roles`, `/users`, `/hiring-managers` |
| Analytics | `GET /analytics/summary` |
| Exceptions | `GET /exceptions`, `/exceptions/summary`, `POST /exceptions/scan`, `PATCH /exceptions/{id}` |
| Requisitions | `GET/POST /requisitions`, `GET/PUT /requisitions/{id}`, `POST /requisitions/{id}/submit` |
| Approvals | `GET /approvals`, `POST /approvals/{id}/decision` |
| Candidates | `GET /candidates`, `GET /candidates/{id}`, `GET/PUT /candidates/me`, `POST /candidates/me/resume` |
| Applications | `GET/POST /applications`, `GET /applications/me`, `POST /applications/{id}/transition`, `/withdraw` |
| Interviews | `GET/POST /interviews`, `/interviews/me`, `/reschedule`, `/cancel`, `/complete`, `/panels` |
| Availability | `GET/POST /availability/{user_id}`, `DELETE /availability/{user_id}/{id}` |
| Evaluation | `/rubrics`, `/interviews/{id}/scorecards`, `/evaluations`, `/evaluation/finalize` |
| Offers | `GET/POST /offers`, `/offers/me`, `/submit`, `/release`, `/respond` |
| Admin | `GET/POST /users`, `PUT /users/{id}`, `GET /audit-logs` |
| Misc | `GET /notifications/me`, `PATCH /notifications/{id}/read` |

## Backend Compatibility

The frontend was designed against the full contract above. The current FastAPI backend implements a smaller
set (auth, jobs, applications, admin role management) with different roles (`candidate`, `recruiter`, `admin`)
and response shapes. With `VITE_USE_MOCK_DATA=false`, pages whose endpoints are not yet implemented will fail
until the backend is extended or the page's field names are adjusted. Each page is self-contained, so
changes stay local.

## Build and Deploy

```bash
npm run build
```

Serve the `dist/` folder from any static host and set `VITE_API_URL` to the deployed API before building.
Add the deployed origin to the backend's CORS list.
