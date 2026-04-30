**Authorization**
:   A check that a card or wallet has enough funds and is willing to be charged. An authorization holds funds but does not move them.

**Capture**
:   The step that converts a successful authorization into a settled charge. Captures can be made for the full or partial authorized amount.

**Settlement**
:   The transfer of captured funds from the issuing bank to PayCart, and from PayCart to the merchant's payout account, typically two business days later.

**Idempotency key**
:   A client-generated unique string sent on write requests so that retries do not produce duplicates.

**Webhook**
:   An HTTPS callback that PayCart sends to a URL you control whenever an event (such as `payment.succeeded`) occurs in your account.
