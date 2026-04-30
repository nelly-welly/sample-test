---
title: Authentication
description: How to authenticate with the PayCart Merchant Payments Platform.
---

# Authentication

PayCart uses two credential types, and which one you use depends on what you're calling.

| Credential | Use it for | Where it goes |
| --- | --- | --- |
| **API key** (`sk_live_…` or `sk_sandbox_…`) | Server-side calls. | `Authorization: Bearer <key>` |
| **Access token** (returned by the OAuth endpoint) | Calls on behalf of a connected merchant. | `Authorization: Bearer <token>` |

For most integrations, the API key alone is enough. The OAuth flow only matters if you're a platform that operates on behalf of multiple merchants.

--8<-- "includes/notices/sandbox-only.md"

## Quick reference

| Environment | Base URL | Key prefix |
| --- | --- | --- |
| Sandbox | `https://sandbox.paycart.example` | `sk_sandbox_…` |
| Live | `https://api.paycart.example` | `sk_live_…` |

## Use an API key

Set the `Authorization` header on every request:

```bash
curl -X GET https://sandbox.paycart.example/v1/payments \
  -H "Authorization: Bearer sk_sandbox_sample_2X9k"
```

That's it. There's no second step for API key authentication.

## Use OAuth (platforms only)

Trade your client credentials for a 60-minute access token:

--8<-- "includes/snippets/curl-auth.md"

The response includes the token to use on subsequent calls:

```json
{
  "access_token": "tok_sample_2X9k_…",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

Cache the token until it's within five minutes of expiry, then refresh.

## Restricted keys

For workloads that only need to do one thing, create a *restricted* key. In **Dashboard → Developers → API keys → Create restricted key**, choose only the scopes the workload needs:

- `payments.read`, `payments.write`
- `refunds.write`
- `disputes.read`, `disputes.write`
- `webhooks.read`

A read-only reporting job, for example, should use a key with only `payments.read` and `payouts.read`. If the key leaks, the blast radius is small.

## Key rotation

--8<-- "includes/notices/rotating-keys.md"

The procedure:

1. **Dashboard → Developers → API keys → Rotate**. PayCart issues a new key and shows it once.
2. Add the new key to your secrets manager and deploy.
3. Confirm logs are using the new key (no `sk_live_<old>` calls in the last hour).
4. Click **Revoke old key**.

PayCart honors pending captures and refunds for 24 hours after revocation; new authorizations stop immediately.

## Common errors

| Error | Meaning | Fix |
| --- | --- | --- |
| `401 Unauthorized` | Missing or wrong key. | Confirm key prefix matches the environment. |
| `403 Forbidden` | Restricted key doesn't have the needed scope. | Add the scope or use a wider key. |
| `429 Too Many Requests` | Rate limit. | Back off; see retry guidance below. |

For the full code list, see [Status codes](../reference/status-codes.md).

## Rate limits

Default limits are 100 requests/second per key in live mode and 25/second in sandbox. Bursts up to 200/second are allowed for short windows. Exceeded requests return `429` with a `Retry-After` header — respect it rather than hammering.

## Related reading

- [Quickstart](../getting-started/quickstart.md) — your first authenticated call.
- [Webhooks](webhooks.md) — signature verification, which uses a separate webhook secret.
- [Status codes](../reference/status-codes.md)
