---
title: Before you start
description: Prerequisites, the payment lifecycle in one paragraph, and why idempotency keys are non-negotiable.
type: concept
topic_id: c_before-you-begin
audience: all
level: foundational
sequence: 2
estimated_time: "~5 minutes to read"
status: published
last_reviewed: 2026-05-04
---

# Before you start

*Concept · For all readers · Foundational · Step 2 of 7 · ~5 minutes*

## Short description

This page covers prerequisites for **all three steps** of the workflow. Read it once; the rest of the workflow assumes it.

--8<-- "includes/notices/sandbox-only.md"

## Description

### What you need

| Role | What you need | Where to get it |
| ---- | ------------- | --------------- |
| Developer | A sandbox API key (prefix `sk_test_`) | Dashboard → **Developers** → **API keys** → **Reveal test key** |
| Operations | A user account with the **Operations** role | Ask your account admin |
| Finance | A user account with the **Finance** role and access to the daily ledger export | Ask your account admin |

You also need a **captured** payment to refund. Use payment ID `pay_01HABCDEF12345` in the sandbox — it's pre-loaded with $100.00 USD captured against a sample card.

### The payment lifecycle in one paragraph

A PayCart payment moves through five states: **created → authorized → captured → refunded** (or, if a chargeback is opened, **disputed → won/lost**). A refund is only valid against a payment in the **captured** state. Issuing a refund creates a new resource — a `refund` — that is linked to the original payment and reduces its remaining refundable balance. A payment can have many partial refunds, but the sum of refunded amounts can never exceed the captured amount.

```text
created → authorized → captured ── refund (full)  → refunded
                                └─ refund (partial) → captured (with reduced remaining balance)
                                └─ chargeback opened → disputed → won | lost
```

### Why idempotency matters here

Refunds are the most expensive operation to issue twice. A duplicate refund moves money out of your settlement account that you cannot reclaim without contacting the customer.

Every request to `POST /v1/refunds` **must** include an `Idempotency-Key` header. The key is a string you generate; PayCart caches the response under that key for 24 hours. If you retry the request with the same key, you get the same response — guaranteed, not just usually.

--8<-- "includes/notices/idempotency.md"

A common, safe pattern: `Idempotency-Key: refund-<your-internal-refund-request-id>`. Use the same key on automatic retries (network timeout, 5xx). Never reuse a key for a different refund.

## Example

A correct retry pattern after a 504 Gateway Timeout from `POST /v1/refunds`:

1. Original request: `Idempotency-Key: refund-req-9f2a1c`. Times out at 30 s.
2. Retry: same `Idempotency-Key: refund-req-9f2a1c`. Returns the cached response (whether the original ultimately succeeded or not).
3. The reader of step 1 now has a deterministic answer with no possibility of double-charging.

## Related links

- [Step 1 — Create the refund](create-refund.md)
- [Reference — `POST /v1/refunds`](reference.md#post-v1refunds)
- [Troubleshoot — Duplicate refund created](troubleshoot.md#duplicate-refund-created)
