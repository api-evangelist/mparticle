# mParticle (mparticle)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

mParticle is a customer data platform (CDP) that helps brands collect, unify, and activate customer data across mobile, web, OTT, and server sources, then forward it in real time to hundreds of analytics, marketing, and warehouse destinations. The mParticle developer platform exposes server Events, IDSync, Profile, Warehouse Sync, Calculated Attributes, and Platform APIs that let teams ingest events, resolve identity, manage configurations, and orchestrate audiences using HTTP Basic and bearer token authentication.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/mparticle/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/mparticle/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Customer Data Platform
- CDP
- Analytics
- Identity Resolution
- Audience
- Data Pipeline
- Marketing Data

## Timestamps

- **Created:** 2026-05-11
- **Modified:** 2026-05-11

## APIs

### mParticle Events API

Server-to-server REST API for sending event batches, bulk uploads, and historical data into mParticle from backend systems. Authenticates with HTTP Basic auth using a server-side API key and secret pair.

- **Human URL:** [https://docs.mparticle.com/developers/apis/http/](https://docs.mparticle.com/developers/apis/http/)
- **Base URL:** `https://s2s.mparticle.com/v2`

#### Tags

- REST
- Server-to-Server
- Events
- Bulk
- HTTP Basic

#### Properties

- [Documentation](https://docs.mparticle.com/developers/apis/http/)
- [J S O N  Reference](https://docs.mparticle.com/developers/apis/json-reference/)
- [Postman Collection](collections/mparticle.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/mparticle.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### mParticle IDSync API

Identity resolution REST API used to match, link, and modify user identities across devices and channels in mParticle, returning a stable mParticle ID (MPID) for downstream use.

- **Human URL:** [https://docs.mparticle.com/developers/apis/idsync/](https://docs.mparticle.com/developers/apis/idsync/)
- **Base URL:** `https://identity.mparticle.com/v1`

#### Tags

- REST
- Identity
- IDSync
- MPID

#### Properties

- [Documentation](https://docs.mparticle.com/developers/apis/idsync/)
- [Postman Collection](collections/mparticle.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/mparticle.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### mParticle Profile API

REST API for retrieving unified user profiles, identities, attributes, and audience memberships at scale to personalize downstream applications.

- **Human URL:** [https://docs.mparticle.com/developers/apis/profile-api/](https://docs.mparticle.com/developers/apis/profile-api/)
- **Base URL:** `https://api.mparticle.com/userprofile/v1`

#### Tags

- REST
- Profiles
- Attributes
- Audiences

#### Properties

- [Documentation](https://docs.mparticle.com/developers/apis/profile-api/)
- [Postman Collection](collections/mparticle.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/mparticle.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### mParticle Platform API

Management REST API used to programmatically configure mParticle inputs, outputs, filters, audiences, data plans, and workspace settings as part of a fully versioned CDP-as-code workflow.

- **Human URL:** [https://docs.mparticle.com/developers/apis/platform/overview/](https://docs.mparticle.com/developers/apis/platform/overview/)
- **Base URL:** `https://api.mparticle.com/platform/v2`

#### Tags

- REST
- Management
- Inputs
- Outputs
- Data Plans
- Bearer Token

#### Properties

- [Documentation](https://docs.mparticle.com/developers/apis/platform/overview/)
- [Postman Collection](collections/mparticle.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/mparticle.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/mparticle)
- [LinkedIn](https://www.linkedin.com/company/mparticle-inc-)
- [Website](https://www.mparticle.com/)
- [Documentation](https://docs.mparticle.com/)
- [Developer  Portal](https://www.mparticle.com/developers/)
- [Pricing](https://www.mparticle.com/pricing/)
- [Sign Up](https://www.mparticle.com/get-demo/)
- [L L Ms Txt](https://docs.mparticle.com/llms.txt)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
