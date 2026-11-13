# HireFlow – Frontend

React + Vite + Tailwind CSS, Axios, React Router, Recharts, Lucide React
(same stack as the Assignment Lifecycle Management project).

## Run (PowerShell)

```powershell
cd frontend
copy .env.example .env      # set VITE_API_URL if the backend is not on :8000
npm install
npm run dev                 # http://localhost:5173
```

The backend must allow CORS for `http://localhost:5173`.

## Structure

```
src/
  components/   ui kit, DataTable, ResourceListPage, ApprovalsPanel, Sidebar, Topbar, analytics widgets
  layouts/      AppLayout (sidebar + topbar), AuthLayout
  pages/        auth, dashboard, requisitions, candidates, interviews, evaluations, offers, admin, portal
  services/     api.js  (Axios instance, JWT header, 401 handling, error formatting)
  hooks/        useFetch.js
  context/      AuthContext.jsx (login / register / logout / role checks)
```

Every page calls the real API – there is no mock data. Menus and routes are filtered by role
(`CANDIDATE, HIRING_MANAGER, HR_PARTNER, INTERVIEWER, IT_OPS, SYSTEM_ADMIN`); the backend
remains the source of truth for permissions and business rules.

## API contract the frontend expects (base `/api/v1`)

Responses use `{ "success": true, "data": ..., "meta": { "page", "page_size", "total" } }`.
Lists accept `search, page, page_size, sort_by, sort_dir` plus the filters named below.

| Area | Endpoints |
|---|---|
| Auth | `POST /auth/login` → `{access_token, user:{id, full_name, email, roles[]}}`, `POST /auth/register` |
| Meta | `GET /meta/departments`, `GET /roles`, `GET /users?role=`, `GET /hiring-managers` |
| Analytics | `GET /analytics/summary?date_from&date_to&department` → `{kpis{…}, applications_by_stage[{stage,count}], offers_by_status[{status,count}], hires_by_month[{month,hires}], interviewer_utilization[{interviewer,utilization}]}` |
| Exceptions | `GET /exceptions`, `GET /exceptions/summary`, `POST /exceptions/scan`, `PATCH /exceptions/{id}` (`ground_truth_label` is never shown in the UI) |
| Requisitions | `GET/POST /requisitions`, `GET/PUT /requisitions/{id}`, `POST /requisitions/{id}/submit` |
| Approvals | `GET /approvals?entity_type=REQUISITION\|OFFER&status=`, `POST /approvals/{id}/decision {decision, comment}` |
| Candidates | `GET /candidates`, `GET /candidates/{id}`, `GET /candidates/{id}/resume`, `GET/PUT /candidates/me`, `POST /candidates/me/resume` |
| Applications | `GET/POST /applications`, `GET /applications/me`, `GET /applications/{id}` (incl. `allowed_transitions`, `stage_history`, `interviews`), `POST …/transition {to_stage, comment}`, `POST …/withdraw` |
| Interviews | `GET/POST /interviews`, `GET /interviews/me`, `GET /interviews/{id}`, `POST …/reschedule`, `POST …/cancel`, `POST …/complete`, `GET /panels`, `POST /panels`, `GET /panels/{id}/common-slots` (409 on scheduling conflicts) |
| Availability | `GET/POST /availability/{user_id}`, `DELETE /availability/{user_id}/{id}` |
| Evaluation | `GET/POST /rubrics`, `GET /interviews/{id}/scorecard-form`, `POST /interviews/{id}/scorecards`, `GET /evaluations`, `GET /interviews/{id}/evaluation`, `POST …/evaluation/finalize` |
| Offers | `GET/POST /offers`, `GET /offers/me`, `GET/PUT /offers/{id}`, `POST …/submit`, `POST …/release`, `POST …/respond {decision}` |
| Admin | `GET/POST /users`, `PUT /users/{id}`, `GET /audit-logs` |
| Misc | `GET /notifications/me`, `PATCH /notifications/{id}/read` |

If the backend response shapes differ, adjust the field names in the affected page – each page is self-contained.


if backend is working 
 make change in .env.example
 VITE_USE_MOCK_DATA=false


 system administrator
 email=adarsh@example.com
password=Adarsh@12345

candidate
email=candidate2@example.com
password=Candidate@123