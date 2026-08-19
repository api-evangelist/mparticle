---
name: Govern mParticle data plans
description: Create, version and activate an mParticle data plan, and validate an event batch against it before you ingest anything.
api: openapi/mparticle-dataplanning-openapi-original.yml
operations:
  - get-plans
  - create-plan
  - get-plan
  - update-plan
  - delete-plan
  - create-plan-version
  - get-plan-version
  - update-plan-version
  - delete-plan-version
  - validate-plan-document
generated: '2026-08-13'
method: generated
source: openapi/mparticle-dataplanning-openapi-original.yml, https://docs.mparticle.com/developers/apis/dataplanning-api/
---

# Govern mParticle data plans

A data plan is the schema contract for the events a workspace accepts. Server base URL is
`https://api.mparticle.com/platform/v2/workspaces/{workspace_id}/plans` — `workspace_id` is
a required server variable, so every path below is relative to that.

## Authenticate — OAuth 2.0 client credentials

```
POST https://sso.auth.mparticle.com/oauth/token
Content-Type: application/json

{ "client_id": "...", "client_secret": "...",
  "audience": "https://api.mparticle.com",
  "grant_type": "client_credentials" }
```

Send the result as `Authorization: Bearer {access_token}`. Tokens live about **8 hours**
and **cannot be revoked**, so cache them and never mint one per request. Discovery for the
issuer is at `https://sso.auth.mparticle.com/.well-known/openid-configuration`
(`well-known/mparticle-openid-configuration.json`). No scopes are involved.

## Inspect what exists — `get-plans`, `get-plan`

- `get-plans` — `GET /` returns every plan in the workspace. **There is no pagination** and
  no filter parameters; you get the whole collection.
- `get-plan` — `GET /{plan_id}` returns one plan with its `data_plan_versions`.

## Create and version — `create-plan`, `create-plan-version`

- `create-plan` — `POST /` with `data_plan_id`, `data_plan_name`, `data_plan_description`.
  The plan is a container; it carries no rules of its own.
- `create-plan-version` — `POST /{plan_id}/versions` with a `version_document`. The version
  document is the actual plan: a `data_points[]` array where each point is a `match` (which
  event or attribute) plus a `validator` (the JSON Schema it must satisfy).

Never edit a live version in place when instrumentation is shipping against it — cut a new
version instead. Use `update-plan-version` (`PATCH /{plan_id}/versions/{plan_version}`) only
while a version is still a draft.

## Validate before you ingest — `validate-plan-document`

`POST /validate` with `{ "document": <version_document>, "batch": <event batch> }`.

This is the **dry run**. It tells you what the batch would violate without writing anything.
Run it in CI on your fixture batches, and run it from an agent before the first
`uploadEvents` call of a new integration.

Errors come back as `SchemaErrorList`:

```json
{ "errors": [ {
  "message": "...", "match_key": "...", "error_type": "unplanned",
  "value": "...", "schema_pointer": "...", "event_pointer": "...", "keyword": "..." } ] }
```

`error_type` is one of `unknown`, `unplanned`, `missing_required`, `invalid_value`.
`event_pointer` points at the offending place in *your* batch — use it to report the fix
precisely rather than dumping the whole error list.

## Deleting

`delete-plan` (`DELETE /{plan_id}`) and `delete-plan-version`
(`DELETE /{plan_id}/versions/{plan_version}`) both return `204` and are irreversible. Treat
them as human-approved operations.

## Rate limits and errors

3,000 requests/minute per account and 6,000 per organization. `429` on exhaustion — back
off exponentially. Error envelope is `{"errors":[{"message": "..."}]}`; not RFC 9457.

## Same work from the command line

`@mparticle/cli` (`mp planning:*`) drives every operation above, including
`mp planning:batches:validate --batchFile ./batch.json`. It has not shipped a release since
2022 — see `cli/mparticle-cli.yml` before you depend on it.
