---
title: Webhooks
description: Receive PayCart events at a URL you control.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [integration, webhooks, events]
---

# Webhooks

Webhooks are how PayCart tells your application that something has happened — a payment succeeded, a dispute was opened, a payout was sent. They are the single most important piece of integration work after authentication, because polling alternatives are slow and brittle.

--8<-- "includes/notices/sandbox-only.md"

## How webhooks work

1. You register one or more HTTPS endpoints in **Dashboard → Developers → Webhooks**.
2. PayCart sends a `POST` to your endpoint each time a subscribed event occurs.
3. Your endpoint replies with a `2xx` status within ten seconds.
4. If your endpoint replies with a non-`2xx` status, times out, or is unreachable, PayCart retries with exponential backoff for 72 hours.

## Subscribe to events

In the dashboard, choose which events your endpoint should receive. We recommend starting small:

- `payment.succeeded`
- `payment.failed`
- `payment.refunded`
- `dispute.created`
- `payout.paid`

You can add more later from the same screen without redeploying.

## What a webhook payload looks like

Every webhook body has the same envelope:

```json
{
  "id": "evt_sample_8aH2",
  "type": "payment.succeeded",
  "created_at": "2026-04-29T14:02:11Z",
  "livemode": false,
  "data": {
    "object": {
      "id": "pay_sample_3fE7",
      "status": "succeeded",
      "amount": 4200,
      "currency": "USD",
      "metadata": { "order_id": "7421" }
    }
  }
}
```

The shape of `data.object` matches the shape of the corresponding object elsewhere in PayCart — a `payment.*` event holds a payment, a `dispute.*` event holds a dispute, and so on.

## Verify the signature

Every webhook PayCart sends carries a `PayCart-Signature` header. Verify it before trusting the body. Pseudocode:

```text
expected = HMAC-SHA256(webhook_secret, timestamp + "." + raw_body)
reject if expected != signature_header.v1
reject if abs(now − timestamp) > 5 minutes
```

A real implementation in your language of choice would replace the pseudocode with the equivalent crypto primitives. Treat the webhook secret like any other credential — store it in your secret manager, not in code.

## Make your handler safe to retry

PayCart retries any non-`2xx` response, so your handler must be idempotent.

The simplest way: store the event `id` in your database with a `UNIQUE` constraint and skip the handler body if you've seen it before.

--8<-- "includes/notices/idempotency.md"

## Test in the sandbox

Two ways to send a test webhook:

=== "From the dashboard"

    **Developers → Webhooks → Send test event**. Choose any event type and PayCart will deliver a fully formed sample payload to the endpoint you select.

=== "By triggering a real event"

    Create a payment in the sandbox using a [test card](../reference/test-data.md). The matching `payment.*` events fire to all subscribed endpoints exactly as they would in production.

## Common pitfalls

- **Returning `200` after enqueuing.** Good. PayCart only needs to know you've received the event, not that you've finished processing it.
- **Returning `200` only after processing.** Risky if processing takes more than a few seconds. Your endpoint will time out and trigger duplicates.
- **Trusting the `data.object` blindly.** Always re-fetch the underlying object via the API if you're about to move money. Webhooks can arrive out of order.
- **Forgetting the signature check in non-production.** It's tempting to skip it in dev. Don't — bugs in signature verification only show up under load, which is exactly when you don't want to discover them.

## Related reading

- [Authentication](authentication.md)
- [Transaction states](../concepts/transaction-states.md) — the lifecycle these events describe.
- [Troubleshooting](../troubleshooting/common-issues.md) — what to do when webhooks aren't firing.
