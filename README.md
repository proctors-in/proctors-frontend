# Momentum / Proctors — Frontend

Web client for a school assessment platform: curriculum management, AI-assisted question generation, oral/voice assessments, and learning analytics.

Built with **Next.js 16 (App Router)**, **React 19**, **TypeScript**, and a **FastAPI** backend.

## What's inside

| Area | Routes | Description |
| --- | --- | --- |
| Marketing & auth | `/`, `/login`, `/signup` | Landing page, login, 3-step school onboarding |
| Admin console | `/dashboard`, `/syllabus`, `/classes`, `/subjects`, `/chapters`, `/assessments`, `/students`, `/reports`, `/team`, `/school-settings`, `/control-panel` | Role-aware management of classes → subjects → chapters, question banks, assessments, students, faculty, and platform diagnostics |
| Student portal | `/assessment/join`, `/assessment/verify`, `/assessment/[sessionId]`, `/result/[id]` | Token-based entry, assessment session, result summary |
| Voice interview | `/interview/[interviewId]`, `/interview/[interviewId]/result` | Full-screen speech-driven assessment with the "Buddy" assistant |

## Quick start

**Prerequisites:** Node.js 20+, npm, and a running backend API.

```bash
npm install
cp .env.example .env.local   # set NEXT_PUBLIC_API_URL if needed
npm run dev                  # http://localhost:3000
```

Default backend URL: `http://localhost:5001/api`.

## Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Development server |
| `npm run build` | Production build |
| `npm run start` | Serve production build |
| `npm run lint` | ESLint |

## Tech stack

- **Next.js 16.2.6 / React 19.2.4 / TypeScript 5**
- **Axios** service layer with bearer-token interceptor
- **Tailwind CSS 3** tokens + a large CSS-variable design system in `src/app/globals.css`
- **Lenis** smooth scrolling, **lucide-react** icons
- Browser **Web Speech API**, **MediaRecorder**, and **WebSocket** for the voice interview

## Repository layout

```text
src/
├── app/          # App Router pages: public, (admin), (student), interview
├── components/   # layout, common, cards, forms, tables, student(s), team
├── hooks/        # useApi, useAuth, useAssessment
├── lib/          # Axios instance + JWT helpers
├── services/     # Typed API clients per domain + voice subsystem
├── types/        # Shared domain interfaces
├── utils/        # Constants, formatters, helpers, storage
├── validations/  # Lightweight form validators
└── middleware.ts # Edge JWT route gate + role/feature RBAC
```

## Documentation

- **[DOCUMENTATION.md](./DOCUMENTATION.md)** — complete technical documentation: architecture, folder structure, routing, auth, data layer, flows, API reference, conventions, and known gaps.
- **[ONBOARDING.md](./ONBOARDING.md)** — developer environment setup.
- **[PRODUCT_DOCUMENTATION.md](./PRODUCT_DOCUMENTATION.md)** — product feature catalog and backend overview.
- **[INTERVIEW_SYSTEM.md](./INTERVIEW_SYSTEM.md)** — voice interview, STT/TTS, and AI evaluation architecture.

## Authentication model

Login stores the JWT in `localStorage` (for Axios) and a `token` cookie (for Next.js middleware). Middleware decodes the JWT to gate dashboard routes by role and `allowed_features`; the backend remains the source of truth for authorization.

## Environment

| Variable | Default | Purpose |
| --- | --- | --- |
| `NEXT_PUBLIC_API_URL` | `http://localhost:5001/api` | Backend API base URL (baked in at build time) |

## Docker

```bash
docker build -t proctors-frontend .
docker run -p 3000:3000 proctors-frontend
```

Pass `NEXT_PUBLIC_API_URL` at **build** time, since `NEXT_PUBLIC_*` values are embedded in the client bundle.
