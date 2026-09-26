# Contributing

## Workflow

1. Branch from `main`: `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, `chore/<topic>`.
2. Keep changes small and focused; one concern per PR.
3. Decisions that affect architecture or service boundaries need an ADR in `docs/adr/` (copy the shape of an existing one, next number) before or with the code.
4. Open a PR using the template. CI must pass before merge.

## Commits

Conventional style: `type(scope): summary`, e.g. `feat(auth): add refresh token rotation`.
Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`. Scope is a service or area (`auth`, `users`, `catalog`, `frontend`, `docs`).

## Docs

- Product and system-wide docs live in root `docs/`.
- Update docs in the same PR as the behaviour change.
- Link to other docs; do not copy their content.

## Build, test, lint

Commands will be added here once the first service exists.
