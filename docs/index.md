---
title: Home
description: Senior technical-writer portfolio — a single, end-to-end refund workflow on a fictional payments platform.
type: topic
topic_id: topic_home
audience: all
last_reviewed: 2026-05-06
---

# A documentation walkthrough, not a product

*Topic · For all readers*

This site is a **portfolio piece** for a senior technical writer. Instead of a sprawling product docs site, it focuses on **one** workflow — issuing and reconciling a refund — and follows it end-to-end across the three audiences who actually own that work. Every page is structured against [DITA](https://en.wikipedia.org/wiki/Darwin_Information_Typing_Architecture) topic types so a reviewer can see the editorial discipline directly in the markup.

> [!NOTE]  abstract "What this site demonstrates"
    - **One workflow, three audiences.** The same refund travels from a developer's API call to an ops associate's dashboard to a finance analyst's ledger. Each page is written for the reader who actually does it.
    - **DITA topic typing.** Every page declares a `concept`, `task`, `reference`, or `troubleshooting` type and conforms to that type's body structure.
    - **Restraint in reference.** The Reference page lists *only* the API fields, status codes, and webhook events used in this workflow — not a full API.
    - **Reusable content** via the [`pymdownx.snippets`](https://facelessuser.github.io/pymdown-extensions/extensions/snippets/) extension — DITA's `conref` pattern in markdown form. The summary below is reused from the [refund concept topic](concept-topics/refund-overview.md) — single source, two locations.

## What is a refund?

--8<-- "concept-topics/refund-overview.md:refund-intro"

For the full concept — lifecycle, states, constraints, and an example — read [Refund a payment](concept-topics/refund-overview.md).

## Pick your starting point

=== "Developer"

    You issue the refund via the API.

    Start at **[Step 1 — Create the refund](refund-workflow/create-refund.md)**.

=== "Operations"

    You confirm the refund landed correctly in the dashboard.

    Start at **[Step 2 — Confirm in the dashboard](refund-workflow/confirm-in-the-dashboard.md)**.

=== "Finance"

    You reconcile the refund against the daily ledger export.

    Start at **[Step 3 — Reconcile the ledger](refund-workflow/reconcile-ledger.md)**.

=== "Reviewer"

    You're evaluating the writing, not running the workflow.

    Start at **[Refund workflow overview](refund-workflow/index.md)**.

## Read in ten minutes

If you have ten minutes, read the [refund workflow overview](refund-workflow/index.md) end to end. If you have one minute, the abstract above is the whole pitch.

---

*PayCart is a fictional payment platform. Every name, ID, card number, customer, merchant, amount, and event in this site is invented.*
