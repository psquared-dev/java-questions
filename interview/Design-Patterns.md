<!-- TOC -->
* [Q-1 What are different categories of design patterns?](#q-1-what-are-different-categories-of-design-patterns)
  * [1. Creational Patterns](#1-creational-patterns)
  * [2. Structural Patterns](#2-structural-patterns)
  * [3. Behavioral Patterns](#3-behavioral-patterns)
* [Q-2 Diff b/w Factory pattern and Abstract factory pattern](#q-2-diff-bw-factory-pattern-and-abstract-factory-pattern)
  * [Examples](#examples)
  * [Example of Factory Pattern](#example-of-factory-pattern)
    * [1. The Paths API Example](#1-the-paths-api-example)
  * [Example of Abstract Factory Pattern](#example-of-abstract-factory-pattern)
    * [2. The Selector.open() Example](#2-the-selectoropen-example)
    * [1. The JDBC Example (java.sql.Connection)](#1-the-jdbc-example-javasqlconnection)
* [Q-3 How to implement Factory Pattern?](#q-3-how-to-implement-factory-pattern)
    * [The Scenario](#the-scenario)
    * [1. The Common Interface](#1-the-common-interface)
    * [2. The Concrete Implementations](#2-the-concrete-implementations)
    * [3. The Simple Factory](#3-the-simple-factory)
    * [4. The Client Code (Realistic Usage)](#4-the-client-code-realistic-usage)
    * [Why is this "Realistic" & Useful?](#why-is-this-realistic--useful)
* [Q-4 How to implement Abstract Factory Pattern?](#q-4-how-to-implement-abstract-factory-pattern)
  * [1. The Abstract Products (The Interfaces)](#1-the-abstract-products-the-interfaces)
  * [2. The Concrete Products (The "Family" Members)](#2-the-concrete-products-the-family-members)
  * [3. The Abstract Factory](#3-the-abstract-factory)
  * [4. The Concrete Factories](#4-the-concrete-factories)
  * [5. The Client Code (Realistic Usage)](#5-the-client-code-realistic-usage)
    * [1. The Configuration (application.properties)](#1-the-configuration-applicationproperties)
    * [2. The Abstract Factory & Implementations](#2-the-abstract-factory--implementations)
    * [3. The DI Configuration Class](#3-the-di-configuration-class)
    * [4. The Client Code (Your Service)](#4-the-client-code-your-service)
    * [Summary of Execution](#summary-of-execution)
* [Q-5 What is Prototype pattern?](#q-5-what-is-prototype-pattern)
  * [Implementation Requirement](#implementation-requirement)
* [Q-6 What is Builder pattern](#q-6-what-is-builder-pattern)
<!-- TOC -->

# Q-1 What are different categories of design patterns?

Design patterns are categorized into three main groups based on the type of problem they solve:

## 1. Creational Patterns

These patterns deal with **Object Creation**. They help create objects in a manner suitable 
to the situation, hiding the logic of how exactly the object is created (so you aren't spamming `new` everywhere).

* Goal: "How do I instantiate this class comfortably?"
* Examples:
    * **Singleton:** Ensures a class has only one instance (e.g., Database connection).
    * **Factory Method:** Creates objects without specifying the exact class (e.g., `Calendar.getInstance()`).
    * **Builder:** Constructs complex objects step-by-step (e.g., `StringBuilder` or a Pizza builder).


## 2. Structural Patterns

These patterns deal with **Class & Object Composition**. They show you how to assemble different   
classes into larger structures while keeping them flexible.

* Goal: "How do I make these different classes work together?"
* Examples:
   * **Adapter:** Makes incompatible interfaces work together (e.g., Power Adapter).
   * **Decorator:** Dynamically adds behavior to an object (e.g., Adding "Scrollbars" to a Window).
   * **Facade:** Provides a simplified interface to a complex system (e.g., A "Car Start" button that 
internally handles fuel, spark, engine).

## 3. Behavioral Patterns

These patterns deal with **Communication between Objects**. They focus on how objects 
interact and distribute responsibilities.

* Goal: "How do these objects talk to each other?"
* Examples:
    * **Observer:** A subscription mechanism to notify objects of events (e.g., YouTube notifications).
    * **Strategy:** Defines a family of algorithms and makes them interchangeable (
e.g., `Collections.sort()` using different Comparators).
    * **Iterator:** Traversing a collection without exposing its underlying representation (e.g., `for(Item i : list)`).

| Category       | Focus         | Key Word        | Popular Examples             |
|:---------------|:--------------|:----------------|:-----------------------------|
| **Creational** | Instantiation | **"New"**       | Singleton, Factory, Builder  |
| **Structural** | Composition   | **"Structure"** | Adapter, Decorator, Proxy    |
| **Behavioral** | Interaction   | **"Talk"**      | Observer, Strategy, Iterator |

# Q-2 Diff b/w Factory pattern and Abstract factory pattern

The main difference is the scope of what they create.

* **Factory Method:** Creates one specific type of object (e.g., "Make me a Button").
* **Abstract Factory:** Creates a family of related objects that belong together 
(e.g., "Make me a whole Windows-style UI," which includes a Windows Button, 
a Windows Checkbox, and a Windows Scrollbar).

## Examples

## Example of Factory Pattern

Both `java.nio.file.Paths` and `java.nio.channels.Selector` are excellent, real-world examples of the 
Factory Pattern (specifically the Static Factory Method).

They perfectly demonstrate the main goal of the Factory pattern: 
**"Hiding the logic of creation and the specific implementation class from the user."**

### 1. The Paths API Example

When you call `Paths.get()`, you don't care how the path is created or what operating system you are on. 
You just want a `Path` object.

* **The Request:** `Paths.get("data.txt")`
* **The Hidden Logic:** Java checks your OS.
   * If you are on Windows, it creates a `WindowsPath`.
   * If you are on Linux/Mac, it creates a `UnixPath`.
* **The Pattern:** You never see new WindowsPath(...). The Factory handles it.

```java
// Client Code (You write this)
Path p = Paths.get("file.txt");

// Internal Java Logic (The Factory)
public static Path get(String first, String... more) {
    return FileSystems.getDefault().getPath(first, more);
}
```

## Example of Abstract Factory Pattern

### 2. The Selector.open() Example

This is an even stronger example of a Factory abstracting complexity.

* **The Request:** `Selector.open()`
* **The Hidden Logic:** Java asks the underlying `SelectorProvider` for the best implementation for your hardware.
   * On Linux, it might give you an `EPollSelectorImpl`.
   * On Mac, it might give you a `KQueueSelectorImpl`.
   * On Windows, it might give you a `WindowsSelectorImpl`.
* **The Pattern:** You just work with the interface Selector. The factory ensures you get the 
highly optimized version for your specific machine.

```java
// Client Code
Selector selector = Selector.open();

// Internal Java Logic
public static Selector open() throws IOException {
    return SelectorProvider.provider().openSelector();
}
```


The best real-world example for Abstract Factory in Java is JDBC (Java Database Connectivity).

### 1. The JDBC Example (java.sql.Connection)

In JDBC, the `Connection` interface acts as an Abstract Factory.

Why? Because a Connection doesn't just establish a link; it is responsible for creating a family of related 
objects that execute queries. These objects must match the specific database you are using (MySQL, Oracle, PostgreSQL).

* **The Abstract Factory:** `java.sql.Connection`
* **The Family of Products:**
    * `Statement` (via `.createStatement()`)
    * `PreparedStatement` (via `.prepareStatement()`)
    * `CallableStatement` (via `.prepareCall()`)
    * `Blob` / `Clob` (via `.createBlob()`)

How it works in practice:

If you connect to an Oracle database:

* The Driver gives you an `OracleConnection` (The Concrete Factory).
* When you call `conn.createStatement()`, it secretly creates an `OracleStatement`.
* When you call `conn.prepareStatement()`, it secretly creates an `OraclePreparedStatement`.

**The Constraint:** You cannot mix them. You cannot try to execute a `MySQLStatement` over an `OracleConnection`. 
The Abstract Factory pattern ensures that if you are holding an Oracle connection, everything it produces is 
compatible with Oracle.

# Q-3 How to implement Factory Pattern?

A classic, realistic example of the **Simple Factory** pattern is a **Payment Gateway**.

In an e-commerce app, a user selects a payment method (PayPal, Stripe, Credit Card) at checkout. 
Your code shouldn't have if `(type == "PayPal")` logic scattered everywhere. Instead, you centralize 
that decision in a **Simple Factory**.

### The Scenario

You receive a payment type from the frontend (e.g., "PAYPAL"), and you need to instantiate the correct
class to handle the transaction.

### 1. The Common Interface

All payment methods must look the same to the rest of the app.

```java
public interface PaymentProcessor {
    void processPayment(double amount);
}
```

### 2. The Concrete Implementations

The actual logic for connecting to different banks/APIs.

```java
class PayPalProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Redirecting to PayPal for $" + amount);
    }
}

class StripeProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Charging Credit Card via Stripe: $" + amount);
    }
}

class CryptoProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Generating Bitcoin wallet address for $" + amount);
    }
}
```

### 3. The Simple Factory

This is the core of the pattern: A single class that encapsulates the `switch` statement. 
This keeps your main business logic clean.

```java
public class PaymentFactory {
    
    // The "Simple Factory" method
    public static PaymentProcessor getProcessor(String type) {
        if (type == null) {
            throw new IllegalArgumentException("Payment type cannot be null");
        }
        
        switch (type.toUpperCase()) {
            case "PAYPAL":
                return new PayPalProcessor();
            case "STRIPE":
                return new StripeProcessor();
            case "CRYPTO":
                return new CryptoProcessor();
            default:
                throw new IllegalArgumentException("Unknown payment type: " + type);
        }
    }
}
```

### 4. The Client Code (Realistic Usage)

Your Controller or Service layer just asks the factory for the object. It doesn't know how `PayPalProcessor` is created.

```java
public class CheckoutService {
    public static void main(String[] args) {
        // Imagine this comes from a user clicking a button on the UI
        String userSelection = "PAYPAL"; 
        double billAmount = 99.99;

        // 1. Ask Factory for the correct object
        PaymentProcessor processor = PaymentFactory.getProcessor(userSelection);

        // 2. Use it (Polymorphism)
        processor.processPayment(billAmount);
    }
}
```

### Why is this "Realistic" & Useful?

* **Centralized Change:** If you want to replace `PayPalProcessor` with a new version (e.g., `PayPalV2Processor`), 
you only change code in one place (the Factory). You don't have to hunt down every `new PayPalProcessor()` in your app.
* **Clean Controllers:** Your checkout logic doesn't care about the messy setup required for Stripe or Crypto; 
it just asks for a processor and pays.

**Critique:** The Simple Factory is great for small sets of classes (3-5 types). However, if you have 50 
different payment types, the switch statement becomes a "god method" and is hard to maintain. 
In that case, you would upgrade to the Factory Method or use a Map-based registry.
  
# Q-4 How to implement Abstract Factory Pattern?

A very common, realistic scenario for backend engineering is Multi-Cloud Support.

Imagine you are building an application that needs to be deployed on AWS (Amazon) or GCP (Google Cloud).

* You need Storage (S3 vs. Google Cloud Storage).
* You need Compute (EC2 vs. Google Compute Engine).

**The Rule:** If you are running on AWS, your app must use both S3 and EC2. 
You cannot accidentally mix an AWS Server with Google Storage. The Abstract Factory ensures this consistency.


## 1. The Abstract Products (The Interfaces)

First, we define what our application needs, regardless of the provider.

```java
// Product A: Storage
interface Storage {
    void storeFile(String filename);
}

// Product B: Compute Instance
interface Instance {
    void start();
}
```

## 2. The Concrete Products (The "Family" Members)

Now we implement the specific versions for each cloud.

```java
// --- Family 1: AWS Implementation ---
class AwsS3 implements Storage {
    public void storeFile(String filename) {
        System.out.println("Uploading " + filename + " to AWS S3 Bucket...");
    }
}
class AwsEC2 implements Instance {
    public void start() {
        System.out.println("Starting AWS EC2 Instance...");
    }
}

// --- Family 2: Google Cloud Implementation ---
class GoogleGCS implements Storage {
    public void storeFile(String filename) {
        System.out.println("Uploading " + filename + " to Google Cloud Storage...");
    }
}
class GoogleComputeEngine implements Instance {
    public void start() {
        System.out.println("Starting Google Compute Engine VM...");
    }
}
```

## 3. The Abstract Factory

This interface defines the "Family" creation. Note that it creates both storage and compute.

```java
interface CloudFactory {
    Storage createStorage();
    Instance createInstance();
}
```

## 4. The Concrete Factories

These factories group the correct products together.

```java
// AWS Factory: Guarantees you get S3 + EC2
class AwsFactory implements CloudFactory {
    public Storage createStorage() { return new AwsS3(); }
    public Instance createInstance() { return new AwsEC2(); }
}

// Google Factory: Guarantees you get GCS + GCE
class GoogleFactory implements CloudFactory {
    public Storage createStorage() { return new GoogleGCS(); }
    public Instance createInstance() { return new GoogleComputeEngine(); }
}
```

## 5. The Client Code (Realistic Usage)

Your main application logic does not know about "AWS" or "Google". It just asks for "Storage" and "Instance".

```java
public class CloudApp {
    private final Storage storage;
    private final Instance instance;

    // Dependency Injection: The Factory is passed in
    public CloudApp(CloudFactory factory) {
        this.storage = factory.createStorage();
        this.instance = factory.createInstance();
    }

    public void deploy() {
        instance.start();
        storage.storeFile("app-logs.txt");
    }

    public static void main(String[] args) {
        // Configuration: Change this ONE line to switch the entire cloud provider
        // CloudFactory myFactory = new GoogleFactory(); 
        CloudFactory myFactory = new AwsFactory();

        CloudApp app = new CloudApp(myFactory);
        app.deploy();
    }
}
```

Note that, in a real-world enterprise application, you would never hardcode `new AwsFactory()` inside `main`. 
That defeats the purpose of flexibility.

Here is a realistic example using Spring Boot, which is the industry standard for Dependency Injection (DI) in Java.

In this approach, you never write `new AwsFactory()`. The Spring Container reads the config and "injects" 
the correct factory into your code automatically.

### 1. The Configuration (application.properties)

This is the only thing you change to switch providers.

```text
# Toggle this between AWS or GOOGLE
app.cloud.provider=AWS
```

### 2. The Abstract Factory & Implementations

(Assuming these interfaces and classes exist from our previous step).

```java
public interface CloudFactory {
    Storage createStorage();
    Instance createInstance();
}

public class AwsFactory implements CloudFactory { ... }
public class GoogleFactory implements CloudFactory { ... }
```

### 3. The DI Configuration Class

This replaces the manual "Factory Maker" logic. We use @ConditionalOnProperty to tell Spring which bean to create.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;

@Configuration
public class CloudConfig {

    // If app.cloud.provider=AWS, this method runs and creates the AWS Factory
    @Bean
    @ConditionalOnProperty(name = "app.cloud.provider", havingValue = "AWS")
    public CloudFactory awsFactory() {
        return new AwsFactory();
    }

    // If app.cloud.provider=GOOGLE, this method runs instead
    @Bean
    @ConditionalOnProperty(name = "app.cloud.provider", havingValue = "GOOGLE")
    public CloudFactory googleFactory() {
        return new GoogleFactory();
    }
}
```

### 4. The Client Code (Your Service)

Notice there is no if/else logic here. You just ask for `CloudFactory`. Spring injects the one that matches the config.

```java
import org.springframework.stereotype.Service;

@Service
public class BackendService {

    private final CloudFactory cloudFactory;

    // CONSTRUCTOR INJECTION
    // Spring sees you need a CloudFactory. 
    // It looks at the Config, sees the active one (AWS), and passes it in here.
    public BackendService(CloudFactory cloudFactory) {
        this.cloudFactory = cloudFactory;
    }

    public void runJob() {
        // We don't know (or care) if this is AWS or Google.
        // The injected factory handles it.
        Storage storage = cloudFactory.createStorage();
        Instance instance = cloudFactory.createInstance();

        instance.start();
        storage.storeFile("backup.zip");
    }
}
```

### Summary of Execution

* **App Start:** Spring reads application.properties → sees AWS.
* **Config Phase:** Spring activates awsFactory() bean and ignores googleFactory().
* **Injection Phase:** Spring finds `BackendService`. It sees the constructor needs `CloudFactory`. 
It plugs in the AwsFactory object it just created.
* **Runtime:** BackendService runs on AWS infrastructure.

# Q-5 What is Prototype pattern?

The Prototype Pattern allows you to create new objects by **copying an existing object** rather 
than creating a new one from scratch.

This is useful when object creation is **expensive** (e.g., database calls, complex calculations) or
repetitive (setting many default values).

## Implementation Requirement

To implement this pattern, you must **set up the `clone()` method correctly**.

This is the most critical step. You cannot just use the default Java cloning if your object contains other 
objects (like a list or a custom class). You must manually implement a **Deep Copy** inside `clone()` to ensure the 
new object is truly independent of the original.

Here is the implementation using the User class. Notice how the `clone()` method manually creates a
new `Address` to prevent the "shared reference" bug.

```java
// 1. The Mutable Dependency
class Address {
    String city;
    String street;

    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }

    @Override
    public String toString() { return city + ", " + street; }
}

// 2. The Prototype Class
class User implements Cloneable {
    String name;       // Simple (String is safe)
    Address address;   // Mutable Object (Requires DEEP COPY)

    public User(String name, String city, String street) {
        // Imagine this constructor is 'expensive' (e.g., DB calls)
        this.name = name;
        this.address = new Address(city, street);
    }

    // 3. The Clone Logic (The Core of the Pattern)
    @Override
    public User clone() {
        try {
            // Step A: Shallow Copy
            // (Copies the 'name' and the POINTER to 'address')
            User clonedUser = (User) super.clone();

            // Step B: DEEP COPY (The Critical Fix)
            // We must manually create a NEW Address object for the clone.
            // If we skip this, both users will share the same address object.
            clonedUser.address = new Address(this.address.city, this.address.street);

            return clonedUser;
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }

    @Override
    public String toString() {
        return "User{name='" + name + "', address=" + address + "}";
    }
}

// 4. Usage
public class Main {
    public static void main(String[] args) {
        // Step 1: Create the Prototype (Expensive setup happens here ONCE)
        User master = new User("John", "New York", "5th Avenue");

        // Step 2: Clone it (Instant memory copy)
        User clone = master.clone();

        // Step 3: Modify the Clone
        clone.name = "Steve";           
        clone.address.city = "London";  // This modification is safe due to Deep Copy logic

        // Verify that Master is untouched
        System.out.println("Master: " + master); 
        System.out.println("Clone:  " + clone);
    }
}
```

**Output:**

```text
Master: User{name='John', address=New York, 5th Avenue}
Clone:  User{name='Steve', address=London, 5th Avenue}
```

# Q-6 What is Builder pattern

A creational design pattern that lets you construct complex objects step-by-step. 
It allows you to produce different types and representations of an object using the same construction code.

Example:

```java
public class User {
    private String name;
    private String email;

    private User(){
    }

    public static class Builder {
        private final User user;

        public Builder() {
            this.user = new User();
        }

        public Builder withName(String name) {
            this.user.setName(name);
            return this;
        }

        public Builder withEmail(String email) {
            this.user.setEmail(email);
            return this;
        }

        public User build() {
            return this.user;
        }
    }

    private void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    private void setEmail(String email) {
        this.email = email;
    }

    public static void main(String[] args) {
        User user = new User.Builder()
                .withName("John")
                .withEmail("a@mail.com")
                .build();

        System.out.println(user.getEmail());
        System.out.println(user.getName());
    }

}
```








