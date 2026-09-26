# 0001: Split the backend into auth, users, and catalog

- Status: accepted
- Date: 2026-09-26
- Services: all

## Context

The product is small, but the goal of the project is to practise stability and scalability problems in a multi-service backend.

## Decision

Three Rust (Axum) services, each with its own database:

- `auth`: credentials and tokens.
- `users`: profiles.
- `catalog`: courses and subscriptions.

Subscriptions live in `catalog` so that course validity and course deletion are handled in one transaction.

## Consequences

- Clear ownership and independent scaling and deployment.
- No cross-service joins; data needed by the UI is composed by the client.
- More setup and operational overhead than a single service. Accepted deliberately.
- Alternative considered: one modular service with a workspace layout that can be split later. Rejected because the aim is to practise the multi-service problems now.
