---
title: About this portfolio
description: Editorial decisions behind the site — why one workflow, the DITA topic mapping, what was left out, and the voice rules.
type: topic
topic_id: topic_about
audience: reviewer
status: published
last_reviewed: 2026-05-04
---

# About this portfolio

*Topic · For reviewers*

This page is the meta-doc for reviewers. It explains the editorial decisions and the structured-authoring discipline that shape the rest of the site.

## Why one workflow

A junior portfolio shows you can fill a sitemap. A senior portfolio shows you can **own a real user journey** end-to-end. The refund workflow was chosen because it crosses three distinct audiences (developer, operations, finance) inside a single procedure — which forces every senior-writer skill into a single artifact:

- Switching audience and vocabulary mid-flow without losing the thread.
- Knowing where the concept goes (Before you start), where the task goes (Steps 1–3), and where the reference goes (Reference).
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

## DITA topic typing

Every page is authored against a [DITA](https://en.wikipedia.org/wiki/Darwin_Information_Typing_Architecture) topic type. DITA's central principle — *separate structure from style; type your content* — predates Markdown by two decades and remains the gold standard for structured authoring in technical communication. This site implements it in markdown shape: the `type` frontmatter field declares the topic type, and the body sections within each page conform to that type's canonical element structure.

### The four topic types used here

| DITA topic type | Purpose | Body structure (mapped to markdown headings) |
| --------------- | ------- | -------------------------------------------- |
| `concept` | Explains *what* something is and *why* it matters. | *Short description → Description → Example → Related links* |
| `task` | Procedural — guides the reader through ordered steps. | *Short description → Prerequisites → Context → Steps → Result → Example → What to do next → Related links* |
| `reference` | Lookup — properties, syntax, examples, quickly scannable. | *Short description → Synopsis → Properties → Example* (per entity) |
| `troubleshooting` | Diagnostic — symptom-driven, decision-tree shaped. | *Short description → For each symptom: Condition → Cause → Remedy* |
| `topic` | Generic; used only for the landing and this About page where strict structure would be artificial. | No required body structure. |

Authoring against fixed body structures has measurable effects: every task page covers the same set of considerations (prereqs, context, result, postreq); every troubleshooting symptom forces *cause* to be written before *remedy*; every reference entity has the same scannable shape. A reader who learns the structure once can navigate any page on the site at the same speed as the previous page.

## Metadata schema

Every page carries YAML frontmatter that classifies the content. The schema is small and uses DITA-aligned vocabulary so a real CMS or static-site stack can index from these fields without reading the body.

| Field | Required | Values |
| ----- | -------- | ------ |
| `title` | yes | Display title. |
| `description` | yes | One-sentence summary used for SEO and link previews. |
| `type` | yes | DITA topic type: `concept`, `task`, `reference`, `troubleshooting`, or `topic`. |
| `topic_id` | yes | Stable, type-prefixed identifier: `c_…` (concept), `t_…` (task), `r_…` (reference), `ts_…` (troubleshooting), `topic_…` (generic). DITA topics require a unique ID; this is its markdown-shape equivalent. |
| `audience` | yes | `developer`, `operations`, `finance`, `all`, `reviewer`. The primary reader. Maps to DITA `audience` on `prolog`/`metadata`. |
| `level` | when meaningful | `foundational`, `intermediate`, `advanced`. Reader-expertise level. Omitted on pure reference and on the landing page. |
| `sequence` | for workflow pages | Integer 1–7 defining the canonical reading order across the workflow. |
| `estimated_time` | when meaningful | Human-readable, e.g. `~5 minutes` or `~1 minute on the wire`. |
| `status` | yes | `draft`, `review`, `published`. |
| `last_reviewed` | yes | ISO date (`YYYY-MM-DD`). |

The italicised line directly under each H1 surfaces `type · audience · level · sequence · time` so a reader does not have to open the source to see them.

### Reading order at a glance

| # | Page | Topic type | `topic_id` | Audience | Level |
| - | ---- | ---------- | ---------- | -------- | ----- |
| 1 | [Refund workflow overview](refund-workflow/index.md) | concept | `c_workflow-overview` | all | foundational |
| 2 | [Before you start](refund-workflow/before-you-begin.md) | concept | `c_before-you-begin` | all | foundational |
| 3 | [Step 1 — Create the refund](refund-workflow/create-refund.md) | task | `t_create-refund` | developer | intermediate |
| 4 | [Step 2 — Confirm in the dashboard](refund-workflow/confirm-in-the-dashboard.md) | task | `t_confirm-refund` | operations | foundational |
| 5 | [Step 3 — Reconcile the ledger](refund-workflow/reconcile-ledger.md) | task | `t_reconcile-refund` | finance | intermediate |
| 6 | [Troubleshoot](refund-workflow/troubleshoot.md) | troubleshooting | `ts_refund-failures` | all | advanced |
| 7 | [Reference](refund-workflow/reference.md) | reference | `r_refund-api` | developer | — |

## Voice and style rules

Every page on the site follows the same rules. They're listed here so a reviewer can audit them.

- **Address the reader as "you."** PayCart is the system; the reader is the protagonist.
- **Lead with the outcome.** Every task page begins with a *Short description* element.
- **Verbs in headings on the page title and step commands.** *Create the refund*, not *Refunds*. *Send the request*, not *Request*.
- **Explain *why* before *how*** when behavior is non-obvious (idempotency keys, T+1 export delay, separate refund IDs). When it's obvious, skip the *why*.
- **No marketing language.** No "robust," "world-class," "seamless." Replace with the concrete capability the word was hiding.
- **Time travels.** Relative dates ("last quarter") are converted to absolute dates so pages age well.
- **No filler examples.** Every code sample, error code, and dashboard column is one that the workflow actually exercises.

## Reuse patterns (DITA `conref` in markdown)

DITA's content-reference (`conref`) mechanism — write once, transclude everywhere — is implemented here via the [`pymdownx.snippets`](https://facelessuser.github.io/pymdown-extensions/extensions/snippets/) extension. Reusable fragments live in `docs/includes/`:

| Fragment | Purpose | Used on |
| -------- | ------- | ------- |
| `notices/sandbox-only.md` | Reminds the reader they are in sandbox, not production. | Before you start, Step 1 |
| `notices/idempotency.md` | The idempotency-key warning callout. | Before you start, Troubleshoot |
| `notices/dummy-data.md` | Reminds the reader every ID and amount is fictional. | Step 2, Step 3 |
| `snippets/curl-create-refund.md`, `node-create-refund.md`, `python-create-refund.md` | The canonical "create a refund" code sample, in three languages. | Step 1 |

The include syntax is `--8<-- "includes/notices/sandbox-only.md"`. Updating the fragment file once changes every page that includes it.

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
