---
title: About these docs
description: Information architecture, voice, and reuse patterns used in this sample documentation site.
type: reference
author: Sample Author
draft: false
created: 2026-04-30
updated: 2026-04-30
tags: [meta, style, information-architecture]
---

# About these docs

This page is a sample / portfolio note about *how* this site is built. In a real product, this content would live in a contributor handbook or in-repo `STYLE.md` file. It's exposed here so a reviewer can see the shape of the editorial decisions.

--8<-- "includes/notices/dummy-data.md"

## Audience

The site assumes three primary audiences. Most pages call out which audience they're written for in the first paragraph or the heading.

| Audience | What they want | Where they live in the IA |
| --- | --- | --- |
| **Developers** | A working integration as fast as possible. | *Concepts*, *Getting started*, *Integration*, *Reference*. |
| **Operations and finance** | A reliable answer to a customer- or accounting-driven question. | *Guides*, *Dashboard*, *Troubleshooting*, *FAQ*. |
| **Business owners** | Confidence the platform can handle their volume and compliance posture. | *Home*, *Onboarding*, *Release notes*. |

The home page routes each audience explicitly with a tabbed *Pick a path* block, rather than asking everyone to read the full TOC.

## Information architecture

The nav is organized by **purpose**, not by product surface. That's why there is no top-level *API* section: API content is woven into the integration and reference sections where the reader actually needs it.

The order of the top-level groups follows a reader's natural progression:

1. **Concepts** — the model the rest of the docs assume.
2. **Getting started / Onboarding** — first-time setup.
3. **Integration** — recurring developer work.
4. **Guides** — recurring ops work.
5. **Dashboard** — recurring non-developer work.
6. **Samples** — copy-pasteable examples.
7. **Troubleshooting / FAQ** — exception handling.
8. **Release notes** — what changed.
9. **Reference** — lookup tables.
10. **About these docs** — meta.

## Voice and style

A few rules every page follows:

- **Address the reader directly** ("you"). PayCart is the system; the reader is the protagonist.
- **Lead with the outcome.** Every section's first sentence tells the reader what they'll get from reading it.
- **Use verbs in headings.** *Process refunds*, not *Refunds*. *View transactions*, not *Transactions*.
- **Explain the why before the how** when behavior is non-obvious (idempotency, signature verification, capture vs authorize). When it's obvious, skip it.
- **Avoid marketing language.** No "robust," "world-class," "seamless," or "powerful." Replace with the concrete capability the word was hiding.
- **Time travels.** Convert relative dates ("last quarter," "Thursday") to absolute dates so the page ages well.

## Reuse patterns

The site is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and the `pymdownx.snippets` extension. Three reuse patterns appear repeatedly.

### 1. Reusable notices

Frequently shown admonitions live in [`includes/notices/`](https://example.com/sample-org/paycart-docs). One page, included from many. Examples:

- `includes/notices/sandbox-only.md` — used on *Webhooks* and *Test data*.
- `includes/notices/dummy-data.md` — used on the home page, every concept page, and every reference page.
- `includes/notices/idempotency.md` — used on *Transaction states* and *Webhooks*.

The include syntax is `--8<-- "includes/notices/sandbox-only.md"`. Updating the file once changes every page that reuses it.

### 2. Reusable code snippets

Code samples live in [`includes/snippets/`](https://example.com/sample-org/paycart-docs) so the same canonical example can appear in *Quickstart*, *Sample requests*, and *Authentication* without drifting out of sync. When the API changes, one file updates and every reuse site stays correct.

### 3. Reusable glossary blocks

Definition lists are kept in `includes/glossary/`. The full [Glossary](reference/glossary.md) page includes the canonical block; concept pages can include the same block when the terms are central to the page.

## Linking conventions

- **Forward links go to the next thing the reader will need.** Every page ends with a *Related reading* section.
- **Definitions link to the glossary** the first time they appear in any non-glossary page; subsequent uses on the same page do not.
- **External links are only used for resources the reader genuinely needs to leave the site for** (npmjs, nodejs.org). No "click here for our blog post."

## Content not on this site

A real PayCart docs site would also include: an OpenAPI reference, SDKs and changelogs per language, a status page, a billing/pricing reference, regional compliance addenda, and a localized version per supported language. They are deliberately out of scope for this sample.

## Building and previewing

```bash
pip install mkdocs-material
mkdocs serve
```

Then open `http://127.0.0.1:8000`.
