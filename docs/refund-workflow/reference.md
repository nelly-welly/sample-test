---
title: Reference
description: Only the API fields, status codes, and webhook events used in this workflow.
type: reference
audience: developer
sequence: 7
status: published
last_reviewed: 2026-05-04
---

# Reference

*Reference · For developers · Step 7 of 7*

Only the surface area used in this workflow is listed here. For everything else, consult a real PayCart product (this is a portfolio sample).

## `POST /v1/refunds`

Issue a refund against a captured payment.

### Request

```http
POST /v1/refunds HTTP/1.1
Host: sandbox.paycart.example
Authorization: Bearer sk_test_…
Idempotency-Key: refund-req-9f2a1c
Content-Type: application/json

{
  "payment_id": "pay_01HABCDEF12345",
  "amount": 2500,
  "currency": "USD",
  "reason": "customer_request"
}
```

| Field | Type | Required | Notes |
| ----- | ---- | -------- | ----- |
| `payment_id` | string | yes | Must reference a payment in the `captured` state. |
| `amount` | integer | yes | Smallest currency unit (cents for USD). Must be ≤ `remaining_refundable_amount` on the payment. |
| `currency` | string | yes | ISO 4217. Must match the original payment's currency. |
| `reason` | string | no | One of: `customer_request`, `duplicate`, `fraudulent`, `other`. Used for analytics; does not affect processing. |

`Idempotency-Key` is **required** as a header on this endpoint. See [Before you start — Why idempotency matters here](before-you-start.md#why-idempotency-matters-here).

### Response

`201 Created` with the new refund resource. See [Refund object](#refund-object).

`400 amount_exceeds_remaining` if the amount exceeds `remaining_refundable_amount`. See [Troubleshoot](troubleshoot.md#amount-exceeds-remaining).

`409 idempotency_conflict` if the same `Idempotency-Key` was used for a different request body within the last 24 hours.

## Refund object

```json
{
  "id": "rfd_01HXYZ7890ABCDEF",
  "object": "refund",
  "payment_id": "pay_01HABCDEF12345",
  "amount": 2500,
  "currency": "USD",
  "status": "pending",
  "reason": "customer_request",
  "failure_code": null,
  "created_at": "2026-04-12T14:22:08Z",
  "updated_at": "2026-04-12T14:22:08Z"
}
```

`failure_code` is `null` until status becomes `failed`.

## Refund status codes

| Status | Meaning | Terminal? |
| ------ | ------- | --------- |
| `pending` | Submitted to the card network; awaiting confirmation. | No |
| `succeeded` | Funds have moved out of your settlement account toward the customer. | Yes |
| `failed` | The card network or PayCart rejected the refund. See `failure_code`. | Yes |
| `canceled` | A `pending` refund canceled before it became terminal. | Yes |

## Webhooks

Two events are emitted for every refund.

### `refund.succeeded`

Fires when a `pending` refund moves to `succeeded`. Use this as the trigger for marking the customer ticket resolved.

```json
{
  "id": "evt_01HZ123ABC456",
  "type": "refund.succeeded",
  "created_at": "2026-04-12T14:22:35Z",
  "data": {
    "id": "rfd_01HXYZ7890ABCDEF",
    "payment_id": "pay_01HABCDEF12345",
    "amount": 2500,
    "currency": "USD",
    "status": "succeeded"
  }
}
```

### `refund.failed`

Fires when a `pending` refund moves to `failed`. Use this as the trigger for [Troubleshoot — Refund failed in terminal state](troubleshoot.md#refund-failed-in-terminal-state).

```json
{
  "id": "evt_01HZ987XYZ654",
  "type": "refund.failed",
  "created_at": "2026-04-12T14:23:01Z",
  "data": {
    "id": "rfd_01HXYZ7890ABCDEF",
    "payment_id": "pay_01HABCDEF12345",
    "amount": 2500,
    "currency": "USD",
    "status": "failed",
    "failure_code": "card_declined"
  }
}
```

## What's not in this reference

- Authentication setup, rotating keys, organization-level scopes.
- Payment creation, capture, void.
- Disputes and chargeback handling.
- SDK installation and configuration.

A real product reference would cover them. They're omitted here on purpose — see [About this portfolio](../about.md).
