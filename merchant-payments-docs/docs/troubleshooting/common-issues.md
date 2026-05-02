---
title: Troubleshooting
description: Common PayCart problems and how to fix them.
type: troubleshooting
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [troubleshooting, support]
---

# Troubleshooting

This page is organized by symptom. Find the row that matches what you're seeing, then follow the link to the fix.

--8<-- "includes/notices/dummy-data.md"

## Authentication and access

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `401 Unauthorized` on every call | Wrong key for the environment (live vs sandbox). | Compare the key prefix: `sk_live_…` vs `sk_sandbox_…`. |
| `401` only on some calls | Restricted key. | Check the key's allowed scopes in **Dashboard → Developers → API keys**. |
| `403 Forbidden` | Your team role lacks permission. | Ask an owner to upgrade your role to **Admin** or **Developer**. |
| Token expires sooner than expected | Token TTL is 60 minutes. | Refresh with the `client_credentials` grant on each call, or cache for less than 50 minutes. |

See [Authentication](../integration/authentication.md) for the full flow.

## Payments not succeeding

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `card_declined` on every test card | You're hitting production with sandbox cards. | Switch the base URL to `https://sandbox.paycart.example`. |
| Stuck in `requires_action` | The customer hasn't completed 3-D Secure. | Redirect them to `next_action.redirect_to_url` and listen for `payment.succeeded`. |
| Stuck in `processing` for more than five minutes | Network outage at the issuer. | Wait — PayCart will resolve to `succeeded` or `failed` within 24 hours. Don't retry. |
| `409 Conflict` on capture | The payment is past its authorization window. | Re-authorize. Or set `capture_method: "automatic"` to avoid the gap. |

For the full state machine, see [Transaction states](../concepts/transaction-states.md).

## Webhooks

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Webhook never arrives | Endpoint is HTTP not HTTPS, or returns non-`2xx`. | Check **Dashboard → Developers → Webhooks → Recent deliveries**. |
| Same event delivered multiple times | Your handler returned non-`2xx`, even briefly. | Make the handler idempotent (see [Webhooks](../integration/webhooks.md)). |
| Signature verification fails | You're hashing the parsed JSON instead of the raw body. | Hash the raw request body bytes before any parser touches them. |
| Events arrive out of order | This is normal at scale. | Re-fetch the underlying object via the API instead of trusting `data.object`. |

## Refunds and disputes

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Refund returns `refund_window_exceeded` | The original payment is more than 180 days old. | Issue a credit out-of-band; the card networks won't accept the refund. |
| Refund returns `insufficient_funds` | Your PayCart balance is too low to cover the refund. | Top up the balance from **Settings → Balance → Add funds**. |
| Dispute response submitted but case still open | This is normal. | Issuers take 60–75 days to decide. |
| `dispute.created` for a payment we already refunded | Race between refund and dispute. | Accept the dispute; the customer keeps the original refund and the dispute closes as a wash. |

## Reconciliation

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Bank line doesn't match the payouts report total | You're missing fees and disputes in your reconciliation. | Use the formula in [Money movement](../concepts/money-movement.md#fees). |
| One payment appears in two payouts | A partial refund split across days. | Group reconciliation by `payout_id`, not by date. |
| Currency conversion is off by a fraction | FX rate timing. | PayCart locks the rate at capture, not at payout. The dashboard shows both values. |

## Still stuck?

If nothing here matches, capture:

1. The payment, refund, dispute, or webhook event ID.
2. The exact request you sent and the exact response you got back.
3. A timestamp in UTC.

These three things make any support investigation 10x faster than a screenshot.

## Related reading

- [FAQ](../faq.md)
- [Status codes](../reference/status-codes.md)
