# O — Open/Closed Principle (OCP)
Open for extension, closed for modification.
Meaning:
You should add new behavior without modifying existing code.

## ❌ Bad Example
```php
class PaymentService
{
    public function pay(string $type)
    {
        if ($type === 'stripe') {}
        if ($type === 'razorpay') {}
    }
}
```
Every new gateway → modify class.

Breaks OCP.

## ✅ Correct Polymorphic Design
```php
interface PaymentGateway
{
    public function pay(float $amount): string;
}
```
Add new gateway:
```php
class PayPalPayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Paid via PayPal";
    }
}
```
No modification. Only extension.

✔ OCP satisfied.

## Laravel Mapping
Laravel drivers:

- Cache
- Queue
- Mail
- Notification

All follow OCP.