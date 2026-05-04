```javascript
import PayCart from "@paycart/node";

const paycart = new PayCart(process.env.PAYCART_KEY);

const refund = await paycart.refunds.create(
  {
    payment_id: "pay_01HABCDEF12345",
    amount: 2500,
    currency: "USD",
    reason: "customer_request",
  },
  { idempotencyKey: "refund-req-9f2a1c" },
);

console.log(refund.id, refund.status);
```
