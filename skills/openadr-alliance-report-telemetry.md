---
name: Report telemetry back to a VTN
description: The VEN-side flow — read the reportDescriptors a VTN attached to an event, then post interval readings, usage, operating state or storage levels back as report objects.
api: openapi/openadr-3-1-1-openapi.yaml
generated: '2026-07-27'
method: generated
operations:
  - fetchToken
  - searchAllEvents
  - searchEventsByID
  - createReport
  - searchAllReports
  - searchReportsByReportID
  - updateReport
  - deleteReport
---

# Report telemetry back to a VTN

Reporting is the **VEN side** of OpenADR 3. Only a client holding `write_reports` may write
one — a BL client cannot. Reports are what turn a dispatch into a settled, measurable
demand-response event.

## 1. Authenticate

`fetchToken` with `write_reports` plus your read scope (`read_ven_objects` or
`read_targets`).

## 2. Find out what the VTN asked for

Call `searchAllEvents` (filter by `programID`, and `active=true` to skip transpired events)
or `searchEventsByID`. Each event may carry `reportDescriptors[]`, and that is the request:

- `payloadType` — required. The permitted values are published in
  `json-schema/openadr-3-1-1-report-payloads.schema.yaml`:
  - metering — `READING`, `USAGE`, `DEMAND`, `DELTA_USAGE`, `BASELINE`, `DATA_QUALITY`
  - control state — `SETPOINT`, `OPERATING_STATE`, `SIMPLE_LEVEL`
  - flexibility — `UP_REGULATION_AVAILABLE`, `DOWN_REGULATION_AVAILABLE`,
    `REGULATION_SETPOINT`, `LOAD_SHED_DELTA_AVAILABLE`, `GENERATION_DELTA_AVAILABLE`
  - storage — `STORAGE_USABLE_CAPACITY`, `STORAGE_CHARGE_LEVEL`,
    `STORAGE_MAX_DISCHARGE_POWER`, `STORAGE_MAX_CHARGE_POWER`, `STORAGE_DISPATCH_FORECAST`
  - forecast and capacity — `USAGE_FORECAST`, `IMPORT_RESERVATION_CAPACITY`,
    `IMPORT_RESERVATION_FEE`, `EXPORT_RESERVATION_CAPACITY`, `EXPORT_RESERVATION_FEE`
- `readingType` — from `json-schema/openadr-3-1-1-reading-types.schema.yaml`:
  `DIRECT_READ`, `ESTIMATED`, `SUMMED`, `MEAN`, `PEAK`, `FORECAST`, `AVERAGE`, `NORMAL`, `ERROR`
- `units` — from `json-schema/openadr-3-1-1-units.schema.yaml`: `KWH`, `KW`, `THERMS`,
  `GHG`, `VOLTS`, `AMPS`, `CELSIUS`, `FAHRENHEIT`, `PERCENT`, `KVAH`, `KVARH`, `KVA`, `KVAR`
- timing — `startInterval`, `numIntervals`, `historical`, `frequency`, `repeat`,
  `reportIntervals` (the timing controls were expanded in 3.1.0, issue 271)
- `aggregate` — whether the VTN wants per-resource detail or a rolled-up number
- `targets` — which resources the descriptor applies to

Answer what was asked for. A report the VTN did not request may be rejected or ignored.

## 3. Post the report

`createReport` (`POST /reports`) takes a `reportRequest`. Required: `eventID`, `clientName`,
`resources`. Also settable: `reportName`, `payloadDescriptors`.

- `resources[]` — one entry per reporting resource. Each requires `resourceName` and
  `intervals[]`, and may carry its own `intervalPeriod`.
- Each `interval` requires `id` and `payloads`, and may carry its own `intervalPeriod`;
  otherwise it inherits the report's or the event's.
- `payloadDescriptors[]` are `reportPayloadDescriptor` objects: `objectType`, `payloadType`,
  plus `readingType`, `units`, `accuracy` and `confidence`. Send them — a bare float with no
  descriptor is not interpretable, and `accuracy`/`confidence` are how you signal an
  estimated read honestly.
- `programID` was **removed from report in 3.1.0** (issue 253). The event carries the program
  linkage. Do not send it to a 3.1.x VTN.
- Serial data such as a long price or usage series has a compact representation as of 3.1.0
  (issue 238) — use it rather than one interval per point where the VTN supports it.

## 4. Corrections

- There is no idempotency key. A retried `createReport` produces a **duplicate** report, and
  duplicate telemetry corrupts settlement. On any ambiguous failure, call `searchAllReports`
  filtered by `programID`, `eventID` and `clientName` before re-posting.
- To correct a report already accepted, `updateReport` (`PUT /reports/{reportID}`) with the
  full object — there is no PATCH. `deleteReport` withdraws it.
- Use the `DATA_QUALITY` payload type (`OK`, `MISSING`, `ESTIMATED`, `BAD`) to flag a gap
  rather than silently omitting an interval.

## Errors

`403` means you are not holding `write_reports`, or you are a BL client trying to report.
`404` on `eventID` usually means the event was cancelled (`deleteEvent`) while you were
measuring. All errors use the `problem` envelope — see
`errors/openadr-alliance-problem-types.yml`.
