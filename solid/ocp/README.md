### The Open/Closed Principle (OCP)

The second principle of SOLID is the Open/Closed Principle. It states that software entities (classes, modules, functions, etc.) should be:

> **Open for extension, but closed for modification.**

This sounds like a contradiction. How can we extend the behavior of a class without modifying it?

The answer lies in **abstraction**.

If you have a class that works perfectly and has been fully tested, you should not touch its source code just to add a new feature. Modifying existing code introduces the risk of breaking old functionality. Instead, you should be able to extend the code by adding new classes.

### Example 1: The Payment Nightmare

Imagine a `PaymentProcessor` class that handles different payment methods. A beginner might write it like this:

```java
public class PaymentProcessor {
    public void process(String paymentMethod) {
        if (paymentMethod.equals("credit_card")) {
            System.out.println("Processing credit card payment...");
        } else if (paymentMethod.equals("paypal")) {
            System.out.println("Processing PayPal payment...");
        } else if (paymentMethod.equals("upi")) {
            System.out.println("Processing UPI payment...");
        } else {
            throw new IllegalArgumentException("Unsupported payment method");
        }
    }
}

```

**Why this violates OCP:**
Every time we want to add a new payment method (like Bitcoin or Apple Pay), we have to open the `PaymentProcessor` class and modify the `process` method. We are touching tested code, which creates a risk of bugs.

#### The Solution: Use Interfaces (The Strategy Pattern)

Instead of writing the implementation of every payment method inside the `PaymentProcessor`, we should depend on an **Interface**. The `PaymentProcessor` doesn't need to know _how_ the payment works; it just needs to know _that_ it can pay.

Here is the refactored code:

```java
// 1. Define the contract (Interface)
public interface PaymentStrategy {
    void pay();
}

// 2. Create concrete implementations
public class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("Credit card payment processed");
    }
}

public class PayPalPayment implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("PayPal payment processed");
    }
}

public class UPIPayment implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("UPI payment processed");
    }
}

// 3. The Processor now depends on the interface, not the specific types
public class PaymentProcessor {
    private PaymentStrategy strategy;

    public PaymentProcessor(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void process() {
        this.strategy.pay();
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor = new PaymentProcessor(new UPIPayment());
        processor.process();
    }
}

```

Now, if we want to add Bitcoin, we just create a `BitcoinPayment` class that implements `PaymentStrategy`. We never have to touch the `PaymentProcessor` code again. It is closed for modification, but open for extension.

### Example 2: The Famous "Shapes" Example (Inheritance)

You mentioned the "shapes" example, which is the classic way to teach OCP using **Inheritance** (often called "Heritage" in other languages).

Imagine we have a function that calculates the total area of a list of shapes.

#### The Bad Way (Modification):

```java
public double calculateTotalArea(List<Object> shapes) {
    double total = 0;
    for (Object shape : shapes) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            total += Math.PI * c.radius * c.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            total += r.width * r.height;
        }
    }
    return total;
}

```

If we add a Triangle, we have to modify this function.

#### The Good Way (Extension via Inheritance):

```java
// Base class
public abstract class Shape {
    public abstract double area();
}

public class Circle extends Shape {
    public double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * this.radius * this.radius;
    }
}

public class Rectangle extends Shape {
    public double width;
    public double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return this.width * this.height;
    }
}

// Now the calculator never changes, no matter how many shapes we add
public double calculateTotalArea(List<Shape> shapes) {
    double total = 0;
    for (Shape shape : shapes) {
        total += shape.area();
    }
    return total;
}

```

### A Note on "Heritage" vs. Interfaces

You mentioned "using heritage" (Inheritance). In modern programming, we often prefer **Interfaces** (like your Payment example) over **Inheritance** (like the Shape example).

Interfaces are more flexible because a class can implement multiple interfaces, but usually can only inherit from one parent class. However, both achieve the Open/Closed Principle perfectly.
