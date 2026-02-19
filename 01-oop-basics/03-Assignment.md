# Step 1 — Abstract Base Class
```php
abstract class Notification
{
    abstract public function send(string $message): string;
}
```
# Step 2 — Concrete Implementations
```php
class EmailNotification extends Notification
{
    public function send(string $message): string
    {
        return "Email sent: $message";
    }
}

class SMSNotification extends Notification
{
    public function send(string $message): string
    {
        return "SMS sent: $message";
    }
}
```

# Step 3 — Polymorphic Usage
```php
function notifyUser(Notification $notification)
{
    return $notification->send("Order Placed");
}
```
✔ Abstraction
✔ Inheritance
✔ Polymorphism