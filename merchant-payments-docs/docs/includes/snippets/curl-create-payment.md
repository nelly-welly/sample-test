```bash
curl -X POST https://sandbox.paycart.example/v1/payments \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN" \
  -H "Idempotency-Key: order-7421-attempt-1" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 4200,
    "currency": "USD",
    "payment_method": "pm_sample_card_visa",
    "description": "Order #7421 — 1x Sample Mug"
  }'
```
