---
title: Adapter
description: Wrapping an incompatible interface so a client can use it unchanged.
tags:
  - design-patterns
  - structural
date: 2026-08-28
---

> **Placeholder content.** Stub so the `structural` folder appears in the tree. Replace before
> publishing.

An adapter converts one interface into another that a client already expects, letting two
classes work together without either knowing about the other.

## Shape

```java
public interface PaymentGateway {
  void charge(long cents);
}

public class StripeAdapter implements PaymentGateway {

  private final StripeClient stripe;

  @Override
  public void charge(long cents) {
    stripe.createCharge(cents / 100.0, "usd"); // translate the call
  }
}
```

> The value is not the wrapping — it is that the client depends on `PaymentGateway`, so
> swapping Stripe for something else touches one class.
