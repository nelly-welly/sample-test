```javascript
import { PayCart } from "@paycart/sample-sdk";

const paycart = new PayCart(process.env.PAYCART_SECRET_KEY);

const payment = await paycart.payments.create(
  {
    amount: 4200,
    currency: "USD",
    payment_method: "pm_sample_card_visa",
    description: "Order #7421 — 1x Sample Mug",
  },
  { idempotencyKey: "order-7421-attempt-1" }
);

console.log(payment.id, payment.status);
```
