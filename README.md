# FixMate — Client (Next.js)

**FixMate** is a full-stack, role-based *on-demand device & home appliance repair service booking platform*. This repo is the **Next.js 15 (App Router) frontend** — public pages, role-guarded dashboards, BetterAuth authentication and the Stripe payment UI.

## Purpose
Customers browse admin-approved repair services, send repair requests and pay **only after** the job is completed. Technicians list services and run jobs through a status pipeline. Admins moderate everything.

## Live URL
- Production: *(`YOUR-VERCEL-DOMAIN` — fill after deploy)*
- API server repo: `fixmate-server` (Express + MongoDB + Stripe)

## Test accounts (created by `npm run seed` — password for all: `Fixmate123`)
| Role | Email |
|---|---|
| 🔑 Admin | `admin@fixmate.test` |
| 🔑 Technician | `technician@fixmate.test` · `technician2@fixmate.test` |
| 🔑 Customer | `customer@fixmate.test` · `customer2@fixmate.test` |

## Key features
- **3 roles** — Customer / Technician / Admin dashboards with route guards & Forbidden screen (reload never kicks a logged-in user to login)
- **Browse services** with *server-side* search, category/mode filters, price sorting and pagination that survive reloads (Challenge 1)
- **Job pipeline** requested → accepted → in-progress → completed → paid, enforced on the server (Challenge 3) + visual timeline tracker
- **Stripe pay-after-service** checkout, success/cancel pages, idempotent verification & webhook (Challenge 4)
- **Ratings & reviews, PDF invoice download, dark/light mode** (optional features)
- Loading skeletons, custom 404 (tools-themed), error boundary, sonner toasts, confirm modals, empty states with CTAs
- Responsive from 360px, DaisyUI custom themes, X (new logo) in footer

## Tech stack / npm packages
- `next` 15 (App Router) · `react` 19 · `typescript` · `tailwindcss` · `daisyui`
- `better-auth` (+ `mongodb` adapter) — sign-up/sign-in, additional fields `role, phone, area, bio`
- `jsonwebtoken` — bridges the BetterAuth session into a role JWT for the Express API
- `sonner` (toasts) · `recharts` (dashboard charts) · `lucide-react` (icons) · `jspdf` (invoices) · `tsx` (seed script)

## Run locally
```bash
npm install
cp .env.example .env.local     # fill values (see table in SETUP-AND-DEPLOY.md)
npm run seed                   # realistic connected data (MongoDB must be reachable)
npm run dev                    # http://localhost:3000
```
> The browser calls same-origin `/backend/*` and Next.js proxies to the Express server (`next.config.ts`).
> For production you can either keep the proxy or point `NEXT_PUBLIC_API_URL` directly at the
> deployed API (then add your domain to the server's `CORS_ORIGINS`).

## Scripts
| Command | Purpose |
|---|---|
| `npm run dev` | dev server |
| `npm run build` / `npm start` | production build / serve |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run seed` | seed users, services, bookings, payments |

## Environment (`.env.local` — never commit)
`NEXT_PUBLIC_API_URL` · `API_URL_INTERNAL` · `MONGODB_URI` · `BETTER_AUTH_SECRET` · `BETTER_AUTH_URL` · `JWT_SECRET` (must match server)
