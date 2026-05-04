---
title: Home
description: Senior technical-writer portfolio — a single, end-to-end refund workflow on a fictional payments platform.
type: landing
audience: all
status: published
last_reviewed: 2026-05-04
---

# A documentation walkthrough, not a product

*Landing · For all readers*

This site is a **portfolio piece** for a senior technical writer. Instead of a sprawling product docs site, it focuses on **one** workflow — issuing and reconciling a refund — and follows it end-to-end across the three audiences who actually own that work.

If you have ten minutes, start at [the workflow overview](refund-workflow/index.md). If you have one minute, read the box below.

!!! abstract "What this site demonstrates"
    - **One workflow, three audiences.** The same refund travels from a developer's API call to an ops associate's dashboard to a finance analyst's ledger. Each step is written for the reader who actually does it.
    - **Concept-led structure.** The mental model comes before the procedure. The reference comes after.
    - **Restraint in reference.** The Reference page lists *only* the API fields, status codes, and webhook events used in this workflow — not a full API.
    - **Reusable callouts and snippets.** Sandbox notices and code samples are written once and included on every page that needs them.

## Pick your starting point

=== "Developer"

    You issue the refund via the API.

    Start at **[Step 1 — Create the refund](refund-workflow/1-create.md)**.

=== "Operations"

    You confirm the refund landed correctly in the dashboard.

    Start at **[Step 2 — Confirm in the dashboard](refund-workflow/2-confirm.md)**.

=== "Finance"

    You reconcile the refund against the daily ledger export.

    Start at **[Step 3 — Reconcile the ledger](refund-workflow/3-reconcile.md)**.

=== "Reviewer"

    You're evaluating the writing, not running the workflow.

    Start at **[Refund workflow overview](refund-workflow/index.md)**, then read **[About this portfolio](about.md)** for the editorial decisions.

---

*PayCart is a fictional payment platform. Every name, ID, card number, customer, merchant, amount, and event in this site is invented.*
