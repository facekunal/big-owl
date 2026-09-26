# Architecture overview

Status: accepted (design section 1). Data model and per-service APIs are not yet designed.

## Services

| Service | Owns | Does not know about |
|---|---|---|
| **auth** | Credentials (username, argon2id hash, role), access and refresh tokens, login rate limiting | Profiles, courses |
| **users** | Profiles and onboarding info | Credentials, courses |
| **catalog** | Courses, subscriptions, teacher ownership | Credentials, profiles |

### Auth independence

Auth knows only about identity: user ID, username, password hash, and **role**. It makes no calls to `users` or `catalog` and runs no logic when profiles or courses change. Dependencies are one-way: other services trust tokens auth issued; auth trusts nothing from them.

- **Role lives in auth.** It is chosen atomically at signup and carried in the token as an authorization claim, so `catalog` can check it without a cross-service call. See [ADR 0003](../adr/0003-identity-ownership.md).
- **Username lives only in auth.** It is unique and never copied. Profiles in `users` have their own `display_name` and do not store the username, so there is no copy to drift.
- **Account deletion or disabling is out of scope for v1.** Nothing would notify `users` or `catalog`, leaving orphaned profiles and courses. It returns with the event bus (see [roadmap](../roadmap.md)).

## Data

Each service has its own database. Locally they share one Postgres instance. There are no cross-service joins. The only shared identifier is the user ID (UUID) issued by auth.

## Authentication and tokens

- Auth signs JWTs with an asymmetric key (EdDSA) and publishes the public key at a JWKS endpoint.
- `users` and `catalog` verify tokens locally. There is no call to auth per request.
- Access tokens live about 10 minutes and carry `sub` (user ID) and `role`.
- Refresh tokens are opaque, stored hashed in auth, and rotated on use.
- Logout revokes the refresh token. The session ends when the current access token expires (up to about 10 minutes). See [ADR 0002](../adr/0002-token-strategy.md).

## Communication

No synchronous calls between services in core flows.

- **Signup and onboarding:** signup creates only the credential in auth. After login, `PUT /users/me` creates the profile in users. No distributed transaction, no half-created accounts.
- **Course listings:** catalog stores only `teacher_id`. The frontend fetches display names from users when needed.
- **Course deletion:** a single transaction in catalog (soft delete plus subscriptions marked inactive).

## Entry point

A reverse proxy (Caddy or Traefik, configuration only) routes `/auth/*`, `/users/*`, `/catalog/*` and handles CORS and coarse rate limiting. Services enforce authorization themselves and never trust the proxy for it.

## Deferred

Message broker, event bus, custom gateway, service mesh. Introduce only when a concrete problem needs them, with an ADR.
