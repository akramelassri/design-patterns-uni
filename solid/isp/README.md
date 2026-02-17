# Interface Segregation Principle (ISP): Don't Force Me to Do Things I Can't Do

The **Interface Segregation Principle (ISP)** is simple:

> "Clients should not be forced to depend on interfaces they do not use."

**In simple English:** Don’t make a class sign a contract for a job it isn't going to do.

If you have a class that implements an interface, but it throws an error for half the methods because "I don't support this," you have a **Fat Interface**. You need to put that interface on a diet.

---

## Example 1: The Payment Gateway Trap

Imagine a universal `PaymentGateway` interface. It seems logical to put all payment-related methods in one place:

```java
public interface PaymentGateway {
    void pay();
    void refund();
    void schedule();
}

```

This works great for a sophisticated service like Stripe or PayPal. But what happens when we add **Cash On Delivery (COD)**?

```java
public class CashOnDelivery implements PaymentGateway {
    @Override
    public void pay() {
        System.out.println("COD payment accepted.");
    }

    // VIOLATION: COD cannot refund money digitally!
    @Override
    public void refund() {
        throw new UnsupportedOperationException("Refund not supported for COD");
    }

    // VIOLATION: You cannot schedule cash!
    @Override
    public void schedule() {
        throw new UnsupportedOperationException("Scheduling not supported");
    }
}

```

The `CashOnDelivery` class is forced to implement methods it doesn't need. This violates ISP and pollutes the code with useless error throwing.

### The Fix: Role-Based Interfaces

Instead of one giant interface, we break it down into capabilities:

```java
public interface Payable {
    void pay();
}

public interface Refundable {
    void refund();
}

public interface Schedulable {
    void schedule();
}

```

Now, our classes are clean. No more "Not Implemented" errors.

```java
// Now COD only implements what it actually does
public class CashOnDelivery implements Payable {
    @Override
    public void pay() {
        System.out.println("COD payment");
    }
}

// Stripe is powerful, so it implements all of them
public class StripePayment implements Payable, Refundable, Schedulable {
    @Override
    public void pay() { /* ... */ }

    @Override
    public void refund() { /* ... */ }

    @Override
    public void schedule() { /* ... */ }
}

```

---

## Example 2: The File Handler (Using Inheritance)

Sometimes, breaking interfaces apart can feel messy. You don't want to import 10 different interfaces just to open a file. You can use **Interface Inheritance** to group common logic while keeping specific logic separate.

**The Problem:**
A `FileHandler` that forces a `LogFileReader` to implement a `write()` method.

**The Solution:**

```java
// Base capability everyone needs
public interface FileOpener {
    void open();
    void close();
}

// Readers only care about reading
public interface FileReader extends FileOpener {
    String read();
}

// Writers only care about writing
public interface FileWriter extends FileOpener {
    void write(String data);
}

// Now the Reader is safe from accidental writes
public class LogFileReader implements FileReader {
    @Override
    public void open() { /* ... */ }

    @Override
    public String read() {
        return "log data";
    }

    @Override
    public void close() { /* ... */ }
}

```

---

## Conclusion

When designing interfaces, think about **roles**, not just objects.

- If a class has to implement a method just to throw an error, **split the interface**.
- Small, focused interfaces are easier to test, easier to maintain, and much more flexible.
