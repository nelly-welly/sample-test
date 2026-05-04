```python
import paycart

paycart.api_key = "sk_test_REPLACE_ME"

refund = paycart.Refund.create(
    payment_id="pay_01HABCDEF12345",
    amount=2500,
    currency="USD",
    reason="customer_request",
    idempotency_key="refund-req-9f2a1c",
)

print(refund.id, refund.status)
```
