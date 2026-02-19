# PHP OOP & Laravel Architecture — Deep Mastery Guide

---

# 🧠 PHASE 1 — OOP Fundamentals (Very Deep)

---

## 1️⃣ OOP Basics

### Topics Covered

- Class & Object  
- Properties & Methods  
- Access Modifiers  
- Constructor & Destructor  
- $this keyword  
- Static properties & methods  

---

## Class & Object

A class is a blueprint.  
An object is an instance of that class.

```php
class User
{
    private string $name;
    protected string $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function getEmail(): string
    {
        return $this->email;
    }
}
```

### Key Concepts

- private → accessible only inside class
- protected → accessible inside class + child classes
- public → accessible everywhere
- $this → refers to current object instance

### Static Properties & Methods
Belong to class, not object.

```php
class Counter {
    public static int $count = 0;

    public static function increment() {
        self::$count++;
    }
}
```
### Laravel Mapping

| OOP Concept     | Laravel Example     |
|-----------------|--------------------|
| Class           | Eloquent Models    |
| Methods         | Controllers        |
| Encapsulation   | Form Requests      |
| Service Layer   | Service Classes    |


## Encapsulation (Real-World Use)
Encapsulation = Hide internal state & control access.

### Private vs Protected

- private → strict protection
- protected → extendable protection

### Getter & Setter with Validation
```php
class User {
    private string $email;

    public function setEmail(string $email): void {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException("Invalid Email");
        }

        $this->email = $email;
    }

    public function getEmail(): string {
        return $this->email;
    }
}
```

### Laravel Example
```php
protected $fillable = ['name', 'email'];
```
Mass assignment protection works because attributes are protected internally.
You explicitly control what can be set.

## Inheritance
Allows one class to reuse another.
```php
class Person {
    public function speak() {
        return "Hello";
    }
}

class Student extends Person {
    public function study() {
        return "Studying";
    }
}
```

### Laravel Mapping

- BaseController
- BaseRepository
- Custom Abstract Classes

## Polymorphism
Same method → Different behavior.

### Interface Example
```php
interface PaymentGateway {
    public function pay(float $amount);
}

class StripePayment implements PaymentGateway {
    public function pay(float $amount) {
        return "Paid via Stripe";
    }
}
```
### Laravel Mapping

- Notification Channels
- Payment Drivers
- Cache Drivers
- Mail Drivers

## Abstraction
Hide implementation details.
```php
abstract class Report {
    abstract public function generate();
}
```
Child classes must implement generate().

### Laravel Uses Abstraction In
- Service Providers
- Contracts
- Abstract Classes

🧠 PHASE 2 — Advanced OOP (Architect Level)

## SOLID Principles

S — Single Responsibility
One class → One reason to change.

O — Open Closed
Extend behavior without modifying existing code.

L — Liskov Substitution
Child class must replace parent safely.

I — Interface Segregation
Use small, focused interfaces.

D — Dependency Inversion
Depend on abstractions, not concrete classes.

### Laravel Example
```php
class OrderController
{
    public function __construct(PaymentGateway $gateway)
    {
        $this->gateway = $gateway;
    }
}
```
This demonstrates Dependency Injection + Dependency Inversion Principle.

## Dependency Injection

### Constructor Injection
```php
public function __construct(PaymentGateway $gateway)
```

### Method Injection
```php
public function store(Request $request)
```

### Service Container Binding
```php
app()->bind(PaymentGateway::class, StripePayment::class);
```

## Traits
Traits allow reusable method sharing.
```php
trait Logger {
    public function log($msg) {
        echo $msg;
    }
}
```

### Laravel Traits
- SoftDeletes
- HasFactory
- Notifiable

## Static vs Singleton

### Static
Belongs to class. No object required.

### Singleton
Only one instance allowed in application lifecycle.

### Laravel Usage
- Facades (Static Proxy Pattern)
- App Container Singleton Bindings

## Design Patterns (Laravel Core Understanding)

| Pattern    | Laravel Example             |
|------------|----------------------------|
| Singleton  | Service Container           |
| Factory    | Model Factories             |
| Repository | Custom Repository           |
| Strategy   | Payment Drivers             |
| Observer   | Model Events                |
| Decorator  | Middleware                  |
| Adapter    | Third-party Integrations    |
| Builder    | Query Builder               |

🧠 PHASE 3 — Laravel Architecture Deep Dive

## Laravel Request Lifecycle
### Flow:
- index.php
- HTTP Kernel
- Service Providers
- Middleware
- Controller
- Response

## Service Container Deep Dive
- Binding
- Singleton
- Contextual Binding
- Automatic Resolution
- Reflection-based Injection

Laravel auto-resolves dependencies using Reflection.

## Contracts & Interfaces
### Example:
```php
use Illuminate\Contracts\Queue\Queue;
```
Laravel prefers contracts because of Dependency Inversion Principle.
You depend on abstraction, not concrete implementation.

## Eloquent Internals

### Active Record Pattern
Model represents database table.

### Query Builder
Fluent interface pattern for database queries.

### Model Events
- creating
- created
- updating
- deleted

### Scopes
Reusable query constraints.

### Global Scopes
Applied automatically to all queries.

🎯 Final Outcome
After mastering this document, you understand:

- Core OOP deeply
- SOLID principles
- Dependency Injection
- Laravel internal architecture
- Design patterns used in real SaaS systems