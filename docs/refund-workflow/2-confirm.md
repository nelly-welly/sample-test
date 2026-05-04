---
title: Step 2 — Confirm in the dashboard
description: Verify the refund landed correctly. Operations-owned step.
type: procedure
audience: operations
level: foundational
sequence: 4
estimated_time: "~5 minutes"
status: published
last_reviewed: 2026-05-04
---

# Step 2 — Confirm in the dashboard

*Procedure · For operations · Foundational · Step 4 of 7 · ~5 minutes*

**You are:** an operations associate handling a customer-facing refund request.
**Goal:** Confirm the refund posted, link it to the support ticket, and notify the customer.
**Time:** ~5 minutes per refund.

--8<-- "includes/notices/dummy-data.md"

## Find the refund

1. Sign in to the [PayCart dashboard](https://dashboard.paycart.example).
2. In the left nav, click **Payments → Refunds**.
3. Filter by the refund ID your developer gave you (e.g. `rfd_01HXYZ7890ABCDEF`), or by date range and the customer's email.

The refund row shows:

| Column | What it tells you |
| ------ | ----------------- |
| **ID** | The refund's own ID. Click to open the detail panel. |
| **Linked payment** | The original payment ID. Click to see the payment that's being refunded. |
| **Amount** | The refund amount (negative, displayed in red). For a partial refund, this is less than the original payment. |
| **Status** | One of: **Pending**, **Succeeded**, **Failed**, **Canceled**. See [the status reference](reference.md#refund-status-codes). |
| **Created** | When the refund request was submitted. |
| **Updated** | When the status last changed. |

## Confirm the status is "Succeeded"

A refund in **Pending** state has not finished yet — wait. In sandbox this resolves in seconds; in production allow up to two minutes before escalating.

If the status is **Failed**, read the failure reason in the detail panel and follow [Troubleshoot — Refund failed in terminal state](troubleshoot.md#refund-failed-in-terminal-state) before notifying the customer.

## Verify the link to the original payment

Click the linked payment ID. The payment detail panel shows:

- A **Refunds** subsection listing every refund issued against this payment.
- A **Remaining refundable** amount (original captured amount minus all `Succeeded` refunds).
- A status badge that flips from **Captured** to **Partially refunded** or **Refunded** depending on whether the cumulative refunds equal the captured amount.

If the customer is asking for an additional partial refund, the **Remaining refundable** number is the maximum you can issue. If you submit more than that, the API rejects the request with `400 amount_exceeds_remaining` (see [Troubleshoot](troubleshoot.md#amount-exceeds-remaining)).

## Notify the customer

Once status is **Succeeded**:

1. Reply to the customer's ticket with the refund amount, the last four digits of their card, and the standard 3–10 business day messaging.
2. Attach the refund ID (`rfd_…`) for their records.
3. Mark the ticket **Refund issued** and close it.

## Hand off

At end of day, finance will [reconcile this refund against the ledger export](3-reconcile.md). You don't need to do anything for that — they pull the export themselves — but if you've issued an unusually large refund (>$5,000 USD), email finance directly so they aren't surprised by it in the morning report.

## Related

- [Reference — Refund status codes](reference.md#refund-status-codes)
- [Troubleshoot — Refund stuck in pending](troubleshoot.md#refund-stuck-in-pending)
