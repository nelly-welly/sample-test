---
title: Sample requests
description: Copy-pasteable PayCart request and response pairs for the most common operations.
---

# Sample requests

Each section below pairs a request with the matching response. All requests target the sandbox; swap the base URL for the live host when you're ready.

--8<-- "includes/notices/sandbox-only.md"

## Authenticate

### Request

--8<-- "includes/snippets/curl-auth.md"

### Response

```json
{
  "access_token": "tok_sample_2X9k_…",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Create a payment

### Request

--8<-- "includes/snippets/curl-create-payment.md"

### Response

```json
{
  "id": "pay_sample_3fE7",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "description": "Order #7421 — 1x Sample Mug",
  "metadata": {},
  "created_at": "2026-04-29T14:02:11Z"
}
```

## Retrieve a payment

### Request

```bash
curl -X GET https://sandbox.paycart.example/v1/payments/pay_sample_3fE7 \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN"
```

### Response

```json
{
  "id": "pay_sample_3fE7",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "description": "Order #7421 — 1x Sample Mug",
  "fees": [
    { "type": "card_processing", "amount": 128 }
  ],
  "created_at": "2026-04-29T14:02:11Z"
}
```

## Refund a payment

### Request

--8<-- "includes/snippets/curl-refund.md"

### Response

```json
{
  "id": "ref_sample_9bC4",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "payment_id": "pay_sample_3fE7",
  "reason": "customer_requested",
  "created_at": "2026-04-29T15:10:42Z"
}
```

## List recent payments

### Request

```bash
curl -X GET "https://sandbox.paycart.example/v1/payments?limit=5&status=succeeded" \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN"
```

### Response

```json
{
  "data": [
    {
      "id": "pay_sample_3fE7",
      "status": "succeeded",
      "amount": 4200,
      "currency": "USD",
      "created_at": "2026-04-29T14:02:11Z"
    }
  ],
  "has_more": false,
  "next_cursor": null
}
```

## Send a test webhook

Useful when you want to confirm your endpoint is wired up without creating a payment.

### Request

```bash
curl -X POST https://sandbox.paycart.example/v1/webhook_endpoints/whe_sample_test/send_test \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ "event_type": "payment.succeeded" }'
```

### Response

```json
{
  "event_id": "evt_sample_8aH2",
  "delivered": true,
  "endpoint_response_code": 200
}
```

## Errors

Every error follows the same envelope. See [Status codes](../reference/status-codes.md) for the full taxonomy.

```json
{
  "error": {
    "code": "card_declined",
    "type": "card_error",
    "message": "The card was declined.",
    "decline_code": "insufficient_funds",
    "doc_url": "https://docs.paycart.example/reference/status-codes#insufficient_funds"
  }
}
```

## Related reading

- [Quickstart](../getting-started/quickstart.md) — these requests in workflow form.
- [Test data](../reference/test-data.md) — cards and customers that drive each outcome.
