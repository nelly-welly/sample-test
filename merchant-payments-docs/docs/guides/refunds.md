---
title: Process refunds
description: How to refund a captured payment in PayCart, including partial refunds.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [refunds, payments, operations]
---

# Process refunds

A refund returns captured funds to the customer. Any payment in `succeeded` or `partially_refunded` state can be refunded up to the remaining amount.

If you need to cancel a payment *before* it's been captured, you want a [void](../concepts/transaction-states.md), not a refund.

--8<-- "includes/notices/dummy-data.md"

## Refund from the dashboard

This is the right path for one-off support cases.

1. **Dashboard → Payments**, find the payment, open the side panel.
2. Click **Refund** in the top right.
3. Choose **Full** or **Partial**. For a partial refund, enter the amount.
4. Pick a reason — this is for your records and shows up in reports.
5. Click **Refund**.

The customer's bank typically posts the refund within five to ten business days. The PayCart side completes immediately.

## Refund through the API

For programmatic refunds, send a `POST` to `/v1/refunds`.

--8<-- "includes/snippets/curl-refund.md"

A successful response:

```json
{
  "id": "ref_sample_9bC4",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "payment_id": "pay_sample_3fE7",
  "reason": "customer_requested",
  "created_at": "2026-04-29T15:10:42Z"
}
```

--8<-- "includes/notices/idempotency.md"

## Partial refunds

Pass an `amount` lower than the original payment. You can issue multiple partial refunds on the same payment until the total matches the original captured amount.

```bash
curl -X POST https://sandbox.paycart.example/v1/refunds \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN" \
  -H "Idempotency-Key: refund-7421-shipping-only" \
  -H "Content-Type: application/json" \
  -d '{
    "payment_id": "pay_sample_3fE7",
    "amount": 800,
    "reason": "shipping_refund"
  }'
```

After a partial refund, the payment's `status` becomes `partially_refunded`. After the cumulative refunds equal the captured amount, it becomes `refunded`.

## Refund reasons

Use the reason field consistently. Reports group by reason, so trends are only useful when teams pick from a known list.

| `reason` | Use it for |
| --- | --- |
| `customer_requested` | Default. Customer asked for the refund without a more specific cause. |
| `duplicate_charge` | The customer was charged twice for the same order. |
| `fraudulent` | Refunding because the charge wasn't authorized. |
| `defective_product` | Customer received a defective item. |
| `shipping_refund` | Shipping cost only; the goods stay sold. |
| `goodwill` | Customer-service gesture not tied to a specific defect. |

## Common errors

| Error | Meaning | Fix |
| --- | --- | --- |
| `state_invalid` | The payment isn't refundable in its current state. | Check `payment.status`. |
| `refund_window_exceeded` | Original payment is older than 180 days. | Send funds out-of-band. |
| `insufficient_funds` | Your PayCart balance can't cover the refund. | Top up via **Settings → Balance**. |

For the full list, see [Status codes](../reference/status-codes.md).

## Related reading

- [Manage disputes](disputes.md) — when to refund instead of fight a chargeback.
- [Reconcile transactions](reconciliation.md) — how refunds appear on your payouts.
- [Transaction states](../concepts/transaction-states.md)
