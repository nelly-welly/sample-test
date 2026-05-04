---
title: Troubleshoot
description: Decision tree for the failure patterns that account for ~95% of refund issues.
type: troubleshooting
audience: all
level: advanced
sequence: 6
status: published
last_reviewed: 2026-05-04
---

# Troubleshoot

*Troubleshooting · For all readers · Advanced · Step 6 of 7*

If something looks wrong, jump to the symptom that matches.

--8<-- "includes/notices/idempotency.md"

## Symptoms

- [Refund stuck in pending](#refund-stuck-in-pending)
- [Refund failed in terminal state](#refund-failed-in-terminal-state)
- [Amount exceeds remaining](#amount-exceeds-remaining)
- [Duplicate refund created](#duplicate-refund-created)
- [Refund missing from export](#refund-missing-from-export)

## Refund stuck in pending

The refund returned `201 Created` with `status = "pending"` more than 5 minutes ago and you haven't received a webhook.

1. **Check sandbox vs. production.** Sandbox always resolves in <5 seconds. If you're in sandbox and it's been longer, your webhook receiver is probably the problem — see step 3.
2. **Check the dashboard.** **Payments → Refunds** → find the row. If the dashboard shows **Succeeded** but you have no webhook, your webhook subscription is misconfigured.
3. **Check your webhook delivery log.** Dashboard → **Developers → Webhooks** → the endpoint → **Recent deliveries**. Look for retries with non-2xx responses. Fix your endpoint, then click **Resend** on the failed event.
4. **If still pending after 30 minutes in production**, file a support ticket with the refund ID. Do **not** retry the request — the original is still in flight, and a retry with a new idempotency key would create a duplicate.

## Refund failed in terminal state

The refund returned a `refund.failed` webhook or shows **Failed** in the dashboard.

The `failure_code` in the webhook payload tells you why:

| `failure_code` | Meaning | What to do |
| --------------- | ------- | ---------- |
| `card_declined` | The issuing bank rejected the credit attempt. Usually a closed card. | Contact the customer. The refund must go to an alternative method (ACH, store credit, manual). |
| `account_closed` | The merchant settlement account is closed or frozen. | Escalate to the account admin. No customer-facing action until resolved. |
| `network_error` | A transient network failure between PayCart and the card network. | Reissue the refund with a **new** idempotency key. The original is terminal; it will not retry itself. |
| `processor_error` | An error inside PayCart's processor. Rare. | File a support ticket. Do not retry without guidance. |

## Amount exceeds remaining

The API rejected the request with `400 amount_exceeds_remaining`.

The captured payment has less refundable balance than the amount you requested. This usually means a previous partial refund has already been issued.

1. Get the **remaining refundable** value from `GET /v1/payments/{payment_id}` (field: `remaining_refundable_amount`) or from the dashboard.
2. Reissue the refund with `amount` ≤ that value.
3. If the customer is owed more than `remaining_refundable_amount` and the original payment is fully refunded, the additional credit must be issued outside PayCart (manual ACH, store credit).

## Duplicate refund created

Two refund rows exist for what should have been one refund.

This happens when a retry was sent without reusing the original `Idempotency-Key`.

1. **Determine which refund is the canonical one** — usually the earlier `created_at`.
2. **Cancel the duplicate**: `POST /v1/refunds/{id}/cancel`. Only refunds in `pending` state can be canceled. If both are `succeeded`, you cannot cancel — you must issue a new **payment** to the customer for the duplicate amount.
3. **Fix the retry path** so future retries reuse the original `Idempotency-Key`. See [Before you start — Why idempotency matters here](before-you-start.md#why-idempotency-matters-here).

## Refund missing from export

A refund shows **Succeeded** in the dashboard but is absent from the daily ledger export.

1. **Check the timestamp.** The export covers events with `occurred_at` between 00:00 and 23:59:59 UTC of the export date. A refund that succeeded at 23:59:55 UTC may appear in either day's export depending on processing delay; allow one day before escalating.
2. **Check the export version.** If you downloaded before 06:00 UTC, you got a partial export. Re-download after 06:00 UTC.
3. **Still missing after 24 hours?** File a support ticket with the refund ID and the export date. This is a known but rare reconciliation gap; PayCart will issue a corrected export.
