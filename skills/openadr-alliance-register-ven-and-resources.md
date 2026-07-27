---
name: Register a VEN and its resources
description: Enroll a Virtual End Node on a VTN and attach the devices it controls as first-class resource objects, from either the BL side or the VEN side.
api: openapi/openadr-3-1-1-openapi.yaml
generated: '2026-07-27'
method: generated
operations:
  - fetchToken
  - createVen
  - searchVens
  - searchVenByID
  - updateVen
  - createResource
  - searchVenResources
  - searchVenResourceByID
  - updateVenResource
---

# Register a VEN and its resources

A **VEN** (Virtual End Node) is the client that receives events and reports back. A
**resource** is a device or load the VEN controls — an EV charger, a battery, a heat pump,
an inverter.

> **3.1.0 moved resources.** Up to 3.0.1 they lived at `/vens/{venID}/resources`. From 3.1.0
> a resource is a first-level object at `/resources` (`?venID=` to scope). The
> `operationId`s are unchanged (`createResource`, `searchVenResources`,
> `searchVenResourceByID`, `updateVenResource`, `deleteVenResource`) — only the paths moved.
> Check the VTN's version before choosing a path.

## 1. Authenticate

`fetchToken` with `write_vens` plus whichever read scope your role gets — `read_all` for BL,
`read_ven_objects` or `read_targets` for a VEN.

## 2. Know which request shape you are

Both `venRequest` and `resourceRequest` are a `oneOf` over a BL variant and a VEN variant,
discriminated by `objectType`. **This is the access-control boundary, not a convenience.**

| Writing as | VEN body | Resource body |
|---|---|---|
| BL / VTN-side | `BlVenRequest` — required `objectType`, `clientID`, `venName`; may also set `targets`, `attributes` | `BlResourceRequest` — required `objectType`, `resourceName`, `venID`; may also set `targets`, `attributes` |
| VEN | `VenVenRequest` — required `objectType`, `venName`; may set `attributes` | `VenResourceRequest` — required `objectType`, `resourceName`; may set `attributes` |

A VEN cannot set `clientID`, `venID` or `targets`. Those are the fields object privacy is
built on: the VTN assigns `clientID` so it can decide which objects that VEN may read, and
`targets` decide which events reach it. If you send a BL body with a VEN token you get `403`.

## 3. Create the VEN

`createVen` (`POST /vens`) returns `201` with the server-assigned `id`. Call `searchVens`
first with the `venName` filter — re-creating a VEN is not idempotent and there is no
`Idempotency-Key` in this protocol.

`attributes` on a VEN or resource is a valuesMap whose allowed keys are published in
`json-schema/openadr-3-1-1-ven-resource-attributes.schema.yaml`: `LOCATION`, `AREA`,
`MAX_POWER_CONSUMPTION`, `MAX_POWER_EXPORT`, `DESCRIPTION`.

## 4. Attach resources

`createResource` once per controllable device. On 3.1.x set `venID` (BL body) or let the
VTN infer it from your token (VEN body). `MAX_POWER_CONSUMPTION` and `MAX_POWER_EXPORT`
are what make a resource dispatchable — set them or the VTN cannot size a dispatch.

## 5. Read back and maintain

- `searchVens` — filters: `venName`, `targets`, `skip`, `limit` (max 50).
- `searchVenResources` — filters: `resourceName`, `venID`, `targets`, `skip`, `limit`.
- `updateVen` / `updateVenResource` are full-object PUTs. There is no PATCH: fetch with
  `searchVenByID` / `searchVenResourceByID`, mutate, put the whole thing back, or you will
  silently drop fields.

## Errors

`403` on a write almost always means role mismatch, not a bad token — check you sent the
right `oneOf` variant. `409` on create is a uniqueness conflict; search rather than retry.
`404` on the 3.1.x `/resources/{resourceID}` path may mean you are talking to a 3.0.x VTN
that still nests resources under the VEN. See `errors/openadr-alliance-problem-types.yml`.
