---
name: Send events to mParticle
description: Ingest one or many event batches into mParticle from a server, with the right auth, size limits, backpressure handling and retry semantics.
api: openapi/mparticle-events-openapi-original.yml
operations:
  - uploadEvents
  - bulkUploadEvents
generated: '2026-08-13'
method: generated
source: openapi/mparticle-events-openapi-original.yml, https://docs.mparticle.com/developers/apis/http/, https://docs.mparticle.com/guides/default-service-limits/
---

# Send events to mParticle

Server-to-server ingestion. Base URL `https://s2s.mparticle.com/v2` (regional hosts:
`s2s.us1`, `s2s.us2`, `s2s.eu1`, `s2s.au1` — send to the region the workspace lives in).

## Authenticate

HTTP Basic. Username is the **workspace input API key**, password is the **API secret**.

```
Authorization: Basic base64(<apiKey>:<apiSecret>)
Content-Type: application/json
```

Keys are issued per input in the mParticle UI. There is no OAuth on this API and no
test-mode key.

## Set the environment field — this is the only test/live guardrail

`environment` is required on every batch and defaults to `production`. Use `development`
for anything that is not real traffic. The same credential serves both, so a wrong value
sends test data to live destinations.

```json
{
  "environment": "development",
  "user_identities": { "email": "user@example.com" },
  "user_attributes": { "plan": "pro" },
  "events": [
    { "event_type": "custom_event",
      "data": { "event_name": "Checkout Started", "custom_event_type": "transaction" } }
  ]
}
```

Valid `event_type` values (from the spec's `EventType` enum): `session_start`, `session_end`,
`screen_view`, `custom_event`, `crash_report`, `opt_out`, `first_run`, `pre_attribution`,
`push_registration`, `application_state_transition`, `push_message`, `network_performance`,
`breadcrumb`, `profile`, `push_reaction`, `commerce_event`, `user_attribute_change`,
`user_identity_change`, `uninstall`.

## Send one batch — `uploadEvents`

`POST /events` with a single batch document. Success is **`202 Accepted` with no body** —
acceptance, not processing. Nothing downstream is confirmed by the 202.

## Send up to 100 batches — `bulkUploadEvents`

`POST /bulkevents` with a JSON **array** of batch documents. Prefer this for anything
throughput-shaped.

## Respect the size limits before you send

- One batch: **128 KB** maximum.
- One request: **256 KB** maximum.
- One `/bulkevents` request: **100 batches** maximum.

Split client-side. A batch over the limit is rejected, not truncated.

## Read the backpressure signal

Every 2xx carries `X-mp-rate-limit-percentage-used`. **Throttle on this before you get
throttled** — it is the cheapest signal mParticle gives you and most clients ignore it.

On `429`:
- `X-mp-rate-limit-exceeded` names which limit was hit — `org`, `account`, `app`, `feed`,
  `workspace`, `system` or `acceleration`. An `app`-level 429 is not fixed by slowing the
  whole account down.
- `Retry-After` may be present with a whole number of seconds. Honor it.
- Default account limit is ~270 batches/second and mParticle will raise it on request.

## Handle errors

Error body is `{"errors":[{"code":"...","message":"..."}]}` — not RFC 9457 problem+json.

| Status | Meaning | Do |
|---|---|---|
| 400 | Malformed JSON or a missing required field | Fix the payload. Never retry unchanged. |
| 401 | Auth header missing/malformed | Fix credentials. |
| 403 | Key and secret valid-looking but not valid for this workspace | Check the input. |
| 429 | Rate limited | Backoff per the headers above. |
| 503 | Temporarily unavailable | Retry with exponential backoff; do not drop the batch. |

## Retries are not idempotent

mParticle publishes **no idempotency key**. A retried 429/503 can ingest the batch twice.
Put your own stable identifier on each event and deduplicate downstream, or accept
double-counting.

## Optional: validate first

If the workspace has a data plan, `POST .../plans/validate` on the Data Planning API tells
you what the batch would violate **without ingesting it**. See
`skills/mparticle-govern-data-plans.md`.
