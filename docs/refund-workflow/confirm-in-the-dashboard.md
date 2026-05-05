---
title: Step 2 — Confirm in the dashboard
description: Verify the refund landed correctly. Operations-owned task.
type: task
topic_id: t_confirm-refund
audience: operations
level: foundational
sequence: 4
estimated_time: "~5 minutes"
status: published
last_reviewed: 2026-05-04
---

# Step 2 — Confirm in the dashboard

*Task · For operations · Foundational · Step 4 of 7 · ~5 minutes*

## Short description

Verify that a refund issued in [Step 1](create-refund.md) posted correctly, link it to the originating support ticket, and notify the customer. Operations owns this step.

## Prerequisites

- A user account with the **Operations** role on the [PayCart dashboard](https://dashboard.paycart.example).
- The refund ID (`rfd_…`) from the developer.
- The original payment ID (`pay_…`).
- The customer's support ticket open in your ticketing system.

--8<-- "includes/notices/dummy-data.md"

## Context

Use this task whenever a developer has issued a refund and the customer is awaiting confirmation, or when an ops associate is reviewing the day's refund queue. The dashboard is the canonical source of truth for refund state — preferred over reading API responses directly, because the dashboard surfaces both the refund and its linkage to the original payment in one view.

## Steps

1. **Open the dashboard refund list.** Sign in to the [PayCart dashboard](https://dashboard.paycart.example) → **Payments → Refunds**.

2. **Filter to the refund** by ID (e.g. `rfd_01HXYZ7890ABCDEF`), or by date range plus the customer's email. The refund row shows:

    | Field | What it tells you |
    | ----- | ----------------- |
    | **ID** | The refund's own ID. Click to open the detail panel. |
    | **Linked payment** | The original payment ID. Click to see the payment that's being refunded. |
    | **Amount** | The refund amount (negative, displayed in red). For a partial refund, less than the original payment. |
    | **Status** | One of: **Pending**, **Succeeded**, **Failed**, **Canceled**. See [the status reference](reference.md#refund-status-codes). |
    | **Created** | When the refund request was submitted. |
    | **Updated** | When the status last changed. |

3. **Confirm `Status = Succeeded`.** A refund in **Pending** has not finished — wait. In sandbox this resolves in seconds; in production allow up to two minutes before escalating. If **Failed**, follow [Troubleshoot — Refund failed in terminal state](troubleshoot.md#refund-failed-in-terminal-state) before contacting the customer.

4. **Click the linked payment ID** to verify the link is correct. The payment detail panel shows:

    - A **Refunds** subsection listing every refund issued against this payment.
    - A **Remaining refundable** amount (original captured amount minus all `Succeeded` refunds).
    - A status badge that flips from **Captured** to **Partially refunded** or **Refunded** depending on whether the cumulative refunds equal the captured amount.

5. **Reply to the customer's ticket** with the refund amount, the last four digits of their card, the refund ID (`rfd_…`), and the standard 3–10 business day messaging. Mark the ticket **Refund issued** and close it.

## Result

The customer ticket is closed with a refund-issued note containing the refund ID. The dashboard accurately reflects both the refund's terminal state and the payment's reduced refundable balance.

## Example

The customer's ticket asked for a $25 refund against a $100 charge. After step 4, the payment detail panel shows:

- **Refunds (1):** `rfd_01HXYZ7890ABCDEF` — $25.00 USD — Succeeded — 2026-04-12.
- **Remaining refundable:** $75.00 USD.
- **Status badge:** *Partially refunded.*

If the customer later asks for a second partial refund, **$75.00 USD** is the maximum that can be issued; submitting more triggers `400 amount_exceeds_remaining` (see [Troubleshoot](troubleshoot.md#amount-exceeds-remaining)).

## What to do next

At end of day, finance will [reconcile this refund against the ledger export](reconcile-ledger.md). You don't need to do anything for that — they pull the export themselves — but if you've issued an unusually large refund (>$5,000 USD), email finance directly so they aren't surprised by it in the morning report.

## Related links

- [Reference — Refund status codes](reference.md#refund-status-codes)
- [Troubleshoot — Refund stuck in pending](troubleshoot.md#refund-stuck-in-pending)
- [Step 3 — Reconcile the ledger](reconcile-ledger.md)
