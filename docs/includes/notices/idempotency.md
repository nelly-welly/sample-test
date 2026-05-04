!!! warning "Idempotency keys are required for refunds"
    `POST /v1/refunds` requires an `Idempotency-Key` header. PayCart caches the response under that key for 24 hours: retries with the same key return the original response — guaranteed. Retries with a *different* key create a **second refund**, which moves money you cannot reclaim without contacting the customer.
