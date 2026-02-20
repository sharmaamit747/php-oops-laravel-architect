# D — Dependency Inversion Principle (DIP)
Depend on abstractions, not concrete classes.

This is the MOST IMPORTANT principle in Laravel.

## ❌ Bad Example
```php
class OrderService
{
    private StripePayment $payment;

    public function __construct()
    {
        $this->payment = new StripePayment();
    }
}
```
Tightly coupled.

## ✅ Correct Example
```php
class OrderService
{
    private PaymentGateway $payment;

    public function __construct(PaymentGateway $payment)
    {
        $this->payment = $payment;
    }
}
```
Now we depend on abstraction.

## Laravel Container Binding

```php
app()->bind(PaymentGateway::class, StripePayment::class);
```

Laravel resolves automatically.

This is DIP implemented at framework level.