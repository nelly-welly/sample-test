# PayCart Merchant Docs (Sample)

This is a sample documentation site for a fictional payment platform called **PayCart**, built as a portfolio piece for a senior technical writing interview. Every name, ID, card, customer, merchant, amount, and event in this repository is fictional.

The site demonstrates:

- A purpose-driven information architecture for three audiences (developers, ops, business owners).
- Concept-first content organization (lifecycle and state model before how-to).
- Audience-routed home page with tabbed entry points.
- Strong content reuse via the MkDocs Material `pymdownx.snippets` extension.
- Conversational, action-led voice in headings and prose.

For an in-depth tour of the editorial decisions, see [`docs/about-these-docs.md`](docs/about-these-docs.md).

## Layout

```
merchant-payments-docs/
├── mkdocs.yml
├── README.md
├── LICENSE
└── docs/
    ├── index.md                       Home with audience routing
    ├── about-these-docs.md            Style, IA, and reuse notes
    ├── faq.md
    ├── release-notes.md
    │
    ├── concepts/                      Models the rest of the site assumes
    │   ├── payment-lifecycle.md
    │   ├── transaction-states.md
    │   └── money-movement.md
    │
    ├── getting-started/
    │   └── quickstart.md
    │
    ├── onboarding/
    │   └── overview.md
    │
    ├── integration/                   Recurring developer work
    │   ├── authentication.md
    │   └── webhooks.md
    │
    ├── guides/                        Recurring ops and finance work
    │   ├── refunds.md
    │   ├── reconciliation.md
    │   └── disputes.md
    │
    ├── dashboard/                     For non-developers
    │   ├── navigate.md
    │   └── view-transactions.md
    │
    ├── samples/
    │   ├── sample-merchant.md
    │   └── sample-requests.md
    │
    ├── troubleshooting/
    │   └── common-issues.md
    │
    ├── reference/
    │   ├── glossary.md
    │   ├── test-data.md
    │   └── status-codes.md
    │
    └── includes/                      Reusable fragments (not in nav)
        ├── notices/                   Admonitions reused across pages
        │   ├── sandbox-only.md
        │   ├── dummy-data.md
        │   ├── idempotency.md
        │   └── rotating-keys.md
        ├── snippets/                  Code samples reused across pages
        │   ├── curl-auth.md
        │   ├── curl-create-payment.md
        │   ├── curl-refund.md
        │   ├── node-create-payment.md
        │   ├── node-payments.md
        │   └── python-create-payment.md
        └── glossary/
            └── core-terms.md
```

## Reuse patterns

Three reuse patterns appear repeatedly across the site:

1. **Notices** in `docs/includes/notices/` are admonitions (warnings, tips, info) that need to appear on more than one page. They're inserted via `--8<-- "includes/notices/<name>.md"`.
2. **Code snippets** in `docs/includes/snippets/` are canonical request examples used by the Quickstart, Sample requests, and feature pages. Updating one file updates every reuse site.
3. **Glossary blocks** in `docs/includes/glossary/` define terms once and are included by both the full glossary and any concept page that depends on them.

For the in-depth rationale, see [`docs/about-these-docs.md`](docs/about-these-docs.md).

## Run locally

```bash
pip install mkdocs-material
cd merchant-payments-docs
mkdocs serve
```

Open `http://127.0.0.1:8000`.

## License

MIT — see [LICENSE](LICENSE). Provided for portfolio and educational purposes.
