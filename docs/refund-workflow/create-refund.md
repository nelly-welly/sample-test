---
title: Step 1 — Create the refund
description: Issue a refund against a captured payment via the API. Developer-owned task.
type: task
topic_id: t_create-refund
audience: developer
level: intermediate
sequence: 3
estimated_time: "~1 minute on the wire"
status: published
last_reviewed: 2026-05-04
---

# Step 1 — Create the refund

*Task · For developers · Intermediate · Step 3 of 7 · ~1 minute on the wire*

## Short description

Issue a refund against a captured PayCart payment using the `POST /v1/refunds` endpoint. The endpoint returns immediately with a `pending` refund; the terminal state arrives by webhook within seconds (sandbox) or minutes (production).

## Prerequisites

- A captured payment ID. The sandbox provides `pay_01HABCDEF12345` pre-loaded with $100.00 USD.
- A sandbox API key (prefix `sk_test_`). See [Before you start — What you need](before-you-begin.md#what-you-need).
- A registered webhook endpoint subscribed to `refund.succeeded` and `refund.failed`.
- An idempotency-key generation strategy. See [Before you start — Why idempotency matters here](before-you-begin.md#why-idempotency-matters-here).

--8<-- "includes/notices/sandbox-only.md"

## Context

Use this task when a customer requests a full or partial refund on a captured payment and your service initiates the refund programmatically. The same effect can be achieved manually from the dashboard, but this workflow assumes API issuance — for example, a refund triggered automatically from a cancellation flow in your application.

## Steps

1. **Send a `POST /v1/refunds` request** with `payment_id`, `amount` (smallest currency unit), `currency`, an optional `reason`, and an `Idempotency-Key` header.

    === "cURL"

        --8<-- "includes/snippets/curl-create-refund.md"

    === "Node.js"

        --8<-- "includes/snippets/node-create-refund.md"

    === "Python"

        --8<-- "includes/snippets/python-create-refund.md"

2. **Verify the response is `201 Created`** with `status: "pending"` and a refund ID prefixed `rfd_`.

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

    Persist the `id`. Both operations and finance use it downstream.

3. **Listen for the terminal-state webhook** — `refund.succeeded` or `refund.failed`. Do not poll `GET /v1/refunds/{id}` in a loop.

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

## Result

When `refund.succeeded` arrives, the refund is committed. The customer will see the credit on their statement in 3–10 business days depending on their issuer.

If `refund.failed` arrives instead, your task is not complete — see [Troubleshoot — Refund failed in terminal state](troubleshoot.md#refund-failed-in-terminal-state) for the four failure codes and their remedies.

## Example

A partial refund of $25 against a $100 captured payment:

- Request `amount`: `2500` (cents) against `pay_01HABCDEF12345`.
- Response: `status: "pending"`, refund ID `rfd_01HXYZ7890ABCDEF`.
- Webhook within ~1 second (sandbox): `refund.succeeded`.
- Resulting payment state: `remaining_refundable_amount` drops from `10000` to `7500`. The payment status flips from **Captured** to **Partially refunded**.

## What to do next

Hand off to the operations team to [confirm the refund landed in the dashboard](confirm-in-the-dashboard.md). Tell them:

- The refund ID (`rfd_01HXYZ7890ABCDEF`).
- The original payment ID (`pay_01HABCDEF12345`).
- The customer's name or email so they can match against support tickets.

## Related links

- [Reference — `POST /v1/refunds`](reference.md#post-v1refunds)
- [Reference — Refund status codes](reference.md#refund-status-codes)
- [Reference — `refund.succeeded` and `refund.failed` webhooks](reference.md#webhooks)
- [Troubleshoot — Refund stuck in pending](troubleshoot.md#refund-stuck-in-pending)
