---
title: Status codes
description: HTTP status codes and PayCart error codes you'll encounter, and what they mean.
type: reference
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [errors, reference, status-codes]
---

# Status codes

PayCart returns standard HTTP status codes plus a string error code in the body. The HTTP code tells you what category of problem; the error code tells you which specific problem.

## HTTP status codes

| Code | Meaning | Common causes |
| --- | --- | --- |
| `200 OK` | Success on a `GET` or idempotent retry. | Normal. |
| `201 Created` | Success on a `POST` that created a resource. | Normal. |
| `400 Bad Request` | The request was malformed. | Missing required field, wrong type, invalid JSON. |
| `401 Unauthorized` | Credentials missing or wrong. | Wrong key, expired token, sandbox key in live mode. |
| `403 Forbidden` | Credentials valid but the role can't do this. | Restricted key, insufficient team role. |
| `404 Not Found` | No object with that ID exists. | Typo in the ID, wrong environment, object deleted. |
| `409 Conflict` | The request conflicts with the current state. | Capture after expiry, refund on a refunded payment. |
| `422 Unprocessable Entity` | The request was understood but rejected by business rules. | Refund larger than the original payment. |
| `429 Too Many Requests` | Rate limit exceeded. | Bursting beyond your tier's limit. |
| `500 Internal Server Error` | Something broke on PayCart's side. | Rare. Retry with backoff. |
| `503 Service Unavailable` | Temporary outage. | Check the status page; retry with backoff. |

## PayCart error codes

The body of every non-`2xx` response contains:

```json
{
  "error": {
    "code": "card_declined",
    "message": "The card was declined.",
    "type": "card_error",
    "decline_code": "insufficient_funds",
    "doc_url": "https://docs.paycart.example/reference/status-codes#insufficient_funds"
  }
}
```

### Card errors (`type: "card_error"`)

| `code` | When you see it |
| --- | --- |
| `card_declined` | The issuer declined the charge. Inspect `decline_code` for the reason. |
| `expired_card` | The card's expiry date has passed. |
| `incorrect_cvc` | The CVC didn't match what the issuer expects. |
| `processing_error` | Issuer-side error. Retry. |

### Decline codes

These appear in the `decline_code` field on `card_declined` errors.

| `decline_code` | Meaning |
| --- | --- |
| `insufficient_funds` | Not enough money on the card. |
| `lost_card` | The card has been reported lost. |
| `stolen_card` | The card has been reported stolen. |
| `generic_decline` | The issuer didn't say why. |
| `do_not_honor` | The issuer flatly refused. |
| `try_again_later` | Transient issuer error. Retry. |

### Validation errors (`type: "validation_error"`)

| `code` | When you see it |
| --- | --- |
| `parameter_missing` | A required field was not sent. |
| `parameter_invalid` | A field is the wrong type or format. |
| `idempotency_key_required` | A write call was sent without `Idempotency-Key`. |
| `idempotency_key_in_use` | The key is currently in use by another in-flight request. |

### State errors (`type: "state_error"`)

| `code` | When you see it |
| --- | --- |
| `state_invalid` | The action isn't allowed in the object's current state. |
| `refund_window_exceeded` | The original payment is older than 180 days. |
| `authorization_expired` | Capture attempted after the authorization window closed. |

## Dispute reasons

Card schemes use their own reason codes for disputes. PayCart normalizes them to a consistent vocabulary on the dashboard, but exposes the raw code in `dispute.reason`.

| `dispute.reason` | Customer's claim |
| --- | --- |
| `fraudulent` | "I didn't authorize this charge." |
| `unrecognized` | "I don't recognize this charge." |
| `duplicate` | "I was charged twice." |
| `subscription_canceled` | "I canceled before this charge." |
| `product_unacceptable` | "The product wasn't what I ordered." |
| `product_not_received` | "I never got what I ordered." |
| `credit_not_processed` | "I was promised a refund but didn't get one." |
| `general` | None of the above; the card scheme didn't categorize it. |

## Related reading

- [Troubleshooting](../troubleshooting/common-issues.md) — symptom-based fixes for the most common errors.
- [Manage disputes](../guides/disputes.md) — how to respond to each reason code.
