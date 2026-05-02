---
title: Release notes
description: Recent changes to the PayCart Merchant Payments Platform.
type: release-notes
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [changelog, release-notes]
---

# Release notes

A reverse-chronological log of changes. Each entry lists what changed, who is affected, and what (if anything) you need to do.

--8<-- "includes/notices/dummy-data.md"

## 2026-04-22 — Partial captures, configurable per merchant

**Affects:** Developers and finance.

**What changed:** You can now configure each merchant under your account to allow partial captures. Previously, partial captures were on or off globally.

**What you need to do:** Nothing, unless you want to opt in. Toggle **Settings → Payments → Allow partial captures** per merchant.

## 2026-04-08 — Saved views in the Payments table

**Affects:** Dashboard users.

**What changed:** The Payments table now supports saved filter sets. Click **Save view** in the top right after applying filters; saved views appear in the left sidebar and are shared across your team.

**What you need to do:** Nothing.

## 2026-03-19 — `payment.requires_action` webhook for 3-D Secure

**Affects:** Developers using webhooks for payment status.

**What changed:** PayCart now emits `payment.requires_action` when a payment moves into the `requires_action` state. Previously you had to poll for the state change.

**What you need to do:** Subscribe to the new event in **Dashboard → Developers → Webhooks** and update your handler. Polling continues to work; the new event is additive.

## 2026-03-04 — Dispute evidence templates

**Affects:** Operations teams.

**What changed:** The dispute response form now suggests an evidence template based on the reason code. The fields most likely to win the case are highlighted.

**What you need to do:** Nothing.

## 2026-02-12 — Idempotency keys now required on writes

**Affects:** Developers.

**What changed:** All `POST` requests to `/v1/payments`, `/v1/refunds`, and `/v1/transfers` must include an `Idempotency-Key` header. Requests without one return `400 Bad Request` with code `idempotency_key_required`.

**What you need to do:** Audit your write endpoints and add a unique `Idempotency-Key` per logical operation. See [Webhooks](integration/webhooks.md) for guidance on idempotency.

!!! info "Migration grace period ended 2026-02-26"
    Live keys without an idempotency header now hard-fail. Sandbox accepted both modes through 2026-03-26 and now also enforces.

## 2026-01-30 — New Sample Merchant Dashboard launch

**Affects:** Everyone.

**What changed:** The Merchant Dashboard was redesigned around the **Home → Payments → Disputes → Payouts** flow. The legacy dashboard remains available at `/legacy` until 2026-06-30.

**What you need to do:** Nothing immediate. After 2026-06-30, only the new dashboard will be available.

## Older releases

Releases before 2026 are archived in the dashboard's *Help → Changelog* view (in a real PayCart account). This sample site does not include the archive.
