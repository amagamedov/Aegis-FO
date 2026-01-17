# Portfolio Ledger

## What This Is
Portfolio tracking for family office CIOs replacing Excel. Users manage $50M-$500M across public equities, private funds, real estate, and unusual assets.

## The Problem
Excel fails at this: formulas drift, history gets overwritten, no audit trail, multi-entity reporting is manual, nobody trusts the numbers at quarter-end.

## Core Promise
A canonical ledger that knows what's owned, what it's worth, and how it got there. Every change recorded, never overwritten.

## Tech Stack
- **Frontend:** React + Tailwind, deployed on Vercel
- **Backend:** Python/FastAPI, deployed on Railway
- **Database:** Postgres on Railway
- **Auth:** Clerk (authn only — our code handles authz + tenancy)

## Key Concepts
- **Append-only ledger:** Never edit entries — record new ones. History is sacred.
- **Two entry types:** `valuation` (mark-to-market) and `cash_flow` (money moved)
- **Cash accounts are assets:** category='cash', linked via transaction_id on buys/sells
- **RLS for tenant isolation:** Enforced at DB level, not query level

## How to Use These Docs
1. Read `INVARIANTS.md` first — four rules that break the product if violated
2. Work through packages in order: 00 → 01 → 02 → 03 → 04 → 05
3. Each package has acceptance criteria — verify before moving on
4. Original spec is in `docs/spec/` if you need deeper context

## Development Workflow
1. Run Postgres via `docker compose up -d`
2. Run backend: `cd backend && uv run uvicorn app.main:app --reload`
3. Run frontend: `cd frontend && npm run dev`
4. Migrations: `uv run alembic revision --autogenerate -m "message"` then `uv run alembic upgrade head`
5. Tests: `uv run pytest`

## Project Structure
```
portfolio/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── config.py
│   │   ├── db.py
│   │   ├── deps.py
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── routers/
│   │   └── services/
│   ├── tests/
│   ├── alembic/
│   ├── pyproject.toml
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── views/
│   │   ├── api.ts
│   │   ├── hooks.ts
│   │   ├── utils.ts
│   │   └── App.tsx
│   ├── package.json
│   └── .env.example
├── docker-compose.yml
└── docs/
    ├── CONTEXT.md
    ├── INVARIANTS.md
    └── packages/
```