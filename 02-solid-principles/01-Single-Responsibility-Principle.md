# S — Single Responsibility Principle (SRP)
A class should have only ONE reason to change.

## ❌ Bad Example
```php
class OrderService
{
    public function createOrder() {}
    public function sendEmail() {}
    public function generateInvoicePDF() {}
    public function logActivity() {}
}
```

### Problems:

- Business logic
- Email logic
- PDF logic
- Logging logic

One change in email → modify OrderService
One change in PDF → modify OrderService

This class has 4 responsibilities.

## ✅ Correct Design
```php
class OrderService
{
    public function createOrder() {}
}

class EmailService
{
    public function send() {}
}

class InvoiceService
{
    public function generate() {}
}
```
Now each class changes for ONE reason only.

## Laravel Mapping

- Controllers → Handle HTTP
- Services → Business logic
- Jobs → Background processing
- Events → Side effects

SRP = Clean Laravel architecture.