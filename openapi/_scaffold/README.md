# Quarantined — API Evangelist-authored scaffold specs

These three OpenAPI documents were **written by the API Evangelist pipeline**, not published by
mParticle. They were a hand-authored approximation of the mParticle Events API (`/events`,
`/bulkevents`, `/bulkevents/historical`) produced before mParticle's own machine-readable specs were
located.

On **2026-08-13** the real, first-party specs were found published by mParticle at
`https://docs.mparticle.com/downloads/` and harvested verbatim into `openapi/`:

| Real spec (provider-published) | Source URL |
|---|---|
| `openapi/mparticle-events-openapi-original.yml` | https://docs.mparticle.com/downloads/mparticle.events.oas.yaml |
| `openapi/mparticle-dataplanning-openapi-original.yml` | https://docs.mparticle.com/downloads/mparticle.dataplanning.oas.yaml |
| `openapi/mparticle-identity-swagger-original.yml` | https://docs.mparticle.com/downloads/identity-swagger.yaml |

The scaffold files are retained here for audit only. **They are not referenced from `apis.yml` and
must not be scored, derived from, or presented as mParticle artifacts.** The provider's own Events
API spec supersedes them completely (mParticle's operationIds are `uploadEvents` / `bulkUploadEvents`,
not the scaffold's `uploadEvent` / `uploadBulkEvents` / `uploadHistoricalBulkEvents`).

See the network memory note `scaffold-fabrication-sweep` for the network-wide policy: quarantine
rather than delete, so the absence reads as deliberate.
