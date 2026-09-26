# 0003: Auth owns identity and role; profiles are separate

- Status: accepted
- Date: 2026-09-26
- Services: auth, users, catalog

## Context

Auth should be an independent service with no knowledge of profiles or courses. But tokens must carry enough to authorize requests in `users` and `catalog` without calling auth.

## Decision

- Auth owns user ID, unique username, password hash, and **role**. Role is a token claim, not profile data.
- `users` owns profile data only, keyed by the user ID. It uses its own `display_name` and does not store the username.
- Account deletion and disabling are out of scope for v1.

## Consequences

- Auth has no dependency on other services, and other services need no call to auth to authorize.
- Role changes need a new token (up to the access-token lifetime, see ADR 0002).
- Username is stored once, so it cannot drift between services. The UI cannot show it next to a profile unless a later decision adds it to the token or a lookup.
- Without deletion, no orphaned profiles or courses can arise in v1. Supporting it later requires events.
- Alternative considered: role in `users`. Rejected because it removes the role from the token and forces a cross-service lookup on every catalog authorization.
