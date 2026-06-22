# Security Model (Public Overview)

## Core Principles

- **One person, one vote** — enforced at the `VoterToken` level with atomic updates.
- **Vote secrecy** — public results never expose the link between a voter and their choices.
- **Sealed results** — vote counts are only revealed after `has_ended` + `results_published`.
- **Defense in depth** — rate limiting, CSRF, role checks, input validation at multiple layers.

## Authentication

- Primary: Registration number + password (custom backend).
- Password recovery: Multiple paths (email token, security questions, SMS OTP).
- Brute force protection: Redis-backed attempt counters + temporary lockouts.
- Sessions stored in Redis.

## Authorization

- Role-based (Voter, Candidate, Class Leader, Commissioner, Observer).
- Additional checks on sensitive actions (publishing results, user verification, data uploads).
- Staff users inherit commissioner-like privileges where appropriate.

## Data Protection

- Candidate images and reports stored on Cloudinary (not local disk).
- No voter identity stored with individual votes in public-facing result views.
- Sensitive operations (token consumption) use atomic database updates.

## Auditability

- Upload batches are recorded (`CollegeDataUploadBatch`).
- Election reports are archived with metadata.
- All votes carry a reference to the token that authorized them.

This public document describes the intended security properties. Detailed implementation controls are maintained in the application source.
