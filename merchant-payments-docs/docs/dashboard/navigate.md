---
title: Navigate the dashboard
description: A first-day tour of the PayCart Merchant Dashboard.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [dashboard, getting-started]
---

# Navigate the dashboard

The PayCart Merchant Dashboard is where you spend most of your time once you're live. This tour walks through the seven areas you'll touch in your first week.

--8<-- "includes/notices/dummy-data.md"

## At a glance

| Area | What it's for | Who uses it |
| --- | --- | --- |
| **Home** | Today's volume, pending settlements, alerts. | Everyone. |
| **Payments** | Search and inspect any transaction. | Support, finance, ops. |
| **Customers** | The list of saved payment methods and their owners. | Support, ops. |
| **Disputes** | Open chargebacks and the evidence you've submitted. | Ops, finance. |
| **Payouts** | The history of bank transfers from PayCart to you. | Finance. |
| **Reports** | Saved CSV exports for accounting. | Finance. |
| **Settings** | Team, branding, payment methods, payout schedule. | Owners and admins. |

## Home

The Home tab shows three cards by default. You cannot remove them, but you can reorder them by dragging:

1. **Today's volume** — gross captured amount and count, compared to the same day last week.
2. **Needs attention** — pending disputes, payments stuck in `requires_action`, expiring API keys.
3. **Recent activity** — a feed of every event in the last 24 hours.

If you only check one thing each morning, check **Needs attention**.

## Payments

This is the table you'll use most. Three things to know:

1. **Search by anything.** The search bar accepts payment IDs, customer emails, last-four card digits, amount ranges (`>500`, `100..200`), and metadata keys.
2. **Save your filters.** Click **Save view** in the top right to keep a filter set in the sidebar. A common one to save: `status:disputed OR status:requires_action`.
3. **Open the side panel.** Clicking a row opens a side panel with the full timeline, related events, and the receipt PDF. You don't need to leave the table.

## Customers

A *customer* in PayCart is the persistent record of someone who has paid you, including their saved payment methods and metadata you've attached. You can:

- Look up a customer by email or by their internal customer ID.
- See every payment they've ever made, including refunded and disputed ones.
- Update their metadata (for example, a CRM ID).

## Disputes

Each row is one chargeback. The **Respond by** column is the only thing that matters day-to-day — miss that date and you lose by default. See [Manage disputes](../guides/disputes.md) for the response workflow.

## Payouts

Each row is one bank transfer. Click a row to see the list of payments included in the transfer and to download the [reconciliation report](../guides/reconciliation.md) as CSV.

## Settings

The first five things to set up:

1. **Team** — invite at least one other admin so you're not the only one with access.
2. **Branding** — upload a logo for receipts.
3. **Payout schedule** — daily, weekly, or manual.
4. **Payment methods** — turn on the wallets and card networks you want to accept.
5. **Webhooks** — point at least one URL at your application. See [Webhooks](../integration/webhooks.md).

## Keyboard shortcuts

| Keys | What it does |
| --- | --- |
| <kbd>g</kbd> <kbd>p</kbd> | Go to Payments |
| <kbd>g</kbd> <kbd>d</kbd> | Go to Disputes |
| <kbd>/</kbd> | Focus the search bar |
| <kbd>?</kbd> | Show all shortcuts |

## Related reading

- [View transactions](view-transactions.md) — how to find a specific payment fast.
- [Process refunds](../guides/refunds.md) — the most common dashboard action.
