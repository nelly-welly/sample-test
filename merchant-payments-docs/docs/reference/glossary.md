---
title: Glossary
description: Single-sentence definitions for the terms used throughout PayCart docs.
type: reference
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [glossary, reference]
---

# Glossary

These are the terms used most often in PayCart docs. The first definition for each term is canonical — every other page links here rather than redefining.

## Payment terms

--8<-- "includes/glossary/core-terms.md"

**Authorization window**
:   The period during which an authorization remains valid for capture. Seven days for most card networks; 30 days for AmEx travel and lodging.

**Chargeback**
:   See *Dispute*.

**Dispute**
:   A customer's request to their bank to reverse a charge. Disputes have hard response deadlines and can result in lost funds and a fee.

**Issuer**
:   The bank that issued the customer's card.

**Acquirer**
:   The bank that holds PayCart's clearing account and moves funds between issuers and merchants.

**Card scheme** (also: *card network*)
:   The brand that runs the rails — Visa, Mastercard, American Express, Discover, JCB.

## Account terms

**Merchant**
:   A business that uses PayCart to accept payments. One PayCart account can hold multiple merchants.

**Owner**
:   The team role with full administrative rights, including the ability to delete the account.

**Admin**
:   The team role that can manage payments, refunds, disputes, settings, and team members, but cannot delete the account.

**Developer**
:   The team role that can manage API keys, webhooks, and the sandbox, but cannot move money.

## Money terms

**Balance**
:   The funds PayCart is holding for you. Captured payments arrive here at settlement; payouts move them to your bank.

**Payout**
:   A bank transfer from your PayCart balance to your merchant bank account. Paid out daily by default.

**Fee**
:   The amount PayCart deducts per captured payment, listed on every payment record as a separate line.

**Reconciliation**
:   The process of matching your bank lines to PayCart payouts and the underlying payments inside them.

## Integration terms

**Sandbox**
:   PayCart's test environment. Funds are simulated and never reach the card networks.

**Live mode**
:   PayCart's production environment. Funds move for real.

**API key**
:   A credential used to authenticate calls to the PayCart API. `sk_live_…` for production, `sk_sandbox_…` for sandbox.

**Webhook**
:   See *Webhook* in [Core terms](#payment-terms).

## See also

- [Status codes](status-codes.md) — error codes and what they mean.
- [Test data](test-data.md) — sandbox card numbers, customers, and merchants.
