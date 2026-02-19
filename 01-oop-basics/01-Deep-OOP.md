# Lesson 1 — Deep OOP with Production Mindset

## What is OOP in Production?

### Most developers think:

“OOP = class + object”

❌ Wrong mindset.

### In production systems like:

- Laravel
- Symfony
- CodeIgniter

#### OOP means:

- Maintainablity
- Extensibility
- Testability
- Scalibility
- Loose coupling


## Class vs Object

### ❌ Beginner Example
```php
class User{
    public $name;
}
```
this is not production-ready.

#### Problems:

- No type sefty
- No encapsulation
- No validation
- Public properties = dangerous

### Production-Level Example
```php
class User
{
    private string $name;
    private string $email;

    public function __construct(string $name, string $email)
    {
        $this->setName($name);
        $this->setEmail($email);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getEmail(): string
    {
        return $this->email;
    }

    public function setName(string $name): void
    {
        if (strlen($name) < 3) {
            throw new InvalidArgumentException("Name too short");
        }

        $this->name = $name;
    }

    public function setEmail(string $email): void
    {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException("Invalid Email");
        }

        $this->email = $email;
    }
}
$user = new User("Amit", "sharma@gmail.com");

echo $user->getName();
echo $user->getEmail();

```
#### Why This Is Production Mindset?

- Validation inside class
- State protection
- Type enforcement
- Fail fast


## Mapping to Laravel

### Example: Eloquent Model

```php
class User extend Model
{
    protected $fillable = ['name', 'email'];
}
```
Why $fillable exists?

- Encapsulation
- Protection against mass assignment
- Controlled state mutation

Laravel internally follows OOP principles everywhere.

## $this Keyword

```php
$this->name
```
### Means:

"Current object instance"

#### Production Importance:

- Accessing state safely
- Avoiding static misuse
- Instance-specific behavior


## Static vs Instance

### Static Example
```php
class MathHelper
{
    public static function add(int $a, int $b):int
    {
        return $a + $b;
    }
}

MathHelper::add(2,5);
```
✔ No object needed
✔ Stateless

### Instance Example
```php
$service = new PaymentService();
$service->process();
```

✔ Has internal state
✔ Can be injected
✔ Testable

## Constructor

Constructor is NOT for heavy logic.

### Bad:
```php
public function __constructor()
{
    $this->connectToDatabase(); // ❌
}
```
### Good:
```php
public function __constructor(DatabaseConnection $db)
{
    $this->db = $db; 
}
```
Constructor = Dependency injection point.


## Payment system Example

### Step 1 — Define Contract
```php
interface PaymentGateway
{
    public function pay(float $amount): string;
} 
```
### Step 2 — Concrete Implementation
```php
class StripePayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Paid ₹{$amount} via Stripe";
    }
}
class RazorPayPayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Paid ₹{$amount} via RazorPay";
    }
}
```
### Step 3 — Use Dependency Injection
```php
class OrderService
{
    private PaymentGateway $gateway;

    public function __construct(PaymentGateway $gateway)
    {
        $this->gateway = $gateway;
    }

    public function checkout(float $amount)
    {
        return $this->gateway->pay($amount);
    }
}
```
### Use:
```php
// Create implementation first
$gateway = new StripePayment();
$razor = new RazorPayPayment();
// Inject into service
$orderService = new OrderService($gateway);
// Call checkout
echo $orderService->checkout(100);
```

### What Did We Achieve?
- Loose coupling
- Testable code
- Replaceable payment driver
- Follows SOLID

his is how Laravel is architected internally.


## Object Lifecycle in Laravel

### Flow:

- HTTP Request
- Service Container resolves dependencies
- Controller injected
- Services injected
- Response returned

All OOP driven.

## Production Checklist for Every Class

Before writing any class, ask:

- Does this class have ONE responsibility?
- Is state protected?
- Is it testable?
- Can it be replaced without breaking code?
- Is it injectable?

If answer is NO → redesign.