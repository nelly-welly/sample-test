# sample-test

Sample documentation portfolio — a fictional payments platform called **PayCart**, built as a senior technical writing interview piece. Every name, ID, card, customer, merchant, amount, and event in this repository is fictional.

## Read the docs

**Live site: https://nelly-welly.github.io/sample-test/**

The site is published from `main` via GitHub Pages, so you don't need to install anything to read it. The rest of this README is the **rationale** behind how the site is built — what decisions were made and why.

The active project lives in [`merchant-payments-docs/`](merchant-payments-docs/).

## Why this project exists

This repository is a portfolio piece. Its purpose is to demonstrate, on a non-trivial subject (payments), the editorial and IA decisions a senior technical writer makes when starting a new docs site from scratch:

- A purpose-driven information architecture for three distinct audiences.
- Concept-first content organization (lifecycle and state model before how-to).
- An audience-routed home page with tabbed entry points.
- Strong content reuse via the MkDocs Material `pymdownx.snippets` extension.
- Conversational, action-led voice in headings and prose.

A real PayCart docs site would also ship an OpenAPI reference, per-language SDK changelogs, a status page, a billing/pricing reference, regional compliance addenda, and localized versions. Those are deliberately **out of scope** for a portfolio piece — the goal is to show editorial judgment, not exhaustive coverage.

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

Every page follows the same rules:

- **Address the reader directly** ("you"). PayCart is the system; the reader is the protagonist.
- **Lead with the outcome.** Every section's first sentence tells the reader what they'll get from reading it.
- **Use verbs in headings.** *Process refunds*, not *Refunds*. *View transactions*, not *Transactions*.
- **Explain the why before the how** when behavior is non-obvious (idempotency, signature verification, capture vs authorize). When it's obvious, skip it.
- **Avoid marketing language.** No "robust," "world-class," "seamless," or "powerful." Replace with the concrete capability the word was hiding.
- **Time travels.** Convert relative dates ("last quarter," "Thursday") to absolute dates so the page ages well.

## Reuse patterns

The site is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and the `pymdownx.snippets` extension. Three reuse patterns appear repeatedly across the site.

### 1. Reusable notices

Frequently shown admonitions live in [`docs/includes/notices/`](merchant-payments-docs/docs/includes/notices/). One page, included from many. Examples:

- `includes/notices/sandbox-only.md` — used on *Webhooks* and *Test data*.
- `includes/notices/dummy-data.md` — used on the home page, every concept page, and every reference page.
- `includes/notices/idempotency.md` — used on *Transaction states* and *Webhooks*.

The include syntax is `--8<-- "includes/notices/sandbox-only.md"`. Updating the file once changes every page that reuses it.

### 2. Reusable code snippets

Code samples live in [`docs/includes/snippets/`](merchant-payments-docs/docs/includes/snippets/) so the same canonical example can appear in *Quickstart*, *Sample requests*, and *Authentication* without drifting out of sync. When the API changes, one file updates and every reuse site stays correct.

### 3. Reusable glossary blocks

Definition lists are kept in [`docs/includes/glossary/`](merchant-payments-docs/docs/includes/glossary/). The full Glossary page includes the canonical block; concept pages can include the same block when the terms are central to the page.

## Linking conventions

- **Forward links go to the next thing the reader will need.** Every page ends with a *Related reading* section.
- **Definitions link to the glossary** the first time they appear in any non-glossary page; subsequent uses on the same page do not.
- **External links are only used for resources the reader genuinely needs to leave the site for** (npmjs, nodejs.org). No "click here for our blog post."

## Layout

```
merchant-payments-docs/
├── mkdocs.yml
├── README.md
├── LICENSE
└── docs/
    ├── index.md                       Home with audience routing
    ├── faq.md
    ├── release-notes.md
    │
    ├── concepts/                      Models the rest of the site assumes
    ├── getting-started/
    ├── onboarding/
    ├── integration/                   Recurring developer work
    ├── guides/                        Recurring ops and finance work
    ├── dashboard/                     For non-developers
    ├── samples/
    ├── troubleshooting/
    ├── reference/
    └── includes/                      Reusable fragments (not in nav)
        ├── notices/
        ├── snippets/
        └── glossary/
```

## Contributing — preview locally

You don't need to run anything locally to read the docs (use the [live site](https://nelly-welly.github.io/sample-test/)). To preview edits before pushing:

```bash
pip install mkdocs-material
cd merchant-payments-docs
mkdocs serve
```

Then open `http://127.0.0.1:8000`. The site auto-deploys from `main` once your PR is merged.

## License

MIT — see [LICENSE](merchant-payments-docs/LICENSE). Provided for portfolio and educational purposes.
