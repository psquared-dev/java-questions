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
* [Q-6 Explain Saga pattern](#q-6-explain-saga-pattern)
  * [Saga Pattern: Distributed Transactions](#saga-pattern-distributed-transactions)
  * [**Approach 1: Choreography (The "Dance")**](#approach-1-choreography-the-dance)
  * [Approach 2: Orchestration (The "Conductor")](#approach-2-orchestration-the-conductor)
  * [**Comparison Cheat Sheet**](#comparison-cheat-sheet)
  * [**Summary for Interview**](#summary-for-interview)
* [Q-7 Explain Bulkhead Pattern](#q-7-explain-bulkhead-pattern)
  * [The Problem: "Resource Exhaustion" (The Sinking Ship)](#the-problem-resource-exhaustion-the-sinking-ship)
  * [The Solution: The Bulkhead Pattern](#the-solution-the-bulkhead-pattern)
  * [Java Implementation (Resilience4j)](#java-implementation-resilience4j)
* [Q-8 Explain Circuit Breaker pattern](#q-8-explain-circuit-breaker-pattern)
  * [The core idea](#the-core-idea)
  * [The Problem: "Cascading Failure" ( The Domino Effect)](#the-problem-cascading-failure--the-domino-effect)
  * [The Solution: The State Machine](#the-solution-the-state-machine)
  * [The Solution: The State Machine](#the-solution-the-state-machine-1)
    * [A. CLOSED (Normal Operation)](#a-closed-normal-operation)
    * [B. OPEN (The "Cut-Off")](#b-open-the-cut-off)
    * [C. HALF-OPEN (The "Probing Phase")](#c-half-open-the-probing-phase)
  * [Configuration (Resilience4j via `application.yml`)](#configuration-resilience4j-via-applicationyml)
  * [Java Implementation (Resilience4j)](#java-implementation-resilience4j-1)
* [Q-9 Explain Retry pattern](#q-9-explain-retry-pattern)
  * [The core idea](#the-core-idea-1)
  * [When retries make sense (important)](#when-retries-make-sense-important)
  * [Retry strategies (from naive → correct)](#retry-strategies-from-naive--correct)
    * [1. Immediate retry (bad)](#1-immediate-retry-bad)
    * [2. Fixed delay](#2-fixed-delay)
    * [3. Exponential backoff (recommended)](#3-exponential-backoff-recommended)
    * [4. Exponential backoff + jitter (best)](#4-exponential-backoff--jitter-best)
  * [Key configuration knobs](#key-configuration-knobs)
  * [Retry + Circuit Breaker (must be combined carefully)](#retry--circuit-breaker-must-be-combined-carefully)
  * [Retry vs Circuit Breaker vs Bulkhead](#retry-vs-circuit-breaker-vs-bulkhead)
  * [Java Implementation (Resilience4j)](#java-implementation-resilience4j-2)
* [Q-10 What is N+1 problem?](#q-10-what-is-n1-problem)
  * [1. The Scenario: "Authors and Books"](#1-the-scenario-authors-and-books)
  * [2. The Bad Code (The Trap)](#2-the-bad-code-the-trap)
  * [3. The Problem (The Math)](#3-the-problem-the-math)
  * [4. The Solution: "JOIN FETCH"](#4-the-solution-join-fetch)
* [Q-11 Explain SOLID](#q-11-explain-solid)
  * [S - Single Responsibility Principle (SRP)](#s---single-responsibility-principle-srp)
    * [The Bad Example (The "Swiss Army Knife")](#the-bad-example-the-swiss-army-knife)
    * [The Good Example (The Specialist)](#the-good-example-the-specialist)
  * [O - Open/Closed Principle (OCP)](#o---openclosed-principle-ocp)
    * [The Bad Example (The "If-Else" Hell)](#the-bad-example-the-if-else-hell)
    * [The Good Example (Polymorphism)](#the-good-example-polymorphism)
  * [L - Liskov Substitution Principle (LSP)](#l---liskov-substitution-principle-lsp)
    * [The Bad Example (The "Fake" Implementation)](#the-bad-example-the-fake-implementation)
    * [The Good Example](#the-good-example)
  * [I - Interface Segregation Principle (ISP)](#i---interface-segregation-principle-isp)
    * [The Bad Example (The "Fat" Interface)](#the-bad-example-the-fat-interface)
    * [The Good Example (Segregated Interfaces)](#the-good-example-segregated-interfaces)
  * [D - Dependency Inversion Principle (DIP)](#d---dependency-inversion-principle-dip)
    * [The Bad Example (Tightly Coupled)](#the-bad-example-tightly-coupled)
    * [The Good Example (Dependency Injection)](#the-good-example-dependency-injection)
  * [Summary Cheat Sheet](#summary-cheat-sheet)
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

Here are the top 4 categories of patterns you must know, explained from the ground up.

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

# Q-6 Explain Saga pattern

## Saga Pattern: Distributed Transactions

**The Problem:**

In a Microservices architecture we often use database-per-service model. 
Hence, a single transaction cannot span multiple services. If a business 
process (like "Book Trip") spans 3 services, and the last one fails, you cannot 
simply `ROLLBACK` the first two.

**The Solution:**

A **Saga** is a sequence of **local transactions**. Each service updates its 
own database and publishes an event/message to trigger the next step.

**The Undo Button (Compensating Transactions):**

If a step fails, the Saga executes **Compensating Transactions** to undo the 
changes made by the previous steps.

* **Transaction:** `reserveCredit()`  **Compensation:** `refundCredit()`
* **Transaction:** `bookSeat()`  **Compensation:** `releaseSeat()`

---

## **Approach 1: Choreography (The "Dance")**

**Concept:** Decentralized. No central manager. Services listen for events and decide what to do.

**The Happy Path (Success):**

1. **Order Service:** Creates Order  Publishes `OrderCreated`.
2. **Payment Service:** Listens to `OrderCreated`  Charges Card  Publishes `PaymentProcessed`.
3. **Inventory Service:** Listens to `PaymentProcessed`  Reserves Stock  Publishes `StockReserved`.
4. **Order Service:** Listens to `StockReserved`  Updates Order to `COMPLETED`.

**The Failure Path (Rollback):**

*Scenario: Inventory is Out of Stock.*

1. **Inventory Service:** Fails to reserve stock  Publishes `StockFailed`.
2. **Payment Service:** Listens to `StockFailed`  **Executes Refund**  Publishes `RefundProcessed`.
3. **Order Service:** Listens to `StockFailed`  Updates Order to `CANCELLED`.

---

## Approach 2: Orchestration (The "Conductor")

**Concept:** Centralized. An **Orchestrator** (e.g., a specific Class or Service) tells every 
participant what to do.

**The Happy Path (Success):**

1. **Orchestrator:** Sends command `ExecutePayment` to **Payment Service**.
2. **Payment Service:** Replies `Success`.
3. **Orchestrator:** Sends command `ReserveStock` to **Inventory Service**.
4. **Inventory Service:** Replies `Success`.
5. **Orchestrator:** Ends Saga  Updates Order to `COMPLETED`.

**The Failure Path (Rollback):**

*Scenario: Inventory is Out of Stock.*

1. **Orchestrator:** Sends command `ReserveStock` to **Inventory Service**.
2. **Inventory Service:** Replies `Failed`.
3. **Orchestrator:** Detects failure. Immediately sends command `RefundPayment` to **Payment Service**.
4. **Payment Service:** Replies `RefundSuccess`.
5. **Orchestrator:** Updates Order to `CANCELLED`.

---

## **Comparison Cheat Sheet**

| Feature        | Choreography (Events)                     | Orchestration (Command)                   |
|----------------|-------------------------------------------|-------------------------------------------|
| **Coupling**   | **Low** (Services don't know each other). | **Higher** (Orchestrator knows everyone). |
| **Complexity** | Becomes "Spaghetti" at scale.             | Clean, centralized logic.                 |
| **Debugging**  | Hard (Must trace events across logs).     | Easy (Check Orchestrator state).          |
| **Best For**   | Simple flows (2-3 steps).                 | Complex flows (4+ steps).                 |


## **Summary for Interview**

>
> "The Saga pattern manages distributed transactions by breaking them into local steps. 
> If a step fails, we execute **Compensating Transactions** to undo previous work.
> I prefer **Orchestration** for complex business logic (like Order Fulfillment) because
> it centralizes the state and makes error handling/timeouts much easier to manage than 
> the event-chain of Choreography."
>

---


# Q-7 Explain Bulkhead Pattern

The Bulkhead Pattern isolates parts of a system so that failure or overload in one part does 
not cascade and take down everything else - analogous to watertight compartments in a ship.

![bulk head](../images/bulkhead.png)

If we don't implement bulkhead pattern then one heavy feature (e.g., Image Processing) uses up 
all threads/connections, starving the critical features (e.g., Login).

In Microservices, **Threads** are the water. If one service floods your app with requests, you 
don't want it to sink the whole container.

---

## The Problem: "Resource Exhaustion" (The Sinking Ship)

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

## The Solution: The Bulkhead Pattern

We artificially restrict how many resources (threads) each feature can use.

We split the 100 Tomcat threads into distinct pools:

* **Pool A (Product Details):** Max 60 threads.
* **Pool B (PDF Invoice):** Max 40 threads.

**The New Scenario:**

* 100 users request "Generate PDF Invoice."
* The first 40 get a thread from **Pool B**.
* The other 60 are immediately rejected (Fast Fail). **Pool B is full.**
* **Meanwhile:** A user requests "Get Product Details."
* **Result:** **Success!** Pool A still has 60 threads completely free. The "ship" (app) is still 
  floating, even though one compartment (PDFs) is flooded.

---

## Java Implementation (Resilience4j)

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


# Q-8 Explain Circuit Breaker pattern

The Circuit Breaker Pattern prevents a system from repeatedly 
calling a failing or slow dependency. Instead of waiting for 
timeouts on every request, it fails fast and protects your service.

Here is the **Circuit Breaker Pattern**, explained with the same structure.

## The core idea

```text
Detect failures → stop calls temporarily → probe for recovery → resume safely
```

---

## The Problem: "Cascading Failure" ( The Domino Effect)

Imagine **Order Service** calls **Payment Service**.

* **Scenario:** The Payment Service is down (or very slow, taking 30 seconds to timeout).
* **The Traffic:** 1,000 users click "Pay" per second.
* **The Crash:**
    * 1,000 threads in Order Service are now stuck waiting for Payment Service.
    * They are holding memory and CPU connections.
    * **Result:** The Order Service runs out of resources and crashes. 
    * **Outcome:** One bad service took down the whole company.

---

## The Solution: The State Machine

We wrap the dangerous call in a **Circuit Breaker** object. 
It monitors failures and has three distinct states:

Here is the updated explanation for your notes, correcting the "Half-Open" behavior 
to match modern standards (Resilience4j) and including the necessary configuration.

## The Solution: The State Machine

We wrap the dangerous call in a **Circuit Breaker** object. It monitors failures and 
transitions between three distinct states based on the health of the downstream service.

### A. CLOSED (Normal Operation)

* **Behavior:** Requests flow through normally to the external service.
* **Monitoring:** The breaker counts failures. If the failure rate exceeds the 
  threshold (e.g., 50%) within a specific window, the breaker **Trips** to OPEN.

### B. OPEN (The "Cut-Off")

* **Behavior:** The breaker blocks **ALL** requests immediately. It does not even try to call the external service.
* **Response:** It throws a `CallNotPermittedException` (or executes a fallback method) instantly. **No waiting.**
* **Duration:** It stays open for a configurable time (e.g., 10 seconds) to give the struggling service time to recover.

### C. HALF-OPEN (The "Probing Phase")

* **Behavior:** After the wait duration expires, the breaker transitions to **HALF-OPEN**.
* **The Test:** It allows a **limited, configurable number of requests** (e.g., 3 calls) to pass through to test 
   if the service has recovered.
    * **If Success:** If the failure rate of these 3 calls is below the threshold, it resets to **CLOSED**.
    * **If Failure:** If the failure rate is still high, it trips back to **OPEN** for another wait duration.


---

## Configuration (Resilience4j via `application.yml`)

This configuration controls exactly when the state changes happen.

```yaml
resilience4j:
  circuitbreaker:
    instances:
      paymentService:
        # 1. CLOSED -> OPEN Rules
        slidingWindowSize: 10          # Monitor the last 10 calls
        failureRateThreshold: 50       # Trip if 50% (5 out of 10) fail
        
        # 2. OPEN -> HALF-OPEN Rules
        waitDurationInOpenState: 10s   # Stay OPEN for 10 seconds before testing
        
        # 3. HALF-OPEN -> CLOSED Rules
        permittedNumberOfCallsInHalfOpenState: 3  # Let 3 requests through to test
```

---

## Java Implementation (Resilience4j)

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


---


# Q-9 Explain Retry pattern


The **Retry Pattern** automatically **re-attempts a failed operation** when 
the failure is likely **transient** (temporary), such as a brief network glitch or momentary overload.

---

## The core idea

> **Some failures are temporary — retrying after a short delay can succeed.**
> 

But retries must be **controlled**, or they make outages worse.

---

## When retries make sense (important)

* ✅ Network timeouts
* ✅ Connection resets
* ✅ 5xx from remote service
* ✅ Leader re-election / brief unavailability

* ❌ Invalid input
* ❌ Authentication failures
* ❌ Deterministic business errors

---

## Retry strategies (from naive → correct)

### 1. Immediate retry (bad)

```text
fail → retry now → retry now → retry now
```

* ❌ Causes retry storms
* ❌ Amplifies load during outages

---

### 2. Fixed delay

```text
retry after 100ms, 100ms, 100ms
```

* ✔ Simple
* ❌ Still synchronized across clients

---

### 3. Exponential backoff (recommended)

```text
100ms → 200ms → 400ms → 800ms
```

* ✔ Reduces pressure on failing service
* ✔ Industry standard

---

### 4. Exponential backoff + jitter (best)

```text
random(0, base * 2^n)
```

* ✔ Prevents thundering herd
* ✔ Cloud-native best practice

---

## Key configuration knobs

* **Max attempts** (e.g. 3–5)
* **Initial delay**
* **Backoff multiplier**
* **Max delay**
* **Which exceptions are retryable**

---

## Retry + Circuit Breaker (must be combined carefully)

Correct order:

```text
Retry → Circuit Breaker
```

Why:

* Retry handles **transient** failures
* Circuit breaker stops retries when failures are **persistent**

Bad combination:

```text
Retry without circuit breaker
```

→ retry storm → cascading failure

---

## Retry vs Circuit Breaker vs Bulkhead

| Pattern         | Purpose                          |
|-----------------|----------------------------------|
| Retry           | Recover from transient failures  |
| Circuit Breaker | Stop calling persistent failures |
| Bulkhead        | Isolate resources                |

* 👉 **Retry alone is dangerous**
* 👉 **Retry + Circuit Breaker + Bulkhead = resilient system**

---

## Java Implementation (Resilience4j)

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


# Q-10 What is N+1 problem?


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


---

# Q-11 Explain SOLID

Here is the **SOLID** breakdown with "Bad" vs. "Good" Java examples.

## S - Single Responsibility Principle (SRP)

**Definition:** A class should have **one, and only one, reason to change.**

* *Don't create "God Classes" that do everything.*

### The Bad Example (The "Swiss Army Knife")

Here, the `Invoice` class handles math, database logic, and printing. If 
the **Database** changes, this class changes. If the **Print Format** changes, this class changes.

```java
class Invoice {
    public void calculateTotal() { /* ... */ }
    public void saveToDB() { /* JDBC Code ... */ }  // Violation
    public void print() { /* System.out.println ... */ } // Violation
}

```

### The Good Example (The Specialist)

Split the responsibilities into focused classes.

```java
class Invoice {
    public void calculateTotal() { /* logic */ }
}

class InvoiceRepository {
    public void save(Invoice invoice) { /* DB logic */ }
}

class InvoicePrinter {
    public void print(Invoice invoice) { /* Print logic */ }
}

```

---

## O - Open/Closed Principle (OCP)

**Definition:** Software entities should be **Open for Extension, but Closed for Modification.**

* *You should be able to add new features without touching existing, tested code.*

### The Bad Example (The "If-Else" Hell)

Every time you add a new payment method (e.g., Bitcoin), you have to modify 
this class and risk breaking existing logic.

```java
class PaymentProcessor {
    public void process(String type) {
        if (type.equals("PayPal")) {
            // process PayPal
        } else if (type.equals("CreditCard")) {
            // process CreditCard
        }
        // Changing this file for every new type violates OCP
    }
}
```

### The Good Example (Polymorphism)

Use an interface. To add Bitcoin, you just create a *new* class. You never touch `PaymentProcessor`.

```java
interface PaymentMethod {
    void pay();
}

class PayPal implements PaymentMethod {
    public void pay() { /* ... */ }
}

class PaymentProcessor {
    public void process(PaymentMethod method) {
        method.pay(); // Works for PayPal, CreditCard, Bitcoin...
    }
}
```

---

## L - Liskov Substitution Principle (LSP)

**Definition:** Subtypes must be **substitutable** for their base types without breaking the program.

* _"If the parent class can do X, the child class MUST also be able to do X."_

### The Bad Example (The "Fake" Implementation)

Here is a **practical, real-world scenario** that happens in almost every legacy codebase: **Read-Only Files.**

You are building a system to manage documents.
You have a base class `Document` that assumes all documents can be **Opened** and **Saved**.

You create a base class with a `save()` method.
Then, you introduce a `ReadOnlyDocument` (like a PDF report or a historical archive) that **cannot be modified**.

```java
// Parent Class
class Document {
    public void open() { /* logic */ }
    public void save() { 
        System.out.println("Saving to disk..."); 
    }
}

// Child Class (Violates LSP)
class ReadOnlyDocument extends Document {
    @Override
    public void save() {
        // BREAKS THE TRUST!
        // The parent said "I can save", but the child says "I crash if you try".
        throw new UnsupportedOperationException("Cannot save read-only file!");
    }
}
```


You have a `ProjectManager` class that saves all open documents when the app closes. 
It expects every `Document` to behave like the parent.

```java
public void saveAllProjects(List<Document> docs) {
    for (Document doc : docs) {
        doc.save(); 
    }
}
```

* **If the list contains standard Documents:** It works perfectly.
* **If the list contains ONE Read-Only Document:** The entire application **crashes** with 
  an Exception. The auto-save fails, and the user might lose data from the *other* valid 
  documents because the loop stopped halfway.

---

### The Good Example

The problem is that `Document` assumed **everything** is writable. That was a lie.
We fix this by splitting the capabilities.

**Step 1: Create Specific Interfaces**

```java
interface Openable {
    void open();
}

interface Savable extends Openable {
    void save();
}

```

**Step 2: Implement Honestly**

```java
// Standard Doc can do both
class StandardDocument implements Savable {
    public void open() { /*...*/ }
    public void save() { /*...*/ }
}

// Read-Only Doc only implements Openable
class ReadOnlyDocument implements Openable {
    public void open() { /*...*/ }
    // It physically DOES NOT HAVE a save() method.
}

```

**Step 3: Update the Manager**

Now, the `saveAllProjects` method can only accept `Savable` objects.

```java
public void saveAllProjects(List<Savable> docs) {
    for (Savable doc : docs) {
        doc.save(); // 100% safe. No crashes possible.
    }
}

```

**The Compilation Safety:**
If you try to add a `ReadOnlyDocument` to that list, the **compiler** will stop you 
immediately: *"Error: ReadOnlyDocument is not Savable."*

---

## I - Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on methods they do not use.

* *Make fine-grained interfaces, not huge "Fat" interfaces.*

### The Bad Example (The "Fat" Interface)

A `Robot` worker implements `Worker`, but it has to implement `eat()` even though robots don't eat.

```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() { /* ... */ }
    public void eat() { 
        // Forced to implement dummy code
        throw new RuntimeException("I don't eat"); 
    }
}
```

### The Good Example (Segregated Interfaces)

Break it down.

```java
interface Workable { void work(); }
interface Eatable { void eat(); }

class Robot implements Workable {
    public void work() { /* ... */ }
}

class Human implements Workable, Eatable {
    public void work() { /* ... */ }
    public void eat() { /* ... */ }
}
```

---

## D - Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend on low-level modules. 
Both should depend on **Abstractions**.

* *Don't use `new` to create dependencies inside your class. Ask for them in the constructor.*

### The Bad Example (Tightly Coupled)

The `Store` is hard-coded to use `Stripe`. You cannot easily switch to PayPal or test 
this class without a real Stripe API.

```java
class Store {
    private StripePaymentService stripe;

    public Store() {
        this.stripe = new StripePaymentService(); // Hard dependency!
    }
}

```

### The Good Example (Dependency Injection)

The `Store` doesn't care *what* payment service you use, as long as it follows the contract.

```java
class Store {
    private PaymentService paymentService;

    // Inject via Constructor
    public Store(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
// Usage: new Store(new PayPalService());
```

---

## Summary Cheat Sheet

| Principle | Meaning               | The Fix                                                           |
|-----------|-----------------------|-------------------------------------------------------------------|
| **SRP**   | Single Responsibility | **Split classes** that do too much.                               |
| **OCP**   | Open/Closed           | Use **Interfaces/Polymorphism** instead of `if/else`.             |
| **LSP**   | Liskov Substitution   | Subclasses should not **throw exceptions** for parent methods.    |
| **ISP**   | Interface Segregation | Split **fat interfaces** into smaller ones.                       |
| **DIP**   | Dependency Inversion  | **Inject dependencies** (Constructor Injection) instead of `new`. |

