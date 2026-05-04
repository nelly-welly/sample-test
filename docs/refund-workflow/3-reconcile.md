---
title: Step 3 — Reconcile the ledger
description: Match refunds against the daily ledger export. Finance-owned step.
type: procedure
audience: finance
level: intermediate
sequence: 5
estimated_time: "~15 minutes (T+1)"
status: published
last_reviewed: 2026-05-04
---

# Step 3 — Reconcile the ledger

*Procedure · For finance · Intermediate · Step 5 of 7 · ~15 minutes (T+1)*

**You are:** a finance analyst running the daily reconciliation between PayCart and your general ledger.
**Goal:** Confirm every refund processed yesterday is reflected in the ledger export with the right sign, amount, and reference, and flag any discrepancies.
**Time:** ~15 minutes for a typical day's volume.

--8<-- "includes/notices/dummy-data.md"

## Pull the daily export

The ledger export covers the previous calendar day in UTC and is available **after 06:00 UTC the next day** (T+1). Earlier exports are partial.

1. Sign in to the [PayCart dashboard](https://dashboard.paycart.example).
2. **Reports → Ledger exports** → click the row for yesterday's date.
3. Download as **CSV** (recommended) or **JSON Lines**.

Each row is one financial event: a payment capture, a refund, a fee, or an adjustment.

## Find refunds in the export

Refunds appear as rows where `event_type = refund.succeeded`. The signed `amount` is **negative** (money leaving your settlement account).

```csv
event_id,event_type,amount,currency,reference_id,linked_payment_id,occurred_at
evt_01HZ123,refund.succeeded,-2500,USD,rfd_01HXYZ7890ABCDEF,pay_01HABCDEF12345,2026-04-12T14:22:35Z
```

Five fields matter for reconciliation:

| Field | What to check |
| ----- | ------------- |
| `event_type` | Must be `refund.succeeded`. `refund.failed` rows have `amount = 0` and should not affect your ledger. |
| `amount` | Negative integer in the smallest currency unit. Sum across all refunds = total refund debit for the day. |
| `currency` | Must match the original payment's currency. Cross-currency refunds are not supported. |
| `reference_id` | The refund ID. Use this to match against the operations team's ticketing system. |
| `linked_payment_id` | The original payment ID. Use this to confirm the refund is reducing the right captured amount. |

## Match against the ledger

For each refund row in the export:

1. Locate the corresponding **debit** entry in your general ledger keyed by `reference_id`.
2. Confirm the absolute amount and currency match.
3. Confirm the linked payment exists in your ledger as a prior **credit** entry (the original capture).

If all three match, the refund is reconciled. Mark the row.

## When numbers don't match

Three discrepancy patterns recur:

| Pattern | Likely cause | What to do |
| ------- | ------------ | ---------- |
| Refund in export, missing from ledger | Webhook never reached your backend | File a ticket. Send the export row and the `reference_id` to the developer team. |
| Refund in ledger, missing from export | Refund still in `pending` at export cutoff | Wait one day; it should appear in tomorrow's export. If not, escalate. |
| Amounts off by exact FX delta | A multi-currency edge case the workflow doesn't cover | Escalate to your account manager — do not auto-correct. |

## Sign off

When every `refund.succeeded` row reconciles, mark the day's reconciliation **complete** in your finance system and archive the export.

## Related

- [Reference — Webhook event payloads](reference.md#webhooks)
- [Troubleshoot — Refund missing from export](troubleshoot.md#refund-missing-from-export)
