<!-- TOC -->
  * [Q - What are different categories of design patterns?](#q---what-are-different-categories-of-design-patterns)
    * [1. Creational Patterns](#1-creational-patterns)
    * [2. Structural Patterns](#2-structural-patterns)
    * [3. Behavioral Patterns](#3-behavioral-patterns)
  * [Q - How to implement Singleton Design Pattern](#q---how-to-implement-singleton-design-pattern)
    * [Sequence of events (step-by-step)](#sequence-of-events-step-by-step)
    * [Why volatile is required?](#why-volatile-is-required)
  * [Q - How to implement Factory Pattern?](#q---how-to-implement-factory-pattern)
    * [The Scenario](#the-scenario)
    * [1. The Common Interface](#1-the-common-interface)
    * [2. The Concrete Implementations](#2-the-concrete-implementations)
    * [3. The Simple Factory](#3-the-simple-factory)
    * [4. The Client Code (Realistic Usage)](#4-the-client-code-realistic-usage)
    * [Why is this "Realistic" & Useful?](#why-is-this-realistic--useful)
  * [Q - How to implement Abstract Factory Pattern?](#q---how-to-implement-abstract-factory-pattern)
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
  * [Q - Diff b/w Factory pattern and Abstract factory pattern](#q---diff-bw-factory-pattern-and-abstract-factory-pattern)
    * [Examples of Factory Pattern](#examples-of-factory-pattern)
      * [1. The Paths API Example](#1-the-paths-api-example)
      * [2. The Selector.open() Example](#2-the-selectoropen-example)
    * [Examples of Abstract Factory Pattern](#examples-of-abstract-factory-pattern)
      * [1. The JDBC Example (java.sql.Connection)](#1-the-jdbc-example-javasqlconnection)
  * [Q - How to implement Builder Pattern](#q---how-to-implement-builder-pattern)
  * [Q - What is Prototype pattern?](#q---what-is-prototype-pattern)
    * [Implementation Requirement](#implementation-requirement)
  * [Q - What is Adapter pattern?](#q---what-is-adapter-pattern)
    * [The Best Analogy: The Power Plug](#the-best-analogy-the-power-plug)
    * [Why is it needed in code?](#why-is-it-needed-in-code)
    * [The Implementation Structure](#the-implementation-structure)
    * [Adapter Pattern in Java](#adapter-pattern-in-java)
      * [1. Arrays.asList() (Array to List Adapter)](#1-arraysaslist-array-to-list-adapter)
      * [2. InputStreamReader (Bytes to Characters Adapter)](#2-inputstreamreader-bytes-to-characters-adapter)
    * [Implementation of Adapter pattern](#implementation-of-adapter-pattern)
      * [The Scenario](#the-scenario-1)
      * [1. The Target Interface (Your App's Standard)](#1-the-target-interface-your-apps-standard)
      * [2. The Adaptee (The 3rd Party Library)](#2-the-adaptee-the-3rd-party-library)
      * [3. The Adapter (The Bridge)](#3-the-adapter-the-bridge)
      * [4. Client Code (The Application)](#4-client-code-the-application)
      * [Why this is realistic?](#why-this-is-realistic)
  * [Q - What is Decorator pattern?](#q---what-is-decorator-pattern)
    * [Why is it required? (The Problem with Inheritance)](#why-is-it-required-the-problem-with-inheritance)
    * [The Solution: The Decorator Pattern](#the-solution-the-decorator-pattern)
      * [How it works (The "Coffee" Analogy):](#how-it-works-the-coffee-analogy)
      * [Key Benefits](#key-benefits)
    * [Real World Example of Decorator pattern in Java](#real-world-example-of-decorator-pattern-in-java)
    * [Implementation of Decorator Pattern](#implementation-of-decorator-pattern)
      * [Why your CoffeeDecorator (Abstract Class) is important](#why-your-coffeedecorator-abstract-class-is-important)
  * [Q - What is Proxy pattern?](#q---what-is-proxy-pattern)
    * [Why is it required?](#why-is-it-required)
    * [Real-world Java proxies you already use](#real-world-java-proxies-you-already-use)
      * [Spring AOP (Most Common)](#spring-aop-most-common)
    * [Implementation of Proxy pattern](#implementation-of-proxy-pattern)
      * [The Goal](#the-goal)
      * [Step 1: Create the Annotation](#step-1-create-the-annotation)
      * [Step 2: The Business Logic (Target)](#step-2-the-business-logic-target)
      * [Step 3: The Proxy Handler (The "Magic")](#step-3-the-proxy-handler-the-magic)
      * [Step 4: The Factory (Wiring it up)](#step-4-the-factory-wiring-it-up)
      * [Step 5: Putting it all together (Demo)](#step-5-putting-it-all-together-demo)
    * [Key Components in the program](#key-components-in-the-program)
      * [1. Target (Real Object)](#1-target-real-object)
      * [2. Proxy Object](#2-proxy-object)
      * [3. InvocationHandler](#3-invocationhandler)
    * [Proxy.newProxyInstance(...) — Core API](#proxynewproxyinstance--core-api)
      * [Parameter-by-Parameter Breakdown](#parameter-by-parameter-breakdown)
    * [What Happens When proxyService.pay() Is Called](#what-happens-when-proxyservicepay-is-called)
    * [Object Creation Summary](#object-creation-summary)
    * [Interview One-Liners](#interview-one-liners)
  * [Q - What is Strategy pattern?](#q---what-is-strategy-pattern)
    * [Implementation of Strategy pattern](#implementation-of-strategy-pattern)
      * [1. The Strategy Interface](#1-the-strategy-interface)
      * [2. The Concrete Strategies](#2-the-concrete-strategies)
      * [3. The Context (The Shopping Cart)](#3-the-context-the-shopping-cart)
      * [4. Usage (Swapping behavior at runtime)](#4-usage-swapping-behavior-at-runtime)
  * [Q - What is Chain of Responsibility (COR) design Pattern](#q---what-is-chain-of-responsibility-cor-design-pattern)
<!-- TOC -->

## Q - What are different categories of design patterns?

Design patterns are categorized into three main groups based on the type of problem they solve:

### 1. Creational Patterns

These patterns deal with **Object Creation**. They help create objects in a manner suitable 
to the situation, hiding the logic of how exactly the object is created (so you aren't spamming `new` everywhere).

* Goal: "How do I instantiate this class comfortably?"
* Examples:
    * **Singleton:** Ensures a class has only one instance (e.g., Database connection).
    * **Factory Method:** Creates objects without specifying the exact class (e.g., `Calendar.getInstance()`).
    * **Builder:** Constructs complex objects step-by-step (e.g., `StringBuilder` or a Pizza builder).


### 2. Structural Patterns

These patterns deal with **Class & Object Composition**. They show you how to assemble different   
classes into larger structures while keeping them flexible.

* Goal: "How do I make these different classes work together?"
* Examples:
   * **Adapter:** Makes incompatible interfaces work together (e.g., Power Adapter).
   * **Decorator:** Dynamically adds behavior to an object (e.g., Adding "Scrollbars" to a Window).
   * **Facade:** Provides a simplified interface to a complex system (e.g., A "Car Start" button that 
internally handles fuel, spark, engine).

### 3. Behavioral Patterns

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



------------------------------------


## Q - How to implement Singleton Design Pattern

A Singleton class is a design pattern in object-oriented programming in which only one
instance of the class can ever exist during the lifetime of an application.

**Approach 1**

A simple implementation of Singleton class:

```java
class Config {
    private static volatile Config instance;

    private int id;
    protected String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Config(int id, String name) {
        this.id = id;
        this.name = name;
    }

    synchronized public static Config getInstance() {
        if (null == instance) {
            instance = new Config(1, "Default");
        }
        return instance;
    }
}

public class Example12 {
    public static void main(String[] args) {
        Config instance1 = Config.getInstance();
        Config instance2 = Config.getInstance();
        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

**Output:**

```text
org.example.sec02.Config@58372a00
org.example.sec02.Config@58372a00
```

This implementation is simple and it works but the biggest drawback is:

After the instance is created, every call still has to acquire the lock on the method, which makes:

* Access slower
* Unnecessary synchronization after the object already exists

This is why the simple synchronized Singleton is safe but considered inefficient.

Solution is to use **Double-Checked Locking with volatile keyword**


-------


**Approach 2**

```java
class Config {
    private static volatile Config instance;

    private int id;
    protected String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Config(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public static Config getInstance() {
        if (null == instance) {                 // 1st check
            synchronized (Config.class){
                if (null == instance) {         // 2nd check
                    instance = new Config(1, "Default");
                }
            }
        }
        return instance;
    }
}

public class Example12 {
    public static void main(String[] args) {
        Config instance1 = Config.getInstance();
        Config instance2 = Config.getInstance();
        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

Here is why how double check works and why `volatile` is needed:

### Sequence of events (step-by-step)

Initial state:

```text
instance = null
```

1\. `T1` checks first `if (instance == null)` (1st check)

Result → true

`T1` proceeds toward the synchronized block.


2\. `T2` checks first `if (instance == null)` (1st check)

Result → true

`T2` ALSO proceeds toward the synchronized block.

Both threads think they should create the instance.


3. `T1` acquires the lock first

`T1` enters:

```text
instance = new Config(...);
```

Instance is now created.

4. `T1` releases the lock

5. `T2` now acquires the lock

`T2` enters, the second check:

```text
synchronized (Config.class){
    if (null == instance) {         // 2nd check
```

Since `instance` is now not `null`, `T2` skips instance creation and returns the same instance created by `T1`.

Had there been no second check, `T2` would have created another instance overwriting the one `T1` created.

This is why **double-checked locking** is needed.

### Why volatile is required?

The line `instance = new Config(1, "Default");` looks like one instruction, but at the bytecode level, it is actually three:

1. `memory = allocate()` (Allocate memory for the object)
2. `ctorInstance(memory)` (Initialize the object / Run constructor)
3. `instance = memory` (Assign the reference to the static field)

Without volatile, the JIT compiler or CPU is allowed to reorder
instructions 2 and 3 for optimization (Instruction Reordering).

If the order becomes **1 -> 3 -> 2**:

* **Thread A** executes 1 and 3. The instance variable is now non-null, but the object is not initialized yet.
* **Thread A** gets paused.
* **Thread B** comes in, hits Check 1 (`if instance == null`), sees it is not `null`, and returns the instance.
* **Thread B** tries to use the object and crashes (or sees invalid null values for internal fields)
  because Step 2 (Constructor) hasn't happened yet.

With `volatile`: It creates a Memory Barrier (specifically a "Happens-Before" relationship).
It prevents the write to instance from being reordered with the initialization of the object.
It ensures the write to memory is visible to all other threads only after the constructor has finished.



-----------------------------


## Q - How to implement Factory Pattern?

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

## Q - How to implement Abstract Factory Pattern?

A very common, realistic scenario for backend engineering is Multi-Cloud Support.

Imagine you are building an application that needs to be deployed on AWS (Amazon) or GCP (Google Cloud).

* You need Storage (S3 vs. Google Cloud Storage).
* You need Compute (EC2 vs. Google Compute Engine).

**The Rule:** If you are running on AWS, your app must use both S3 and EC2.
You cannot accidentally mix an AWS Server with Google Storage. The Abstract Factory ensures this consistency.


### 1. The Abstract Products (The Interfaces)

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

### 2. The Concrete Products (The "Family" Members)

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

### 3. The Abstract Factory

This interface defines the "Family" creation. Note that it creates both storage and compute.

```java
interface CloudFactory {
    Storage createStorage();
    Instance createInstance();
}
```

### 4. The Concrete Factories

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

### 5. The Client Code (Realistic Usage)

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

#### 1. The Configuration (application.properties)

This is the only thing you change to switch providers.

```text
# Toggle this between AWS or GOOGLE
app.cloud.provider=AWS
```

#### 2. The Abstract Factory & Implementations

(Assuming these interfaces and classes exist from our previous step).

```java
public interface CloudFactory {
    Storage createStorage();
    Instance createInstance();
}

public class AwsFactory implements CloudFactory { ... }
public class GoogleFactory implements CloudFactory { ... }
```

#### 3. The DI Configuration Class

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

#### 4. The Client Code (Your Service)

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

#### Summary of Execution

* **App Start:** Spring reads application.properties → sees AWS.
* **Config Phase:** Spring activates awsFactory() bean and ignores googleFactory().
* **Injection Phase:** Spring finds `BackendService`. It sees the constructor needs `CloudFactory`.
  It plugs in the AwsFactory object it just created.
* **Runtime:** BackendService runs on AWS infrastructure.


---------------------


## Q - Diff b/w Factory pattern and Abstract factory pattern

The main difference is the scope of what they create.

* **Factory Method:** Creates one specific type of object (e.g., "Make me a Button").
* **Abstract Factory:** Creates a family of related objects that belong together 
(e.g., "Make me a whole Windows-style UI," which includes a Windows Button, 
a Windows Checkbox, and a Windows Scrollbar).

Here are a few examples:

### Examples of Factory Pattern

Both `java.nio.file.Paths` and `java.nio.channels.Selector` are excellent, real-world examples of the 
Factory Pattern (specifically the Static Factory Method).

They perfectly demonstrate the main goal of the Factory pattern: 
**"Hiding the logic of creation and the specific implementation class from the user."**

#### 1. The Paths API Example

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

#### 2. The Selector.open() Example

This is an even stronger example of a factory pattern.

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

### Examples of Abstract Factory Pattern

The best real-world example for Abstract Factory in Java is JDBC (Java Database Connectivity).

#### 1. The JDBC Example (java.sql.Connection)

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



---------------------



## Q - How to implement Builder Pattern

A creational design pattern that lets you construct complex objects step-by-step.
It allows you to produce different types and representations of an object using the same construction code.

```java
package practice;

public class User {
	private final String userName;
	private final String email;
	private final int age;

	private User(Builder builder) {
		this.userName = builder.userName;
		this.email = builder.email;
		this.age = builder.age;
	}

	public String getUserName() {
		return userName;
	}

	public String getEmail() {
		return email;
	}

	public int getAge() {
		return age;
	}

	public static class Builder {
		private String email;
		private String userName;
		private int age;

		public Builder() {
		}

		public Builder email(String email) {
			this.email = email;
			return this;
		}

		public Builder userName(String userName) {
			this.userName = userName;
			return this;
		}

		public Builder age(int age) {
			this.age = age;
			return this;
		}

		public User build() {
			return new User(this);
		}
	}

}


class Tmp {
	public static void main(String[] args) {
		// Case 1: Building with all properties chained
		User user1 = new User.Builder()
				.userName("john_doe")
				.email("john@example.com")
				.age(28)
				.build();

		// Case 2: Building with optional fields omitted
		User user2 = new User.Builder()
				.userName("alice_w")
				.email("alice@example.com")
				.build();

		System.out.println("User 1: " + user1);
		System.out.println("User 2: " + user2);
		System.out.println("User 1 Age: " + user1.getAge());
}
```

----------------------------------


## Q - What is Prototype pattern?

The Prototype Pattern allows you to create new objects by **copying an existing object** rather
than creating a new one from scratch.

This is useful when object creation is **expensive** (e.g., database calls, complex calculations) or
repetitive (setting many default values).

### Implementation Requirement

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


-----------------------


## Q - What is Adapter pattern?

The Adapter Pattern allows objects with incompatible interfaces to collaborate.
It acts as a bridge between two objects that otherwise couldn't work together.

### The Best Analogy: The Power Plug

* **The Problem:** You have a laptop with a **US Plug** (The Client).
* **The Obstacle:** You are in a hotel in London, and the wall has a **UK Socket** (The Incompatible Service).
* **The Solution:** You use a **Travel Adapter**. It takes the US plug on one side and fits into the UK socket on the other.

### Why is it needed in code?

1. **Legacy Code Integration:** You have an old system that expects data in `XML` format, but your new
   modern library returns `JSON`. You write an adapter to convert JSON to XML on the fly.

2. **3rd Party Libraries:** You want to use a fancy charting library, but its methods (`drawGraph(x, y)`) don't match
   the interface your app uses (`render(Coordinate c)`). You wrap the library in an adapter.

### The Implementation Structure

There are 3 main players:

* **Target (Client Interface):** What your code expects to see.
* **Adaptee (Incompatible Class):** The useful class you want to use, but can't directly.
* **Adapter:** The wrapper class that translates calls.

### Adapter Pattern in Java

Here are the two most famous examples of the Adapter Pattern inside the Java Standard Library (JDK).
You have likely used them without realizing they were adapters.


#### 1. Arrays.asList() (Array to List Adapter)

* **The Problem:** You have a legacy Array (`String[]`), but your API requires a `List<String>`.
  Arrays and Lists have completely different interfaces (e.g., arrays use `.length`, lists use `.size()`).
* **The Adapter:** `Arrays.asList()` acts as the bridge. It wraps the array and makes it look and behave like a List.


```java
String[] namesArray = {"John", "Steve", "Mike"}; // Legacy Array

// ❌ You can't pass an array to a method expecting a List
// printList(namesArray); // Compile Error

// ✅ The Adapter: Wraps the array inside a List interface
List<String> namesList = Arrays.asList(namesArray);

// Now it works!
System.out.println(namesList.get(0)); // Output: John
```

**Note:** This is a special adapter where changes to the List actually modify the original Array (passed by reference).


#### 2. InputStreamReader (Bytes to Characters Adapter)

* **The Problem:** The `System.in` stream (keyboard input) provides Bytes. But Java's `BufferedReader`
  (which reads lines of text) expects Characters. Bytes and Characters are incompatible types.
* The Adapter: `InputStreamReader`. It sits in the middle, translating byte streams into character streams.

```java
// 1. The Source (Bytes)
InputStream input = System.in; 

// 2. The Adapter (Bytes -> Characters)
// "Adapt this byte stream into a character reader"
InputStreamReader adapter = new InputStreamReader(input);

// 3. The Client (Expects Characters)
BufferedReader reader = new BufferedReader(adapter);
```

### Implementation of Adapter pattern

Here is a highly realistic example that happens in almost every enterprise application: **Payment Gateway Integration**.

#### The Scenario

Your e-commerce application is built to accept payments.
You have a standard interface `PaymentProcessor` that your checkout page uses.

* **The Problem:** You want to add PayPal support.
* **The Constraint:** The PayPal SDK is a 3rd-party library (jar file). **You cannot change their code**.
  Their method names and parameters are completely different from your interface.
  * Your App expects: pay(amount)
  * PayPal requires: sendPayment(amount, currency, apiKey)

This is the perfect use case for an **Adapter**.

#### 1. The Target Interface (Your App's Standard)

This is the interface your `CheckoutService` talks to. It doesn't know about PayPal or Stripe specifically.

```java
public interface PaymentProcessor {
    void pay(double dollars);
}
```

#### 2. The Adaptee (The 3rd Party Library)

Imagine this class comes from a library you downloaded (`paypal-sdk.jar`). You cannot edit this file.
Notice the incompatible method signature (different name, different units/currency).

```java
// "Adaptee" - We can't change this code!
public class PayPalApi {
    public void sendPayment(double amount, String currency) {
        System.out.println("PayPal: Processing payment of " + amount + " " + currency);
    }
}
```


#### 3. The Adapter (The Bridge)

This class implements your interface, but internally translates the call to the PayPal way of doing things.

```java
public class PayPalAdapter implements PaymentProcessor {
    // 1. Hold a reference to the incompatible object
    private final PayPalApi payPalApi;

    public PayPalAdapter(PayPalApi payPalApi) {
        this.payPalApi = payPalApi;
    }

    @Override
    public void pay(double dollars) {
        // 2. Translate the call!
        // Your app sends dollars, but PayPal API needs a currency code too.
        // We handle that translation logic here.
        payPalApi.sendPayment(dollars, "USD");
    }
}
```

#### 4. Client Code (The Application)

Your main application logic (`CheckoutService`) stays clean. It keeps calling `.pay()`, blissfully unaware
that it's actually talking to PayPal.

```java
public class ECommerceApp {
    public static void main(String[] args) {
        // 1. The legacy/incompatible object
        PayPalApi payPal = new PayPalApi();

        // 2. The Adapter makes it look like a "PaymentProcessor"
        PaymentProcessor paymentProcessor = new PayPalAdapter(payPal);

        // 3. The app calls the standard method
        // It doesn't care that internally it's converting to "USD"
        paymentProcessor.pay(50.00); 
    }
}
```

#### Why this is realistic?

* **Vendor Lock-in Prevention:** If next year you want to switch from PayPal to Stripe, you just write a `StripeAdapter`.
  You don't have to find-and-replace code in 500 places in your app.
* Data Transformation: Often adapters do more than just forward calls; they convert data.
  * Example: Your app tracks temperature in Celsius, but the US-Weather-Service API returns Fahrenheit.
    The Adapter would perform the math `(F - 32) * 5/9` inside the method.


-------------------------------------


## Q - What is Decorator pattern?

The Decorator Pattern is a structural design pattern that allows you to add new functionality to an 
existing object without altering its structure.

Think of it as "Wrapping". You take a basic object and wrap it in layers, like an onion or a set of 
Russian nesting dolls. Each layer adds a new behavior.

* Official Definition: It attaches additional responsibilities to an object dynamically. 
Decorators provide a flexible alternative to subclassing for extending functionality.

### Why is it required? (The Problem with Inheritance)

The main reason we need the Decorator pattern is to avoid "Class Explosion" (also known as "Inheritance Hell").

Imagine you are building a software system for a **Coffee Shop**.

**Attempt 1:** Using Inheritance (The Bad Way) You start with a Coffee class. 
You need to support every combination of condiments.

1. Espresso
2. EspressoWithMilk
3. EspressoWithSugar
4. EspressoWithMilkAndSugar
5. EspressoWithDoubleMochaAndWhip...

If you have 4 types of coffee and 5 types of toppings, you would need dozens of different classes to 
cover every possible combination. If you add a new topping (e.g., "Soy Milk"), you have to create 
a whole new set of classes (EspressoWithSoy, DecafWithSoy, etc.). This is unmaintainable.

### The Solution: The Decorator Pattern

Instead of creating a new class for every combination, you create one class for the base coffee and 
separate classes for the "toppings" (Decorators).

You then combine them at runtime.

#### How it works (The "Coffee" Analogy):

1. Base Object: You create a simple Espresso object.
    * Cost: $2.00

2. Decorator 1: You wrap it in a Milk decorator.
    * Cost: $2.00 + $0.50

3. Decorator 2: You wrap that in a Sugar decorator.
    * Cost: ($2.00 + $0.50) + $0.20


You end up with a **Sugar(Milk(Espresso))** object.

#### Key Benefits

* **Flexibility:** You can mix and match behaviors at runtime. You don't need to decide the 
exact combination at compile time.
* **Single Responsibility Principle:** You divide a monolithic class (that does everything) into 
small classes that each do one specific thing (one handles "Milk", one handles "Sugar").
* **Open/Closed Principle:** You can add a new decorator (e.g., CaramelSyrup) without touching the
existing Espresso or Milk code.

### Real World Example of Decorator pattern in Java

The Java I/O library is the most famous example of this pattern.

* `FileInputStream` (The Base: reads bytes from a file).
* `BufferedInputStream` (Decorator 1: adds memory buffering for speed).
* `GZipInputStream` (Decorator 2: adds decompression).

You combine them like this:

```java
// A GZipped, Buffered File Reader
// We are wrapping the file stream in layers
InputStream stream = new GZipInputStream(
                        new BufferedInputStream(
                            new FileInputStream("data.txt.gz")
                        )
                     );
```

### Implementation of Decorator Pattern

Here is a clean implementation of the Decorator pattern using the **Coffee Shop** example.

**1. The Common Interface**

This defines the blueprint for both the "Base Object" and the "Decorators". They must look the same to the outside world.

```java
public interface Coffee {
    String getDescription();
    double getCost();
}
```

**2. The Concrete Component (The Base Object)**

This is the object we start with (e.g., a plain, black coffee).

```java
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.00; // Base price
    }
}
```

**3. The Abstract Decorator**

This is the "Wrapper". It implements the `Coffee` interface (so it is a Coffee) but also 
holds a reference to another `Coffee` object (composition).

```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee; // The object we are wrapping

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    // Default behavior: just forward the call to the wrapped object
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

**4. The Concrete Decorators (The Toppings)**

These extend the abstract decorator and add their own special behavior (cost/description).

```java
// Decorator 1: Milk
class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return super.getCost() + 1.50; // Adds cost
    }
}

// Decorator 2: Sugar
class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.50; // Adds cost
    }
}
```

**5. Client Code (Putting it together)**

Notice how we wrap the objects inside each other.

```java
public class CoffeeShop {
    public static void main(String[] args) {
        // 1. Order a plain coffee
        Coffee myCoffee = new SimpleCoffee();
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());

        // 2. Add Milk (Wrap the coffee in Milk)
        myCoffee = new Milk(myCoffee);
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());

        // 3. Add Sugar (Wrap the Milk-Coffee in Sugar)
        myCoffee = new Sugar(myCoffee);
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());
    }
}
```

**Output:**

```text
Simple Coffee $5.0
Simple Coffee, Milk $6.5
Simple Coffee, Milk, Sugar $7.0
```

#### Why your CoffeeDecorator (Abstract Class) is important

You might wonder: "Why do I need this abstract class? Can't Milk just implement Coffee directly?"

You could do that, but the abstract class saves you from code duplication.

* **Without it:** Both `Milk` and `Cream` would have to manually write the code to store the `coffee` object 
and manually write the constructor to set it.
* **With it:** The abstract class handles the "boilerplate" (storage and delegation), so your 
concrete decorators (Milk, Cream) only focus on the new behavior (adding cost/text).

This is a very clean implementation!


-----------------------------------


## Q - What is Proxy pattern?

The **Proxy Pattern** is a structural design pattern where you provide a substitute or placeholder for another object. 
A proxy controls access to the original object, allowing you to perform something either before or after the request 
gets to the original object.

### Why is it required?

The main purpose of a Proxy is Control. You generally use it when you want to add 
functionality (like security, logging, or lazy loading) without changing the actual object's code.

### Real-world Java proxies you already use

#### Spring AOP (Most Common)

When you use:

* @Transactional
* @Async
* @Cacheable
* @Lazy
* @Secured

👉 Spring creates a proxy behind the scenes

Example:

```java
@Service
public class OrderService {

    @Transactional
    public void placeOrder() {
        // business logic
    }
}
```

What actually happens at runtime:

```text
Controller → Proxy → Transaction logic → Real OrderService → Commit/Rollback
```

You are **not calling OrderService directly**. You are calling a **proxy object**.


### Implementation of Proxy pattern

We will implement a custom `@Transactional` annotation that automatically starts and commits transactions 
without touching your business logic. This uses JDK Dynamic Proxies, the exact same mechanism Spring uses for interfaces.

#### The Goal

We want to write code like this:

```java
// The Developer writes this:
@MyTransactional
public void pay() {
    System.out.println("Processing Payment...");
}

// But at runtime, it automatically does this:
// -> "Starting Transaction..."
// -> "Processing Payment..."
// -> "Committing Transaction..."
```

#### Step 1: Create the Annotation

First, we need a marker to tell our framework which methods need a transaction.

```text
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME) // Keep this around at runtime!
public @interface MyTransactional {
}
```

#### Step 2: The Business Logic (Target)

For JDK Dynamic Proxies to work, your **class must implement an interface**.

```text
// 1. The Interface
interface PaymentService {
    void pay();
}

// 2. The Real Implementation (No transaction code here!)
class PaymentServiceImpl implements PaymentService {
    @Override
    @MyTransactional
    public void pay() {
        System.out.println(">> Business Logic: Sending money to Merchant...");
    }
}
```

#### Step 3: The Proxy Handler (The "Magic")

This is where the AOP logic lives. This class intercepts every method call.

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class TransactionHandler implements InvocationHandler {
    private final Object target; // The Real Object

    public TransactionHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 1. Check if the method has our annotation
        // We must check the implementation class, not the interface method
        Method realMethod = target.getClass().getMethod(method.getName(), method.getParameterTypes());
        
        if (realMethod.isAnnotationPresent(MyTransactional.class)) {
            // --- AROUND ADVICE START ---
            System.out.println("[Tx] BEGIN Transaction (Connect to DB)");
            
            try {
                // 2. Call the Real Method
                Object result = method.invoke(target, args);
                
                // 3. Success? Commit!
                System.out.println("[Tx] COMMIT Transaction");
                return result;
                
            } catch (Exception e) {
                // 4. Error? Rollback!
                System.out.println("[Tx] ROLLBACK (Something went wrong)");
                throw e;
            }
            // --- AROUND ADVICE END ---
        } else {
            // If no annotation, just run the method normally
            return method.invoke(target, args);
        }
    }
}
```

#### Step 4: The Factory (Wiring it up)

You need a helper to generate the proxy object. In Spring, the `ApplicationContext` does this for you.

```java
import java.lang.reflect.Proxy;

public class ProxyFactory {
    public static <T> T createProxy(T target, Class<T> interfaceType) {
        return (T) Proxy.newProxyInstance(
            interfaceType.getClassLoader(),
            new Class<?>[] { interfaceType },
            new TransactionHandler(target) // Use our handler
        );
    }
}
```

#### Step 5: Putting it all together (Demo)

```java
public class Main {
    public static void main(String[] args) {
        // 1. Create the Real Object
        PaymentServiceImpl realService = new PaymentServiceImpl();

        // 2. Create the Proxy (This is what Spring does @Autowired)
        // We cast it to the Interface, NOT the class
        PaymentService proxyService = ProxyFactory.createProxy(realService, PaymentService.class);

        // 3. Call the method
        // Notice we are calling the PROXY, not the real service
        System.out.println("--- Client calling pay() ---");
        proxyService.pay();
    }
}
```

**Output**

When you run this, you will see the interception happening:

```text
--- Client calling pay() ---
[Tx] BEGIN Transaction (Connect to DB)
>> Business Logic: Sending money to Merchant...
[Tx] COMMIT Transaction
```


### Key Components in the program

#### 1. Target (Real Object)

```java
PaymentServiceImpl realService = new PaymentServiceImpl();
```

* Contains business logic
* Always created explicitly
* Exists independently of the proxy

#### 2. Proxy Object

Created using:

```java
Proxy.newProxyInstance(...)
```

* A separate JVM object
* Generated at runtime
* Implements the same interface as the target
* Delegates calls via an `InvocationHandler`

#### 3. InvocationHandler

```java
class TransactionHandler implements InvocationHandler
```

* Intercepts every method call on the proxy
* JVM forwards calls here instead of calling the real method directly
* Responsible for:
    * Pre-processing
    * Calling the real method
    * Post-processing
    * Exception handling

### Proxy.newProxyInstance(...) — Core API

```java
Object Proxy.newProxyInstance(
    ClassLoader loader,
    Class<?>[] interfaces,
    InvocationHandler handler
)
```

This method:

* Generates a new proxy class at runtime
* Loads it using the given ClassLoader
* Creates an instance of that class
* Returns the proxy object

#### Parameter-by-Parameter Breakdown

**1. ClassLoader loader**

```java
interfaceType.getClassLoader()
```

Purpose:

* Specifies **which ClassLoader loads the generated proxy class**

Key points:

* Every class/interface in Java is loaded by a ClassLoader
* Interfaces are JVM types just like classes
* The `Class<?>` object stores a reference to its loader

> Bootstrap loader appears as null (e.g., String.class.getClassLoader())

**2. Class<?>[] interfaces**

```java
new Class<?>[] { PaymentService.class }
```

Purpose:
* Defines **what the proxy pretends to be**

Rules:
* Only interfaces are allowed (JDK dynamic proxies)
* Can implement multiple interfaces
* Proxy will NOT extend the implementation class

Conceptually generated code:

```java
class $Proxy0 implements PaymentService {
    InvocationHandler handler;

    public void pay() {
        handler.invoke(this, payMethod, null);
    }
}
```

**3. InvocationHandler handler**

```java
new TransactionHandler(target)
```

Purpose:

* Central interception point
* JVM routes all proxy method calls to invoke()

When this runs:

```java
proxyService.pay();
```

JVM internally does:

```java
handler.invoke(proxy, method, args);
```

**Important:**

* Handler is created once
* Reused for every method call
* Not created per invocation

### What Happens When proxyService.pay() Is Called

Execution flow:

```text
Client
  ↓
proxyService.pay()
  ↓
Generated Proxy ($Proxy0)
  ↓
InvocationHandler.invoke()
  ↓
PaymentServiceImpl.pay()
```

* JVM never calls the real method directly
* All calls pass through `invoke()`

### Object Creation Summary

```text
1 real object        → PaymentServiceImpl
1 proxy object       → $Proxy0
1 handler object     → TransactionHandler
```

* Proxy is created once
* Handler is created once
* Method calls are intercepted repeatedly

### Interview One-Liners

What does `Proxy.newProxyInstance` do?
> It dynamically generates a class that implements the given interfaces, loads it using the 
> specified classloader, and delegates method calls to an `InvocationHandler`.

Does Java create a new proxy per call?
> No. The proxy is created once and reused. Only invoke() runs per call.

Who handles method calls on a proxy?
> The InvocationHandler.


------------------------------------


## Q - What is Strategy pattern?

Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable 
at runtime without changing the client code.

### Implementation of Strategy pattern

You want to charge a customer, but they might pay with a Credit Card, PayPal, or Crypto.

#### 1. The Strategy Interface

This defines the contract. All strategies must obey this.

```java
public interface PaymentStrategy {
    void pay(int amount);
}
```

#### 2. The Concrete Strategies

These are the different ways to perform the action.

```java
public class CreditCardStrategy implements PaymentStrategy {
    private String cardNumber;

    public CreditCardStrategy(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card: " + cardNumber);
    }
}

public class PayPalStrategy implements PaymentStrategy {
    private String email;

    public PayPalStrategy(String email) {
        this.email = email;
    }

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal: " + email);
    }
}
```

#### 3. The Context (The Shopping Cart)

This is the main class. It doesn't know how payment happens, it just knows that it happens. 
It relies on the interface.

```java
public class ShoppingCart {
    // The Context holds a reference to the interface
    private PaymentStrategy paymentStrategy;

    // You can set the strategy at runtime!
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        if (paymentStrategy == null) {
            System.out.println("Please select a payment method!");
        } else {
            paymentStrategy.pay(amount); // Polymorphism in action
        }
    }
}
```

#### 4. Usage (Swapping behavior at runtime)

```java
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // User chooses Credit Card
        cart.setPaymentStrategy(new CreditCardStrategy("1234-5678"));
        cart.checkout(100);

        // User changes mind to PayPal
        // Note: We changed behavior without changing the ShoppingCart code!
        cart.setPaymentStrategy(new PayPalStrategy("jdev.prateek@gmail.com"));
        cart.checkout(200);
    }
}
```


------------------------------------


## Q - What is Chain of Responsibility (COR) design Pattern

Chain of Responsibility (CoR) is a Behavioral Design Pattern that passes a request along a chain of 
potential handlers until one (or more) handlers process the request or the chain ends.

It completely decouples the sender of a request from its receivers, giving multiple objects a chance
to handle the request dynamically at runtime.

```java
interface RequestFilter {
	void handle(String request);

	RequestFilter setNext(RequestFilter next);
}

abstract class AbstractRequestFilter implements RequestFilter {
	protected RequestFilter next;

	public AbstractRequestFilter() {
	}

	public RequestFilter setNext(RequestFilter next) {
		if (this.next == null) {
			this.next = next;
		} else {
			this.next.setNext(next);
		}
		return next;
	}

	@Override
	public void handle(String request) {
		if (next != null) {
			next.handle(request);
		}
	}
}

class IpFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

class AuthenticationFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

class GatewayFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

public class Tmp {
	public static void main(String[] args) {
		RequestFilter requestFilter = new IpFilter();
		requestFilter.setNext(new AuthenticationFilter());
		requestFilter.setNext(new GatewayFilter());


		requestFilter.handle("1234");
	}
}
```