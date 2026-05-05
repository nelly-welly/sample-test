---
title: Step 3 — Reconcile the ledger
description: Match refunds against the daily ledger export. Finance-owned task.
type: task
topic_id: t_reconcile-refund
audience: finance
level: intermediate
sequence: 5
estimated_time: "~15 minutes (T+1)"
status: published
last_reviewed: 2026-05-04
---

# Step 3 — Reconcile the ledger

*Task · For finance · Intermediate · Step 5 of 7 · ~15 minutes (T+1)*

## Short description

Match each `refund.succeeded` row in the daily PayCart ledger export against a debit entry in your general ledger. Confirm sign, amount, currency, and reference alignment, and flag discrepancies. Finance owns this task.

## Prerequisites

- A user account with the **Finance** role and access to the daily ledger export.
- The general ledger for the same calendar day, pulled from your finance system.
- The current calendar day's UTC clock past 06:00 (the export becomes complete at that point — see Context).

--8<-- "includes/notices/dummy-data.md"

## Context

Use this task once per business day, after 06:00 UTC, to reconcile the previous calendar day's refund activity. The ledger export covers the previous **calendar day in UTC** and is published on T+1 — i.e., today's reconciliation reconciles yesterday's events. Earlier than 06:00 UTC the export is partial; reconciling against a partial export is a known false-positive source.

## Steps

1. **Pull yesterday's ledger export** from the [PayCart dashboard](https://dashboard.paycart.example) → **Reports → Ledger exports** → click the row for yesterday's date → download as **CSV** (recommended) or **JSON Lines**.

2. **Filter the export to refund rows** where `event_type = refund.succeeded`. The signed `amount` is **negative** (money leaving your settlement account).

    ```csv
    event_id,event_type,amount,currency,reference_id,linked_payment_id,occurred_at
    evt_01HZ123,refund.succeeded,-2500,USD,rfd_01HXYZ7890ABCDEF,pay_01HABCDEF12345,2026-04-12T14:22:35Z
    ```

3. **Verify the five reconciliation fields** for each refund row:

    | Field | What to check |
    | ----- | ------------- |
    | `event_type` | Must be `refund.succeeded`. `refund.failed` rows have `amount = 0` and should not affect your ledger. |
    | `amount` | Negative integer in the smallest currency unit. Sum across all refunds = total refund debit for the day. |
    | `currency` | Must match the original payment's currency. Cross-currency refunds are not supported. |
    | `reference_id` | The refund ID. Use this to match against the operations team's ticketing system. |
    | `linked_payment_id` | The original payment ID. Use this to confirm the refund is reducing the right captured amount. |

4. **Match each refund row to the general ledger.** For each row:

    1. Locate the corresponding **debit** entry in your general ledger keyed by `reference_id`.
    2. Confirm the absolute amount and currency match.
    3. Confirm the linked payment exists in your ledger as a prior **credit** entry (the original capture).

5. **Mark reconciled rows** in your finance system. If a row matches all three checks, mark it complete.

## Result

Every `refund.succeeded` row from yesterday's export is matched against a corresponding debit entry in your general ledger. Today's reconciliation is signed off and the export is archived.

## Example

A reconciliation pass on 2026-04-13 covering activity from 2026-04-12:

- Export contains 14 `refund.succeeded` rows summing to $1,847.50 USD debit.
- 13 of 14 match cleanly to GL entries within 5 minutes.
- 1 row (the customer's $25 refund from [Step 2's example](confirm-in-the-dashboard.md#example)) fails to match — it's missing from the GL because the developer's webhook handler timed out yesterday and never logged the entry. File a ticket with the developer team using the export row + `reference_id`. Reconciliation is signed off as **complete with one open ticket**.

## What to do next

Three discrepancy patterns to escalate when steps 4–5 fail. Use the table below as a routing rubric.

| Pattern | Likely cause | What to do |
| ------- | ------------ | ---------- |
| Refund in export, missing from ledger | Webhook never reached your backend | File a ticket. Send the export row and the `reference_id` to the developer team. |
| Refund in ledger, missing from export | Refund still in `pending` at export cutoff | Wait one day; it should appear in tomorrow's export. If not, escalate. |
| Amounts off by exact FX delta | A multi-currency edge case the workflow doesn't cover | Escalate to your account manager — do not auto-correct. |

For the third specifically, see [Troubleshoot — Refund missing from export](troubleshoot.md#refund-missing-from-export).

## Related links

- [Reference — Webhook event payloads](reference.md#webhooks)
- [Troubleshoot — Refund missing from export](troubleshoot.md#refund-missing-from-export)
- [Step 2 — Confirm in the dashboard](confirm-in-the-dashboard.md)
