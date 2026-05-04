---
title: Sample merchant
description: A worked example of a fully populated PayCart merchant profile, using fictional data.
type: sample
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [sample, merchant, onboarding]
---

# Sample merchant

This page is a complete example of what a populated PayCart merchant looks like. Every value is fictional and intended only as a reference for documentation, training, and demos.

--8<-- "includes/notices/dummy-data.md"

## Business profile

| Field | Value |
| --- | --- |
| Merchant ID | `mch_sample_aB12` |
| Legal name | Sample Coffee Co. LLC |
| Doing-business-as | Sample Coffee |
| Industry | Retail / hospitality |
| Country of formation | United States |
| Tax ID | `12-3456789` |
| Date of formation | 2019-08-14 |
| Website | `https://sample-coffee.example` |

## Representative

| Field | Value |
| --- | --- |
| Name | Avery Park |
| Role | Owner |
| Email | `avery@sample-coffee.example` |
| Phone | +1 (555) 010-2024 |

## Beneficial owners

| Name | Ownership | Date of birth |
| --- | --- | --- |
| Avery Park | 60% | 1985-03-22 |
| Jordan Park | 40% | 1987-07-09 |

## Bank account

| Field | Value |
| --- | --- |
| Bank | Sample National Bank |
| Routing | `110000000` |
| Account | `000123456789` |
| Account type | Checking |
| Status | Verified by micro-deposit on 2026-01-30 |

## Payment methods enabled

- Visa, Mastercard, American Express, Discover.
- Apple Pay, Google Pay.
- ACH direct debit (US only).

## Webhook endpoints

| URL | Environment | Subscribed events |
| --- | --- | --- |
| `https://sample-coffee.example/hooks/paycart` | Live | `payment.*`, `refund.*`, `dispute.*`, `payout.paid` |
| `https://staging.sample-coffee.example/hooks/paycart` | Sandbox | All events |

## Team

| Email | Role |
| --- | --- |
| `avery@sample-coffee.example` | Owner |
| `jordan@sample-coffee.example` | Admin |
| `dev@sample-coffee.example` | Developer |
| `support@sample-coffee.example` | Support |

## Compliance status

| Check | Status |
| --- | --- |
| KYC | Verified 2026-01-28 |
| AML screening | Cleared 2026-01-28 |
| Bank account verification | Verified 2026-01-30 |
| Live mode | Enabled 2026-02-01 |

## Statement descriptor

The line your customers see on their card statement.

```text
SAMPLECOFFEE  PAYCART
```

For more on what a good descriptor looks like, see [Manage disputes](../guides/disputes.md#reduce-the-rate-at-which-you-get-disputes).

## Related reading

- [Onboarding overview](../onboarding/overview.md) — the steps that produced this profile.
- [Sample requests](sample-requests.md) — request and response examples that use this merchant.
