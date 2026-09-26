# big-owl

A self-learning platform for foreign languages. **Learners** subscribe to courses; **teachers** publish and manage them.

The product surface is intentionally small. The real goal of this repo is a stable, scalable backend built as multiple services.

> Status: pre-implementation. Structure and design docs only; no service code yet.

## Scope (v1)

- Sign up, log in, log out with **username + password only** (no email or phone).
- Two roles, chosen at signup: `learner` and `teacher`.
- Onboarding: users fill in a profile after their first login.
- Teachers create, update, and delete courses.
- Learners subscribe to and unsubscribe from courses.

Out of scope for now: course content, points/progress, payments, search, notifications, password reset. See [docs/product/requirements.md](docs/product/requirements.md) and [docs/roadmap.md](docs/roadmap.md).

## Repository layout

```
.
├── backend/     Rust (Axum) services: auth, users, catalog
├── frontend/    Web client (planned: React + Vite + Tailwind)
├── docs/        Product, architecture, API contract, ADRs
├── scripts/     Dev helpers (seed data, load tests)
└── .github/     PR template, CI workflows
```

## Architecture in one paragraph

Three backend services, each with its own database: **auth** (credentials, tokens), **users** (profiles), **catalog** (courses and subscriptions). Auth issues short-lived JWTs signed with an asymmetric key; other services verify them locally via JWKS. There are no synchronous calls between services in the core flows. A reverse proxy routes `/auth`, `/users`, `/catalog` to the right service. Details: [docs/architecture/overview.md](docs/architecture/overview.md).

## Docs

| Doc | What it covers |
|---|---|
| [requirements](docs/product/requirements.md) | Scope, roles, user stories |
| [architecture overview](docs/architecture/overview.md) | Services, boundaries, communication |
| [ADRs](docs/adr/) | Why each major decision was made |
| [roadmap](docs/roadmap.md) | Deliberately deferred work |
| [CONTRIBUTING](CONTRIBUTING.md) | Branching, commits, PR process |

## Getting started

Not yet available. Run and test commands will be added here once the first service exists.
