# OpenADR Alliance (openadr-alliance)

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

The OpenADR Alliance is a San Ramon, California mutual-benefit membership corporation that develops, certifies, and promotes OpenADR, the open information-exchange model utilities, ISOs/RTOs, aggregators, and device makers use to automate demand response and dispatch distributed energy resources. It is a standards body, not a service operator: it publishes the OpenADR 2.0a/2.0b profile specifications (approved by the IEC as IEC/PAS 62746-10-1) and OpenADR 3.0/3.1, which abandoned the SOAP-era design and is now defined entirely by an OpenAPI 3.0 contract. It also runs the OpenADR and EcoPort (CTA-2045-B) certification programmes and their public certified-product databases.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/openadr-alliance/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/openadr-alliance/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Utilities
- Electricity
- Demand Response
- Grid
- DER
- OpenADR
- Standards
- Smart Grid
- EV Charging
- Certification

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### OpenADR 3 API

OpenADR 3 is the RESTful third generation of the OpenADR protocol, defined in full by a single OpenAPI 3.0 document published by the OpenADR Alliance. It supports energy-retailer-to-energy-customer demand response programs through operations to create, update, delete, and search programs, events, reports, subscriptions, VENs (Virtual End Nodes), and VEN resources, plus an `/auth/token` endpoint. Version 3.1.0 adds MQTT notifier topics, VEN-scoped topics for object privacy, and VTN discovery. Security is OAuth 2.0 client credentials with JWT bearer tokens and role-scoped grants that separate what a Business Logic (BL/VTN) client may write from what a VEN may write.

This is a protocol contract, not an Alliance-hosted service. Every implementer — a utility, aggregator, or vendor VTN — stands up its own base URL, so no single `baseURL` exists.

- **Human URL:** [https://www.openadr.org/specification](https://www.openadr.org/specification)
- **Base URL:** none — implementer-hosted

#### Tags

- Demand Response
- OpenADR
- Energy
- DER

#### Properties

- [OpenAPI](openapi/openadr-3-1-1-openapi.yaml) — OpenADR 3.1.1 (development), 27 paths / 45 operations
- [OpenAPI](openapi/openadr-3-1-0-openapi.yaml) — OpenADR 3.1.0 (released), 27 paths / 45 operations
- [OpenAPI](openapi/openadr-3-0-1-openapi.yaml) — OpenADR 3.0.1 (released), 13 paths / 31 operations
- [OpenAPI](openapi/openadr-3-0-0-openapi.yaml) — OpenADR 3.0.0 (released), 13 paths / 31 operations
- [Documentation](https://www.openadr.org/specification)
- [Documentation](https://www.openadr.org/how-to-build-a-product)
- [Conformance](https://www.openadr.org/openadr-3-certification)
- [Source Code](https://github.com/grid-coordination/openadr3-specification)
- [AsyncAPI](asyncapi/openadr-alliance-notifications-asyncapi.yml) — derived event surface (webhook + MQTT)
- [Webhooks](asyncapi/openadr-alliance-webhooks.yml) — notification catalog
- [JSON Schema](json-schema/_index.yml) — the six Alliance-published 3.1.1 enumeration schemas
- [Examples](examples/openadr-alliance-examples.yml)
- [Data Model](data-model/openadr-alliance-data-model.yml)
- [Error Catalog](errors/openadr-alliance-problem-types.yml)
- [Tool Crosswalk](mcp/openadr-alliance-tool-crosswalk.yml)
- [Overlays](overlays/) — one per specification release

## Common Properties

- [Website](https://www.openadr.org/)
- [Documentation](https://www.openadr.org/specification)
- [Portal](https://www.openadr.org/specification-download)
- [API Reference](https://www.openadr.org/specification)
- [Getting Started](https://www.openadr.org/how-to-build-a-product)
- [Conformance](https://www.openadr.org/openadr-3-certification)
- [Tools](https://test-tool.openadr.org/)
- [Tools](https://www.openadr.org/openadr-test-tool-store) — test tool store
- [Directory](https://products.openadr.org/) — OpenADR certified product database
- [Directory](https://ecoport.openadr.org/) — EcoPort (CTA-2045-B) certified product database
- [Pricing](https://www.openadr.org/join) — membership dues
- [Sign Up](https://www.openadr.org/join)
- [Terms of Service](https://www.openadr.org/assets/docs/OpenADR%20Alliance%20Bylaws%20%26%20Member%20Agreement.zip) — bylaws, membership agreement, IPR policy
- [Support](https://www.openadr.org/faq)
- [Blog](https://www.openadr.org/openadr-alliance-blog)
- [GitHub Organization](https://github.com/oadr3-org)
- [LinkedIn](https://www.linkedin.com/company/openadr-alliance)
- [Login](https://www.openadr.org/login)
- [Privacy Policy](https://www.openadr.org/privacy-policy)
- [Support](https://www.openadr.org/contact-us)

## Enrichment Artifacts

Round 2026-07-27. Provenance is recorded in each file's frontmatter (`method: searched | generated | derived | probed`).

| Artifact | Method | Note |
|---|---|---|
| [`json-schema/`](json-schema/_index.yml) | searched | The six enumeration schemas shipped with OpenADR 3.1.1 — event interval payloads (38), report payloads (24), program attributes, VEN/resource attributes, reading types, units. Saved verbatim. |
| [`changelog/`](changelog/openadr-alliance-changelog.yml) | searched | Structured from the Alliance `changeLog.md` and `VERSIONS.yaml`. 3.1.0 is explicitly **not** backwards compatible with 3.0.1. |
| [`packages/`](packages/openadr-alliance-packages.yml) | searched | Seven third-party libraries; **zero first-party SDKs**. `github.com/oadr3-org` has no public repositories. |
| [`plans/`](plans/openadr-alliance-plans.yml) | searched | Membership dues (revenue-banded $1.5k–$40k/yr), test tool licences, consulting packages. The specification itself is free. |
| [`sandbox/`](sandbox/openadr-alliance-sandbox.yml) | searched | Online test tool, downloadable CI test assets, free Eonti test PKI certificates, SwaggerHub mock. No magic test values are published. |
| [`conformance/`](conformance/openadr-alliance-conformance.yml) | searched | IEC/PAS 62746-10-1, OASIS Energy Interoperation, NIST Smart Grid, CTA-2045-B, ANSI-SCTE 267, MQTT, RFC 3339/8601. RFC 7807 is *partial* — right members, wrong media type. |
| [`lifecycle/`](lifecycle/openadr-alliance-lifecycle.yml) | searched | Semver on the specification; no Sunset/Deprecation headers and no deprecation policy. The real transition commitment is the six-month certification grace period. |
| [`well-known/`](well-known/openadr-alliance-well-known.yml) | probed | **Zero discovery documents.** Every 200 on `www.openadr.org` and `test-tool.openadr.org` is a soft-200 HTML catch-all. |
| [`security/`](security/openadr-alliance-domain-security.yml) | probed | TLS 1.3 + HSTS on the website; no DNSSEC, no CAA, DMARC `p=none`. No security.txt, no VDP, no trust center. |
| [`authentication/`](authentication/openadr-alliance-authentication.yml) · [`scopes/`](scopes/openadr-alliance-scopes.yml) | derived | OAuth 2.0 client credentials + JWT bearer; 9 role-encoded scopes in 3.1.1 (6 in 3.0.x). |
| [`conventions/`](conventions/openadr-alliance-conventions.yml) | derived | skip/limit pagination capped at 50, no total, no next link. **No idempotency key anywhere** — a retried `createEvent` dispatches twice. No rate-limit contract and no 429. |
| [`errors/`](errors/openadr-alliance-problem-types.yml) | derived | Zalando `problem` envelope on `application/json`; `authError` (RFC 6749 §5.2) on the token endpoint only. |
| [`data-model/`](data-model/openadr-alliance-data-model.yml) | derived | Six addressable objects plus the value structures they compose. |
| [`vocabulary/`](vocabulary/openadr-alliance-vocabulary.yml) | derived | 127 terms across 10 groups — objects, roles, operations, payload types, attributes, reading types, units. |
| [`asyncapi/`](asyncapi/openadr-alliance-notifications-asyncapi.yml) | derived | AsyncAPI 3.0.0 for the webhook and MQTT notifier surface. **Not a normative Alliance artifact** — the Alliance publishes no AsyncAPI. |
| [`examples/`](examples/openadr-alliance-examples.yml) | derived | 25 payloads assembled from the specification's own inline example values. |
| [`overlays/`](overlays/) | generated | One Overlay 1.0.0 per release; declares the tag block missing from 3.0.0/3.0.1/3.1.0 and flags the non-production `servers[]` entry. |
| [`skills/`](skills/_index.yml) | generated | Five Agent Skills — authenticate, dispatch an event, register a VEN, subscribe to notifications, report telemetry. Every step grounded in a real `operationId`. |
| [`arazzo/`](arazzo/_index.yml) | generated | Two native workflows: BL-side dispatch, VEN-side onboard-and-subscribe. |
| [`llms/`](llms/openadr-alliance-llms.txt) | generated | `https://www.openadr.org/llms.txt` returns 404. |
| [`mcp/`](mcp/openadr-alliance-tool-crosswalk.yml) | derived | No Alliance-hosted MCP server exists. A 45-tool *candidate* surface is derived from 3.1.1 and crosswalked one-to-one to its backing operations. |
| [`agentic-access/`](agentic-access/openadr-alliance-agentic-access.yml) | generated | 152 operations classified into recommended `x-agentic-access` contracts. |

## Access Posture

The OpenADR 3 OpenAPI is a genuine, parseable machine-readable contract, and the Alliance describes its specifications as license-free. But the Alliance operates no developer portal: `developer.`, `developers.`, `api.`, `docs.`, and `data.` subdomains of openadr.org do not resolve, and `/openapi.json`, `/swagger.json`, and `/api-docs` all return 404. The canonical specification repository, `github.com/oadr3-org`, publishes zero public repositories and is membership-gated. The public route to the spec is a registration form at `/specification-download` that emails download links under terms and conditions. The online certification test tool at `test-tool.openadr.org` is a member login behind a paid licence (USD $1,250 per certification test).

The Alliance publishes no consumer energy data and no open grid or market data. It defines the protocol; other parties implement and operate it.

See [review.yml](review.yml) for the full probe log and mandate assessment.

## Maintainers

- Kin Lane — kin@apievangelist.com
