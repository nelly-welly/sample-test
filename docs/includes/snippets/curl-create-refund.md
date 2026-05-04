```bash
curl https://sandbox.paycart.example/v1/refunds \
  -H "Authorization: Bearer sk_test_REPLACE_ME" \
  -H "Idempotency-Key: refund-req-9f2a1c" \
  -H "Content-Type: application/json" \
  -d '{
    "payment_id": "pay_01HABCDEF12345",
    "amount": 2500,
    "currency": "USD",
    "reason": "customer_request"
  }'
```
