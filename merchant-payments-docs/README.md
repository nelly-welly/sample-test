# PayCart Merchant Docs (Sample)

Sample documentation site for a fictional payment platform called **PayCart**, built as a portfolio piece for a senior technical writing interview. Every name, ID, card, customer, merchant, amount, and event in this repository is fictional.

## Read the docs

**Live site: https://nelly-welly.github.io/sample-test/**

Published from `main` via GitHub Pages — no install required to read.

## Where to find the rationale

The full **project rationale** (audience, IA, voice, reuse patterns, what's deliberately out of scope) lives in the **[top-level README](../README.md)**.

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
        ├── notices/
        ├── snippets/
        └── glossary/
```

## Contributing — preview locally

```bash
pip install mkdocs-material
cd merchant-payments-docs
mkdocs serve
```

Open `http://127.0.0.1:8000`.

## License

MIT — see [LICENSE](LICENSE). Provided for portfolio and educational purposes.
