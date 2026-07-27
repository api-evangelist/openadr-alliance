---
name: Subscribe to VTN notifications over webhook or MQTT
description: Register for object-operation notifications from a VTN — the mandatory webhook binding, or the optional MQTT binding added in 3.1.0 with runtime topic discovery.
api: openapi/openadr-3-1-1-openapi.yaml
generated: '2026-07-27'
method: generated
operations:
  - fetchToken
  - listAllNotifiers
  - createSubscription
  - searchSubscriptions
  - searchSubscriptionByID
  - updateSubscription
  - deleteSubscription
  - listAllMqttNotifierTopicsEvents
  - listAllMqttNotifierTopicsPrograms
  - listAllMqttNotifierTopicsProgramEvents
  - listAllMqttNotifierTopicsReports
  - listAllMqttNotifierTopicsSubscriptions
  - listAllMqttNotifierTopicsVens
  - listAllMqttNotifierTopicsVen
  - listAllMqttNotifierTopicsResources
  - listAllMqttNotifierTopicsVenEvents
  - listAllMqttNotifierTopicsVenPrograms
  - listAllMqttNotifierTopicsVenResources
---

# Subscribe to VTN notifications

Polling `searchAllEvents` works but wastes both sides. OpenADR 3 has two push bindings:
**WEBHOOK**, which every VTN must support, and **MQTT**, optional and added in 3.1.0.

## 1. Authenticate

`fetchToken` with `write_subscriptions` plus a read scope. For MQTT topic metadata a BL
client also needs `read_bl`.

## 2. Ask what the VTN supports

`listAllNotifiers` (`GET /notifiers`) returns a `notifiersResponse`:

- `WEBHOOK` — a boolean, documented as "currently MUST be true".
- `MQTT` — present only if supported. Carries `URIS` (broker connection URIs),
  `serialization` (currently always `JSON`), and `authentication`, which is a `oneOf` over
  anonymous, OAuth2 bearer token, and client certificate.

This operation does not exist before 3.1.0. On a 3.0.x VTN, assume webhook only.

## 3a. Webhook binding

`createSubscription` (`POST /subscriptions`) takes a `subscriptionRequest`:

- `clientName` — required.
- `programID` — optional since 3.1.0, so a VEN can subscribe to its own `ven` object without
  naming a program.
- `objectOperations[]` — required. Each entry has:
  - `objects[]` from `PROGRAM`, `EVENT`, `REPORT`, `SUBSCRIPTION`, `VEN`, `RESOURCE`
  - `operations[]` from `CREATE`, `READ`, `UPDATE`, `DELETE` (CRUD names since 3.1.0 — they
    were HTTP verbs before)
  - `callbackUrl` — **per entry**, so one subscription can fan different object/operation
    combinations to different endpoints
  - `bearerToken` — optional, subscriber-supplied. The VTN presents it back to you so you
    can authenticate its calls without a custom integration.
- `targets[]` — server-side notification filter. Available on the webhook binding only.

The VTN then POSTs a `notification` (`objectType`, `operation`, `object`, `targets`) as
`application/json` to your `callbackUrl`. Return `200` to accept.

**Harden your receiver yourself.** The `notifyEvent` callback is declared with `security: [{}]`
in the specification, so beyond the `bearerToken` there is no signature, no timestamp and no
replay defence. The spec also says nothing about retries, ordering or deduplication —
treat notifications as at-most-once hints and reconcile with a `search*` call.

## 3b. MQTT binding

Do **not** hardcode topic names. They are VTN-assigned and may be scoped per VEN — that is
how object privacy is enforced on a binding that cannot filter by target.

1. Get broker URIs and auth from `listAllNotifiers`.
2. Get topic names from the matching discovery operation:
   - all events: `listAllMqttNotifierTopicsEvents`
   - events of one program: `listAllMqttNotifierTopicsProgramEvents`
   - events for one VEN: `listAllMqttNotifierTopicsVenEvents`
   - programs: `listAllMqttNotifierTopicsPrograms` / `listAllMqttNotifierTopicsProgram`
   - reports: `listAllMqttNotifierTopicsReports`
   - subscriptions: `listAllMqttNotifierTopicsSubscriptions`
   - vens: `listAllMqttNotifierTopicsVens` / `listAllMqttNotifierTopicsVen`
   - resources: `listAllMqttNotifierTopicsResources` / `listAllMqttNotifierTopicsVenResources`
3. Each returns a `notifierTopicsResponse` whose `topics` object holds a topic name per
   operation: `UPDATE` and `DELETE` are required, `CREATE` is absent for
   specific-object endpoints (you cannot subscribe to the creation of an object that does
   not exist yet), and `ALL` is optional — a VTN provides it only when its topic design
   supports a wildcard.
4. Connect to the broker and subscribe. Payloads are the same `notification` object as the
   webhook binding, JSON-serialized.

## 4. Maintain

`searchSubscriptions` filters on `programID`, `clientName`, `objects`, `skip`, `limit`.
`updateSubscription` is a full-object PUT; `deleteSubscription` removes it. Note that
`targets` was removed from `/subscriptions` *queries* in 3.1.0 — it remains a field on the
object.

## See also

- `asyncapi/openadr-alliance-notifications-asyncapi.yml` — the event surface as AsyncAPI 3.0.0
- `asyncapi/openadr-alliance-webhooks.yml` — the webhook catalog
