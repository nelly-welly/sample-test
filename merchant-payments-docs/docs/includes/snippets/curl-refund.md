```bash
curl -X POST https://sandbox.paycart.example/v1/refunds \
  -H "Authorization: Bearer SAMPLE_ACCESS_TOKEN" \
  -H "Idempotency-Key: refund-7421-attempt-1" \
  -H "Content-Type: application/json" \
  -d '{
    "payment_id": "pay_sample_3fE7",
    "amount": 4200,
    "reason": "customer_requested"
  }'
```
