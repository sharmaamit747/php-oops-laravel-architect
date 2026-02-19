# LESSON 2 — Advanced Encapsulation & Domain

## Encapsulation
Encapsulation ≠ just private properties.

It means:
- Protect object state
- Prevent invalid data
- Enforce invariants
- Hide internal logic

### ❌ Wrong Example
```php
class Product
{
    public string $name;
    public float $price;
}
```
Problem:
- Anyone can change price anytime.
- No validation.
- No business rules.

Dangerous in ERP / SaaS systems.

### ✅ Production-Level Product Class
```php
class Product
{
    private string $name;
    private float $price;

    public function __construct(string $name, float $price)
    {
        $this->setName($name);
        $this->setPrice($price);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getPrice(): float
    {
        return $this->price;
    }

    private function setName(string $name): void
    {
        if (strlen($name) < 3) {
            throw new InvalidArgumentException("Product name too short");
        }

        $this->name = $name;
    }

    private function setPrice(float $price): void
    {
        if ($price <= 0) {
            throw new InvalidArgumentException("Price must be positive");
        }

        $this->price = $price;
    }
}
$product = new Product('Car', 1000);
echo $product->getName();
echo $product->getPrice();
```
### Why This Is Important in Real Systems

In large ERP / SaaS:

If price becomes negative → accounting disaster
If name invalid → reporting issue

Encapsulation protects business rules.

## Immutable Objects

Immutable = object state cannot change after creation.
- Why important?
- Thread safety
- Predictability
- No hidden side effects
- Easier debugging

### Example — Immutable Price Value Object
```php
final class Money
{
    private float $amount;

    public function __construct(float $amount)
    {
        if ($amount < 0) {
            throw new InvalidArgumentException("Money cannot be negative");
        }

        $this->amount = $amount;
    }

    public function getAmount(): float
    {
        return $this->amount;
    }

    public function add(Money $money): Money
    {
        return new Money($this->amount + $money->getAmount());
    }
}
```
Notice:

✔ No setter
✔ Returns new object
✔ Immutable

## DTO Pattern
DTO = Data Transfer Object

Used to move structured data between layers.
### Example
```php
class createOrderDTO
{
    public function __constructor(
        public readonly int $userId,
        public readonly array $products,
        public readonly float $total
    ){}
}
```
Used in:

- Service layer
- Jobs
- Controllers → Service communication

## Why Eloquent Is NOT a Pure Domain Model
Laravel Eloquent:

- Active Record pattern
- Contains DB logic
- Business logic + persistence mixed

In large systems:

Better:

Controller → Service → Domain → Repository → Model