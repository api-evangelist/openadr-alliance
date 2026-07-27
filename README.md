# OpenADR Alliance (openadr-alliance)

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

## Common Properties

- [Website](https://www.openadr.org/)
- [Documentation](https://www.openadr.org/specification)
- [Portal](https://www.openadr.org/specification-download)
- [Conformance](https://www.openadr.org/openadr-3-certification)
- [Tools](https://test-tool.openadr.org/)
- [Directory](https://products.openadr.org/) — OpenADR certified product database
- [Directory](https://ecoport.openadr.org/) — EcoPort (CTA-2045-B) certified product database
- [Support](https://www.openadr.org/faq)
- [Blog](https://www.openadr.org/openadr-alliance-blog)
- [GitHub Organization](https://github.com/oadr3-org)
- [LinkedIn](https://www.linkedin.com/company/openadr-alliance)
- [Login](https://www.openadr.org/login)
- [Privacy Policy](https://www.openadr.org/privacy-policy)
- [Support](https://www.openadr.org/contact-us)

## Access Posture

The OpenADR 3 OpenAPI is a genuine, parseable machine-readable contract, and the Alliance describes its specifications as license-free. But the Alliance operates no developer portal: `developer.`, `developers.`, `api.`, `docs.`, and `data.` subdomains of openadr.org do not resolve, and `/openapi.json`, `/swagger.json`, and `/api-docs` all return 404. The canonical specification repository, `github.com/oadr3-org`, publishes zero public repositories and is membership-gated. The public route to the spec is a registration form at `/specification-download` that emails download links under terms and conditions. The online certification test tool at `test-tool.openadr.org` is a member login behind a paid licence (USD $1,250 per certification test).

The Alliance publishes no consumer energy data and no open grid or market data. It defines the protocol; other parties implement and operate it.

See [review.yml](review.yml) for the full probe log and mandate assessment.

## Maintainers

- Kin Lane — kin@apievangelist.com
