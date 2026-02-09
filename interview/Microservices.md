<!-- TOC -->
* [Q-1 Types of caches](#q-1-types-of-caches)
    * [Write through cache](#write-through-cache)
    * [Write around cache](#write-around-cache)
    * [Write back cache](#write-back-cache)
* [Q-2 Difference between Coupling and Cohesion?](#q-2-difference-between-coupling-and-cohesion)
  * [COHESION](#cohesion)
  * [COUPLING](#coupling)
* [Q-3 What are common microserivces design pattern?](#q-3-what-are-common-microserivces-design-pattern)
  * [1. Decomposition Patterns (How to break the Monolith)](#1-decomposition-patterns-how-to-break-the-monolith)
    * [A. Strangler Fig Pattern](#a-strangler-fig-pattern)
    * [B. Decompose by Subdomain (DDD)](#b-decompose-by-subdomain-ddd)
  * [2. Integration Patterns (How services talk)](#2-integration-patterns-how-services-talk)
    * [A. API Gateway Pattern](#a-api-gateway-pattern)
    * [B. Aggregator Pattern](#b-aggregator-pattern)
  * [3. Database Patterns (The hardest part)](#3-database-patterns-the-hardest-part)
    * [A. Database per Service](#a-database-per-service)
    * [B. Saga Pattern (Distributed Transactions)](#b-saga-pattern-distributed-transactions)
    * [C. CQRS (Command Query Responsibility Segregation)](#c-cqrs-command-query-responsibility-segregation)
  * [4. Resilience Patterns (Don't let one crash kill everything)](#4-resilience-patterns-dont-let-one-crash-kill-everything)
    * [A. Circuit Breaker](#a-circuit-breaker)
    * [B. Bulkhead Pattern](#b-bulkhead-pattern)
    * [Summary for the Interview (The "Must-Haves")](#summary-for-the-interview-the-must-haves)
* [Q-4 When should I use an interface vs an abstract class while designing a file uploader with multiple implementations (e.g., S3, GCP)?](#q-4-when-should-i-use-an-interface-vs-an-abstract-class-while-designing-a-file-uploader-with-multiple-implementations-eg-s3-gcp)
  * [Use an INTERFACE when the goal is “capability” or “contract”](#use-an-interface-when-the-goal-is-capability-or-contract)
  * [When to use ABSTRACT CLASS instead](#when-to-use-abstract-class-instead)
* [Q-5 Explain CQRS Pattern](#q-5-explain-cqrs-pattern)
  * [CQRS (Command Query Responsibility Segregation)](#cqrs-command-query-responsibility-segregation)
  * [The Scenario: "The PlayStation 5 Launch"](#the-scenario-the-playstation-5-launch)
    * [1. The "Insanity" of the Normal Approach (CRUD)](#1-the-insanity-of-the-normal-approach-crud)
    * [2. The "Sanity" of CQRS (The Fix)](#2-the-sanity-of-cqrs-the-fix)
  * [3. The Trade-off (The Glue)](#3-the-trade-off-the-glue)
  * [Summary](#summary)
* [Q-6 Explain Bulkhead Pattern](#q-6-explain-bulkhead-pattern)
  * [1. The Real-World Analogy: "The Unsinkable Ship"](#1-the-real-world-analogy-the-unsinkable-ship)
  * [2. The Problem: "Resource Exhaustion" (The Sinking Ship)](#2-the-problem-resource-exhaustion-the-sinking-ship)
  * [3. The Solution: The Bulkhead Pattern](#3-the-solution-the-bulkhead-pattern)
  * [4. Java Implementation (Resilience4j)](#4-java-implementation-resilience4j)
* [Q-7 Explain Circuit Breaker pattern](#q-7-explain-circuit-breaker-pattern)
  * [1. The Real-World Analogy: "The Fuse Box"](#1-the-real-world-analogy-the-fuse-box)
  * [2. The Problem: "Cascading Failure" ( The Domino Effect)](#2-the-problem-cascading-failure--the-domino-effect)
  * [3. The Solution: The State Machine](#3-the-solution-the-state-machine)
    * [A. CLOSED (Normal Operation)](#a-closed-normal-operation)
    * [B. OPEN (The "Cut-Off")](#b-open-the-cut-off)
    * [C. HALF-OPEN (The "Test")](#c-half-open-the-test)
  * [4. Java Implementation (Resilience4j)](#4-java-implementation-resilience4j-1)
* [Q-8 Explain Retry pattern](#q-8-explain-retry-pattern)
  * [1. The Real-World Analogy: "Bad Cell Reception"](#1-the-real-world-analogy-bad-cell-reception)
  * [2. The Solution: Automatic Retry](#2-the-solution-automatic-retry)
  * [3. The Danger: "The Thundering Herd" (Self-Inflicted DDoS)](#3-the-danger-the-thundering-herd-self-inflicted-ddos)
  * [4. The Fix: Exponential Backoff (The Smart Way)](#4-the-fix-exponential-backoff-the-smart-way)
    * [5. Java Implementation (Resilience4j)](#5-java-implementation-resilience4j)
* [Q-9 What is N+1 problem?](#q-9-what-is-n1-problem)
<!-- TOC -->

# Q-1 Types of caches

### Write through cache

Every write goes to both the cache and the underlying storage (memory/disk) at the same time.

### Write around cache

Writes go directly to the storage (skipping the cache). Cache is only updated on a read miss later.

### Write back cache

Writes go to the cache only at first, and are marked as "dirty". Later, the dirty data 
is flushed (written back) to storage asynchronously.


---


# Q-2 Difference between Coupling and Cohesion?

## COHESION

**What it means:**

How strongly the functions inside a single module/class are related to one another.

**High Cohesion = GOOD**

A class does one well-defined job.

**Low Cohesion = BAD**

A class does many unrelated things.

**ELI5 Example:**

A kitchen is cohesive: everything inside is related to cooking.
If you start storing clothes, toys, laptops → cohesion decreases.

**Programming Example (GOOD — High Cohesion)**

```java
class OrderService {
    void createOrder() {}
    void cancelOrder() {}
    void updateOrder() {}
}
```

All methods deal only with orders.

**Programming Example (BAD — Low Cohesion)**

```java
class Utils {
    void parseJson() {}
    void sendEmail() {}
    void writeToFile() {}
    void calculateTax() {}
}
```

This class does too many unrelated things.

## COUPLING

**What it means:**

How dependent one module/class is on another.

**Low Coupling = GOOD**

Classes know very little about each other. Changing one does not force changes in others.

**High Coupling = BAD**

Classes are tightly linked and depend on each other’s details.

**ELI5 Example:**

If your phone charger works only with one special outlet, coupling is high.
If it works with any USB outlet, coupling is low.

**Programming Example (BAD — High Coupling)**

```java
class PaymentService {
    BankApi bank = new BankApi();   // Direct dependency

    void makePayment() {
        bank.transferMoney();
    }
}
```

If BankApi changes → PaymentService breaks.

**Programming Example (GOOD — Low Coupling)**

```java
interface PaymentGateway {
    void pay();
}

class PaymentService {
    PaymentGateway gateway;
}
```

Now `PaymentService` doesn’t care about the specific implementation.

Here is the golden rule of software design: **You want Low Coupling and High Cohesion**.


---


# Q-3 What are common microserivces design pattern?

This is a massive topic. To ace this in an interview, do not just list them. **Categorize them** 
based on the problem they solve.

Here are the top 5 categories of patterns you must know, explained from the ground up.

1. Decomposition Patterns
2. Integration Patterns
3. Database Patterns
4. Resilience Patterns


---


## 1. Decomposition Patterns (How to break the Monolith)

The first challenge is: "How do we split a 10-year-old application into small pieces?"

### A. Strangler Fig Pattern

**The Problem:** You cannot rewrite a massive legacy system from scratch. It’s too risky.

**The Solution:** You create a new microservice for *one specific feature* (e.g., "Search"). You put a proxy in front.

* Calls to `/search` go to the **New Microservice**.
* Calls to `/everything-else` go to the **Old Monolith**.
* Over time, the new system "strangles" the old one until the monolith is gone.

### B. Decompose by Subdomain (DDD)

**The Problem:** "Where do I draw the lines?"

**The Solution:** Use Domain-Driven Design. Break services based on **Business Capabilities**, not technical layers.

* *Bad:* `UserDBService`, `LogicService` (Technical layers).
* *Good:* `OrderService`, `PaymentService`, `InventoryService` (Business subdomains).


---


## 2. Integration Patterns (How services talk)

Once split, these services need to communicate without creating a "spaghetti mess."

### A. API Gateway Pattern

**The Problem:** If you have 50 services, your Front End (React/Mobile) shouldn't 
  know about all 50 IP addresses. It’s a security nightmare.

**The Solution:** Put a single entry point (The Gatekeeper) in front.

* The client talks **only** to the Gateway.
* The Gateway routes the request to the correct internal service.
* **Bonus:** It handles Authentication, SSL, and Rate Limiting centrally.

### B. Aggregator Pattern

**The Problem:** To build a "Profile Page," the client needs 
data from `User`, `Orders`, and `Rewards` services. Making 3 calls from the mobile app is slow.

**The Solution:** Create a helper service (or use GraphQL on the Gateway) that calls
all 3 services, combines the data into one JSON, and sends it back in **one** response.


---


## 3. Database Patterns (The hardest part)

In a monolith, you have one big SQL DB with JOINs. In microservices, **sharing a database is a sin.**

### A. Database per Service

**The Problem:** If Service A and Service B share a DB, and Service A changes a 
table schema, Service B breaks. Tightly coupled.

**The Solution:** Each service has its **own private database**. `OrderService` cannot 
read `CustomerService`'s tables directly. It must call the API.

### B. Saga Pattern (Distributed Transactions)

**The Problem:** You need a transaction that spans multiple services.

* *Scenario:* "Place Order" -> "Deduct Inventory" -> "Charge Payment".
* If "Charge Payment" fails, you must **undo** the "Deduct Inventory" step. 
  You can't use `ROLLBACK` because they are different DBs.

**The Solution:** A sequence of local transactions.

* If a step fails, you execute a **Compensating Transaction** (a localized "Undo" command) to 
  reverse the previous steps.
* *Types:* **Choreography** (Events) vs. **Orchestration** (Central Controller).

### C. CQRS (Command Query Responsibility Segregation)

**The Problem:** "Reads" are vastly different from "Writes."

* *Write:* Complex validation (Create Order).
* *Read:* Fast lookup (Get Order History).
* Using the same model for both is inefficient.

**The Solution:** Split the application into two parts:

* **Command Side:** Handles Creates/Updates (optimized for consistency).
* **Query Side:** Handles Reads (optimized for speed, maybe using a NoSQL view).

---

## 4. Resilience Patterns (Don't let one crash kill everything)

### A. Circuit Breaker

**The Problem:** Service A calls Service B. Service B is down or slow. Service A keeps 
  waiting, threads pile up, and eventually Service A crashes too (Cascading Failure).

**The Solution:** Install a "Circuit Breaker."

* If calls to Service B fail 5 times in a row, the breaker **Trips (Opens)**.
* For the next 60 seconds, Service A **immediately fails** calls to B without waiting (Fast Fail).
* After 60 seconds, it lets one call through to check if B is back online.

### B. Bulkhead Pattern

**The Problem:** One heavy feature (e.g., Image Processing) uses up all threads/connections, starving
the critical features (e.g., Login).

**The Solution:** Isolate resources into pools (like watertight compartments in a ship).

* "Image Processing" gets a max of 10 threads.
* "Login" gets a max of 20 threads.
* If Image Processing fills up, Login is unaffected.

---

### Summary for the Interview (The "Must-Haves")

If asked **"What patterns have you used?"**, pick 3-4 you are comfortable with:

> "In my experience, the most critical patterns I've used are:
> 1. **API Gateway** for centralized routing and security.
> 2. **Database per Service** to ensure loose coupling.
> 3. **Circuit Breaker** (using Resilience4j) to prevent cascading failures.
> 4. **Saga Pattern** for handling distributed transactions like Order Processing."
>
>


---


# Q-4 When should I use an interface vs an abstract class while designing a file uploader with multiple implementations (e.g., S3, GCP)?

## Use an INTERFACE when the goal is “capability” or “contract”

In your case:

* You want to define what uploading means
* You want multiple upload providers (S3, GCP)
* You want the caller to depend only on **the abstraction**, not on any specific cloud provider

This is a classic case where an **interface** is ideal.

```java
public interface FileUploader {
    void upload(String filePath);
}

public class S3Uploader implements FileUploader {
    @Override
    public void upload(String filePath) {
        // logic for AWS S3
    }
}

public class GCPUploader implements FileUploader {
    @Override
    public void upload(String filePath) {
        // logic for GCP Storage
    }
}
```

This gives you:

* Clean separation
* No shared state
* Easy dependency injection
* Easy mocking in tests
* Easy to add new providers later

This is exactly what interfaces are for.

## When to use ABSTRACT CLASS instead

Use an abstract class only if you want to share code, state, or behavior among implementations.

Example:

If `S3Uploader` and `GCPUploader` share:

* Authorization logic
* Retry logic
* Logging

You could extract that into an abstract class:

```java
public abstract class BaseUploader {
    protected void retry(Runnable task) {
        // shared retry logic
    }
}
```

Then use:

```java
public class S3Uploader extends BaseUploader implements FileUploader {
    @Override
    public void upload(String filePath) {
        retry(() -> { /* s3 upload */ });
    }
}
```

**Short Answer**: Start with an **Interface**.

If you find yourself copying and pasting the same code (like logging or file validation) into both classes,
then introduce an Abstract Class in the middle.


---


# Q-5 Explain CQRS Pattern

Here is the concise, interview-ready introduction for your notes.

## CQRS (Command Query Responsibility Segregation)

**Definition:**
A design pattern that segregates the application into two distinct parts:
one for **Writing** data (Commands) and one for **Reading** data (Queries). 
Unlike standard CRUD, they use **different models** and often **different databases**.

**The Problem It Solves:**
In massive systems, **Reads** often outnumber **Writes** by 10,000 to 1. 
Using the same database model for both causes performance 
bottlenecks (complex JOINs lock the DB) and complex code (validation logic mixed with view logic).

**The Architecture:**

1. **Command Side ( The "Writer"):**
    * **Responsibility:** Handles Create/Update/Delete.
    * **Focus:** Complex Business Logic & Validation.
    * **Database:** Normalized **SQL** (Strict ACID consistency).
    * **Output:** Returns `void` or `ID`. Publishes an **Event** on success.

2. **Query Side (The "Reader"):**
    * **Responsibility:** Handles Reads only.
    * **Focus:** Speed & Data Projection.
    * **Database:** Denormalized **NoSQL/Cache** (e.g., Redis, ElasticSearch, Pre-calculated Views).
    * **Output:** Returns **DTOs** (Data Transfer Objects) tailored exactly for the UI.

**Data Synchronization:**

* The two sides are decoupled.
* When a **Command** updates the SQL DB, it fires an **Event** (e.g., `ProductPriceUpdated`).
* A background worker catches the event and updates the **Read DB**.
* **Trade-off:** **Eventual Consistency** (Users might see old data for a few milliseconds).

**When to Use (Sanity Check):**

* **YES:** High-traffic systems where Reads  Writes (e.g., Amazon Product Page, Social Media Feeds).
* **NO:** Simple CRUD apps (Admin panels, Blogs). The complexity overhead is not worth it.

**Key Interview Soundbite:**

> *CQRS allows us to scale Reads and Writes independently. 
> We can optimize the Write side for logic/integrity and the Read side for 
> pure speed (O(1) lookups), at the cost of eventual consistency.*

---


Here is the classic, undeniable use case where CQRS is the **only** sane option: **Amazon’s Product Page.**

If you tried to build Amazon using a standard "Monolith CRUD" approach, it would crash in seconds. Here is why.

## The Scenario: "The PlayStation 5 Launch"

Imagine the PlayStation 5 product page.

* **Writes (Sellers):** 1 seller (Sony) updates the inventory count once every few hours.
* **Reads (Buyers):** 10 million people refresh the page *every second* to see if it’s in stock.

### 1. The "Insanity" of the Normal Approach (CRUD)

In a normal app, you have one `Product` table in a SQL database.

When a user loads the page, the database has to execute a massive **JOIN**:
`SELECT * FROM Product P JOIN Reviews R ON ... JOIN Shipping S ON ... JOIN QnA Q ON ...`

* **The Problem:** You are asking the database to join 5 tables and 
  calculate dynamic pricing **10 million times per second.**
* **The Crash:** The database locks up. The CPU hits 100%. The site goes down. Sony can't even 
  update the stock because the "Readers" are blocking the "Writers."

---

### 2. The "Sanity" of CQRS (The Fix)

Amazon separates this into two completely different systems.

**System A: The Command Side (For Sony)**

* **User:** Sony Admin.
* **Action:** "Update Stock to 500."
* **Logic:**
    * Check warehouse availability.
    * Check regional pricing rules.
    * Check shipping restrictions.
    * (This is complex logic! It takes 200ms).

* **Storage:** A normalized **SQL Database** (highly consistent).
* **Output:** It publishes an event: `ProductStockUpdated`.

**System B: The Query Side (For You)**

* **User:** 10 Million Gamers.
* **Action:** "Load Product Page."
* **Logic:** **Zero.**
* **Storage:** A **NoSQL Document Store** (like DynamoDB or Redis).
    * This database does **NOT** have joined tables.
    * It has one pre-calculated JSON document called `Product_PS5_View`. It contains the 
      title, price, the top 5 reviews, and the shipping date *already baked in*.

* **The Magic:** When you load the page, it just does `GET Product_PS5_View`.
    * **Complexity:** O(1).
    * **Speed:** 2 milliseconds.
    * **Load:** It can handle 100 million requests easily because there are no JOINs and no calculations.


## 3. The Trade-off (The Glue)

So how does the `Product_PS5_View` get updated?

1. Sony updates SQL DB (Command Side).
2. **Event:** `ProductStockUpdated` is fired.
3. **Worker:** A background worker catches this event.
4. **Sync:** It updates the JSON document in the NoSQL DB (Query Side).

**The Cost:**
There is a 1-second delay.

* Sony updates stock at 12:00:00.
* You might still see "Out of Stock" at 12:00:01.
* You refresh at 12:00:02 and see "In Stock."

**Is this sane?**

* For a **Blog**? **NO.** (Insanity).
* For **Amazon**? **YES.** It is the *only* way to survive the traffic.

## Summary

CQRS is "sane" when:

1. **Reads** massively outnumber **Writes** (10,000 to 1).
2. **Reads** are fundamentally different shapes than **Writes** (e.g., Writes are SQL rows, Reads are JSON documents).
3. You can afford **Eventual Consistency** (a 1-second delay is acceptable).

---

# Q-6 Explain Bulkhead Pattern

This is one of the most intuitive patterns because it comes directly from **Shipbuilding**.

## 1. The Real-World Analogy: "The Unsinkable Ship"

A ship's hull is divided into separate watertight compartments (bulkheads).

* **If a rock hits the front:** Only the front compartment floods.
* **The Result:** The ship stays afloat because the other compartments are sealed off.
* **Without Bulkheads:** Water flows from the front to the back, and the entire ship sinks.

In Microservices, **Threads** are the water. If one service floods your app with requests, you 
don't want it to sink the whole container.

---

## 2. The Problem: "Resource Exhaustion" (The Sinking Ship)

Imagine you have a Tomcat server with **100 Threads** total. Your app has two features:

1. **Get Product Details** (Super fast, 10ms).
2. **Generate PDF Invoice** (Super slow, 5 seconds).

**The Disaster:**

* Suddenly, 100 users request "Generate PDF Invoice" at the same time.
* **Result:** All 100 Tomcat threads are now busy generating PDFs.
* **The Victim:** A new user tries to just "Get Product Details."
* **The Failure:** The server rejects them because **0 threads are free**.
* **Conclusion:** The slow "PDF" feature just killed the fast "Product" feature. The whole app is down.

---

## 3. The Solution: The Bulkhead Pattern

We artificially restrict how many resources (threads) each feature can use.

We split the 100 Tomcat threads into distinct pools:

* **Pool A (Product Details):** Max 60 threads.
* **Pool B (PDF Invoice):** Max 40 threads.

**The New Scenario:**

* 100 users request "Generate PDF Invoice."
* The first 40 get a thread from **Pool B**.
* The other 60 are immediately rejected (Fast Fail). **Pool B is full.**
* **Meanwhile:** A user requests "Get Product Details."
* **Result:** **Success!** Pool A still has 60 threads completely free. The "ship" (app) is still floating, even though one compartment (PDFs) is flooded.

---

## 4. Java Implementation (Resilience4j)

In Spring Boot, we use the `@Bulkhead` annotation to enforce this.

```java
@Service
public class InvoiceService {

    // LIMIT: Only 5 concurrent calls allowed for this specific method
    @Bulkhead(name = "invoiceService", type = Bulkhead.Type.SEMAPHORE, maxConcurrentCalls = 5)
    public byte[] generateInvoice(String orderId) {
        // Heavy logic taking 5 seconds...
        return pdfBytes;
    }
}

```

* If a 6th thread tries to call `generateInvoice`, it gets a `BulkheadFullException` immediately. It does not wait.
* Your other services (`ProductService`) are completely unaffected.

---

# Q-7 Explain Circuit Breaker pattern

Here is the **Circuit Breaker Pattern**, explained with the same structure.

## 1. The Real-World Analogy: "The Fuse Box"

In your house, if a toaster shorts out, the **Circuit Breaker** flips (trips).

* **Result:** The power to that specific outlet is cut off instantly.
* **Why?** To prevent the wires in the wall from overheating and burning down the entire house.
* **The Fix:** You fix the toaster, then you manually flip the switch back to "On."

In Microservices, **Network Calls** are the electricity. If one service 
is "shorting out" (failing constantly), you cut it off to save the system.

---

## 2. The Problem: "Cascading Failure" ( The Domino Effect)

Imagine **Order Service** calls **Payment Service**.

* **Scenario:** The Payment Service is down (or very slow, taking 30 seconds to timeout).
* **The Traffic:** 1,000 users click "Pay" per second.
* **The Crash:**
    * 1,000 threads in Order Service are now stuck waiting for Payment Service.
    * They are holding memory and CPU connections.
    * **Result:** The Order Service runs out of resources and crashes.
    * **Domino:** Now the **Frontend** crashes because it's waiting for Order Service.
    * **Outcome:** One bad service took down the whole company.

---

## 3. The Solution: The State Machine

We wrap the dangerous call in a **Circuit Breaker** object. It monitors failures and has three distinct states:

### A. CLOSED (Normal Operation)

* **Behavior:** Requests flow through normally.
* **Monitoring:** If 50% of requests fail (e.g., 5 errors in a row), the breaker **Trips**.

### B. OPEN (The "Cut-Off")

* **Behavior:** The breaker blocks **ALL** requests to the Payment Service immediately.
* **Response:** It throws a `CallNotPermittedException` (or returns a fallback) instantly. **No waiting.**
* **Duration:** It stays open for a set time (e.g., 10 seconds) to give the Payment Service time to recover.

### C. HALF-OPEN (The "Test")

* **Behavior:** After 10 seconds, it lets **one** request through.
    * **If Success:** It assumes the service is fixed. It switches back to **CLOSED**.
    * **If Failure:** It assumes the service is still broken. It switches back to **OPEN** for another 10 seconds.

---

## 4. Java Implementation (Resilience4j)

In Spring Boot, we use the `@CircuitBreaker` annotation.

```java
@Service
public class PaymentService {

    @CircuitBreaker(name = "paymentService", fallbackMethod = "fallbackPay")
    public String processPayment(Order order) {
        // Call external API (might be down)
        return restTemplate.postForObject("http://paypal-api/pay", order, String.class);
    }

    // This runs IMMEDIATELY if the breaker is Open (No waiting)
    public String fallbackPay(Order order, Throwable t) {
        return "Payment System is currently busy. Please try 'Cash on Delivery'.";
    }
}

```

This completes the "Big 3" Resilience patterns (Bulkhead, Circuit Breaker, Retry).


---


# Q-8 Explain Retry pattern

This is the simplest pattern, but also the most **dangerous** if used incorrectly.

## 1. The Real-World Analogy: "Bad Cell Reception"

You are talking to your friend on the phone. Suddenly, the line goes dead (static).

* **What do you do?** You hang up and immediately call back.
* **Why?** You assume it was just a temporary glitch (a tunnel, a tower hand-off).
* **Result:** The second time, the call connects perfectly.

In Microservices, **Transient Failures** (temporary blips) happen all the time. 
A database might be restarting, or a network switch might hiccup for 50 milliseconds.

---

## 2. The Solution: Automatic Retry

Instead of showing the user an error page ("System Unavailable"), the software quietly 
tries the request again in the background.

* **Attempt 1:** Call Payment Service. **Fail** (Network Timeout).
* **Wait:** 1 second.
* **Attempt 2:** Call Payment Service. **Success!**
* **User Experience:** They never knew there was a problem.

---

## 3. The Danger: "The Thundering Herd" (Self-Inflicted DDoS)

This is the most critical part to mention in an interview.

**The Scenario:**

* Your Payment Service is down because it is overloaded (too many requests).
* **Without Retry:** 1,000 users get an error. The service has time to recover.
* **With Naive Retry:**
    * 1,000 users fail.
    * **Immediately**, all 1,000 retry at the exact same millisecond.
    * Now the service has **2,000** requests hitting it.
    * It crashes harder.
    * They retry again. Now it's **3,000**.

* **Result:** You have accidentally launched a DDoS attack on your own system.

---

## 4. The Fix: Exponential Backoff (The Smart Way)

To prevent the "Thundering Herd," we use a strategy called **Exponential Backoff**. We don't retry
immediately; we wait longer and longer each time.

* **Attempt 1:** Fail.
* **Wait:** 1 second.
* **Attempt 2:** Fail.
* **Wait:** 2 seconds ().
* **Attempt 3:** Fail.
* **Wait:** 4 seconds ().
* **Attempt 4:** Fail. **Give Up.**

**Bonus (Senior Level Tip):** Add **"Jitter"** (Randomness).

* Instead of waiting exactly 2000ms, wait .
* This ensures that not all 1,000 users retry at the *exact same millisecond*, spreading the load.

---

### 5. Java Implementation (Resilience4j)

In Spring Boot, we use the `@Retry` annotation.

```java
@Service
public class PaymentService {

    // Retry max 3 times. Wait 2s between attempts.
    @Retry(name = "paymentRetry", fallbackMethod = "fallbackPay")
    public String processPayment(Order order) {
        // Unreliable call
        return restTemplate.postForObject("http://payment-service/api", order, String.class);
    }
    
    public String fallbackPay(Exception e) {
        return "Payment failed after 3 attempts. Please try again later.";
    }
}
```

```yaml
resilience4j:
  retry:
    instances:
      paymentRetry:
        maxRetryAttempts: 3              # Try 3 times total (1 initial + 2 retries)
        waitDuration: 1s                 # Initial wait time
        enableExponentialBackoff: true   # TURN THIS ON
        exponentialBackoffMultiplier: 2  # Double the wait time each failure
        randomizedWaitFactor: 0.5        # Jitter factor (0.5 = +/- 50%)
```

---


# Q-9 What is N+1 problem?


This is the most famous performance issue in ORMs (like Hibernate/JPA).

## 1. The Scenario: "Authors and Books"

Imagine you have a database with **Authors** and **Books**.

* **1 Author** has **Many Books**.

You want to print every Author's name and the title of their first book.

## 2. The Bad Code (The Trap)

You write this simple Java code:

```java
// 1. Fetch ALL authors (SELECT * FROM Author)
List<Author> authors = authorRepository.findAll();

// 2. Loop through them
for (Author author : authors) {
    // 3. Get their books (LAZY LOAD)
    System.out.println(author.getBooks().get(0).getTitle());
}

```

## 3. The Problem (The Math)

Here is what happens in the database logs:

1. **Query 1:** Hibernate runs `SELECT * FROM Author`. (Returns 100 authors).
2. **The Loop:**
    * **Iteration 1:** You call `.getBooks()`. Hibernate runs `SELECT * FROM Book WHERE author_id = 1`.
    * **Iteration 2:** You call `.getBooks()`. Hibernate runs `SELECT * FROM Book WHERE author_id = 2`.
    * ...
    * **Iteration 100:** You call `.getBooks()`. Hibernate runs `SELECT * FROM Book WHERE author_id = 100`.


**Total Queries:**

* **1** (to get the list)
* **+ N** (one for each Author to get their books)
* **= N + 1 Queries**

If you have 1,000 authors, you just ran **1,001 database queries** for a single screen. This kills performance.

## 4. The Solution: "JOIN FETCH"

You need to tell Hibernate: *"When you get the Authors, get their Books at the same time."*

**The Fix (JPQL):**

```java
@Query("SELECT a FROM Author a JOIN FETCH a.books")
List<Author> findAllWithBooks();

```

**The Result:**
Hibernate runs **1 single query**:
`SELECT * FROM Author a INNER JOIN Book b ON a.id = b.author_id`

