---
name: Submit and track a TPA claim
description: Submit a third party administration claim and poll its status.
api: openapi/medigo-openapi-original.yml
operations:
- submitClaim
- getClaim
---

# Submit and track a TPA claim

File a claim against MEDIGO's TPA service and follow it to resolution.

## Auth
Send `Authorization: Bearer <partner-api-token>`; JSON in and out; keep the
`Trace-Id` response header for support. Errors are `{ code, message }`.

## Steps
1. **Submit the claim.** Call `submitClaim` (POST /tpa/claims) with the claim
   details for the insured member. Capture the returned claim `id`.
2. **Track status.** Call `getClaim` (GET /tpa/claims/{id}) to read the current
   claim state and any agent notes.

## Notes
- `submitClaim` can return `400` (invalid/missing data) or `422` (business
  validation) — inspect `message`. `getClaim` returns `404` when the id is
  unknown or not owned by your account. See errors/medigo-problem-types.yml.
- No idempotency key is documented — after a failed `submitClaim`, do not blindly
  resubmit; confirm outcome first where a claim id is available.
