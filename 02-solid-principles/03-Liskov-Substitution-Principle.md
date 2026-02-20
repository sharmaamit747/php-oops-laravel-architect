# L — Liskov Substitution Principle (LSP)
Child classes must be replaceable without breaking behavior.

## ❌ LSP Violation Example
```php
class Bird
{
    public function fly(){}
}

class Ostrich extends Bird
{
    public function fly()
    {
        throw new Exception("Cannot fly");
    }
}
```
Ostrich is not substitutable for Bird.

LSP broken.

## ✅ Correct Design
```php
interface Bird{}

interface FlyBird extends Bird
{
    public function fly();
}

class Sparrow implements FlyBird
{
    public function fly();
}

class Ostrich implements Bird{}
```
Now substitution works safely.

## Laravel Mapping

If your implementation throws unexpected exceptions or removes behavior → LSP violation.

### Example:

- A repository that sometimes returns null instead of model.
- A payment driver that behaves differently than contract promises.