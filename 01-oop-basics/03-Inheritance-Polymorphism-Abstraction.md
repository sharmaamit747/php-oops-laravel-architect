# LESSON 3 — Inheritance, Polymorphism and Abstraction

## Inheritance
Inheritance = Reusing behavior from parent class.

But in production:

❗ Inheritance should represent "is-a" relationship, not code reuse shortcut.

### ❌ Wrong Usage
```php
class DatabaseLogger
{
    public function log($msg) {}
}

class EmailService extends DatabaseLogger
{
    public function send() {}
}
```
❌ EmailService is NOT a DatabaseLogger.
This is abuse of inheritance.

### ✅ Correct Example — Real “is-a” Relationship
```php
abstract class Employee
{
    protected string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }
    
    public function getName(): string
    {
        return $this->name;
    }

    abstract public function calculateSalary(): float; 
}

class FullTimeEmployee extends Employee
{
    public function calculateSalary():float
    {
        return 50000;
    }
}

class ContractEmployee extends Employee
{
    public function calculateSalary(): float
    {
        return 20000;
    }
}

$money = new FullTimeEmployee('Amit');
echo $money->getName().'Earn '.$money->calculateSalary();
$money1 = new ContractEmployee('Rajesh');
echo $money1->getName().'Earn '.$money1->calculateSalary();
```
✔ Shared structure
✔ Different behavior
✔ Proper inheritance

### Use inheritance only when:

- There is real hierarchy
- Behavior must be shared
- Parent defines contract
- Avoid deep inheritance chains.

## Polymorphism

Polymorphism = Same interface, different behavior.

This is how Laravel supports multiple drivers (cache, queue, mail).


### Example — Payment System
```php
interface PaymentGateway
{
    public function pay(float $amount): string;
}
```
### Multiple Implementations
```php
class StripePayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Stripe paid ₹{$amount}";
    }
}

class RazorpayPayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Razorpay paid ₹{$amount}";
    }
}
```

### Usage (Polymorphic Behavior)
```php
function processPayment(PaymentGateway $gateway)
{
    return $gateway->pay(1000);
}
```
Same method → Different behavior.
#### Examples:

- Cache drivers (Redis, File, Database)
- Mail drivers (SMTP, SES)
- Queue drivers
- Notification channels

Polymorphism enables extension without modifying core.

## Abstraction
Abstraction = Hide implementation details, expose only essentials.

Two ways in PHP:

- Abstract classes
- Interfaces

### Abstract Class Example
```php
abstract class Report
{
    protected string $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    abstract public function generate(): string;
}
```
### Concrete Implementation
```php
class PDFReport extends Report
{
    public function generate(): string
    {
        return "Generating PDF for {$this->title}";
    }
}
```

### Interface Example
```php
interface Logger
{
    public function log(string $message): void;
}
```
| Abstract Class                  | Interface                          |
|----------------------------------|------------------------------------|
| Can have properties              | Cannot have properties             |
| Can have implemented methods     | No method implementation (only signatures) |
| Supports single inheritance      | Multiple interfaces can be implemented |

### Use
#### Use Abstract Class when:

- You need shared base logic

#### Use Interface when:

- You need contract only
- You want flexibility

Laravel uses both.