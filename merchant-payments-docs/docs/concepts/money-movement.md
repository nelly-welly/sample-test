---
title: Money movement
description: Who holds the funds at each stage of a PayCart payment, and how that shapes your accounting.
---

# Money movement

The [payment lifecycle](payment-lifecycle.md) describes what happens to a payment object. This page describes what happens to the *money*. The two are not the same thing — a captured payment is settled later, and settled funds are paid out later still. Most reconciliation surprises trace back to confusing one with another.

## Who holds the funds, when

| Stage | Funds are held by | Visible in your bank account? |
| --- | --- | --- |
| **Authorized** | The customer's issuing bank. PayCart has only earmarked them. | No |
| **Captured** | The customer's issuing bank, in transit to the card network. | No |
| **Settled** | PayCart's clearing account. | No (but visible in the dashboard's *Balance* view). |
| **Paid out** | Your merchant bank account. | Yes |

## The settlement window

Captured funds typically arrive in your PayCart balance one to two business days after capture, depending on the card scheme:

- **Visa, Mastercard** — T+1 business day.
- **American Express** — T+2 business days.
- **Discover, JCB** — T+2 to T+3 business days.

Funds in your balance then pay out on your configured schedule (daily by default). You can change the schedule in **Dashboard → Settings → Payouts**.

## What your accounting team needs to know

A payment shows up in three places, often on different days:

1. **The PayCart payment record** — created when the customer pays.
2. **The PayCart payout** — groups settled payments into a single bank transfer.
3. **Your bank statement line** — one line per payout.

To match a bank line back to individual payments, use the **Payouts report** (CSV export from the dashboard). Each row in the report has a `payout_id` that ties back to the corresponding bank line via the descriptor `PAYCART <payout_id>`.

The [Reconcile transactions](../guides/reconciliation.md) guide walks through this end-to-end with sample data.

## Fees

Fees are deducted from each captured payment before it lands in your balance. The fee shows as a separate line on the payment record, so the math always reads:

```
balance impact = amount captured − fee − refunds − disputes
```

--8<-- "includes/notices/dummy-data.md"

## Related reading

- [Payment lifecycle](payment-lifecycle.md)
- [Reconcile transactions](../guides/reconciliation.md)
- [Glossary](../reference/glossary.md)
