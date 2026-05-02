---
title: Transaction states
description: The full state machine for PayCart payment objects.
type: concept
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [payments, states, concepts]
---

# Transaction states

Every PayCart payment object exposes a `status` field. Your application logic, your dashboards, and your reconciliation reports all key off this field, so the meaning of each value matters.

This page enumerates each state, the events that move a payment between states, and what your application is allowed to do in each.

## State summary

| Status | Meaning | Terminal? |
| --- | --- | --- |
| `requires_payment_method` | The payment exists but has not been associated with a card or wallet yet. | No |
| `requires_action` | The customer must complete a step (typically 3-D Secure). | No |
| `processing` | PayCart is talking to the card network. | No |
| `authorized` | The bank has approved the charge; funds are held. | No |
| `succeeded` | Charge captured. The customer's statement will show the amount. | Yes |
| `canceled` | An authorization was voided before capture. | Yes |
| `failed` | The payment was declined or errored. | Yes |
| `refunded` | A captured payment was returned to the customer in full. | Yes |
| `partially_refunded` | A captured payment was returned in part. | No |
| `disputed` | The customer has opened a chargeback with their bank. | No |

## Allowed transitions

```text
requires_payment_method ──► requires_action ──► processing ──► authorized ──► succeeded
                                                       │            │              │
                                                       └──► failed  └──► canceled  ├──► partially_refunded ──► refunded
                                                                                   └──► disputed
```

A payment can move *out* of `disputed` back to `succeeded` (you won the dispute) or to `refunded` (you lost or accepted it). All other terminal states are final.

## What you can do in each state

| Status | `capture` | `cancel` | `refund` | `dispute response` |
| --- | --- | --- | --- | --- |
| `authorized` | Yes | Yes | No | No |
| `succeeded` | No | No | Yes | No |
| `partially_refunded` | No | No | Yes (up to remainder) | No |
| `refunded` | No | No | No | No |
| `disputed` | No | No | Yes (counts as accepting the dispute) | Yes |

Calls that are not allowed in the current state return `409 Conflict` with error code `state_invalid`. See [Status codes](../reference/status-codes.md).

## Webhook events to watch

If you'd rather react to state changes than poll, subscribe to these events:

- `payment.authorized`
- `payment.succeeded`
- `payment.failed`
- `payment.refunded`
- `payment.disputed`

For setup, see [Webhooks](../integration/webhooks.md).

--8<-- "includes/notices/idempotency.md"

## Related reading

- [Payment lifecycle](payment-lifecycle.md) — the conceptual story behind these states.
- [Glossary](../reference/glossary.md) — single-sentence definitions of each term used here.
