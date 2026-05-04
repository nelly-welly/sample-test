---
title: About this portfolio
description: Editorial decisions behind the site — why one workflow, what was left out, and the voice rules.
type: meta
audience: reviewer
status: published
last_reviewed: 2026-05-04
---

# About this portfolio

*Meta · For reviewers*

This page is the meta-doc for reviewers. It explains the editorial decisions that shape the rest of the site.

## Why one workflow

A junior portfolio shows you can fill a sitemap. A senior portfolio shows you can **own a real user journey** end-to-end. The refund workflow was chosen because it crosses three distinct audiences (developer, operations, finance) inside a single procedure — which forces every senior-writer skill into a single artifact:

- Switching audience and vocabulary mid-flow without losing the thread.
- Knowing where the concept goes (Before you start), where the procedure goes (Steps 1–3), and where the reference goes (Reference).
- Restraint: the Reference page lists only the four endpoints/events used in the workflow, not the whole API.
- Linking forward, not backward — every page ends with the next step the reader will need.

A breadth-first sample (a docs site with twenty pages, three sentences each) hides whether the writer can sustain depth. This site is depth-first on purpose.

## What was deliberately left out

A real PayCart docs site would include all of the following. They're absent here so the workflow stays the focal point.

- A full API reference (auth, payments, customers, payouts, disputes, webhooks).
- SDK pages per language with installation, configuration, retries, and changelogs.
- Onboarding and account setup.
- Pricing, fees, and settlement timing reference.
- A status page and an incident playbook.
- Localization and accessibility audits.
- A glossary beyond the few inline definitions.

Including them would have made the site look more complete but read less sharp. A reviewer judging editorial judgment values restraint over completeness.

## Voice and style rules

Every page on the site follows the same rules. They're listed here so a reviewer can audit them.

- **Address the reader as "you."** PayCart is the system; the reader is the protagonist.
- **Lead with the outcome.** Every step page begins with a "You are / Goal / Time" block.
- **Verbs in headings.** *Create the refund*, not *Refunds*. *Confirm in the dashboard*, not *Dashboard*.
- **Explain *why* before *how*** when behavior is non-obvious (idempotency keys, T+1 export delay, separate refund IDs). When it's obvious, skip the *why*.
- **No marketing language.** No "robust," "world-class," "seamless." Replace with the concrete capability the word was hiding.
- **Time travels.** Relative dates ("last quarter") are converted to absolute dates so pages age well.
- **No filler examples.** Every code sample, error code, and dashboard column is one that the workflow actually exercises.

## Reuse patterns

Reusable fragments live in `docs/includes/`:

| Fragment | Purpose | Used on |
| -------- | ------- | ------- |
| `notices/sandbox-only.md` | Reminds the reader they are in sandbox, not production. | Before you start, Step 1 |
| `notices/idempotency.md` | The idempotency-key warning callout. | Before you start, Troubleshoot |
| `notices/dummy-data.md` | Reminds the reader every ID and amount is fictional. | Step 2, Step 3 |
| `snippets/curl-create-refund.md`, `node-create-refund.md`, `python-create-refund.md` | The canonical "create a refund" code sample, in three languages. | Step 1 |

The include syntax is `--8<-- "includes/notices/sandbox-only.md"`. Updating the fragment file once changes every page that includes it. This is the [`pymdownx.snippets`](https://facelessuser.github.io/pymdown-extensions/extensions/snippets/) extension.

## Metadata schema

Every page on this site carries YAML frontmatter that classifies the content. The schema is small but consistent — a real CMS or static-site stack can index from these fields without parsing the body.

| Field | Required | Values |
| ----- | -------- | ------ |
| `title` | yes | Display title. |
| `description` | yes | One-sentence summary used for SEO and link previews. |
| `type` | yes | `landing`, `concept`, `procedure`, `reference`, `troubleshooting`, `meta`. From the DITA topic-typing tradition — what the page does for the reader. |
| `audience` | yes | `developer`, `operations`, `finance`, `all`, `reviewer`. The primary reader. |
| `level` | when meaningful | `foundational`, `intermediate`, `advanced`. Reader-expertise level. Omitted on pure reference and on the landing page. |
| `sequence` | for workflow pages | Integer 1–7 defining the canonical reading order across the workflow. |
| `estimated_time` | when meaningful | Human-readable, e.g. `~5 minutes` or `~1 minute on the wire`. |
| `status` | yes | `draft`, `review`, `published`. |
| `last_reviewed` | yes | ISO date (`YYYY-MM-DD`). |

The italicised line directly under each H1 surfaces `type · audience · level · sequence · time` so a reader does not have to open the source to see them.

### Reading order at a glance

| # | Page | Type | Audience | Level |
| - | ---- | ---- | -------- | ----- |
| 1 | [Refund workflow overview](refund-workflow/index.md) | concept | all | foundational |
| 2 | [Before you start](refund-workflow/before-you-start.md) | concept | all | foundational |
| 3 | [Step 1 — Create the refund](refund-workflow/1-create.md) | procedure | developer | intermediate |
| 4 | [Step 2 — Confirm in the dashboard](refund-workflow/2-confirm.md) | procedure | operations | foundational |
| 5 | [Step 3 — Reconcile the ledger](refund-workflow/3-reconcile.md) | procedure | finance | intermediate |
| 6 | [Troubleshoot](refund-workflow/troubleshoot.md) | troubleshooting | all | advanced |
| 7 | [Reference](refund-workflow/reference.md) | reference | developer | — |

## How this site is built

Built with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme. Source lives in `docs/`. Published automatically to GitHub Pages on every push to `main` via the workflow at `.github/workflows/docs.yml`.

To preview a contribution locally:

```bash
pip install mkdocs-material
mkdocs serve
```

Then open `http://127.0.0.1:8000/`.

## License

Sample content provided for portfolio and educational purposes. PayCart and every name, ID, card number, customer, merchant, amount, and event in this site are fictional.
