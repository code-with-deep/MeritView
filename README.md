# ArguLens

AI-powered dispute analysis. Two parties submit briefs; multiple independent LLMs
evaluate them in parallel and an aggregator synthesizes an impartial, confidence-scored
opinion.

> Decision support, not legal advice. No verdict is ever rendered.

## Tech stack

| Layer | Choice |
| --- | --- |
| Framework | Next.js 16 (App Router, React 19, Server Actions) |
| Language | TypeScript (strict) |
| UI | Tailwind CSS v4, shadcn/ui (Radix), Framer Motion, lucide-react |
| Auth | Clerk |
| Database | PostgreSQL 16 + Prisma 7 (pg driver adapter) |
| Cache / queue store | Redis |
| Object storage | S3-compatible (MinIO locally) |
| State | TanStack Query + Zustand |

Infrastructure (Postgres, Redis, MinIO) runs locally via Docker Compose.

## Prerequisites

- Node.js 20+ (22 recommended)
- Docker Desktop (running)
- A Clerk application (publishable + secret keys)

## Getting started

```bash
# 1. Start infrastructure (Postgres :5433, Redis :6379, MinIO :9000/:9001)
docker compose up -d

# 2. Install dependencies
npm install

# 3. Apply the database schema
npm run db:migrate

# 4. Run the app
npm run dev
```

App: http://localhost:3000

## Environment variables

Copy `.env.example` to `.env` and fill in values. Clerk keys are required; the
database/redis/storage defaults match the Docker Compose services.

## Useful scripts

| Script | Description |
| --- | --- |
| `npm run dev` | Start the dev server |
| `npm run build` | Production build |
| `npm run lint` | ESLint |
| `npm run db:migrate` | Create & apply a Prisma migration |
| `npm run db:studio` | Open Prisma Studio (DB GUI) |
| `npm run db:generate` | Regenerate the Prisma client |

## Project structure

```
src/
  app/
    (marketing)/      Landing + pricing (public)
    (auth)/           Clerk sign-in / sign-up
    (app)/dashboard/  Authenticated app shell + pages
    api/webhooks/     Clerk user-sync webhook
  components/         UI + app shell + marketing components
  modules/            Domain logic (identity, …)
  lib/                env, prisma, redis, auth helpers
prisma/               schema + migrations + seed
```

## Implementation roadmap

- **Part 1 — Foundation & Auth** ✅ (this milestone)
- Part 2 — Dispute lifecycle & AI brief preparation
- Part 3 — Multi-model evaluation, aggregation & opinion delivery
- Part 4 — Payments, security/compliance, monitoring & deploy
