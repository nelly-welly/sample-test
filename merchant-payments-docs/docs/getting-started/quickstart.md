---
title: Quickstart
description: Make your first sandbox payment with PayCart in under ten minutes.
---

# Quickstart

This walks you through making your first sandbox payment end-to-end. By the end you'll have a successful payment, a webhook delivery, and a refund — all against fake money.

If you don't already understand the [payment lifecycle](../concepts/payment-lifecycle.md), read that first. Five minutes there will save you twenty minutes here.

--8<-- "includes/notices/sandbox-only.md"

## Before you start

You'll need:

- A PayCart sandbox account. (Sandbox is free and instant; no verification required.)
- Your sandbox publishable and secret keys, from **Dashboard → Developers → API keys**.
- Node.js 18+ or any HTTP client that can send `POST` requests with a JSON body.

## Step 1: Authenticate

Trade your client credentials for an access token.

--8<-- "includes/snippets/curl-auth.md"

A successful response looks like:

```json
{
  "access_token": "tok_sample_2X9k_…",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

Tokens are valid for one hour. See [Authentication](../integration/authentication.md) for the long version.

## Step 2: Create a payment

Use the access token from step 1.

=== "cURL"

    --8<-- "includes/snippets/curl-create-payment.md"

=== "Node.js"

    --8<-- "includes/snippets/node-create-payment.md"

=== "Python"

    --8<-- "includes/snippets/python-create-payment.md"

A successful response:

```json
{
  "id": "pay_sample_3fE7",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "description": "Order #7421 — 1x Sample Mug",
  "created_at": "2026-04-29T14:02:11Z"
}
```

If the response status is `requires_action`, the customer must complete 3-D Secure. Redirect them to the URL in `next_action.redirect_to_url` and listen for the `payment.succeeded` webhook.

## Step 3: Watch the webhook

In a second terminal, expose a local URL with `ngrok http 4242` (or any tunnel). Register that URL in **Dashboard → Developers → Webhooks** and subscribe to `payment.succeeded`.

Re-run step 2. Your endpoint should receive an event whose `data.object.id` matches the payment ID from the response.

If it doesn't, check **Dashboard → Developers → Webhooks → Recent deliveries** — every attempt is logged with the response code and body.

## Step 4: Refund the payment

--8<-- "includes/snippets/curl-refund.md"

The response:

```json
{
  "id": "ref_sample_9bC4",
  "status": "succeeded",
  "amount": 4200,
  "currency": "USD",
  "payment_id": "pay_sample_3fE7"
}
```

Your sandbox now shows the original payment as `refunded` in the dashboard.

## What's next

- [Authentication](../integration/authentication.md) for production credentials, key rotation, and scoped keys.
- [Webhooks](../integration/webhooks.md) for the full event catalog and signature verification.
- [Test data](../reference/test-data.md) for the cards, customers, and merchants you can use in the sandbox.
- [Sample requests](../samples/sample-requests.md) for fully formed request/response pairs you can copy.
