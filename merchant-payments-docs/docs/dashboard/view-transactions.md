---
title: View transactions
description: Find any payment in the PayCart Merchant Dashboard in seconds.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [dashboard, payments, operations]
---

# View transactions

If you only learn one workflow in PayCart, learn this one. Most support questions, refund requests, and accounting questions start with "what happened on payment X?" — and the answer always lives on the **Payments** page.

--8<-- "includes/notices/dummy-data.md"

## Find a payment

You can search the Payments page by any of:

- Payment ID (`pay_sample_3fE7`).
- Customer email.
- Last four digits of the card.
- Amount, with operators: `4200`, `>500`, `100..200`.
- Metadata key:value (`order_id:7421`).
- Date range, using the date picker.

The search bar combines filters with `AND`. To combine with `OR`, click **Advanced search** to the right of the search bar.

## Read a payment record

Clicking any row opens a side panel with four tabs.

### Timeline

A reverse-chronological list of every event that affected the payment. A typical succeeded payment looks like:

```text
2026-04-29 14:02:11   succeeded         Captured 4200 USD from card ending XXXX
2026-04-29 14:02:09   authorized        Visa approved
2026-04-29 14:02:08   processing        Sent to network
2026-04-29 14:02:07   created           Payment intent created
```

If anything looks unexpected, the timeline is the first place to look.

### Customer

The customer's email, saved payment methods, and a link to the full customer record. You'll use this when a customer calls in and you need to confirm the payment is theirs.

### Metadata

The free-form key/value pairs you attached when creating the payment. Useful columns to add to the table view:

- `order_id`
- `subscription_id`
- `pos_terminal`

### Receipt

A live preview of the receipt sent to the customer, including the PayCart-hosted permanent link.

## Common tasks from this page

| You want to | Click |
| --- | --- |
| Refund the whole thing | **Refund** in the top right of the side panel |
| Refund part of it | **Refund** → **Partial** |
| Issue a duplicate receipt | **Receipt** tab → **Resend** |
| Investigate a dispute | **Timeline** → the `disputed` event → **Open dispute** |
| Add or change metadata | **Metadata** tab → **Edit** |

## Export a list

The **Export** button in the top right of the table sends a CSV of the *currently filtered* rows to your email. The file includes every column whether or not the column is visible.

## Related reading

- [Navigate the dashboard](navigate.md) — the rest of the tour.
- [Process refunds](../guides/refunds.md)
- [Manage disputes](../guides/disputes.md)
- [Transaction states](../concepts/transaction-states.md) — what each `status` means.
