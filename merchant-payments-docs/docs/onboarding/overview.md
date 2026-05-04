---
title: Onboarding overview
description: How to register a merchant on PayCart and what verification requires.
type: task
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [onboarding, kyc, compliance]
---

# Onboarding overview

This page describes how a business goes from "interested in PayCart" to "accepting live payments." If you only need to test, you don't need to onboard — sandbox accounts are free and instant. Onboarding is the gate to **live mode**.

--8<-- "includes/notices/dummy-data.md"

## At a glance

```text
sign up → submit details → verify → approve → go live
  (5m)        (15m)       (3-5 days)  (instant)  (you choose)
```

| Step | Owner | Typical time |
| --- | --- | --- |
| 1. Create the PayCart account | The business owner. | 5 minutes. |
| 2. Submit business and banking details | An admin or owner. | 15 minutes. |
| 3. Verification | PayCart's compliance team. | 3–5 business days. |
| 4. Approval and live keys issued | Automated. | Instant. |
| 5. Switch to live mode | An admin or owner. | Whenever you're ready. |

## Step 1: Create the account

Sign up at the registration link your account manager provided. The first email becomes the account *owner* — the only role that can later delete the account, so use a long-lived address (an alias or shared inbox is best).

## Step 2: Submit business and banking details

In **Dashboard → Settings → Business profile**, provide:

- **Business identity** — legal name, doing-business-as name, tax ID, formation country.
- **Industry classification** — the closest match to your *primary* business activity. Picking the wrong category slows verification.
- **Beneficial owners** — anyone who owns 25% or more of the business, with their full legal name and date of birth.
- **A representative** — one named person responsible for the account.
- **A payout bank account** — routing and account number. PayCart sends two micro-deposits to verify ownership.

For a worked example using fictional data, see [Sample merchant](../samples/sample-merchant.md).

## Step 3: Verification

PayCart's compliance team reviews everything you submitted against KYC ([know your customer](../reference/glossary.md)) and AML (anti-money-laundering) rules. The review usually takes three to five business days; high-risk industries take up to ten.

You'll get one of three outcomes:

- **Approved** — proceed to step 4.
- **More information requested** — the dashboard banner names the missing or unclear field. Resubmit and the clock resets to two business days.
- **Declined** — rare. Most declines come from sanctioned jurisdictions or prohibited industries.

## Step 4: Approval and live keys issued

When verification passes, PayCart automatically:

- Issues your live API keys in **Dashboard → Developers → API keys**.
- Enables the **Go live** toggle.
- Sends a welcome email to the account owner.

## Step 5: Switch to live mode

Flip the **Go live** toggle in the top left of the dashboard. From that moment, calls to live API endpoints move real money.

Before you flip the toggle, check off the [pre-launch checklist](#pre-launch-checklist).

## Pre-launch checklist

- [ ] At least two team members have **Owner** or **Admin** role (so you're never one missing person away from being locked out).
- [ ] At least one webhook endpoint is registered for live mode.
- [ ] Your branding (logo, statement descriptor, support contact) is set in **Settings → Branding**.
- [ ] Your payout schedule is set deliberately, not left at default.
- [ ] You've tested a full payment, refund, and dispute flow in sandbox.
- [ ] Your secrets manager holds your live keys; they aren't in source control.

--8<-- "includes/notices/rotating-keys.md"

## What can go wrong

- **The two-deposit bank check fails.** The most common cause is an account that doesn't accept ACH credits. Switch to a different account or contact your bank.
- **Compliance asks for documents you don't have.** Your local equivalents are usually accepted. Reply through the dashboard with what you have, and the team will tell you whether it suffices.
- **Your industry classification is wrong after the fact.** You can change it in settings, but if it materially changes the risk profile, verification reruns.

## Related reading

- [Quickstart](../getting-started/quickstart.md) — the developer counterpart to this page.
- [Sample merchant](../samples/sample-merchant.md) — a worked example with fictional data.
- [Glossary](../reference/glossary.md) — KYC, AML, and other terms used here.
