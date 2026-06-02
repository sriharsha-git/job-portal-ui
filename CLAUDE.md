# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — Start Vite dev server
- `npm run build` — Production build
- `npm run lint` — ESLint (flat config, JS/JSX only)
- `npm run preview` — Preview production build

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — Start Vite dev server
- `npm run build` — Production build
- `npm run lint` — ESLint (flat config, JS/JSX only)
- `npm run preview` — Preview production build

## Repository Structure

```
job-portal-ui/
├── public/               # Static assets (favicons, company logos)
├── src/
│   ├── components/       # Reusable UI components (Navbar, Footer, Layout, ProtectedRoute, etc.)
│   ├── context/          # Core React contexts: AuthContext, JobContext, ThemeContext
│   ├── contexts/         # Data-fetching contexts: JobsDataContext, CompaniesContext
│   ├── data/             # mockData.js — all seed data (jobs, companies, users)
│   ├── pages/            # Route-level page components
│   │   └── admin/        # Admin-only pages (Dashboard, CompanyManagement, etc.)
│   ├── services/         # Simulated async API service functions
│   ├── utils/            # Shared utilities (delay.js)
│   ├── App.jsx           # Root component — router + provider tree
│   ├── main.jsx          # Entry point
│   └── index.css         # Global styles (Tailwind imports)
├── eslint.config.js      # ESLint flat config
├── vite.config.js        # Vite configuration
└── index.html            # HTML entry point
```

**Where to look:**

- Adding a new page → `src/pages/` + register route in `App.jsx`
- Shared UI → `src/components/`
- Auth logic → `src/context/AuthContext.jsx`
- Job/application logic → `src/context/JobContext.jsx`
- Mock data changes → `src/data/mockData.js`
- API simulation → `src/services/`

## Git Conventions

### Branching

```
feature/add-job-filter-sidebar         # New features
fix/employer-route-redirect-loop       # Bug fixes
docs/update-readme                     # Documentation only
chore/upgrade-dependencies             # Maintenance, tooling
refactor/simplify-auth-context         # Code refactoring
style/mobile-job-card-spacing          # Visual/style changes
```

- Branch off `main` for all new work
- Keep branches short-lived; open a PR when ready
- Delete branches after merging

### Commit Messages

Follow **Conventional Commits**:

```
feat: add saved jobs count to navbar
fix: correct role guard on employer routes
docs: update README with localStorage keys
chore: upgrade react-router to v7.8
refactor: extract job card into reusable component
style: fix spacing on mobile job list
```

- Use present tense, lowercase, no period at the end
- Keep the subject line under 72 characters
- Add a body for non-obvious changes

### Pull Requests

- PR title should match the commit message format
- Include a summary and test plan in the PR description
- Target `main` as the base branch

## Coding Standards

### General

- **No TypeScript** — plain JSX throughout; do not add `.ts`/`.tsx` files
- **Functional components only** — no class components
- **Named exports** preferred over default exports for components
- Keep components focused — extract reusable pieces into `src/components/`

### Styling

- Use **Tailwind CSS utility classes** exclusively — no inline styles, no CSS modules
- Follow mobile-first responsive design (`sm:`, `md:`, `lg:` breakpoints)
- Dark mode via `ThemeContext` — use conditional class toggling, not `dark:` variants

### State & Data

- Use React Context for shared state — no external state library
- Do not fetch data directly in page components; use services in `src/services/`
- All async service calls must use `delay()` to simulate latency
- Persist user-specific data to localStorage using the established key pattern (`{entity}_{userId}`)

### Naming

- Components: `PascalCase` (e.g., `JobCard.jsx`)
- Variables/functions: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Files: match the default export name (e.g., `JobCard.jsx` exports `JobCard`)

### ESLint

Flat config (`eslint.config.js`). The `no-unused-vars` rule ignores variables starting with uppercase or underscore (`varsIgnorePattern: '^[A-Z_]'`). Run `npm run lint` before committing.

## Architecture

React 19 SPA using Vite 7, Tailwind CSS 4, and React Router 7. No TypeScript — plain JSX throughout.

### State Management

Two layers of React Context:

- **`src/context/`** — Core contexts: `AuthContext` (auth + dummy users + localStorage persistence), `JobContext` (applications, saved jobs, employer job CRUD), `ThemeContext`
- **`src/contexts/`** — Data-fetching contexts: `JobsDataContext` (cached job list with 5-min TTL), `CompaniesContext`

Provider nesting order (in App.jsx): AuthProvider → JobsDataProvider → JobProvider → CompaniesProvider → ThemeProvider

### Data Layer

Currently uses **mock data** with localStorage persistence — no real backend. Services in `src/services/` simulate async API calls using `delay()` from `src/utils/delay.js`. Data originates from `src/data/mockData.js`.

Key localStorage keys: `jobPortalUser`, `authToken`, `registeredUsers`, `globalPostedJobs`, `jobApplications_{userId}`, `savedJobs_{userId}`, `postedJobs_{userId}`.

### Routing & Roles

Three roles with route protection via `ProtectedRoute` component:

- **ROLE_JOB_SEEKER** — profile, applied-jobs, saved-jobs
- **ROLE_EMPLOYER** — post-job, employer/jobs, job-applicants/:jobId
- **ROLE_ADMIN** — admin/\*, admin pages in `src/pages/admin/`

### Key Libraries

- Font Awesome + Lucide React for icons
- react-toastify for notifications

### ESLint

Flat config (`eslint.config.js`). The `no-unused-vars` rule ignores variables starting with uppercase or underscore (`varsIgnorePattern: '^[A-Z_]'`).

## Architecture

React 19 SPA using Vite 7, Tailwind CSS 4, and React Router 7. No TypeScript — plain JSX throughout.

### State Management

Two layers of React Context:

- **`src/context/`** — Core contexts: `AuthContext` (auth + dummy users + localStorage persistence), `JobContext` (applications, saved jobs, employer job CRUD), `ThemeContext`
- **`src/contexts/`** — Data-fetching contexts: `JobsDataContext` (cached job list with 5-min TTL), `CompaniesContext`

Provider nesting order (in App.jsx): AuthProvider → JobsDataProvider → JobProvider → CompaniesProvider → ThemeProvider

### Data Layer

Currently uses **mock data** with localStorage persistence — no real backend. Services in `src/services/` simulate async API calls using `delay()` from `src/utils/delay.js`. Data originates from `src/data/mockData.js`.

Key localStorage keys: `jobPortalUser`, `authToken`, `registeredUsers`, `globalPostedJobs`, `jobApplications_{userId}`, `savedJobs_{userId}`, `postedJobs_{userId}`.

### Routing & Roles

Three roles with route protection via `ProtectedRoute` component:
- **ROLE_JOB_SEEKER** — profile, applied-jobs, saved-jobs
- **ROLE_EMPLOYER** — post-job, employer/jobs, job-applicants/:jobId
- **ROLE_ADMIN** — admin/*, admin pages in `src/pages/admin/`

### Key Libraries

- Font Awesome + Lucide React for icons
- react-toastify for notifications

### ESLint

Flat config (`eslint.config.js`). The `no-unused-vars` rule ignores variables starting with uppercase or underscore (`varsIgnorePattern: '^[A-Z_]'`).
