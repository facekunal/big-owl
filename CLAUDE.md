# big-owl: guidance for AI assistants

Monorepo for a language-learning platform. Read [README.md](README.md) first, then the docs it links.

## Status

Pre-implementation. Only docs and empty `backend/` / `frontend/` exist. Do not invent code, dependencies, or features beyond what the docs describe. If something is undecided, ask.

## Stack

- Backend: Rust + Axum, three services (`auth`, `users`, `catalog`), Postgres (one database per service).
- Frontend: planned React + Vite + Tailwind (not yet confirmed).
- Build/test/lint commands: **TBD**. They will be listed here once the first service exists. Do not guess them.

## Hard rules

- Auth is username + password only. Never add email, phone, or OAuth.
- Passwords are hashed with argon2id. Never log or store plaintext passwords or tokens.
- Services own their data. No cross-service database access or joins; only the user ID (UUID) is shared.
- No synchronous service-to-service calls in core flows (see ADR 0001, 0002). Adding one needs a new ADR.
- Every course mutation checks role **and** ownership in the service, regardless of the proxy.
- Subscriptions live in `catalog`. Profiles live in `users`. Credentials live in `auth`.
- Out of scope: course content, points, payments, search, notifications. See `docs/roadmap.md`.

## Where things go

- Cross-cutting docs, the API contract (`docs/api/openapi.yaml`), and ADRs: root `docs/`.
- Conventions for one half of the stack: `backend/CLAUDE.md`, `frontend/CLAUDE.md` (added with the code).
- Do not duplicate docs across levels; link instead.

## Working agreements

- Architectural changes get an ADR in `docs/adr/` before code.
- Keep PRs small and follow `.github/pull_request_template.md`.
