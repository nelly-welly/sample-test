---
title: Manage disputes
description: How to respond to chargebacks in PayCart.
---

# Manage disputes

A *dispute* (or *chargeback*) is a customer telling their bank they don't recognize, didn't authorize, or didn't receive what they paid for. The bank pulls the money from your account and asks you to either accept the loss or push back with evidence.

Disputes have hard deadlines. Miss the deadline and you lose by default — even if your evidence would have won.

--8<-- "includes/notices/dummy-data.md"

## When you'll hear about a dispute

PayCart fires a `dispute.created` webhook the moment the issuer raises the chargeback, and the dispute appears in **Dashboard → Disputes** within seconds. The funds for the disputed amount are pulled from your next payout.

## The response window

| Card scheme | Response window |
| --- | --- |
| Visa | 30 calendar days |
| Mastercard | 45 calendar days |
| American Express | 20 calendar days |

The clock starts on the dispute's `created_at` date, not on the date you read about it. Build alerts that fire at least seven days before the deadline.

## Respond to a dispute

1. Open **Dashboard → Disputes** and click the disputed payment.
2. Read the **Reason** field. Each scheme uses its own reason codes; they're translated to plain English in the dashboard. For the raw codes, see [Status codes](../reference/status-codes.md#dispute-reasons).
3. Choose a path:

    - **Accept the dispute.** You agree the customer is owed the money. PayCart closes the case and the loss is final. Use this when the customer is correct, when the disputed amount is below your evidence-gathering cost, or when you've already refunded.
    - **Submit evidence.** Click **Respond** and attach the artifacts that prove the customer authorized and received what they paid for.

4. Click **Submit**. After submission you cannot edit the response, so review carefully.

## What evidence helps

The evidence you can submit varies by reason code, but these four are useful in almost every case:

- **Receipt** — the email or PDF the customer received at the time of purchase.
- **Proof of delivery** — a tracking number with a delivery confirmation.
- **Customer communication** — chat or email logs showing the customer acknowledged the order.
- **Service usage logs** — for digital goods, the IP address, device, and timestamps of the customer using the product.

The dashboard's evidence form lists the specific fields most likely to win the assigned reason code.

## Outcomes

| Outcome | What happens |
| --- | --- |
| **Won** | The funds return to your next payout. PayCart marks the dispute `won`. |
| **Lost** | The funds remain with the customer. PayCart marks the dispute `lost`. |
| **Accepted** | You explicitly accepted; same financial outcome as `lost`. |

Issuers' decisions on disputes can take 60 to 75 days after you submit evidence. Don't expect a same-week resolution.

## Reduce the rate at which you get disputes

A few changes reduce dispute volume more than evidence quality ever will:

- **Make your descriptor recognizable.** The line on the customer's bank statement should clearly say your brand and ideally a short order reference. Configure this in **Settings → Branding**.
- **Send receipts immediately.** Customers who got a receipt rarely file "I don't recognize this" disputes.
- **Refund quickly when asked.** A direct refund avoids a chargeback and the associated fee.
- **Use 3-D Secure for risky transactions.** When a payment goes through 3-D Secure and is later disputed for fraud, liability shifts to the issuer.

## Related reading

- [Process refunds](refunds.md)
- [Transaction states](../concepts/transaction-states.md) — what `disputed` means in the payment lifecycle.
- [Troubleshooting](../troubleshooting/common-issues.md)
