<!-- TOC -->
* [Designing a Rate Limiter](#designing-a-rate-limiter)
  * [Resources](#resources)
  * [Goal](#goal)
  * [High-level architecture (big picture first)](#high-level-architecture-big-picture-first)
  * [High-level components](#high-level-components)
  * [Component-by-component explanation](#component-by-component-explanation)
    * [1. Spring Cloud Gateway (multiple instances)](#1-spring-cloud-gateway-multiple-instances)
    * [2. Rate Limiting Filter (Gateway Filter)](#2-rate-limiting-filter-gateway-filter)
    * [3 Rule Engine (decision logic)](#3-rule-engine-decision-logic)
  * [Rate-limit rules (VERY IMPORTANT SECTION)](#rate-limit-rules-very-important-section)
    * [1. Rules Database (source of truth)](#1-rules-database-source-of-truth)
    * [2. Sample rate_limit_rules table (with EVERY column explained)](#2-sample-rate_limit_rules-table-with-every-column-explained)
    * [3. What each column does (ELI5 + interview clarity)](#3-what-each-column-does-eli5--interview-clarity)
    * [4 Rules Cache (performance layer)](#4-rules-cache-performance-layer)
  * [Redis Runtime Store (critical component)](#redis-runtime-store-critical-component)
    * [1. What Redis stores](#1-what-redis-stores)
    * [2. Why Redis?](#2-why-redis)
  * [Lua Script (inside Redis)](#lua-script-inside-redis)
  * [End-to-end request flow (step by step)](#end-to-end-request-flow-step-by-step)
<!-- TOC -->

# Designing a Rate Limiter

## Resources

* [System Design Interview - Rate Limiting (local and distributed) ](https://www.youtube.com/watch?v=FU4WlwfS3G0)
* [Rate Limiting system design | TOKEN BUCKET, Leaky Bucket, Sliding Logs ](https://www.youtube.com/watch?v=mhUQe4BKZXs)

## Goal

We need to design a distributed rate-limiting system for APIs, enforced at the gateway layer, that supports multiple 
rules, high throughput, horizontal scaling, and high availability.

We are using:

* Spring Cloud Gateway (multiple instances)
* Redis for runtime enforcement
* Database for rule management

## High-level architecture (big picture first)

```text
Clients
   ↓
Load Balancer
   ↓
Spring Cloud Gateway (N instances)
   ↓
Rate Limiter (Gateway Filter)
   ↓
Redis (Lua, runtime state)
   ↓
Downstream Services
```

Separately:

```text
Admin / Config Service
   ↓
Rate Limit Rules DB
   ↓
Rules Cache (Redis / Local Cache)
```

## High-level components

1. Spring Cloud Gateway
2. Rate Limiting Filter
3. Rule Engine
4. Rules Database
5. Rules Cache
6. Redis Runtime Store
7. Lua Script (inside Redis)

This shows structure and clarity — interviewers like this.


## Component-by-component explanation

### 1. Spring Cloud Gateway (multiple instances)

**Responsibility**

* Acts as the entry point
* Enforces rate limits before traffic hits services
* Horizontally scalable

**Why gateway-level rate limiting?**

* Protects all downstream services
* Centralized enforcement
* No duplication in services

Each gateway instance is stateless.


### 2. Rate Limiting Filter (Gateway Filter)

This is a custom GlobalFilter or RouteFilter.

Responsibilities

* Extract request attributes:
    * userId / API key
    * IP address
    * HTTP method
    * API path
* Decide which rule applies
* Ask Redis: "Can I allow this request?"
* Return:
    * 200 → forward request
    * 429 → block request

❗ The filter **does not store counters locally**.


### 3 Rule Engine (decision logic)

Responsibility

* Matches incoming request to the correct rule
* Supports multiple scopes:
    * Per user
    * Per API
    * Per IP
    * Per tenant
* Resolves precedence

Example precedence:

```text
User-specific rule
→ Tenant rule
→ API default rule
```

This logic runs inside the gateway.


## Rate-limit rules (VERY IMPORTANT SECTION)

This is where interviewers pay attention.

### 1. Rules Database (source of truth)

Stored in Postgres / MySQL.

**Why a DB?**

* Rules change rarely
* Admin-managed
* Auditable
* Strong consistency


### 2. Sample rate_limit_rules table (with EVERY column explained)

| Column        | Meaning                                               |
|---------------|-------------------------------------------------------|
| `id`          | Unique rule identifier                                |
| `scope_type`  | Level of enforcement (USER / IP / API / TENANT)       |
| `scope_key`   | Value of the scope (userId, IP range, tenantId, etc.) |
| `http_method` | GET / POST / PUT (nullable for all)                   |
| `api_path`    | `/orders/**`, `/payments/*`                           |
| `max_tokens`  | Bucket capacity (burst size)                          |
| `refill_rate` | Tokens added per second                               |
| `algorithm`   | TOKEN_BUCKET / SLIDING_WINDOW                         |
| `priority`    | Rule precedence (higher wins)                         |
| `enabled`     | Whether rule is active                                |
| `created_at`  | Audit                                                 |
| `updated_at`  | Audit                                                 |


### 3. What each column does (ELI5 + interview clarity)

`scope_type`

Defines **WHO is limited**.

Examples:

* `USER` → per user
* `IP` → per IP
* `TENANT` → per tenant
* `API` → global API limit

---

`scope_key`

Defines **which exact entity**.

Examples:

* `user123`
* `192.168.0.0/24`
* `tenantA`

---

`http_method` + `api_path`

Defines **WHAT is protected**.

Examples:

* `POST /orders`
* `GET /search`

---

`max_tokens`

Controls burst behavior.

Example:

```text
max_tokens = 100
```


Means:
> User can make up to 100 requests instantly if idle.


---

`refill_rate`

Controls long-term rate.

Example:

```text
refill_rate = 100 / 60 ≈ 1.67 tokens/sec
```

Means:
> Sustained rate of 100 requests per minute.
> 

---

`algorithm`

Allows extensibility:

* Token Bucket (default)
* Sliding Window (if needed)

---

`priority`

Handles conflicts.

Example:

* User-specific rule → priority 100
* API default rule → priority 10

Highest priority wins.

---

`enabled`

Allows:

* Temporary disabling
* Gradual rollout


### 4 Rules Cache (performance layer)

**Why cache rules?**

* Avoid DB lookup on every request
* Reduce latency

**Options**

* Redis (separate instance)
* Local cache (Caffeine)

TTL:

* 30s – 5min
* Or invalidation on change


## Redis Runtime Store (critical component)

This Redis is write-heavy and separate from rules cache (recommended).

### 1. What Redis stores

For each rate-limit key:

```text
rate_limit:{scope}:{id}:{api}
```

Example:

```text
rate_limit:user:user123:POST:/orders
```

Stored as Redis Hash:

```text
tokens       → current available tokens
lastRefill   → last refill timestamp
```

This is the **runtime state**, not rules.


### 2. Why Redis?

* Extremely fast
* Shared across gateway instances
* Atomic Lua execution
* Supports sharding (Redis Cluster)


## Lua Script (inside Redis)

**Responsibilities**

* Refill tokens based on time
* Cap bucket size
* Check availability
* Deduct tokens
* Persist state

**Why Lua?**

* Redis executes Lua atomically
* Prevents race conditions across gateways

No Java instance ever updates tokens directly.


## End-to-end request flow (step by step)

This is the most important part.

**Step 1: Request arrives**

```text
POST /orders
User: user123
```

**Step 2: Gateway extracts attributes**

```text
userId = user123
api = POST /orders    
```

**Step 3: Rule Engine resolves rule**

Example:

```text
USER + POST /orders → rule #12
max_tokens = 100
refill_rate = 1.67
```

Fetched from cache (DB fallback).


**Step 4: Gateway calls Redis Lua**

Inputs:

* Redis key
* max_tokens
* refill_rate
* tokens_required = 1
* current_time


**Step 5: Lua executes atomically**

* Refill tokens
* Check limit
* Deduct token (if allowed)

Returns:

* `1` → allow
* `0` → reject


**Step 6: Gateway acts**

* `1` → forward request
* `0` → return 429 Too Many Requests

