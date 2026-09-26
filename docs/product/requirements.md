# Requirements (v1)

## Purpose

A self-learning platform for foreign languages. The product surface is deliberately thin; the engineering focus is backend stability and scalability.

## Roles

A user has exactly one role, chosen at signup: **learner** or **teacher**.

## In scope

### Accounts (auth)
- Sign up with username + password and a role.
- Log in, log out.
- Session refresh without re-entering the password.
- No email, phone, OAuth, or password reset.

### Profile / onboarding (users)
- After first login, a user completes a profile (display name, native language, languages learning or teaching, short bio).
- Users can view and update their own profile.
- Profile fields beyond these are open to change while the users service is designed.

### Courses (catalog)
- A teacher can create, update, and delete their own courses. A course has a title, description, target language, and level.
- Anyone signed in can browse and view published courses.
- Only the owning teacher can modify or delete a course.

### Subscriptions (catalog)
- A learner can subscribe to and unsubscribe from a course.
- Subscribing twice is a no-op (idempotent).
- Deleting a course is a soft delete; its subscriptions become inactive in the same transaction.

## Out of scope (for now)

Course content and lessons, points or progress tracking, payments, search and recommendations, notifications, admin tooling, password reset, social login. See [roadmap](../roadmap.md).

## Non-functional goals

- Stability: timeouts, graceful shutdown, health/readiness endpoints, consistent error responses.
- Scalability: stateless services, pagination on all list endpoints, no per-request dependency between services.
- Security: argon2id hashing, login rate limiting and lockout, short-lived access tokens, revocable refresh tokens.
- Operability: structured logs with request IDs, metrics, integration tests against real Postgres, load tests.

## Open questions

- Exact profile fields.
- Course `level` scheme (CEFR A1-C2 or free text).
- Whether teachers can unpublish (draft state) in v1.
