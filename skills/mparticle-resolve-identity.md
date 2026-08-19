---
name: Resolve a user identity with mParticle IDSync
description: Turn a set of user identities into a stable mParticle ID (MPID), move a user between authenticated and anonymous states, and mutate an existing identity graph.
api: openapi/mparticle-identity-swagger-original.yml
operations:
  - "POST /identify"
  - "POST /login"
  - "POST /logout"
  - "POST /{mpid}/modify"
generated: '2026-08-13'
method: generated
source: openapi/mparticle-identity-swagger-original.yml, https://docs.mparticle.com/developers/apis/idsync/
---

# Resolve a user identity with mParticle IDSync

Base URL `https://identity.mparticle.com/v1`. Every operation returns an **MPID** — the
64-bit identifier mParticle uses for a person across devices and channels. The MPID is what
you stamp on event batches and what you query the Profile API with.

## Authenticate — prefer the HMAC digest

Two schemes are in the published Swagger:

1. **`BasicSecurity`** — `Authorization: Basic base64(<apiKey>:<apiSecret>)`.
2. **`ApiKeyDigest`** — mParticle's recommended scheme. Send three headers:
   - `x-mp-key`: the API key
   - `Date`: an ISO 8601 timestamp
   - `x-mp-signature`: hex HMAC-SHA256, keyed with the API **secret**, over
     `"<HTTP METHOD>\n<ISO 8601 date>\n<request path><request body>"`

Use the digest when you can: the secret never leaves your process, and the signature binds
the request path and body.

## `POST /identify` — get or create an MPID

Send the identities you know. mParticle matches against its graph and returns the MPID,
creating one if nothing matches.

```json
{
  "client_sdk": { "platform": "web", "sdk_vendor": "mparticle", "sdk_version": "2.78.0" },
  "environment": "development",
  "request_id": "<your correlation id>",
  "known_identities": { "email": "user@example.com", "customerid": "12345" }
}
```

Response carries `mpid`, `context` and `matched_identities`.

## `POST /login` — move to an authenticated state

Same request shape. Call it when the user signs in and you now know a stronger identity
(`customerid`, `email`). This is what links the previously anonymous device profile to the
known person.

## `POST /logout` — move to an anonymous state

Call on sign-out so subsequent events are not attributed to the authenticated profile.

## `POST /{mpid}/modify` — change the identity graph

Path parameter is the MPID. Body carries `identity_changes[]`, each with
`identity_type`, `old_value`, `new_value`. Use it to correct or remove an identity on an
existing profile.

**This is the highest-consequence operation on the API.** It rewrites who a profile *is*.
Do not let an agent call it unattended: see `agentic-access/mparticle-agentic-access.yml`
for the recommended execution contract (short-lived token, audit required, human in the
loop on abnormal or high-value changes).

## Errors

`{"errors":[{"code":"...","message":"..."}]}`.

| Status | Do |
|---|---|
| 400 | Read the body. Fix the request; do not retry unchanged. |
| 401 | Credentials or signature wrong — recompute the digest, check the `Date` skew. |
| 429 | Back off exponentially. |
| 5xx | Retry with exponential backoff. |

## Spec gap to know about

The docs also describe `POST /search` (does this identity already exist?). It is **not in
the published Swagger document**, so it is not modelled here. Verify against the docs
before relying on it.
