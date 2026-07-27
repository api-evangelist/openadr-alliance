---
name: Authenticate against an OpenADR 3 VTN
description: Discover a VTN's token endpoint and obtain a scoped OAuth 2.0 client-credentials JWT before calling any other OpenADR 3 operation.
api: openapi/openadr-3-1-1-openapi.yaml
generated: '2026-07-27'
method: generated
operations:
  - getAuthServerInfo
  - fetchToken
---

# Authenticate against an OpenADR 3 VTN

OpenADR 3 has no Alliance-hosted API. Every VTN — a utility, ISO/RTO, aggregator or
in-building system — publishes its own base URL. You must be told that base URL out of
band; there is no registry and no `/.well-known/` discovery document on any Alliance host.

## 1. Find the token endpoint

Call `getAuthServerInfo` (`GET /auth/server`). It is unauthenticated and returns an
`authServerInfo` object carrying the URL of the token endpoint. This is the OpenADR
analogue of RFC 8414 authorization-server metadata. It was added in 3.1.0 — on a 3.0.x VTN
it does not exist, and the token endpoint is `POST /auth/token` on the same base URL.

## 2. Get a token

Call `fetchToken` (`POST /auth/token`). It is unauthenticated and takes a
`clientCredentialRequest` as `application/x-www-form-urlencoded` — not JSON. Ask only for
the scopes you need; a VTN will refuse scopes your client role is not entitled to.

Scopes, and the role each belongs to:

| Scope | Who may hold it |
|---|---|
| `read_all` | BL (business-logic / VTN-side) clients only |
| `read_ven_objects` | VENs — objects whose `clientID` matches their own |
| `read_targets` | VENs — objects whose `targets` they can match |
| `read_bl` | BL-scoped notifier topic metadata |
| `write_programs` | BL only |
| `write_events` | BL only |
| `write_reports` | VENs only |
| `write_subscriptions` | VENs and BL |
| `write_vens` | VENs and BL |

On 3.0.x only six scopes exist (`read_all`, `write_programs`, `write_events`,
`write_reports`, `write_subscriptions`, `write_vens`); the read split arrived in 3.1.0.

## 3. Use the token

Send the JWT as `Authorization: Bearer <token>` on every other operation. `fetchToken` and
`getAuthServerInfo` are the only two operations that do not require it.

## Failure modes

- `400` on `fetchToken` returns an `authError` object in the RFC 6749 §5.2 shape, **not**
  the `problem` envelope every other operation uses. Parse it differently.
- `401` means the token is missing, expired or malformed. `403` means the token is valid
  but the scope or role is wrong — retrying will not help; request different scopes.
- `501` on `fetchToken` means this VTN does not implement the token endpoint and expects
  credentials from an external authorization server.

## Notes

- Transport security is not optional in practice: the Alliance runs its own PKI (RSA and
  ECC) for OpenADR device certificates through Eonti, and 3.1.0 shipped explicit TLS
  hardening. Ask the VTN operator whether client certificates are required in addition to
  the bearer token.
- See `authentication/openadr-alliance-authentication.yml` and
  `scopes/openadr-alliance-scopes.yml`.
