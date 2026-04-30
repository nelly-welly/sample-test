!!! tip "Use an idempotency key"
    Always send a unique `Idempotency-Key` header on write requests. PayCart safely retries calls with the same key for up to 24 hours, so a network glitch never produces a duplicate charge or refund.
