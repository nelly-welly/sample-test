---
title: Reconcile transactions
description: How to match PayCart payouts to your bank statement and your accounting system.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [reconciliation, finance, operations]
---

# Reconcile transactions

Reconciliation is the answer to the question "did the money I think I earned actually arrive in my bank account?" In PayCart, you reconcile against three things: your bank statement, your PayCart payouts, and your accounting system's order or invoice records.

If you're new to the way PayCart moves money, read [Money movement](../concepts/money-movement.md) first.

--8<-- "includes/notices/dummy-data.md"

## What you'll match

For any single business day:

```text
bank statement line   ←→   PayCart payout   ←→   payments inside the payout
```

Each level rolls up into the next:

- One **bank line** equals one **PayCart payout**.
- One **PayCart payout** contains many **payments**, **refunds**, **fees**, and possibly **disputes**.

## A daily reconciliation routine

This is the workflow most finance teams settle into:

1. **Open Dashboard → Payouts.** The most recent row is the payout that landed today.
2. **Compare totals.** The payout's net amount should match the bank line exactly. Bank descriptors look like `PAYCART po_sample_8aH2` — the suffix is the payout ID.
3. **Export the payout's reconciliation report.** Click the payout, then **Download CSV**.
4. **Import the CSV into your accounting system.** Match each row to the corresponding order, invoice, or refund record.
5. **Investigate anything that doesn't match.** See [common discrepancies](#common-discrepancies) below.

## What's in the reconciliation CSV

Each row is one *balance impact*:

| Column | Example | Meaning |
| --- | --- | --- |
| `id` | `pay_sample_3fE7` | The PayCart object id. |
| `type` | `payment` | One of: `payment`, `refund`, `dispute`, `fee`, `adjustment`. |
| `amount_gross` | `4200` | The amount before fees, in the smallest currency unit. |
| `amount_fee` | `-128` | The PayCart fee, always negative. |
| `amount_net` | `4072` | What landed in your balance for this row. |
| `currency` | `USD` | ISO 4217 currency code. |
| `metadata.order_id` | `7421` | Anything you attached to the original payment. |
| `payout_id` | `po_sample_8aH2` | Groups rows that landed in the same bank transfer. |

The sum of `amount_net` for a `payout_id` equals the bank line for that payout.

## Common discrepancies

| Symptom | Cause | What to do |
| --- | --- | --- |
| Bank line is less than payout total. | A reversed payout from an earlier day is netting against today's. | Check the *Payouts* tab for `status: failed`. The CSV shows the offsetting `adjustment` row. |
| Payment appears in two payouts. | A partial refund on a different day. | Group reconciliation by `payout_id`, not by payment date. |
| FX rate seems off by a fraction. | PayCart locks the FX rate at capture, not payout. | Both rates appear on the payment record; use the capture-time rate for revenue. |
| One payment is in the report but not in your accounting system. | Order created in your system after the payment ID was issued. | Backfill using `metadata.order_id`. |

## A worked example

Say you captured the following on 2026-04-28 (sandbox numbers, USD):

- 100 payments at $42.00 each (gross $4,200.00, fee $1.28 each).
- 2 refunds of $42.00 each (gross −$84.00, no fee).
- 1 disputed payment of $42.00 with a $15.00 dispute fee.

The payout that lands on 2026-04-30 looks like:

```text
gross payments     +$4,200.00
gross refunds       -$84.00
gross disputes      -$42.00
fees on payments    -$128.00
dispute fee         -$15.00
                  -----------
payout net         +$3,931.00
```

Your bank statement on 2026-04-30 should show one line of `$3,931.00` from PayCart with descriptor `PAYCART po_sample_…`.

## Automate it

If you process more than a few hundred payments a day, automate the steps above:

1. Subscribe to the `payout.paid` webhook.
2. On the event, fetch the payout's CSV via `GET /v1/payouts/{id}/reconciliation_report`.
3. Push rows into your accounting system using the `payout_id` as the unique key.

## Related reading

- [Money movement](../concepts/money-movement.md)
- [Process refunds](refunds.md)
- [Manage disputes](disputes.md)
