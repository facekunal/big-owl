# 0002: Short-lived JWT access tokens with revocable refresh tokens

- Status: accepted
- Date: 2026-09-26
- Services: auth, users, catalog

## Context

Users and catalog need to authenticate requests without depending on auth for every call.

## Decision

- Auth issues JWTs signed with an asymmetric key (EdDSA) and exposes a JWKS endpoint.
- Access tokens last about 10 minutes and carry `sub` and `role`.
- Refresh tokens are opaque, stored hashed in auth, and rotated on use.
- Logout revokes the refresh token.
- Users and catalog verify access tokens locally.

## Consequences

- Auth is not a per-request dependency, so it is not a scaling or availability bottleneck for other services.
- After logout, an already-issued access token stays valid until it expires (up to about 10 minutes). Accepted.
- Key rotation needs a JWKS strategy (multiple active keys). To be designed with the auth service.
- Alternative considered: a revocation check on every request. Rejected because it makes auth a hard dependency of every call.
