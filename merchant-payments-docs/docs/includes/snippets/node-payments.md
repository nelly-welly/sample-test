```javascript
import { PayCart } from "@paycart/sample-sdk";

const paycart = new PayCart(process.env.PAYCART_SECRET_KEY);

async function chargeOrder(order) {
  const payment = await paycart.payments.create(
    {
      amount: order.totalCents,
      currency: order.currency,
      payment_method: order.paymentMethodId,
      description: `Order #${order.id}`,
      metadata: { order_id: order.id, customer_id: order.customerId },
    },
    { idempotencyKey: `order-${order.id}-attempt-${order.attempt}` }
  );

  return payment;
}
```
