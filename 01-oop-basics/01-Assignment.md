# Build:

- Product class with validation
- Order class
- Payment interface
- Two payment drivers
- Inject payment into order service

```php

declare(strict_types=1);

/**
 * PRODUCT ENTITY
 */
class Product
{
    private string $name;
    private float $price;

    public function __construct(string $name, float $price)
    {
        $this->setName($name);
        $this->setPrice($price);
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
            throw new InvalidArgumentException("Price must be greater than 0");
        }

        $this->price = $price;
    }

    public function getPrice(): float
    {
        return $this->price;
    }

    public function getName(): string
    {
        return $this->name;
    }
}


/**
 * ORDER ENTITY
 */
class Order
{
    private Product $product;
    private int $quantity;

    public function __construct(Product $product, int $quantity)
    {
        if ($quantity <= 0) {
            throw new InvalidArgumentException("Quantity must be greater than 0");
        }

        $this->product = $product;
        $this->quantity = $quantity;
    }

    public function getTotalAmount(): float
    {
        return $this->product->getPrice() * $this->quantity;
    }

    public function getProduct(): Product
    {
        return $this->product;
    }

    public function getQuantity(): int
    {
        return $this->quantity;
    }
}


/**
 * PAYMENT CONTRACT
 */
interface PaymentGateway
{
    public function pay(float $amount): string;
}


/**
 * STRIPE DRIVER
 */
class StripePayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Paid ₹{$amount} via Stripe";
    }
}


/**
 * RAZORPAY DRIVER
 */
class RazorPayPayment implements PaymentGateway
{
    public function pay(float $amount): string
    {
        return "Paid ₹{$amount} via RazorPay";
    }
}


/**
 * ORDER SERVICE
 */
class OrderService
{
    private PaymentGateway $payment;

    public function __construct(PaymentGateway $payment)
    {
        $this->payment = $payment;
    }

    public function checkout(Order $order): string
    {
        $total = $order->getTotalAmount();
        return $this->payment->pay($total);
    }
}


/**
 * APPLICATION BOOTSTRAP
 */

// Create product
$product = new Product("Laptop", 50000);

// Create order
$order = new Order($product, 2);

// Choose payment driver
$paymentDriver = new RazorPayPayment(); // or new StripePayment()

// Inject payment into service
$orderService = new OrderService($paymentDriver);

// Checkout
echo $orderService->checkout($order);

```