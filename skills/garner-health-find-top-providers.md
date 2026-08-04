---
name: Find Garner Top Providers near a patient
description: >-
  Use the Garner Health API to return a rank-ordered list of Top Providers for a
  specialty near a patient's location, filtered to the patient's carrier network.
api: openapi/garner-health-openapi-original.yml
operations:
- GetRankedProviders
---

# Find Garner Top Providers

Garner ranks providers on 550+ specialty-specific quality and efficiency metrics derived
from 60B+ de-identified claims. This skill returns Top Providers in rank order (best first).

## Auth
1. Exchange your `client_id` + `client_secret` for a bearer token:
   `POST https://api.getgarner.com/oauth2/token` with
   `grant_type=client_credentials` (form-encoded). Cache the `access_token` — it is valid
   ~15 minutes.
2. Send `Authorization: Bearer <access_token>` on every call.

## Steps
1. Call **GetRankedProviders** (`GET /providers`).
   - Origin: pass either `lat`+`lng` OR a 5-digit `zipCode` (never both).
   - `specialty`: a Garner specialty code (e.g. `adult_general_gastroenterologist`).
   - `networkId` (required): the patient's carrier network.
   - Optional filters: `gender`, `language` (ISO 639-3), `limit` to cap results.
   - `plan`: the data subscription plan value from your account manager.
2. Read the `providers[]` array — it is already in rank order; index 0 is the most
   recommended by distance, quality, cost, and reviews.

## Rules
- `limit` caps result size; there is no cursor/offset pagination.
- Validation failures return HTTP 422 with a `ServiceError` body (`{requestId, message, data}`) —
  surface `message` and log `requestId` for support. See errors/garner-health-problem-types.yml.
- Reads are idempotent; no idempotency key is required.
