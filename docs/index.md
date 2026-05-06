---
title: Home
description: Single, end-to-end refund workflow on a fictional payments platform.
type: topic
topic_id: topic_home
audience: all
last_reviewed: 2026-05-06
---

# A documentation walkthrough, not a product

This site is a **portfolio piece** for a senior technical writer. Instead of a sprawling product docs site, it focuses on **one** workflow — issuing and reconciling a refund — and follows it end-to-end across the three audiences who actually own that work. Every page is structured against [DITA](https://en.wikipedia.org/wiki/Darwin_Information_Typing_Architecture) topic types so a reviewer can see the editorial discipline directly in the markup.

!!! abstract "What this site demonstrates"
    - **One workflow, three audiences.** The same refund travels from a developer's API call to an ops associate's dashboard to a finance analyst's ledger. Each page is written for the reader who actually does it.
    - **DITA topic typing.** Every page declares a `concept`, `task`, `reference`, or `troubleshooting` type and conforms to that type's body structure.
    - **Restraint in reference.** The Reference page lists *only* the API fields, status codes, and webhook events used in this workflow — not a full API.
    - **Reusable content** via the [`pymdownx.snippets`](https://facelessuser.github.io/pymdown-extensions/extensions/snippets/) extension — DITA's `conref` pattern in markdown form. The summary below is reused from the [refund concept topic](concept-topics/refund-overview.md) — single source, two locations.

## How to read this site

The pages are written to be read in this order on a first pass. Each layer builds on the one above it. If you already know what you're looking for, skip straight to the section you need.

### 1. Start with the concepts — *what the operations are*

The four payment operations and how they relate to each other. Read these before any procedure so you can recognize which operation a given task uses, and why.

- [Payment operations at a glance](concept-topics/index.md) — start here for a one-page tour of all four
- [Authorize a payment](concept-topics/authorize-overview.md) — place a hold on funds
- [Charge a payment](concept-topics/charge-overview.md) — capture the funds
- [Cancel a payment](concept-topics/cancel-overview.md) — release a hold before charge
- [Refund a payment](concept-topics/refund-overview.md) — return funds after charge

### 2. Walk the workflow — *how a refund travels through three teams*

A single refund, end-to-end, written for the audience that actually owns each step.

- [Refund workflow overview](refund-workflow/index.md) — the three steps in one diagram
- [Before you start](refund-workflow/before-you-begin.md) — prerequisites and idempotency
- [Step 1 — Create the refund](refund-workflow/create-refund.md) — *for developers*
- [Step 2 — Confirm in the dashboard](refund-workflow/confirm-in-the-dashboard.md) — *for operations*
- [Step 3 — Reconcile the ledger](refund-workflow/reconcile-ledger.md) — *for finance*

### 3. Look things up — *when something goes wrong, or you need exact field names*

The two pages you'll keep open in another tab.

- [Troubleshoot](refund-workflow/troubleshoot.md) — decision tree for the five most common refund failures
- [Reference](refund-workflow/reference.md) — API fields, refund status codes, and webhook events used in this workflow

### Reader-shortcut paths

If you already know your role, jump straight in:

| Role | Start here |
| ---------- | ---------- |
| **Developer** integrating refunds | [Refund a payment](concept-topics/refund-overview.md) → [Step 1 — Create the refund](refund-workflow/create-refund.md) → [Reference](refund-workflow/reference.md) |
| **Operations** associate verifying refunds | [Step 2 — Confirm in the dashboard](refund-workflow/confirm-in-the-dashboard.md) → [Troubleshoot](refund-workflow/troubleshoot.md) |
| **Finance** analyst reconciling | [Step 3 — Reconcile the ledger](refund-workflow/reconcile-ledger.md) → [Reference — Webhook events](refund-workflow/reference.md) |
| **Reviewer** evaluating the writing | [Refund workflow overview](refund-workflow/index.md) → [Payment operations at a glance](concept-topics/index.md) |