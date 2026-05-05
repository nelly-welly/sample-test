---
title: Troubleshoot
description: Decision tree for the failure patterns that account for ~95% of refund issues.
type: troubleshooting
topic_id: ts_refund-failures
audience: all
level: advanced
sequence: 6
status: published
last_reviewed: 2026-05-04
---

# Troubleshoot

*Troubleshooting · For all readers · Advanced · Step 6 of 7*

## Short description

A symptom-driven decision tree for the five failure patterns that account for roughly 95 % of refund issues. Each symptom is structured as **Condition → Cause → Remedy**, so the reader can stop reading the moment they have what they need.

--8<-- "includes/notices/idempotency.md"

## Symptoms

- [Refund stuck in pending](#refund-stuck-in-pending)
- [Refund failed in terminal state](#refund-failed-in-terminal-state)
- [Amount exceeds remaining](#amount-exceeds-remaining)
- [Duplicate refund created](#duplicate-refund-created)
- [Refund missing from export](#refund-missing-from-export)

## Refund stuck in pending

**Condition.** The refund returned `201 Created` with `status = "pending"` more than 5 minutes ago and you have not received a `refund.succeeded` or `refund.failed` webhook.

**Cause.** Most often, the webhook receiver in your service is misconfigured or unreachable. Less often, the refund is genuinely still in flight at the card network, particularly on weekends or during issuer-side incidents.

**Remedy.**

1. **Check sandbox vs. production.** Sandbox always resolves in <5 seconds. Longer than that in sandbox = your webhook receiver is the problem.
2. **Check the dashboard** at **Payments → Refunds**. If it shows **Succeeded** but you have no webhook, your subscription is misconfigured.
3. **Check your webhook delivery log** at Dashboard → **Developers → Webhooks** → endpoint → **Recent deliveries**. Look for retries with non-2xx responses. Fix your endpoint, then click **Resend** on the failed event.
4. **If still pending after 30 minutes in production**, file a support ticket with the refund ID. Do **not** retry the request — the original is still in flight and a retry with a new idempotency key would create a duplicate.

## Refund failed in terminal state

**Condition.** A `refund.failed` webhook arrived, or the dashboard shows **Failed**.

**Cause.** The `failure_code` field in the webhook payload identifies the specific cause. Four codes account for nearly all failures.

**Remedy.** Match the `failure_code` to the appropriate action below.

| `failure_code` | Cause (specific) | Remedy |
| -------------- | ---------------- | ------ |
| `card_declined` | The issuing bank rejected the credit attempt. Usually a closed card. | Contact the customer. The refund must go to an alternative method (ACH, store credit, manual). |
| `account_closed` | The merchant settlement account is closed or frozen. | Escalate to the account admin. No customer-facing action until resolved. |
| `network_error` | A transient network failure between PayCart and the card network. | Reissue the refund with a **new** idempotency key. The original is terminal; it will not retry itself. |
| `processor_error` | An error inside PayCart's processor. Rare. | File a support ticket. Do not retry without guidance. |

## Amount exceeds remaining

**Condition.** The API rejected the request with `400 amount_exceeds_remaining`.

**Cause.** The captured payment has less refundable balance than the amount you requested. Usually a previous partial refund has already been issued against the same payment.

**Remedy.**

1. Get the **remaining refundable** value from `GET /v1/payments/{payment_id}` (field: `remaining_refundable_amount`) or from the dashboard.
2. Reissue the refund with `amount` ≤ that value.
3. If the customer is owed more than `remaining_refundable_amount` and the original payment is fully refunded, the additional credit must be issued outside PayCart (manual ACH, store credit).

## Duplicate refund created

**Condition.** Two refund rows exist for what should have been one refund.

**Cause.** A retry was sent without reusing the original `Idempotency-Key`. PayCart treated the second request as a separate refund and processed it.

**Remedy.**

1. **Determine which refund is the canonical one** — usually the earlier `created_at`.
2. **Cancel the duplicate** with `POST /v1/refunds/{id}/cancel`. Only refunds in `pending` state can be canceled. If both are `succeeded`, you cannot cancel; you must issue a new **payment** to the customer for the duplicate amount.
3. **Fix the retry path** so future retries reuse the original `Idempotency-Key`. See [Before you start — Why idempotency matters here](before-you-begin.md#why-idempotency-matters-here).

## Refund missing from export

**Condition.** A refund shows **Succeeded** in the dashboard but is absent from the daily ledger export.

**Cause.** Three causes, in order of likelihood:

1. The refund's `occurred_at` straddles the export-day cutoff.
2. The export was downloaded before its 06:00 UTC completion time.
3. A genuine reconciliation gap on PayCart's side. Rare.

**Remedy.**

1. **Check the timestamp.** The export covers events with `occurred_at` between 00:00 and 23:59:59 UTC of the export date. A refund that succeeded at 23:59:55 UTC may appear in either day's export depending on processing delay; allow one day before escalating.
2. **Re-download after 06:00 UTC** if you pulled it earlier. Earlier downloads are partial.
3. **If still missing after 24 hours**, file a support ticket with the refund ID and the export date. PayCart will issue a corrected export.
