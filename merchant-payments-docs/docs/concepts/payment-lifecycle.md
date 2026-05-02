---
title: Payment lifecycle
description: How a PayCart payment moves from intent to settled funds.
type: concept
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [payments, lifecycle, concepts]
---

# Payment lifecycle

A PayCart payment is not a single instant. It is a short-lived state machine that moves from a customer's intent to pay through to settled funds in your bank account. Most pages in these docs assume you know this model, so it's worth ten minutes.

## The five stages

```text
  intent  →  authorized  →  captured  →  settled  →  paid out
                ↘                ↘            ↘
              voided          refunded     disputed
```

| Stage | What's happening | Typical duration |
| --- | --- | --- |
| **Intent** | Your application has described what the customer wants to pay for. No money has moved. | Milliseconds. |
| **Authorized** | The issuing bank has approved the charge and earmarked funds. Funds are *not* yours yet. | Up to seven days, depending on card scheme. |
| **Captured** | You have told PayCart to convert the authorization into a real charge. | Seconds. |
| **Settled** | PayCart has received the funds from the card networks. | One to two business days. |
| **Paid out** | Settled funds have been wired to your merchant bank account. | Daily, on your payout schedule. |

## Why two steps for authorize and capture

Authorization without capture lets you separate "the customer agreed to pay" from "we are taking the money." That separation matters when:

- **Goods ship later.** Capture only when the order leaves the warehouse.
- **The amount changes.** A hotel authorizes for an estimate and captures for the final bill.
- **You're running risk checks.** Authorize on checkout, capture only after fraud rules pass.

If you do not need that separation, set `capture_method: "automatic"` and PayCart captures immediately on success.

## What can go wrong, and where

- **At authorization** — declines (insufficient funds, suspected fraud, expired card). The payment never leaves the *intent* stage.
- **Between authorization and capture** — the authorization expires, or you call `void` to cancel it.
- **After capture** — the customer disputes the charge, or you issue a refund.

For the full enumeration of states and transitions, see [Transaction states](transaction-states.md).

## Where money is during each stage

See [Money movement](money-movement.md) for who holds the funds at each stage and what that means for your accounting.

## Related reading

- [Quickstart](../getting-started/quickstart.md) — your first end-to-end payment in the sandbox.
- [Process refunds](../guides/refunds.md) — handling the *refunded* branch.
- [Manage disputes](../guides/disputes.md) — handling the *disputed* branch.
- [Glossary](../reference/glossary.md) — definitions for *authorization*, *capture*, *settlement*, and friends.
