# I — Interface Segregation Principle (ISP)
Clients should not depend on methods they don’t use.

## ❌ Bad Interface
```php
interface Worker
{
    public function work();
    public function eat();
}
```
Robot class:
```php
class Robot implements Worker
{
    public function work() {}
    public function eat() {} // meaningless
}
```
Forces unnecessary method.

## ✅ Correct Design
```php
interface Workable
{
    public function work();
}

interface Eatable
{
    public function eat();
}
```
Now clean.

## Laravel Mapping
Don’t create fat interfaces like:
```php
UserRepositoryInterface
{
    create();
    update();
    delete();
    export();
    sendEmail();
}
```
Split responsibilities.