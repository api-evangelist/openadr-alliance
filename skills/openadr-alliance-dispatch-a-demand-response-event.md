---
name: Publish a program and dispatch a demand response event
description: The BL/VTN-side flow — create a demand response program, publish an event with priced or dispatch-setpoint intervals against it, then amend or cancel that event.
api: openapi/openadr-3-1-1-openapi.yaml
generated: '2026-07-27'
method: generated
operations:
  - fetchToken
  - createProgram
  - searchAllPrograms
  - searchProgramByProgramId
  - createEvent
  - searchAllEvents
  - searchEventsByID
  - updateEvent
  - deleteEvent
---

# Publish a program and dispatch a demand response event

This is the **BL (business-logic) side** of OpenADR 3. Only a client holding
`write_programs` and `write_events` can run it — a VEN cannot.

> **Safety first.** An OpenADR event dispatches real load on a real grid. There is no
> `Idempotency-Key` in this protocol: a retried `createEvent` creates a **second** event, and
> both will dispatch. Before any retry, call `searchAllEvents` with the `programID` filter
> and check whether your event already landed.

## 1. Authenticate

`fetchToken` with `write_programs write_events read_all`. See the *Authenticate against an
OpenADR 3 VTN* skill.

## 2. Create or find the program

A program is the container an event hangs off. Call `searchAllPrograms` first — programs
are long-lived and are usually already there.

If you must create one, `createProgram` takes a `programRequest`. Fields that matter:

- `programName` — short handle, e.g. `ResTOU`.
- `intervalPeriod` — `start` (RFC 3339), `duration` and `randomizeStart` (ISO 8601). A
  `start` of `0001-01-01T00:00:00` means "now"; a `duration` of `P9999Y` means indefinite.
- `attributes` — a valuesMap whose allowed keys are published in
  `json-schema/openadr-3-1-1-program-attributes.schema.yaml`: `PROGRAM_LONG_NAME`,
  `RETAILER_NAME`, `RETAILER_LONG_NAME`, `PROGRAM_TYPE`, `COUNTRY`,
  `PRINCIPAL_SUBDIVISION`, `BINDING_EVENTS`, `LOCAL`.
- `targets` — plain strings as of 3.1.0. These are not just a filter: they are the access
  control boundary for VENs holding `read_targets`.
- `payloadDescriptors` — supply an `eventPayloadDescriptor` per payload type you will use,
  giving `units` and `currency`. Without it a bare `PRICE` float is uninterpretable.

`createProgram` returns `201`. `409` means the name or id already exists — search, do not retry.

## 3. Publish the event

`createEvent` takes an `eventRequest`:

- `programID` — the program from step 2.
- `intervals[]` — each interval carries an `intervalPeriod` (or inherits the event's) and a
  `payloads` valuesMap.
- `payloads[].type` must come from
  `json-schema/openadr-3-1-1-event-interval-payloads.schema.yaml`. The main families:
  - pricing — `PRICE`, `PRICE_ALTERNATE`, `EXPORT_PRICE`, `GHG`
  - simple level — `SIMPLE` (0–3), `CONTROL_LEVEL_OFFSET` (−10..+10),
    `CONTROL_LEVEL_OFFSET_PERCENT`
  - dispatch — `DISPATCH_SETPOINT`, `DISPATCH_SETPOINT_RELATIVE`, `DISPATCH_INSTRUCTION`,
    `CONTROL_SETPOINT`, `CHARGE_STATE_SETPOINT`
  - capacity — the `IMPORT_*` / `EXPORT_*` subscription, reservation, available and limit types
  - alerts — `ALERT_GRID_EMERGENCY`, `ALERT_BLACK_START`, `ALERT_FLEX_ALERT`,
    `ALERT_POSSIBLE_OUTAGE`, `ALERT_FIRE`, `ALERT_FREEZING`, `ALERT_WIND`, `ALERT_TSUNAMI`,
    `ALERT_AIR_QUALITY`, `ALERT_OTHER`
  - shapes — `CURVE` (VoltVar / VoltWatt points), `OLS` (optimum load shape, ANSI-SCTE 267)
  - CTA-2045 pass-through — `CTA2045_REBOOT`, `CTA2045_SET_OVERRIDE_STATUS`
- `targets` — who this event is for. 3.1.0 added VEN-private events: scope an event to a
  single VEN and only that VEN can read it.
- `priority` — lower is more important; used when events overlap.
- `reportDescriptors[]` — request telemetry back. See the *report telemetry* skill.

`createEvent` returns `201` with the server-assigned `id`.

## 4. Amend or cancel

- `updateEvent` (`PUT /events/{eventID}`) replaces the whole event. There is no PATCH; send
  the full object or you will drop fields.
- `deleteEvent` (`DELETE /events/{eventID}`) cancels it. This is the cancel path — there is
  no cancelled state on the object.
- Both are idempotent by HTTP method, unlike the create.

## 5. Verify

`searchAllEvents` with `programID` and `active=true` (added 3.1.0) lists the events that
have not yet transpired. Pagination is `skip` + `limit`, capped at 50, and the response is a
bare array with no total — if you get exactly `limit` rows, page again.

## Errors

All non-auth errors return the `problem` envelope (type, title, status, detail, instance) as
`application/json` — note, *not* `application/problem+json`. `403` is a scope/role refusal;
`409` is a uniqueness conflict; `400` is malformed input. See
`errors/openadr-alliance-problem-types.yml`.
