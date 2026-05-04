---
title: Refund workflow overview
description: One refund travels through three teams — developer, operations, finance. Here is how each step connects.
type: concept
audience: all
level: foundational
sequence: 1
estimated_time: "~5 minutes to read"
status: published
last_reviewed: 2026-05-04
---

# Refund workflow

*Concept · For all readers · Foundational · Step 1 of 7 · ~5 minutes*

This workflow follows a single refund from request to reconciliation. It crosses three roles. The handoffs are explicit and the failure modes are documented.

!!! info "When to use this workflow"
    Use this end-to-end procedure when a customer requests a full or partial refund on a captured payment. For chargebacks initiated by the cardholder's bank, see *Manage disputes* — that's a different lifecycle and is out of scope for this portfolio.

## The three steps

| # | Step | Owner | Time | What success looks like |
| - | ---- | ----- | ---- | ---------------------- |
| 1 | [Create the refund](1-create.md) | Developer | ~1 minute on the wire | API returns `refund.status = "pending"` and a refund ID. |
| 2 | [Confirm in the dashboard](2-confirm.md) | Operations | ~5 minutes | Dashboard row shows the refund with status **Succeeded** and the original payment is linked. |
| 3 | [Reconcile the ledger](3-reconcile.md) | Finance | ~15 minutes (T+1) | Each `refund.succeeded` row in the daily export matches a debit in the general ledger. |

## Read this first

[Before you start](before-you-start.md) covers the prerequisites for all three steps — sandbox credentials, the payment lifecycle in one paragraph, and why **idempotency keys** are non-negotiable on this endpoint.

## When something goes wrong

[Troubleshoot](troubleshoot.md) is a decision tree for the five failure patterns that account for ~95 % of refund issues: refund stuck in pending, terminal failure, amount exceeds remaining, accidental duplicate, and refund missing from export.

## What's not on this page

There's no full API reference, no SDK changelog, no auth setup, and no payment-creation procedure. Those exist in a real PayCart product but would distract from the one thing this site is here to demonstrate. See [About this portfolio](../about.md) for the rationale.
