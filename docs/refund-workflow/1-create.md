---
title: Step 1 — Create the refund
description: Issue a refund against a captured payment via the API. Developer-owned step.
type: procedure
audience: developer
level: intermediate
sequence: 3
estimated_time: "~1 minute on the wire"
status: published
last_reviewed: 2026-05-04
---

# Step 1 — Create the refund

*Procedure · For developers · Intermediate · Step 3 of 7 · ~1 minute on the wire*

**You are:** the developer integrating PayCart into your application.
**Goal:** Issue a refund against a captured payment and return success or a typed error to your caller.
**Time:** ~1 minute on the wire; ~30 minutes to wire it into your service if this is the first time.

--8<-- "includes/notices/sandbox-only.md"

## Make the request

Send a `POST` to `/v1/refunds` with the `payment_id`, `amount` (in the smallest currency unit — cents for USD), and an `Idempotency-Key`. `reason` is optional but recommended for audit.

=== "cURL"

    --8<-- "includes/snippets/curl-create-refund.md"

=== "Node.js"

    --8<-- "includes/snippets/node-create-refund.md"

=== "Python"

    --8<-- "includes/snippets/python-create-refund.md"

## What you get back

A successful request returns `201 Created` with the new refund resource:

```json
{
  "id": "rfd_01HXYZ7890ABCDEF",
  "object": "refund",
  "payment_id": "pay_01HABCDEF12345",
  "amount": 2500,
  "currency": "USD",
  "status": "pending",
  "reason": "customer_request",
  "created_at": "2026-04-12T14:22:08Z"
}
```

Two things to notice:

1. **`status` is `pending`, not `succeeded`.** Money has not moved yet. PayCart submits the refund to the card network and the final state arrives via webhook (usually within 30–90 seconds in production; instant in sandbox).
2. **The refund has its own ID** (`rfd_…`), separate from the payment ID. Persist it. Operations and finance will both use this ID to find the refund downstream.

## Wait for the terminal state

Listen for the `refund.succeeded` or `refund.failed` webhook. Do **not** poll `GET /v1/refunds/{id}` in a loop — webhooks are the supported pattern.

```json
{
  "type": "refund.succeeded",
  "data": {
    "id": "rfd_01HXYZ7890ABCDEF",
    "status": "succeeded",
    "amount": 2500
  }
}
```

When `refund.succeeded` arrives, your work is done. The refund is committed; the customer will see the credit on their statement in 3–10 business days depending on their issuer.

If `refund.failed` arrives, see [Troubleshoot — Refund failed in terminal state](troubleshoot.md#refund-failed-in-terminal-state) for what each failure code means and what to do.

## Hand off

The operations team will next [confirm the refund landed in the dashboard](2-confirm.md). Tell them:

- The refund ID (`rfd_01HXYZ7890ABCDEF`).
- The original payment ID (`pay_01HABCDEF12345`).
- The customer's name or email so they can match against support tickets.

## Related

- [Reference — `POST /v1/refunds`](reference.md#post-v1refunds)
- [Reference — Refund status codes](reference.md#refund-status-codes)
- [Reference — `refund.succeeded` and `refund.failed` webhooks](reference.md#webhooks)
