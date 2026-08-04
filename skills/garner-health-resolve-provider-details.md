---
name: Resolve Garner professional and facility details
description: >-
  Given a Garner professional or facility id, fetch full quality metrics, specialties,
  locations, and carrier-network participation from the Garner Health API.
api: openapi/garner-health-openapi-original.yml
operations:
- GetProfessional
- GetFacility
---

# Resolve Garner provider details

After ranking or directory search, resolve a single provider to its full record.

## Auth
Obtain a bearer token via `POST https://api.getgarner.com/oauth2/token`
(`grant_type=client_credentials`) and send `Authorization: Bearer <access_token>`.

## Steps
1. For a professional, call **GetProfessional** (`GET /professionals/{professional_id}`).
   - `professional_id` (path, required): the Garner professional id.
   - `network_id` (query, required): the network(s) to include on the response.
   - Response includes quality `metrics[]`, `specialties[]`, and `locations[]`.
2. For a facility, call **GetFacility** (`GET /facilities/{facility_id}`).
   - `facility_id` (path, required), `location_id` (query, required), and
     `network_id` (query, required).
   - Response includes per-location specialty and network directory data.

## Rules
- A missing id returns HTTP 404 (no body). A missing required query parameter returns
  HTTP 422 with a `ServiceError` envelope.
- All operations are read-only and idempotent.
- See conventions/garner-health-conventions.yml and data-model/garner-health-data-model.yml
  for the response shape.
