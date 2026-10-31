# SOLID Principles – Java Interview Notes

## 1. What is SOLID?

SOLID is a set of five object-oriented design principles used to write maintainable, flexible, reusable and testable software.

**Why do we use SOLID?**
- Reduce tightly coupled code.
- Make applications easier to maintain.
- Make adding new features easier.
- Improve testability.
- Reduce the impact of changes.

The five principles:

| Letter | Principle | Meaning |
|---|---|---|
| S | Single Responsibility Principle | One class, one responsibility |
| O | Open/Closed Principle | Open for extension, closed for modification |
| L | Liskov Substitution Principle | Child objects should preserve parent behavior |
| I | Interface Segregation Principle | Prefer smaller, focused interfaces |
| D | Dependency Inversion Principle | Depend on abstractions, not concrete classes |

---

## 2. S – Single Responsibility Principle (SRP)

**Definition:** A class should have only one reason to change.

### Problem

One class handles multiple responsibilities:

- Order creation
- Database operations
- Sending emails
- Generating invoices

### Better design

Separate the responsibilities into different classes:

```java
class OrderService {
    public void createOrder() {
        System.out.println("Order created");
    }
}

class OrderRepository {
    public void saveToDatabase() {
        System.out.println("Saved to database");
    }
}

class EmailService {
    public void sendEmail() {
        System.out.println("Email sent");
    }
}

class InvoiceService {
    public void generateInvoice() {
        System.out.println("Invoice generated");
    }
}
```

### Real-world example

In Spring Boot:

| Class | Responsibility |
|---|---|
| OrderController | Handles HTTP requests |
| OrderService | Contains business logic |
| OrderRepository | Database operations |
| EmailService | Sends emails |

**Interview answer:** SRP improves maintainability, testability and reduces the impact of changes.

---

## 3. O – Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for modification.

### Problem

Using multiple `if-else` conditions for every payment type requires modifying existing code whenever a new payment method is introduced.

### Better design

Create an interface:

```java
interface Payment {
    void pay();
}
```

Implement it for different payment methods:

```java
class UpiPayment implements Payment {
    public void pay() {
        System.out.println("UPI payment");
    }
}

class CardPayment implements Payment {
    public void pay() {
        System.out.println("Card payment");
    }
}

class PaypalPayment implements Payment {
    public void pay() {
        System.out.println("PayPal payment");
    }
}
```

### Real-world example

Adding a new payment gateway without rewriting existing payment implementations.

**Interview answer:** OCP makes applications easier to extend and reduces changes to tested existing code.

---

## 4. L – Liskov Substitution Principle (LSP)

**Definition:** An object of a child class should be usable wherever its parent class is expected without breaking expected behavior.

### Problem

A Penguin extends a Bird class that promises a `fly()` operation, but a Penguin cannot fly.

```java
class Bird {
    void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```

### Better design

Separate flying behavior into an interface:

```java
class Bird {
    void eat() {
        System.out.println("Eating");
    }
}

interface Flyable {
    void fly();
}

class Sparrow extends Bird implements Flyable {
    public void fly() {
        System.out.println("Sparrow flying");
    }
}

class Penguin extends Bird {
}
```

### Real-world example

A service implementation must honor the behavior promised by its interface or parent class.

**Interview answer:** LSP prevents incorrect inheritance and ensures child classes preserve the behavioral expectations of their parent classes.

---

## 5. I – Interface Segregation Principle (ISP)

**Definition:** A class should not be forced to implement methods it does not need.

### Problem

A basic printer should not be forced to implement scanning and faxing methods.

### Better design

Use separate interfaces:

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface Fax {
    void fax();
}
```

Basic printer:

```java
class BasicPrinter implements Printer {
    public void print() {
        System.out.println("Printing");
    }
}
```

Multifunction printer:

```java
class AdvancedPrinter implements Printer, Scanner, Fax {

    public void print() {
        System.out.println("Printing");
    }

    public void scan() {
        System.out.println("Scanning");
    }

    public void fax() {
        System.out.println("Faxing");
    }
}
```

### Real-world example

Small, focused interfaces make service contracts easier to implement and test.

**Interview answer:** ISP reduces unnecessary dependencies and prevents classes from implementing methods they don't require.

---

## 6. D – Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend directly on low-level modules. Both should depend on abstractions.

### Problem

OrderService directly creates a UpiPayment object.

```java
class UpiPayment {
    void pay() {
        System.out.println("UPI payment");
    }
}

class OrderService {
    private UpiPayment payment = new UpiPayment();

    void placeOrder() {
        payment.pay();
    }
}
```

This creates tight coupling.

### Better design

Create an abstraction:

```java
interface Payment {
    void pay();
}
```

Implementations:

```java
class UpiPayment implements Payment {
    public void pay() {
        System.out.println("UPI payment");
    }
}

class CardPayment implements Payment {
    public void pay() {
        System.out.println("Card payment");
    }
}
```

Use constructor injection:

```java
class OrderService {

    private Payment payment;

    OrderService(Payment payment) {
        this.payment = payment;
    }

    void placeOrder() {
        payment.pay();
    }
}
```

### Real-world example

Spring Boot uses Dependency Injection to provide required implementations to classes.

**Interview answer:** DIP reduces tight coupling and makes implementations easier to replace and test.

---

## 7. Quick Interview Revision

| Principle | Key phrase |
|---|---|
| SRP | One responsibility |
| OCP | Extend without modifying existing code |
| LSP | Child preserves parent behavior |
| ISP | Smaller, focused interfaces |
| DIP | Depend on abstractions |

## 8. Common Interview Questions

**Q1. What is SOLID?**

Five object-oriented design principles that improve maintainability, flexibility and testability.

**Q2. Which SOLID principle is related to Dependency Injection?**

Dependency Inversion Principle (DIP).

**Q3. What is the difference between SRP and ISP?**

- SRP focuses on keeping a class responsible for one thing.
- ISP focuses on keeping interfaces small and focused.

**Q4. Is using an interface automatically SOLID?**

No. The interface and its implementations must follow the relevant design principles.

**Q5. Does SOLID mean creating as many classes as possible?**

No. It means assigning clear responsibilities and avoiding unnecessary coupling. Don't over-engineer simple applications.

---

## 9. Practical Application

Apply SOLID to an e-commerce backend:

| Requirement | Design |
|---|---|
| Create orders | OrderService |
| Save orders | OrderRepository |
| Send email | EmailService |
| Different payment methods | Payment interface |
| Different payment implementations | UpiPayment, CardPayment |
| Inject payment implementation | Constructor injection |

## 10. Final Revision

Remember:

- **S:** One class, one responsibility.
- **O:** Add new functionality without unnecessarily changing existing code.
- **L:** Subclasses must preserve expected parent behavior.
- **I:** Don't force unnecessary methods.
- **D:** Depend on abstractions, not concrete implementations.

**Next topic in the learning roadmap:** String, StringBuilder, immutability and Object class.