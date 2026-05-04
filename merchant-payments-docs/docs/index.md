---
title: PayCart Merchant Docs
description: Documentation home for the PayCart Merchant Payments Platform.
type: landing
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [home, overview]
---

# PayCart Merchant Docs

Welcome. PayCart helps merchants accept, track, and reconcile digital payments across web, mobile, and in-store channels. These docs walk you through onboarding, integration, day-to-day operations, and the concepts behind how money moves through the platform.

--8<-- "includes/notices/dummy-data.md"

## Pick a path

=== "I'm setting up a merchant account"

    Start with onboarding, then hand the integration to your developers.

    1. [Onboarding overview](onboarding/overview.md) — what you'll need to submit and how long verification takes.
    2. [Navigate the dashboard](dashboard/navigate.md) — the day-one tour.
    3. [View transactions](dashboard/view-transactions.md) — how to find any payment in seconds.

=== "I'm a developer integrating PayCart"

    Read the concepts first, then move to the quickstart.

    1. [Payment lifecycle](concepts/payment-lifecycle.md) — the model the rest of the docs assume.
    2. [Quickstart](getting-started/quickstart.md) — your first sandbox payment in under ten minutes.
    3. [Authentication](integration/authentication.md) and [Webhooks](integration/webhooks.md).

=== "I'm on a support or operations team"

    Get to outcomes fast.

    1. [Process refunds](guides/refunds.md)
    2. [Manage disputes](guides/disputes.md)
    3. [Reconcile transactions](guides/reconciliation.md)
    4. [Troubleshooting](troubleshooting/common-issues.md) and [FAQ](faq.md).

## What's in PayCart

| Capability | What it does |
| --- | --- |
| **Accept payments** | Cards, digital wallets (Apple Pay, Google Pay), and bank transfers from a single integration. |
| **Manage merchants** | Onboard, verify, and update merchant profiles with built-in KYC checks. |
| **Track money** | Real-time transaction status, payouts, and reconciliation reports. |
| **Handle exceptions** | Refunds, partial captures, disputes, and chargebacks from one place. |

## What's new

See [Release notes](release-notes.md) for the latest changes. Recent highlights:

- **2026-04** — Partial captures now configurable per merchant.
- **2026-03** — Dashboard adds a saved-views feature for transaction filters.
- **2026-02** — New `payment.requires_action` webhook for 3-D Secure flows.

## Get help

These are sample docs and there is no real support desk behind them. In a real PayCart account you would reach support via the dashboard's Help menu and find current incidents on the status page linked from there.
