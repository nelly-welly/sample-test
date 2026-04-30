```python
from paycart_sample import PayCart

paycart = PayCart(api_key="sk_sandbox_sample_2X9k")

payment = paycart.payments.create(
    amount=4200,
    currency="USD",
    payment_method="pm_sample_card_visa",
    description="Order #7421 — 1x Sample Mug",
    idempotency_key="order-7421-attempt-1",
)

print(payment.id, payment.status)
```
