# HealMind — Admin Frontend

A production-ready **Admin Dashboard** frontend for HealMind, a mental health support platform.
This repository contains **only the Admin Frontend** (no backend, no patient/doctor apps).

Built with React 18, React Router DOM, Bootstrap 5, CSS Modules, Axios, and Font Awesome icons, following the "Serene Path" design system (forest-green, minimalist-organic, calm and professional).

---

## ✨ Features

- **Authentication** — Admin login (JWT-ready, mocked for now)
- **Dashboard** — Platform-wide stats, revenue chart, mini calendar, quick actions, recent activity
- **Doctor Verification** — Review, approve, or reject pending doctor applications
- **Doctors Management** — List, search, filter, sort, view, create, edit, disable, delete
- **Patients Management** — List, search, filter, view, create, edit, suspend, delete
- **Community Access Logic** — Patient Details clearly shows community status, approval history,
  responsible doctor, and decision dates. **Only a doctor can approve/reject/request another
  session** — the Admin can monitor but never override this decision.
- **Tickets** — Full ticket monitoring with payment/session status and doctor decision
- **Sessions Management** — List + Calendar (daily/weekly/monthly), reschedule, cancel, mark
  completed, session type/status, payment status, follow-up flag
- **Payments** — Transaction list, revenue split (doctor vs. platform), payment details
- **Community Moderation** — Posts, Comments, and Reports tabs with hide/delete/resolve actions
- **Reports & Analytics** — Export UI for CSV / Excel / PDF (mocked)
- **Notifications** — Read/unread admin notifications
- **Settings** — General platform settings + notification preferences
- **Admin Profile** — Profile info + change password

---

## 🧱 Tech Stack

| Concern            | Choice                                  |
|---------------------|------------------------------------------|
| UI Library          | React 18                                |
| Routing             | React Router DOM v6 (lazy-loaded routes) |
| Styling             | Bootstrap 5 (grid/utilities) + CSS Modules (component styling) |
| Icons               | Font Awesome (Free) — no other icon libraries |
| HTTP Client         | Axios (configured, ready for a real API) |
| State Management    | React Context (Auth, Toast) + local hooks |
| Build Tool          | Vite |

> **Note on styling approach:** Per the project spec, all component-level styling is written
> exclusively in CSS Modules (`*.module.css`) — no Tailwind, no styled-components, no plain global
> CSS files, no inline styles for decoration. Bootstrap 5 is used for its base reset, responsive
> grid (`row`/`col-*`), and small utility classes (e.g. `table-responsive`), which is standard
> practice alongside CSS Modules and does not conflict with the "CSS Modules only" rule for
> component styling.

---

## 📁 Project Structure

```
src/
├── assets/              # Brand images/icons (add your own here)
├── components/          # Reusable, presentational components (one folder per component)
│   ├── Avatar/  Button/  Card/  Chip/  ConfirmDialog/  DataTable/  DoctorCard/
│   ├── EmptyState/  FilterBar/  LoadingSpinner/  Modal/  NotificationItem/
│   ├── PageHeader/  Pagination/  PatientCard/  SearchBar/  SessionCard/
│   └── StatCard/  StatusBadge/  Toast/
├── constants/            # Route paths, status enums, mock data
├── context/              # AuthContext, ToastContext
├── hooks/                # useAuth, useToast, useDebounce, usePagination, useModal
├── layouts/              # AdminLayout, Sidebar, Navbar, Footer
├── pages/                # One folder per route/page (each with its own .module.css)
│   ├── Login/  Dashboard/  DoctorVerification/
│   ├── Doctors/  DoctorDetails/  DoctorForm/
│   ├── Patients/  PatientDetails/  PatientForm/
│   ├── Tickets/  TicketDetails/
│   ├── Sessions/  SessionsCalendar/  SessionDetails/
│   ├── Payments/  PaymentDetails/
│   ├── Community/ (Posts, Comments, Reports tabs)
│   ├── Notifications/  Reports/  Settings/  Profile/  NotFound/
├── routes/               # AppRoutes.jsx (lazy-loaded), ProtectedRoute.jsx
├── services/             # Axios instance + one mock service per domain
├── styles/               # theme.css — the ONLY global stylesheet (CSS variables/reset)
├── utils/                # formatDate, formatCurrency, classNames
├── App.jsx
└── main.jsx
```

---

## 🚀 Getting Started

### Requirements
- Node.js 18+ and npm 9+

### Install dependencies
```bash
npm install
```

### Run the dev server
```bash
npm start
# or
npm run dev
```
The app runs at **http://localhost:5173** by default.

Login screen accepts **any email + password** (auth is mocked client-side).

### Build for production
```bash
npm run build
```
Output is generated in `dist/`.

### Preview a production build locally
```bash
npm run preview
```

---

## 🔌 Backend / API Integration Notes

All data currently comes from **mock services** in `src/services/*Service.js`, which simulate
network latency and return data from `src/constants/mockData.js`. This lets the whole UI (loading
states, empty states, pagination, filters) work end-to-end without a backend.

To connect a real backend:

1. Set `VITE_API_BASE_URL` in a `.env` file (see `src/services/apiClient.js`).
2. In each `*Service.js` file, replace the mock method bodies with the commented-out
   `apiClient.get/post/put/delete(...)` calls (examples are already included as comments).
3. `apiClient.js` already attaches the JWT from `sessionStorage` to every request via an Axios
   interceptor, and clears it on a `401` response — no changes needed there.
4. `AuthContext.jsx` currently fakes a JWT token on login; swap `login()` to call
   `authService.login()` against your real `/auth/login` endpoint and store the real token.
5. Doctor certificate upload and chat log viewing are UI placeholders — wire them to your file
   storage / messaging service endpoints when available.

No backend code is included in this repository, per the project scope.

---

## ✅ Production Readiness Checklist (performed at final review)

- [x] Project builds successfully (`npm run build`) with no errors
- [x] No unused/duplicate files (stray scaffold files and unused assets removed)
- [x] All routes verified against `src/routes/AppRoutes.jsx` and `src/constants/routePaths.js`
- [x] Every page loads through lazy-loaded, protected routes inside `AdminLayout`
- [x] Responsive layout (Bootstrap grid + CSS Module breakpoints) down to mobile widths
- [x] Bootstrap 5 grid/utilities in use (forms, dashboard stats, responsive tables)
- [x] Component styling is CSS Modules only (single global `theme.css` for CSS variables/reset)
- [x] Font Awesome is the only icon set used throughout
- [x] Consistent `components/`, `pages/`, `layouts/`, `services/`, `hooks/` structure
- [x] No oversized files — pages/components are split into small, focused files
- [x] All mock services connected to their respective pages
