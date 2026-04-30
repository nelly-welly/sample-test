---
title: FAQ
description: Frequently asked questions about the PayCart Merchant Payments Platform.
---

# FAQ

A list of the questions support gets every week. If your question isn't here, the answer probably lives in [Troubleshooting](troubleshooting/common-issues.md).

## Accounts and onboarding

??? question "How long does merchant verification take?"
    Three to five business days for most merchants. High-risk industries (gambling, regulated cannabis, certain cross-border categories) can take up to ten business days because of additional review.

??? question "Can I test PayCart before being verified?"
    Yes. Sandbox accounts are created instantly and use fake money end-to-end. You only need verification before flipping the **Go live** switch.

??? question "Do I need a separate sandbox login?"
    No. Switch environments using the toggle in the top left of the dashboard. The same email signs into both.

??? question "Can I have more than one merchant account on the same PayCart login?"
    Yes. Owners can create additional merchants from **Settings → Account → Add merchant**. Use this for multi-brand or multi-region setups.

## Payments

??? question "Why is my payment in `requires_action`?"
    The customer needs to complete 3-D Secure. Redirect them to `next_action.redirect_to_url` and wait for `payment.succeeded` (or `payment.failed`) on your webhook.

??? question "Can I capture an authorization for less than the original amount?"
    Yes, as long as partial captures are enabled for your merchant. Pass the desired amount in the capture call. The remaining authorization is automatically released.

??? question "What's the maximum authorization window?"
    Seven days for most card networks. American Express allows 30 days for travel and lodging merchant categories. After the window closes, you must re-authorize before capturing.

??? question "How do test cards differ from real cards?"
    Test cards never reach the card network — PayCart simulates the response based on the card number. See [Test data](reference/test-data.md) for the list.

## Refunds and disputes

??? question "How long do refunds take to land on the customer's statement?"
    Five to ten business days, depending on the issuing bank. The PayCart side completes immediately; the delay is the issuer's posting cycle.

??? question "Can I refund more than the original amount?"
    No. Total refunds across a payment are capped at the captured amount. To send the customer extra money, create a new payment with a negative description, or use bank transfer outside PayCart.

??? question "Can I refund a disputed payment?"
    Yes, but it counts as accepting the dispute. Use this when you'd rather close the case quickly than collect evidence.

??? question "What's the difference between a void and a refund?"
    A void cancels an *authorization* before capture — no money ever moves. A refund returns money after a *capture*. The customer-facing experience is similar; the financial paperwork is not.

## Webhooks and integration

??? question "What if my webhook endpoint is down?"
    PayCart retries with exponential backoff for 72 hours. After that the event is marked `delivery_failed` but remains visible in **Dashboard → Developers → Webhooks → Recent deliveries** for 30 days for manual replay.

??? question "How do I rotate my webhook signing secret?"
    Click **Roll secret** in **Dashboard → Developers → Webhooks**. PayCart sends events with both the old and new secret for 24 hours, giving you a window to deploy.

??? question "Are webhooks delivered in order?"
    No. Build for out-of-order delivery: re-fetch the object via the API before acting, especially for actions that move money.

## Money and payouts

??? question "When do I get paid?"
    By default, daily, two business days after capture. You can change this in **Settings → Payouts**.

??? question "Can I pause payouts?"
    Yes. Toggle **Manual payouts** in **Settings → Payouts**. Funds accumulate in your PayCart balance until you trigger a payout manually.

??? question "Why is my payout less than my captured volume?"
    Subtract fees, refunds, disputes, and any holds. The math is in [Money movement](concepts/money-movement.md#fees).

## Compliance

??? question "Is PayCart PCI compliant?"
    These docs are sample / fictional content. A real payments platform would be PCI DSS Level 1 compliant and would link to its current attestation here.

??? question "Where does PayCart store cardholder data?"
    In a sample/fictional context, "in a tokenized vault that you never see." Real implementations would name the vault provider, the regions, and the data residency guarantees.

## Related reading

- [Troubleshooting](troubleshooting/common-issues.md)
- [Glossary](reference/glossary.md)
