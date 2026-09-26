# Roadmap: deliberately deferred

Not forgotten; not needed yet.

## Product
- Course content and lessons
- Points and progress tracking
- Course search and recommendations
- Notifications
- Payments
- Password reset (needs a recovery mechanism that fits username-only accounts)
- Admin tooling

## Platform
- Message broker or event bus (e.g. for account deletion propagating to users and catalog)
- Custom API gateway (a configured reverse proxy is used for now)
- Kubernetes and service mesh
- Distributed tracing beyond request IDs
