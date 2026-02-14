<!-- TOC -->
* [Level 1: Core Java](#level-1-core-java)
  * [Q - What is JDK? What is the diff b/w JDK and JRE?](#q---what-is-jdk-what-is-the-diff-bw-jdk-and-jre)
  * [Q - What are access specifiers?](#q---what-are-access-specifiers)
  * [Q - What are packages?](#q---what-are-packages)
  * [Q - Is Java Pass by Value or Pass by Reference?](#q---is-java-pass-by-value-or-pass-by-reference)
  * [Q - What is the diff b/w objects and references?](#q---what-is-the-diff-bw-objects-and-references)
    * [Objects vs References](#objects-vs-references)
    * [What is allocated where?](#what-is-allocated-where)
    * [Memory Regions - Summary](#memory-regions---summary)
    * [References](#references)
  * [Q - What is shadowing?](#q---what-is-shadowing)
  * [Q - Will the following code compile (Widening vs. Narrowing)?](#q---will-the-following-code-compile-widening-vs-narrowing)
    * [1. Widening = implicit (safe)](#1-widening--implicit-safe)
    * [2. Narrowing = explicit cast required (unsafe)](#2-narrowing--explicit-cast-required-unsafe)
    * [Why `myChar = myByte` is not allowed](#why-mychar--mybyte-is-not-allowed)
    * [Why `myShort = myChar` is not allowed](#why-myshort--mychar-is-not-allowed)
    * [Why `myChar = myShort` is not allowed](#why-mychar--myshort-is-not-allowed)
  * [Q - var is used for Local Variable Type Inference (LVTI). Can we use it as an identifier?](#q---var-is-used-for-local-variable-type-inference-lvti-can-we-use-it-as-an-identifier)
  * [Q - Can you initialize a var variable with null?](#q---can-you-initialize-a-var-variable-with-null)
  * [Q - Mentions some other possible scenarios where we can't use the `var` (LVTI) keyword](#q---mentions-some-other-possible-scenarios-where-we-cant-use-the-var-lvti-keyword)
  * [Q - Do `double` and `float` type overflow?](#q---do-double-and-float-type-overflow)
  * [Q - Is default keyword one of the access modifier?](#q---is-default-keyword-one-of-the-access-modifier)
    * [1. The "Default Access Modifier" (The Invisible One)](#1-the-default-access-modifier-the-invisible-one)
    * [2. The `default` Keyword (The Actual Keyword)](#2-the-default-keyword-the-actual-keyword)
  * [Q - What is Covariant return type?](#q---what-is-covariant-return-type)
  * [Q - What if a method in child class is more restricted than a parent class?](#q---what-if-a-method-in-child-class-is-more-restricted-than-a-parent-class)
  * [Q - Does the finally block execute if there is a return statement inside try or catch?](#q---does-the-finally-block-execute-if-there-is-a-return-statement-inside-try-or-catch)
  * [Q - Is the following program correct?](#q---is-the-following-program-correct)
  * [Q - Can you run the code before executing main methods?](#q---can-you-run-the-code-before-executing-main-methods)
  * [Q - Is it true that main thread doesn't terminate until the child threads are done?](#q---is-it-true-that-main-thread-doesnt-terminate-until-the-child-threads-are-done)
  * [Q - What is hashCode() and how It's related to equals()?](#q---what-is-hashcode-and-how-its-related-to-equals)
    * [The Analogy: The Sectioned Library](#the-analogy-the-sectioned-library)
    * [How it works in a HashMap (The 3 Steps)](#how-it-works-in-a-hashmap-the-3-steps)
    * [The Contract: The "Law" of HashCode](#the-contract-the-law-of-hashcode)
  * [Q - What is marker interface?](#q---what-is-marker-interface)
    * [Resources](#resources)
  * [Q - What are Wrapper classes](#q---what-are-wrapper-classes)
  * [Q - Do local variables in Java have default values?](#q---do-local-variables-in-java-have-default-values)
  * [Q - What is copy constructor?](#q---what-is-copy-constructor)
    * [Copy Constructor vs clone() — Which is better?](#copy-constructor-vs-clone--which-is-better)
      * [1. The "Constructor Bypass" Problem (Critical)](#1-the-constructor-bypass-problem-critical)
      * [2. The Type Casting Tax](#2-the-type-casting-tax)
      * [3. The Exception Nightmare](#3-the-exception-nightmare)
      * [4. The "Marker Interface" Confusion](#4-the-marker-interface-confusion)
  * [Q - What object cloning?](#q---what-object-cloning)
    * [Why does Cloneable matter?](#why-does-cloneable-matter)
    * [Types of Cloning](#types-of-cloning)
      * [Shallow Clone (default)](#shallow-clone-default)
      * [Deep Clone](#deep-clone)
    * [Resources](#resources-1)
  * [Q - Do we have pointers in Java?](#q---do-we-have-pointers-in-java)
  * [Q - What is Java String Pool & String Interning?](#q---what-is-java-string-pool--string-interning)
    * [1. String Literal (Automatic Interning)](#1-string-literal-automatic-interning)
    * [2. The `new` Keyword (Forcing a New Object)](#2-the-new-keyword-forcing-a-new-object)
    * [3. Manual Interning (`.intern()`)](#3-manual-interning-intern)
    * [4. Why is this safe? (Immutability)](#4-why-is-this-safe-immutability)
  * [Q - Which class is thread-safe: StringBuilder or StringBuffer?](#q---which-class-is-thread-safe-stringbuilder-or-stringbuffer)
  * [Q - How many times is the finalize() method called in Java?](#q---how-many-times-is-the-finalize-method-called-in-java)
    * [What is finalize()?](#what-is-finalize)
    * [Key Idea 1: finalize() may run OR may not run](#key-idea-1-finalize-may-run-or-may-not-run)
    * [Key Idea 2: finalize() runs ONCE per object](#key-idea-2-finalize-runs-once-per-object)
    * [Why "ONLY ONCE"? (Simple explanation)](#why-only-once-simple-explanation)
  * [Q - What is Serializable interface? List some real-world use-cases for it.](#q---what-is-serializable-interface-list-some-real-world-use-cases-for-it)
    * [Key statement](#key-statement)
      * [Serialization code (standard)](#serialization-code-standard)
      * [What the JVM actually does internally (THIS is the reflection part)](#what-the-jvm-actually-does-internally-this-is-the-reflection-part)
  * [Q - What are some use cases of reflection](#q---what-are-some-use-cases-of-reflection)
  * [Q - How many methods are there compare strings in Java?](#q---how-many-methods-are-there-compare-strings-in-java)
  * [Q - Will the following statement adds string to the string pool?](#q---will-the-following-statement-adds-string-to-the-string-pool)
    * [The Nuance: The final Keyword](#the-nuance-the-final-keyword)
  * [Q - What happens when we concatenate string with different type?](#q---what-happens-when-we-concatenate-string-with-different-type)
    * [What Actually Happens (Under the Hood)](#what-actually-happens-under-the-hood)
      * [Case 1: Primitive Types](#case-1-primitive-types)
      * [Case 2: Reference Types](#case-2-reference-types)
  * [Q - What is the difference b/w `equals()` and `equalsIgnoreCase()` method?](#q---what-is-the-difference-bw-equals-and-equalsignorecase-method)
  * [Q - What is the difference b/w `isEmpty()` and `isBlank()` method of String object?](#q---what-is-the-difference-bw-isempty-and-isblank-method-of-string-object)
  * [Q - Diff b/w `String`, `StringBuilder` and `StringBuffer`](#q---diff-bw-string-stringbuilder-and-stringbuffer)
  * [Q - What is CharSequence?](#q---what-is-charsequence)
    * [Key Methods](#key-methods)
    * [Why was CharSequence introduced?](#why-was-charsequence-introduced)
    * [Simple Example](#simple-example)
  * [Q - What is serialVersionUID?](#q---what-is-serialversionuid)
    * [Why do we need serialVersionUID?](#why-do-we-need-serialversionuid)
    * [DEFAULT Behavior](#default-behavior)
    * [What happens if serialVersionUID changes?](#what-happens-if-serialversionuid-changes)
    * [What Happens When You Manually Define serialVersionUID](#what-happens-when-you-manually-define-serialversionuid)
      * [Add a new field](#add-a-new-field)
      * [Remove a field](#remove-a-field)
      * [Change field order](#change-field-order)
      * [Add methods](#add-methods)
      * [Changes that break compatibility](#changes-that-break-compatibility)
  * [Q - How to prevent serialization of a field?](#q---how-to-prevent-serialization-of-a-field)
    * [1. The transient Keyword (The Standard Way)](#1-the-transient-keyword-the-standard-way)
    * [2. The static Modifier (The "Class-Level" Rule)](#2-the-static-modifier-the-class-level-rule)
  * [Q - Why `Object.clone()` is defined as protected?](#q---why-objectclone-is-defined-as-protected)
    * [The "Senior" Verdict: Avoid `clone()` entirely](#the-senior-verdict-avoid-clone-entirely)
  * [Q - What are the advantages of String being immutable?](#q---what-are-the-advantages-of-string-being-immutable)
  * [Q - What's the default implementation of `Object.equals()` method?](#q---whats-the-default-implementation-of-objectequals-method)
  * [Q - Does finally block always execute in Java?](#q---does-finally-block-always-execute-in-java)
  * [Q - What are methods provided by the Object class?](#q---what-are-methods-provided-by-the-object-class)
  * [Q - Can you provide default hashcode() implementation in the interface?](#q---can-you-provide-default-hashcode-implementation-in-the-interface)
    * [1. The Conflict Resolution Rule](#1-the-conflict-resolution-rule)
    * [2. The Problem](#2-the-problem)
  * [Q - What is Comparable interface?](#q---what-is-comparable-interface)
  * [Q - Give a walk-through of the new features introduced since Java 8?](#q---give-a-walk-through-of-the-new-features-introduced-since-java-8)
    * [Phase 1: Java 9 - 11 (The "Modernization" Era)](#phase-1-java-9---11-the-modernization-era)
      * [Modules (Jigsaw)](#modules-jigsaw)
      * [Collection Factory Methods](#collection-factory-methods)
      * [Local Variable Type Inference (`var`)](#local-variable-type-inference-var)
      * [New HttpClient (Standardized)](#new-httpclient-standardized)
      * [String Methods (Life Savers)](#string-methods-life-savers)
      * [Running Single-File Source Code](#running-single-file-source-code)
    * [Phase 2: Java 12 - 17 (The "Syntactic Sugar" Era)](#phase-2-java-12---17-the-syntactic-sugar-era)
      * [Records (Data Classes)](#records-data-classes)
      * [Text Blocks (Multi-line Strings)](#text-blocks-multi-line-strings)
      * [Switch Expressions](#switch-expressions)
      * [Pattern Matching for `instanceof`](#pattern-matching-for-instanceof)
      * [Sealed Classes](#sealed-classes)
      * [Helpful NullPointerExceptions](#helpful-nullpointerexceptions)
    * [Phase 3: Java 18 - 21 (The "Concurrency Revolution")](#phase-3-java-18---21-the-concurrency-revolution)
      * [Virtual Threads (Project Loom) - **The Game Changer**](#virtual-threads-project-loom---the-game-changer)
      * [Structured Concurrency](#structured-concurrency)
      * [Sequenced Collections](#sequenced-collections)
      * [Record Patterns](#record-patterns)
      * [Foreign Function & Memory API](#foreign-function--memory-api)
  * [Q - What is shutdown hook?](#q---what-is-shutdown-hook)
    * [How to implement it?](#how-to-implement-it)
    * [When does it run?](#when-does-it-run)
    * [When does it NOT run?](#when-does-it-not-run)
* [Level 2: OOP & Object Model](#level-2-oop--object-model)
  * [Q - What are core principles of OOP?](#q---what-are-core-principles-of-oop)
    * [Runtime Polymorphism](#runtime-polymorphism)
    * [Compile time Polymorphism](#compile-time-polymorphism)
    * [Abstraction](#abstraction)
    * [Encapsulation](#encapsulation)
    * [Diff b/w Abstraction and Encapsulation](#diff-bw-abstraction-and-encapsulation)
  * [Q - What is association, aggregation and composition?](#q---what-is-association-aggregation-and-composition)
    * [Association](#association)
    * [Two forms of association](#two-forms-of-association)
    * [Aggregation (weak ownership)](#aggregation-weak-ownership)
    * [Composition (strong ownership)](#composition-strong-ownership)
  * [Q - Is runtime polymorphism is applicable for fields also?](#q---is-runtime-polymorphism-is-applicable-for-fields-also)
  * [Q - Define Singleton class](#q---define-singleton-class)
    * [Sequence of events (step-by-step)](#sequence-of-events-step-by-step)
    * [Why volatile is required?](#why-volatile-is-required)
  * [Q - How to make a class immutable?](#q---how-to-make-a-class-immutable)
  * [Q - In Java, what is the exact difference between a variable declared as final and an object that is immutable?](#q---in-java-what-is-the-exact-difference-between-a-variable-declared-as-final-and-an-object-that-is-immutable)
  * [Q - Why Java is not completely Object-Oriented?](#q---why-java-is-not-completely-object-oriented)
  * [Q - Why can't we override private and static methods?](#q---why-cant-we-override-private-and-static-methods)
    * [Why you cannot override private methods](#why-you-cannot-override-private-methods)
    * [Why you cannot override static methods](#why-you-cannot-override-static-methods)
      * [1. The Binding Difference](#1-the-binding-difference)
      * [2. They belong to the Class, not the Object](#2-they-belong-to-the-class-not-the-object)
      * [3. What actually happens? (Method Hiding)](#3-what-actually-happens-method-hiding)
  * [Q - What is Dynamic Method Dispatch?](#q---what-is-dynamic-method-dispatch)
* [Level 3: Exception Handling](#level-3-exception-handling)
  * [Q - Explain the hierarchy of exceptions in Java?](#q---explain-the-hierarchy-of-exceptions-in-java)
    * [Checked Exceptions (Compile-Time)](#checked-exceptions-compile-time)
    * [Unchecked Exceptions (Runtime)](#unchecked-exceptions-runtime)
  * [Q - What is Exception chaining?](#q---what-is-exception-chaining)
    * [Why is Exception Chaining needed?](#why-is-exception-chaining-needed)
    * [Real-World Example (ELI5)](#real-world-example-eli5)
    * [Practical Example](#practical-example)
  * [Q - What is the exact difference between ClassNotFoundException and NoClassDefFoundError?](#q---what-is-the-exact-difference-between-classnotfoundexception-and-noclassdeffounderror)
    * [1. ClassNotFoundException (The "Typo")](#1-classnotfoundexception-the-typo)
    * [2. NoClassDefFoundError (The "Ghost")](#2-noclassdeffounderror-the-ghost)
    * [Summary Table (Memorize This)](#summary-table-memorize-this)
  * [Q - What is AutoCloseable interface?](#q---what-is-autocloseable-interface)
    * [1. The Core Purpose: Try-With-Resources](#1-the-core-purpose-try-with-resources)
    * [2. Code Example](#2-code-example)
    * [3. Senior Engineer Nuance: Exception Suppression](#3-senior-engineer-nuance-exception-suppression)
* [Level 4: Collections Framework](#level-4-collections-framework)
  * [Q - What is diff b/w Vector and ArrayList?](#q---what-is-diff-bw-vector-and-arraylist)
  * [Q - Diff b/w Hashtable and HashMap](#q---diff-bw-hashtable-and-hashmap)
  * [Q - When would you use parallelStream()](#q---when-would-you-use-parallelstream)
    * [Parallel Stream — Three Examples Explained (Good vs Bad vs Dangerous)](#parallel-stream--three-examples-explained-good-vs-bad-vs-dangerous)
    * [Example 1 — ✅ GOOD use of parallelStream()](#example-1---good-use-of-parallelstream)
      * [Why this is GOOD](#why-this-is-good)
      * [Final judgment](#final-judgment)
    * [Example 2 — ❌ BAD use of parallelStream()](#example-2---bad-use-of-parallelstream)
      * [Why this is BAD](#why-this-is-bad)
      * [Final judgment](#final-judgment-1)
    * [Example 3 — 💀 DANGEROUS use of parallelStream()](#example-3---dangerous-use-of-parallelstream)
      * [Why this is DANGEROUS (not just slow)](#why-this-is-dangerous-not-just-slow)
      * [Final judgment](#final-judgment-2)
    * [One-page Comparison (Interview Gold)](#one-page-comparison-interview-gold)
    * [Final Rule to Say in Interview (Memorize This)](#final-rule-to-say-in-interview-memorize-this)
  * [Q - Explain collection framework hierarchy?](#q---explain-collection-framework-hierarchy)
  * [Q - Explain the evolution from SortedSet (Java 1.2) to NavigableSet (Java 6). Why was a new interface introduced instead of extending SortedSet, given that TreeSet already existed?](#q---explain-the-evolution-from-sortedset-java-12-to-navigableset-java-6-why-was-a-new-interface-introduced-instead-of-extending-sortedset-given-that-treeset-already-existed)
    * [1. SortedSet](#1-sortedset)
      * [What SortedSet Does Not Guarantee](#what-sortedset-does-not-guarantee)
    * [2. TreeSet Existed Before NavigableSet](#2-treeset-existed-before-navigableset)
    * [3. The Problem Before Java 6](#3-the-problem-before-java-6)
    * [4. NavigableSet](#4-navigableset)
      * [What NavigableSet Adds](#what-navigableset-adds)
    * [5. Why NavigableSet Was Introduced (Despite Existing Capability)](#5-why-navigableset-was-introduced-despite-existing-capability)
      * [What Changed in Java 6](#what-changed-in-java-6)
    * [6. Summary Table (Version-Accurate)](#6-summary-table-version-accurate)
    * [Final Interview-Ready Answer (Concise)](#final-interview-ready-answer-concise)
  * [Q - What are Fail Fast and Fail Safe Iterators?](#q---what-are-fail-fast-and-fail-safe-iterators)
    * [Fail-Fast Iterators](#fail-fast-iterators)
    * [Fail-Safe Iterators](#fail-safe-iterators)
  * [Q - How to create immutable collections in Java?](#q---how-to-create-immutable-collections-in-java)
    * [1. Using the "List.of()" Factory Method (Java 9+)](#1-using-the-listof-factory-method-java-9)
    * [2. Using Stream Collectors (Java 10+)](#2-using-stream-collectors-java-10)
    * [3. Creating a Copy (Java 10+)](#3-creating-a-copy-java-10)
    * [4. The "Unmodifiable View" (The Older Way)](#4-the-unmodifiable-view-the-older-way)
    * [Comparison of Methods](#comparison-of-methods)
* [Level 5: Concurrency & Multithreading](#level-5-concurrency--multithreading)
  * [Q - What are different thread states?](#q---what-are-different-thread-states)
  * [Q - What is daemon thread?](#q---what-is-daemon-thread)
  * [Q - What is BlockingQueue?](#q---what-is-blockingqueue)
    * [Why was BlockingQueue introduced?](#why-was-blockingqueue-introduced)
    * [How BlockingQueue solves the problem](#how-blockingqueue-solves-the-problem)
    * [Core BlockingQueue methods (important)](#core-blockingqueue-methods-important)
      * [put() — blocking insert](#put--blocking-insert)
      * [take() — blocking retrieval](#take--blocking-retrieval)
      * [offer() — non-blocking insert](#offer--non-blocking-insert)
      * [poll() — non-blocking retrieval](#poll--non-blocking-retrieval)
    * [Types of BlockingQueue](#types-of-blockingqueue)
      * [1. ArrayBlockingQueue](#1-arrayblockingqueue)
      * [2. LinkedBlockingQueue](#2-linkedblockingqueue)
      * [3. PriorityBlockingQueue](#3-priorityblockingqueue)
      * [4. DelayQueue](#4-delayqueue)
      * [5. SynchronousQueue](#5-synchronousqueue)
  * [Q - List diff types of Executorservice](#q---list-diff-types-of-executorservice)
  * [Q - What are the motivations for ExecutorService?](#q---what-are-the-motivations-for-executorservice)
    * [1. Resource Management (The "Thread Explosion" Problem)](#1-resource-management-the-thread-explosion-problem)
    * [2. Abstraction (The "Producer-Consumer" Problem)](#2-abstraction-the-producer-consumer-problem)
    * [3. Returning Results (The "Void" Problem)](#3-returning-results-the-void-problem)
    * [Summary Table](#summary-table)
  * [Q - How do you properly shut down an ExecutorService?](#q---how-do-you-properly-shut-down-an-executorservice)
    * [Why shutdown is required](#why-shutdown-is-required)
    * [shutdown() — Graceful shutdown](#shutdown--graceful-shutdown)
    * [shutdownNow() — Immediate shutdown](#shutdownnow--immediate-shutdown)
    * [awaitTermination() — Wait for shutdown to complete](#awaittermination--wait-for-shutdown-to-complete)
    * [Proper shutdown pattern (INTERVIEW GOLD)](#proper-shutdown-pattern-interview-gold)
  * [Q - What's the diff b/w process and threads?](#q---whats-the-diff-bw-process-and-threads)
    * [Process — Components](#process--components)
      * [What a process owns](#what-a-process-owns)
    * [Thread — Components (core focus)](#thread--components-core-focus)
      * [What a thread owns (per thread)](#what-a-thread-owns-per-thread)
    * [Thread Control Block (TCB)](#thread-control-block-tcb)
    * [What Threads Share (inside the same process)](#what-threads-share-inside-the-same-process)
    * [Why Threads Are Lightweight (this is critical)](#why-threads-are-lightweight-this-is-critical)
      * [1. No separate address space](#1-no-separate-address-space)
      * [2. Cheaper context switching](#2-cheaper-context-switching)
      * [3. Minimal metadata](#3-minimal-metadata)
      * [4. Fast communication](#4-fast-communication)
    * [Interview-perfect closing line (memorize)](#interview-perfect-closing-line-memorize)
    * [Resources:](#resources-2)
  * [Q - What is an atomic operation?](#q---what-is-an-atomic-operation)
  * [Q - Which read and write operations are atomic in Java?](#q---which-read-and-write-operations-are-atomic-in-java)
    * [Resources](#resources-3)
  * [Q - What is deadlock?](#q---what-is-deadlock)
    * [Resources:](#resources-4)
  * [Q - Explain synchronized keyword](#q---explain-synchronized-keyword)
    * [Resources](#resources-5)
  * [Q - Explain synchronization problem](#q---explain-synchronization-problem)
    * [Resources](#resources-6)
  * [Q - Explain different ways of inter-thread communication](#q---explain-different-ways-of-inter-thread-communication)
    * [1. wait(), notify(), notifyAll() (Intrinsic Locks)](#1-wait-notify-notifyall-intrinsic-locks)
    * [2. volatile Variables (Visibility-Based Communication)](#2-volatile-variables-visibility-based-communication)
    * [3. Lock and Condition (java.util.concurrent.locks)](#3-lock-and-condition-javautilconcurrentlocks)
    * [4. Blocking Queues (`BlockingQueue`)](#4-blocking-queues-blockingqueue)
    * [5. Semaphores](#5-semaphores)
    * [6. Latches and Barriers](#6-latches-and-barriers)
    * [7. Atomic Variables](#7-atomic-variables)
    * [8. Thread.join()](#8-threadjoin)
    * [Summary Table (Interview Gold)](#summary-table-interview-gold)
    * [Resources](#resources-7)
  * [Q - What are some key points to remember when using virtual threads](#q---what-are-some-key-points-to-remember-when-using-virtual-threads)
    * [1. The Golden Rule: Throughput, Not Latency](#1-the-golden-rule-throughput-not-latency)
    * [2. CPU-Bound Tasks = No Benefit](#2-cpu-bound-tasks--no-benefit)
    * [3. The "Pinning" Problem (Critical Interview Topic)](#3-the-pinning-problem-critical-interview-topic)
    * [4. Do NOT Pool Virtual Threads](#4-do-not-pool-virtual-threads)
    * [5. ThreadLocal Explosion](#5-threadlocal-explosion)
    * [Resources](#resources-8)
  * [Q - Explain the evolution of concurrency API in Java](#q---explain-the-evolution-of-concurrency-api-in-java)
  * [Q - What is CopyOnWriteArrayList and Why is it named CopyOnWriteArrayList, why don't they use something like Collections.synchronizedList()?](#q---what-is-copyonwritearraylist-and-why-is-it-named-copyonwritearraylist-why-dont-they-use-something-like-collectionssynchronizedlist)
    * [CopyOnWriteArrayList](#copyonwritearraylist)
    * [Collections.synchronizedList](#collectionssynchronizedlist)
  * [Q - Which threads are guaranteed to be created when a Java program starts?](#q---which-threads-are-guaranteed-to-be-created-when-a-java-program-starts)
  * [Q - Explain stack and heap memory regions in the context of threads?](#q---explain-stack-and-heap-memory-regions-in-the-context-of-threads)
    * [1. Stack Memory (Thread-specific)](#1-stack-memory-thread-specific)
    * [2. Heap Memory (Shared across threads)](#2-heap-memory-shared-across-threads)
    * [3. Metaspace (not in heap)](#3-metaspace-not-in-heap)
    * [Key clarification (interview-critical)](#key-clarification-interview-critical)
    * [4. Why this matters for threads](#4-why-this-matters-for-threads)
    * [Interview-perfect closing line (memorize)](#interview-perfect-closing-line-memorize-1)
  * [Q - Explain how AtomicInteger works?](#q---explain-how-atomicinteger-works)
    * [1. The Core Concept: "Optimistic Locking"](#1-the-core-concept-optimistic-locking)
    * [2. The Hardware Magic: CAS (Compare-And-Swap)](#2-the-hardware-magic-cas-compare-and-swap)
    * [3. The "Retry Loop" (Spin Lock)](#3-the-retry-loop-spin-lock)
    * [4. The Code (Under the Hood)](#4-the-code-under-the-hood)
    * [5. Pros & Cons (Interview Gold)](#5-pros--cons-interview-gold)
  * [Q - What is Spurious Wakeup?](#q---what-is-spurious-wakeup)
  * [Q - What is class level lock?](#q---what-is-class-level-lock)
  * [Q - How threads communicate using wait() and notify()?](#q---how-threads-communicate-using-wait-and-notify)
    * [The problem we are solving using wait() and notify()](#the-problem-we-are-solving-using-wait-and-notify)
    * [Very important rules (must know)](#very-important-rules-must-know)
    * [The shared resource](#the-shared-resource)
    * [Producer logic (`put()`)](#producer-logic-put)
      * [Step 1: Check the condition](#step-1-check-the-condition)
      * [Step 2: Produce data](#step-2-produce-data)
      * [Step 3: Notify waiting threads](#step-3-notify-waiting-threads)
    * [Consumer logic (`get()`)](#consumer-logic-get)
      * [Step 1: Check the condition](#step-1-check-the-condition-1)
      * [Step 2: Consume data](#step-2-consume-data)
      * [Step 3: Notify waiting threads](#step-3-notify-waiting-threads-1)
    * [`notify()` vs `notifyAll()` (critical distinction)](#notify-vs-notifyall-critical-distinction)
    * [Limitations of `wait()` / `notify()`](#limitations-of-wait--notify)
    * [Thread starvation using `wait()` and `notify()`](#thread-starvation-using-wait-and-notify)
    * [What you are observing](#what-you-are-observing)
    * [Important concept: `notifyAll()` ≠ fairness](#important-concept-notifyall--fairness)
    * [Revised Code (Fair Version) using ReentrantLock](#revised-code-fair-version-using-reentrantlock)
    * [How threads communicate using Lock and Condition](#how-threads-communicate-using-lock-and-condition)
    * [What problem this code solves](#what-problem-this-code-solves)
    * [Why Lock and Condition are used together](#why-lock-and-condition-are-used-together)
      * [`Lock` → mutual exclusion (who can enter)](#lock--mutual-exclusion-who-can-enter)
      * [`Condition` → thread coordination (who should wait and wake)](#condition--thread-coordination-who-should-wait-and-wake)
    * [Why both are required (important)](#why-both-are-required-important)
    * [Step-by-step flow (ELI5)](#step-by-step-flow-eli5)
      * [Producer (`put()`)](#producer-put)
      * [Consumer (`get()`)](#consumer-get)
    * [Why while is used instead of if](#why-while-is-used-instead-of-if)
    * [Why signalAll() is used instead of signal()](#why-signalall-is-used-instead-of-signal)
    * [Why fairness (new ReentrantLock(true)) matters](#why-fairness-new-reentrantlocktrue-matters)
  * [Q - What is CAS (Compare-And-Swap)?](#q---what-is-cas-compare-and-swap)
    * [1. The Mechanics (The 3 Operands)](#1-the-mechanics-the-3-operands)
    * [2. The Atomic Operation](#2-the-atomic-operation)
    * [3. The Retry Loop (Spin Lock)](#3-the-retry-loop-spin-lock-1)
    * [4. Pros & Cons (Summary)](#4-pros--cons-summary)
  * [Q - What is Structured Concurrency (Java 21 Preview)?](#q---what-is-structured-concurrency-java-21-preview)
    * [The Core Problem It Solves](#the-core-problem-it-solves)
    * [The Structured Concurrency Principle](#the-structured-concurrency-principle)
    * [The API: `StructuredTaskScope`](#the-api-structuredtaskscope)
    * [What Just Happened?](#what-just-happened)
    * [Why This Is Powerful](#why-this-is-powerful)
    * [Relationship with Virtual Threads](#relationship-with-virtual-threads)
    * [Two Common Policies](#two-common-policies)
      * [1 ShutdownOnFailure](#1-shutdownonfailure)
      * [2 ShutdownOnSuccess](#2-shutdownonsuccess)
    * [🆚 Compared to CompletableFuture](#-compared-to-completablefuture)
  * [Q - How does ConcurrentHashMap work internally? (Java 7 vs Java 8)](#q---how-does-concurrenthashmap-work-internally-java-7-vs-java-8)
  * [Q - What were the limitations of the Future interface in Java 5, and how does CompletableFuture address them?](#q---what-were-the-limitations-of-the-future-interface-in-java-5-and-how-does-completablefuture-address-them)
    * [1. The Blocking Problem (`get()`)](#1-the-blocking-problem-get)
    * [2. Lack of Composition (Chaining)](#2-lack-of-composition-chaining)
    * [3. Combining Multiple Futures](#3-combining-multiple-futures)
    * [4. No Exception Handling](#4-no-exception-handling)
    * [5. Cannot Be Manually Completed](#5-cannot-be-manually-completed)
    * [Summary Table](#summary-table-1)
* [Level 6: Modern Java (Java 8 to Java 21)](#level-6-modern-java-java-8-to-java-21)
  * [Q - What is functional interface.](#q---what-is-functional-interface)
    * [Examples of Functional Interfaces in Java](#examples-of-functional-interfaces-in-java)
  * [Q - Can you tell few functional interface which is already there before java 8?](#q---can-you-tell-few-functional-interface-which-is-already-there-before-java-8)
  * [Q - What are all functional interface introduced in java 8?](#q---what-are-all-functional-interface-introduced-in-java-8)
  * [Q - What is lambda expression?](#q---what-is-lambda-expression)
  * [Q - What is Stream in java 8?](#q---what-is-stream-in-java-8)
  * [Q - How Java resolves method conflicts from multiple interfaces?](#q---how-java-resolves-method-conflicts-from-multiple-interfaces)
    * [1. No Conflict for Abstract Methods (Pre–Java 8)](#1-no-conflict-for-abstract-methods-prejava-8)
    * [2. Class Always Wins over Interface](#2-class-always-wins-over-interface)
    * [3. Conflict Between Default Methods (Diamond Problem)](#3-conflict-between-default-methods-diamond-problem)
    * [4. Interface Inheritance: Most Specific Default Wins](#4-interface-inheritance-most-specific-default-wins)
    * [5. Abstract vs Default Method](#5-abstract-vs-default-method)
    * [6. Static Methods in Interfaces](#6-static-methods-in-interfaces)
    * [Conflict Resolution Priority (Memory Aid)](#conflict-resolution-priority-memory-aid)
    * [Interview-Ready One-Liner](#interview-ready-one-liner)
  * [Q - Why default methods were introduced in interfaces?](#q---why-default-methods-were-introduced-in-interfaces)
    * [The Problem (Before Java 8)](#the-problem-before-java-8)
    * [The Real-World Scenario](#the-real-world-scenario)
    * [Secondary Benefit: "Optional" Methods](#secondary-benefit-optional-methods)
      * [The Classic "Mouse Listener" Problem](#the-classic-mouse-listener-problem)
      * [1. The "Old Way" (Painful)](#1-the-old-way-painful)
      * [2. The "New Way" (With Default Methods)](#2-the-new-way-with-default-methods)
  * [Q - What are sealed classes?](#q---what-are-sealed-classes)
    * [1. The Syntax](#1-the-syntax)
    * [2. The Three Rules for Subclasses](#2-the-three-rules-for-subclasses)
    * [3. Why use them? (The "Killer Feature")](#3-why-use-them-the-killer-feature)
  * [Q - Why static methods inside interface were introduced in Java?](#q---why-static-methods-inside-interface-were-introduced-in-java)
    * [Important Distinction: No Inheritance](#important-distinction-no-inheritance)
  * [Q - What is Predicate joining?](#q---what-is-predicate-joining)
    * [How it works](#how-it-works)
    * [Code Example](#code-example)
  * [Q - What is Functional joining?](#q---what-is-functional-joining)
    * [The Methods](#the-methods)
    * [Code Example](#code-example-1)
    * [Visualizing andThen vs compose](#visualizing-andthen-vs-compose)
  * [Q - What is Consumer chaining?](#q---what-is-consumer-chaining)
  * [Q - How to use chaining with Supplier?](#q---how-to-use-chaining-with-supplier)
  * [Q - Difference between Optional.of() and Optional.ofNullable()?](#q---difference-between-optionalof-and-optionalofnullable)
* [Level 7: JVM Architecture & Internals](#level-7-jvm-architecture--internals)
  * [Q - What is JIT?](#q---what-is-jit)
  * [Q - What is class Loader?](#q---what-is-class-loader)
  * [Q - What are different types of classloaders?](#q---what-are-different-types-of-classloaders)
  * [Q - What are the diff memory area allocated by JVM?](#q---what-are-the-diff-memory-area-allocated-by-jvm)
  * [Q - Can you explain the architectural change from PermGen to Metaspace in Java 8? Specifically, where are Class definitions and static variables stored in the modern memory model, and what happens at the OS and JVM level if Metaspace reaches its limit?](#q---can-you-explain-the-architectural-change-from-permgen-to-metaspace-in-java-8-specifically-where-are-class-definitions-and-static-variables-stored-in-the-modern-memory-model-and-what-happens-at-the-os-and-jvm-level-if-metaspace-reaches-its-limit)
    * [1. The Old World: PermGen (Java 7 and older)](#1-the-old-world-permgen-java-7-and-older)
    * [2. The New World: Metaspace (Java 8+)](#2-the-new-world-metaspace-java-8)
      * [Key Difference: Location](#key-difference-location)
    * [3. Answering Your Specific Questions](#3-answering-your-specific-questions)
      * ["Where do Class Definitions live?"](#where-do-class-definitions-live)
      * ["Where do Static Variables live?"](#where-do-static-variables-live)
    * [Comparison: PermGen vs. Metaspace](#comparison-permgen-vs-metaspace)
    * [4. What happens if Metaspace fills up?](#4-what-happens-if-metaspace-fills-up)
      * [Common Causes of Metaspace OOM:](#common-causes-of-metaspace-oom)
      * [Summary for the Interview](#summary-for-the-interview)
  * [Q - Explain JVM Architecture?](#q---explain-jvm-architecture)
    * [1. JVM Language Class (.class file)](#1-jvm-language-class-class-file)
    * [2. Class Loader — “The Librarian”](#2-class-loader--the-librarian)
    * [3. JVM Memory (Big Box in Diagram)](#3-jvm-memory-big-box-in-diagram)
      * [3.1 Method Area — "Class Blueprint Shelf"](#31-method-area--class-blueprint-shelf)
      * [3.2 Heap — "Big Toy Box"](#32-heap--big-toy-box)
      * [3.3 Stack — "Each Thread's Notebook"](#33-stack--each-threads-notebook)
      * [3.4 PC Register — "Bookmark"](#34-pc-register--bookmark)
      * [3.5 Native Method Stack — "Foreign Language Notes"](#35-native-method-stack--foreign-language-notes)
    * [4. Execution Engine — "The Brain"](#4-execution-engine--the-brain)
      * [4.1 Interpreter — "Reads Slowly"](#41-interpreter--reads-slowly)
      * [4.2 JIT Compiler — "Learns and Gets Faster"](#42-jit-compiler--learns-and-gets-faster)
      * [4.3 Garbage Collector — "Cleaner"](#43-garbage-collector--cleaner)
    * [5. Native Method Interface (JNI) — "Translator"](#5-native-method-interface-jni--translator)
    * [6 Native Method Libraries — "External Helpers"](#6-native-method-libraries--external-helpers)
    * [How Everything Works Together (Story)](#how-everything-works-together-story)
  * [Q - Explain the JVM heap structure shown in this diagram and describe the role of each memory region.](#q---explain-the-jvm-heap-structure-shown-in-this-diagram-and-describe-the-role-of-each-memory-region)
    * [Big Picture (ELI5)](#big-picture-eli5)
    * [1. Young Generation](#1-young-generation)
      * [1.1 Eden Space (Birthplace)](#11-eden-space-birthplace)
      * [1.2 Survivor Space S0 (First Survival Test)](#12-survivor-space-s0-first-survival-test)
      * [1.3 Survivor Space S1 (Second Survival Test)](#13-survivor-space-s1-second-survival-test)
    * [2. Promotion to Old Generation](#2-promotion-to-old-generation)
    * [3. Old Generation (Tenured)](#3-old-generation-tenured)
    * [4. Why Two Survivor Spaces?](#4-why-two-survivor-spaces)
    * [5. End-to-End Example Flow](#5-end-to-end-example-flow)
    * [Resources](#resources-9)
  * [Q - Explain WeakHashMap](#q---explain-weakhashmap)
    * [The Problem: The "Sticky" Metadata](#the-problem-the-sticky-metadata)
    * [The Solution: `WeakHashMap`](#the-solution-weakhashmap)
    * [When to use this? (The "Metadata" Use Case)](#when-to-use-this-the-metadata-use-case)
  * [Q - Explain SoftReference](#q---explain-softreference)
    * [1. The Use Case: Building an In-Memory Cache](#1-the-use-case-building-an-in-memory-cache)
    * [2. The Mechanics (Ground Level)](#2-the-mechanics-ground-level)
    * [3. The Coding Pattern (The "Check-Check-Reload")](#3-the-coding-pattern-the-check-check-reload)
    * [Summary](#summary)
* [Level 8: Garbage Collection & Performance Tuning](#level-8-garbage-collection--performance-tuning)
  * [Q - How to manually trigger the garbage collection process?](#q---how-to-manually-trigger-the-garbage-collection-process)
  * [Q - Explain Minor GC vs Major GC vs Full GC](#q---explain-minor-gc-vs-major-gc-vs-full-gc)
    * [1. Minor GC — "Clean the kids' room"](#1-minor-gc--clean-the-kids-room)
      * [When does Minor GC happen?](#when-does-minor-gc-happen)
      * [What does Minor GC actually do?](#what-does-minor-gc-actually-do)
      * [Why Minor GC is fast](#why-minor-gc-is-fast)
      * [Interview line (memorize)](#interview-line-memorize)
    * [2. Major GC — "Clean the storage room"](#2-major-gc--clean-the-storage-room)
      * [When does Major GC happen?](#when-does-major-gc-happen)
      * [Why Major GC is slower](#why-major-gc-is-slower)
    * [Important interview clarification](#important-interview-clarification)
    * [3. Full GC — "Clean the entire house"](#3-full-gc--clean-the-entire-house)
      * [When does Full GC happen?](#when-does-full-gc-happen)
      * [Why Full GC is dangerous](#why-full-gc-is-dangerous)
      * [Interview killer line](#interview-killer-line)
    * [Side-by-side comparison (ELI5)](#side-by-side-comparison-eli5)
  * [Q - What is Stop-The-World(STW) problem?](#q---what-is-stop-the-worldstw-problem)
    * [Why does JVM need STW at all?](#why-does-jvm-need-stw-at-all)
    * [What exactly is stopped?](#what-exactly-is-stopped)
    * [Tiny code example](#tiny-code-example)
    * [ELI5 analogy](#eli5-analogy)
    * [Important truth (interview gold)](#important-truth-interview-gold)
  * [Q - What is Allocation Failure?](#q---what-is-allocation-failure)
    * [Simple code example](#simple-code-example)
    * [Step-by-step what JVM does](#step-by-step-what-jvm-does)
  * [Q - What is Promotion Failure?](#q---what-is-promotion-failure)
    * [Step 1: Objects are created in Eden](#step-1-objects-are-created-in-eden)
    * [Step 2: Eden becomes full → Allocation Failure](#step-2-eden-becomes-full--allocation-failure)
    * [Step 3: Minor GC starts (STW)](#step-3-minor-gc-starts-stw)
    * [Step 4: JVM tries to evacuate live objects](#step-4-jvm-tries-to-evacuate-live-objects)
    * [Step 5: JVM attempts promotion](#step-5-jvm-attempts-promotion)
    * [Step 6: Promotion Failure occurs (THIS IS THE MOMENT)](#step-6-promotion-failure-occurs-this-is-the-moment)
    * [Step 7: Old Generation cleanup attempt](#step-7-old-generation-cleanup-attempt)
    * [Step 8: JVM escalates → Full GC](#step-8-jvm-escalates--full-gc)
    * [Step 8: Why Full GC still fails here](#step-8-why-full-gc-still-fails-here)
    * [One-sentence interview answer](#one-sentence-interview-answer)
  * [Q - Explain working of GC Roots?](#q---explain-working-of-gc-roots)
    * [Example 1: Single thread, single object](#example-1-single-thread-single-object)
      * [What memory looks like while main is running](#what-memory-looks-like-while-main-is-running)
      * [How GC works here (step by step)](#how-gc-works-here-step-by-step)
    * [Example 2: Multiple method calls (stack frames)](#example-2-multiple-method-calls-stack-frames)
      * [GC root traversal](#gc-root-traversal)
    * [Example 3: When stack root disappears](#example-3-when-stack-root-disappears)
      * [GC traversal now](#gc-traversal-now)
    * [Example 4: Static variable (class root)](#example-4-static-variable-class-root)
      * [GC traversal](#gc-traversal)
    * [Example 5: Multiple threads](#example-5-multiple-threads)
      * [GC traversal](#gc-traversal-1)
    * [Example 6: Following references (walking the graph)](#example-6-following-references-walking-the-graph)
      * [GC traversal](#gc-traversal-2)
    * [The single rule GC follows (memorize this)](#the-single-rule-gc-follows-memorize-this)
  * [Q - Explain the working of G1 Garbage Collector](#q---explain-the-working-of-g1-garbage-collector)
    * [What is G1 GC?](#what-is-g1-gc)
    * [Code example (we will use this throughout)](#code-example-we-will-use-this-throughout)
    * [Phase 1: Object allocation (NO GC yet)](#phase-1-object-allocation-no-gc-yet)
    * [Phase 2: Remembered Set creation (during normal execution)](#phase-2-remembered-set-creation-during-normal-execution)
    * [Phase 3: GC starts (Stop-the-World)](#phase-3-gc-starts-stop-the-world)
      * [Step 1: GC Root traversal (liveness)](#step-1-gc-root-traversal-liveness)
      * [Step 2: Region accounting (THIS IS CRITICAL)](#step-2-region-accounting-this-is-critical)
      * [Step 3: Region selection (why G1 is called "Garbage First")](#step-3-region-selection-why-g1-is-called-garbage-first)
      * [Step 4: Safety check using remembered sets](#step-4-safety-check-using-remembered-sets)
      * [Step 5: Cleanup result](#step-5-cleanup-result)
    * [Now let's place this into the FULL G1 FLOW](#now-lets-place-this-into-the-full-g1-flow)
      * [Young GC (baseline behavior)](#young-gc-baseline-behavior)
      * [When Old Gen pressure appears](#when-old-gen-pressure-appears)
    * [Full escalation chain (memorize this)](#full-escalation-chain-memorize-this)
    * [When does G1 move to Full GC?](#when-does-g1-move-to-full-gc)
    * [Why classic collectors were slower](#why-classic-collectors-were-slower)
    * [Final interview-ready summary (perfect answer)](#final-interview-ready-summary-perfect-answer)
  * [Q - Java has automatic Garbage Collection, which is supposed to manage memory for us. However, Memory Leaks are still a very real problem in Java applications.](#q---java-has-automatic-garbage-collection-which-is-supposed-to-manage-memory-for-us-however-memory-leaks-are-still-a-very-real-problem-in-java-applications)
  * [Q - Explain Serial GC](#q---explain-serial-gc)
    * [1. What is Serial GC?](#1-what-is-serial-gc)
    * [2. How it Works: The "Stop-The-World" Event](#2-how-it-works-the-stop-the-world-event)
    * [3. The Two Components (Young vs. Old)](#3-the-two-components-young-vs-old)
      * [A. Young Generation (DefNew)](#a-young-generation-defnew)
      * [B. Old Generation (TenuredGeneration)](#b-old-generation-tenuredgeneration)
    * [4. Pros and Cons (Interview Material)](#4-pros-and-cons-interview-material)
    * [5. When should you use it?](#5-when-should-you-use-it)
  * [Q - Explain Parallel GC](#q---explain-parallel-gc)
    * [1. The Core Concept: "Strength in Numbers"](#1-the-core-concept-strength-in-numbers)
    * [2. How it Works (Under the Hood)](#2-how-it-works-under-the-hood)
      * [The "Stop-The-World" Sequence:](#the-stop-the-world-sequence)
    * [3. The Two Components (Young vs. Old)](#3-the-two-components-young-vs-old-1)
      * [A. Young Generation (PSYoungGen)](#a-young-generation-psyounggen)
      * [B. Old Generation (ParallelOld)](#b-old-generation-parallelold)
    * [4. The "Throughput" Focus (Important for Interviews)](#4-the-throughput-focus-important-for-interviews)
    * [5. Pros and Cons](#5-pros-and-cons)
    * [6. Summary Comparison](#6-summary-comparison)
  * [Q - Explain Concurrent Mark Sweep(CMS) GC](#q---explain-concurrent-mark-sweepcms-gc)
    * [1. The Core Concept: "Concurrent"](#1-the-core-concept-concurrent)
    * [2. The Two Components (Young vs. Old)](#2-the-two-components-young-vs-old)
    * [3. How it Works: The 4 Phases](#3-how-it-works-the-4-phases)
      * [Phase 1: Initial Mark (Stop-The-World)](#phase-1-initial-mark-stop-the-world)
      * [Phase 2: Concurrent Mark (App Running)](#phase-2-concurrent-mark-app-running)
      * [Phase 3: Remark (Stop-The-World)](#phase-3-remark-stop-the-world)
      * [Phase 4: Concurrent Sweep (App Running)](#phase-4-concurrent-sweep-app-running)
    * [4. The Fatal Flaw: "Fragmentation" (The Swiss Cheese Problem)](#4-the-fatal-flaw-fragmentation-the-swiss-cheese-problem)
    * [5. The "Concurrent Mode Failure"](#5-the-concurrent-mode-failure)
    * [6. Summary for Interview](#6-summary-for-interview)
  * [Q - Explain G1 GC](#q---explain-g1-gc)
    * [G1GC (Garbage First) – The "Predictable" Collector](#g1gc-garbage-first--the-predictable-collector)
    * [1. The Architecture: "Regions"](#1-the-architecture-regions)
    * [2. The Lifecycle (How it Runs)](#2-the-lifecycle-how-it-runs)
      * [Phase A: Young Only Phase (Normal Mode)](#phase-a-young-only-phase-normal-mode)
      * [Phase B: The Concurrent Marking Cycle (The Proactive Trigger)](#phase-b-the-concurrent-marking-cycle-the-proactive-trigger)
      * [Phase C: The Mixed GC (The "Magic")](#phase-c-the-mixed-gc-the-magic)
    * [3. The Killer Feature: "Predictable Pauses"](#3-the-killer-feature-predictable-pauses)
    * [4. The Failure Mode: "Evacuation Failure"](#4-the-failure-mode-evacuation-failure)
    * [5. Summary for the Interview](#5-summary-for-the-interview)
  * [Q - Can you compare the different Garbage Collectors in Java and explain when to use each one?](#q---can-you-compare-the-different-garbage-collectors-in-java-and-explain-when-to-use-each-one)
    * [The Ultimate Java GC Cheat Sheet](#the-ultimate-java-gc-cheat-sheet)
    * [Notes (Accuracy Improvements)](#notes-accuracy-improvements)
  * [Q - What is a heap dump? Why do we use it? Have you ever taken a heap dump?](#q---what-is-a-heap-dump-why-do-we-use-it-have-you-ever-taken-a-heap-dump)
    * [1. What is a Heap Dump? (The "Crime Scene Photo")](#1-what-is-a-heap-dump-the-crime-scene-photo)
    * [2. Why do we use it?](#2-why-do-we-use-it)
      * [A. The `OutOfMemoryError` (OOM)](#a-the-outofmemoryerror-oom)
      * [B. Memory Leaks](#b-memory-leaks)
    * [3. "Have you ever taken a heap dump?" (The Interview Answer)](#3-have-you-ever-taken-a-heap-dump-the-interview-answer)
      * [Scenario A: The Proactive Setup (Best Practice)](#scenario-a-the-proactive-setup-best-practice)
      * [Scenario B: The Manual Inspection (Debugging a Slow App)](#scenario-b-the-manual-inspection-debugging-a-slow-app)
    * [4. How do you analyze it? (The "Eclipse MAT" Tool)](#4-how-do-you-analyze-it-the-eclipse-mat-tool)
    * [Summary for the Interview](#summary-for-the-interview-1)
  * [Q - What is memory management in Java?](#q---what-is-memory-management-in-java)
  * [Q - What are the types of Heap memory?](#q---what-are-the-types-of-heap-memory)
    * [1. Young Generation (The Nursery)](#1-young-generation-the-nursery)
    * [2. Old Generation (The Retirement Home)](#2-old-generation-the-retirement-home)
      * [Summary Table for Interview](#summary-table-for-interview)
  * [Q - How do you optimize JVM memory?](#q---how-do-you-optimize-jvm-memory)
    * [Step 1: Right-Sizing the Heap ( The Foundation)](#step-1-right-sizing-the-heap--the-foundation)
    * [Step 2: Choosing the Right Collector](#step-2-choosing-the-right-collector)
    * [Step 3: Tuning the "Pause Goal" (The Magic Knob)](#step-3-tuning-the-pause-goal-the-magic-knob)
    * [Step 4: Handling "Metaspace" (The Hidden Memory)](#step-4-handling-metaspace-the-hidden-memory)
      * [Step 5: Enable GC Logging (The Black Box)](#step-5-enable-gc-logging-the-black-box)
      * [Summary for the Interview](#summary-for-the-interview-2)
<!-- TOC -->

# Level 1: Core Java

## Q - What is JDK? What is the diff b/w JDK and JRE?

JDK (Java Development Kit) is a software development environment used to develop Java applications.

* JVM (Java Virtual Machine): The engine that actually runs the code.
* JRE = JVM + Library Classes (`java.lang`, `java.util`, etc.)
* JDK = JRE + Development Tools (compilers like `javac`, `javap`, debuggers, documentation generator etc.)

-----------------------------

## Q - What are access specifiers?

Access Specifiers are predefined keywords used to help
JVM with understanding the scope of a variable, method, and
a class. We have four access specifiers.

1\. Private
* Keyword: `private`
* Scope: Only within the same class.

2\. Default (Package-Private)
* Keyword: (None - simply leave it blank)
* Scope: Only within the same package.

3\. Protected
* Keyword: `protected`
* Scope: Same package + Subclasses (even in different packages).

4\. Public
* Keyword: `public`
* Scope: Everywhere.

-----------------------------


## Q - What are packages?

Just a collection of related classes. The use of packages helps in code reusability and name clash.


-----------------------------


## Q - Is Java Pass by Value or Pass by Reference?

Pass by value

----------------


## Q - What is the diff b/w objects and references?

**1. Object**

An object is the actual data stored in memory (on the heap).

It contains:

* Fields (values)
* Methods (behavior)
* Its own memory address

You cannot directly access an object - you access it through a reference.

**2. Reference**

A reference is like a pointer or address that "points to" an object in memory.

* It does NOT hold the object
* It only holds the location of the object
* Multiple references can point to the same object

Example:

```java
Emp e1 = new Emp(10, "John");  // e1 is a reference, Emp(...) creates an object
Emp e2 = e1;                   // e2 is another reference pointing to the same object
```

Here:

* `new Emp(10, "John")` → creates an object on heap
* `e1` → a reference pointing to that object
* `e2` → another reference pointing to the same object


### Objects vs References

References != Objects

```mermaid
graph LR
  %% Centered title
  title["Objects vs References"]

  subgraph Stack [Stack Memory]
    direction TB
    ref1[referenceVar1]
    ref2[referenceVar2]
  end

  subgraph Heap [Heap Memory]
    obj((Object Instance))
  end

  ref1 --> obj
  ref2 --> obj

  style title fill:#ffffff,stroke:#ffffff,color:black,font-size:18px,font-weight:bold
```

### What is allocated where?

* References:
  * Can be allocated on the stack (e.g., as local variables).
  * Can be allocated on the heap if they are members (fields) of a class.

* Objects:
  * Are always allocated on the heap.


### Memory Regions - Summary

| **Heap (Shared)**                           | **Stack (Exclusive)**                                  |
|---------------------------------------------|--------------------------------------------------------|
| • **Objects** (The actual instances)        | • **Local primitive types** (int, double, etc.)        |
| • **Class members** (Fields inside objects) | • **Local references** (Variables pointing to objects) |
| • **Static variables**                      |                                                        |


### References

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/lecture/11199598#notes


-----------------------------


## Q - What is shadowing?

Shadowing happens when a variable declared in a inner scope has the same name as a variable in an outer scope.

The inner variable shadows (hides) the outer one - meaning the outer variable cannot be accessed in that inner scope.

In simple words:
> The closest variable with that name wins.

Here is an example:

```java
public class Main19 {
    public static void main(String[] args) {
        int i = 10;
        
        class SomeClass {
//            int i = 100;
            {
                 for (int i = 0; i < 10; i++) {
                     System.out.println(i);
                 }
            }
        }

        SomeClass someClass = new SomeClass();
        System.out.println(someClass);
    }
}
```

**What is happening?**

* You declared `int i = 10` in the `main` method.
* Inside the for loop, you declared another `int i`.
* The inner `i` shadows the outer `i`.

**Result:**

Inside the for loop:
* When you say `i`, Java uses the loop's `i`, not the main method's `i`.

So inside the loop:

* The outer `i = 10` becomes invisible.
* Only the loop variable `i` exists.


-----------------------------


## Q - Will the following code compile (Widening vs. Narrowing)?

```java
byte myByte = 'a';
char myChar = 'a';
short myShort;

myChar = myByte;
myShort = myChar;
myChar = myShort; 
```

No.

Here is the general rule:

> Java allows implicit conversions only when the conversion is a widening primitive conversion
> that cannot lose information or change the sign. All narrowing conversions require an
> explicit cast, except when assigning a compile-time constant that fits in the target type.
>

### 1. Widening = implicit (safe)

* Target type can represent all possible values of the source type
* Sign is preserved
* No overflow possible

### 2. Narrowing = explicit cast required (unsafe)

* Target type cannot represent all values
* Sign may change
* Overflow or truncation possible

---

### Why `myChar = myByte` is not allowed

> Because byte → char is not a widening primitive conversion.
>

**Reason:**

* `byte` is signed (`-128 to 127`)
* `char` is unsigned (`0 to 65535`)
* Some `byte` values (negative ones) cannot be represented by `char`


### Why `myShort = myChar` is not allowed

> Because char → short is not a widening primitive conversion.
>
>

**Reason:**

* `char` is unsigned (`0 to 65535`)
* `short` is signed (`-32768 to 32767`)
* Many valid `char` values cannot fit into `short`


### Why `myChar = myShort` is not allowed

> Because short → char is not a widening primitive conversion.
>

**Reason:**

* `short` is signed (`-32768 to 32767`)
* `char` is unsigned (`0 to 65535`)
* Negative `short` values cannot be represented by `char`


-----------------------------


## Q - var is used for Local Variable Type Inference (LVTI). Can we use it as an identifier?

Since Java 10, you can use `var` to let the compiler infer the type:

```java
var name = "John";   // inferred as String
var age = 25;        // inferred as int
```

But this is allowed only for local variables, **not fields, not method parameters, and not return types**.

Also note that, `var` is **not a reserved keyword**. It is a restricted type name.

This means:

* Java treats `var` specially only when used in variable declarations.
* But outside that context, you can use `var` as an identifier.

Example:

```java
int var = 10;        // valid
String var = "Hi";   // valid
class var { }        // valid class name (but discouraged)
```

If `var` appears where the compiler expects a type, it is treated as LVTI. Otherwise, it is treated
as a normal name.

-----------------------------

## Q - Can you initialize a var variable with null?

```java
var name = null;
```

No. because the type of the `name` variable can't be inferred.

-----------------------------

## Q - Mentions some other possible scenarios where we can't use the `var` (LVTI) keyword

```java
// cannot use var declaration in a compound statement
var j = 0, k = 0;

// again, cannot use var declaration in a compound statement
var m, n = 0;

// Cannot declare a var variable without also initializing it
var someObject;

// Cannot assign null to var variable, type cannot be inferred
var newvar = null;

// Cannot use array initializer in var declaration/initialization
var myArray = {"A", "B"};

// Cannot have an array of var
var[] newArray = new int[2];

public class VarDonts {
    // Invalid - Static class variables cannot be declared with var
    static var classVariable = 10;

    // Invalid - class instance variables cannot be declared with var
    var instanceVariable = 20;

    public static void main(String[] args) {
    }

    // Invalid, cannot have a method return type of var
    public static var returnThis(String[] args) {
        return args;
    }

    // Invalid, cannot have method parameter of var
    public static String[] returnThat(var args) {
        return args;
    }
}

// var can't be used as className
class var{

}
```

We can use **LVTI only for local variables in methods, code blocks and loop variables**.


-------------


## Q - Do `double` and `float` type overflow?

Integers **wrap around**, while Floats **explode to Infinity**.

Here is the distinction:

**1\. Integers (Wrap Around)**

When an integer type (`int`, `long`, `byte`, `short`) overflows, it loops back to the minimum value (negative).
This is often a silent bug.

```java
int max = Integer.MAX_VALUE; // 2,147,483,647
int result = max + 1;
// Result is -2,147,483,648 (Wraps to minimum)
```

**2\. Floating Point (Infinity)**

When a `float` or `double` exceeds its maximum storage capacity, it does not wrap around. Instead, it hits a special
value called `Infinity`.

```java
double max = Double.MAX_VALUE; // approx 1.8 x 10^308
double result = max * 1.1;     
// Result is Infinity
```

----------------------------


## Q - Is default keyword one of the access modifier?

No, the `default` keyword is NOT an access modifier keyword.

Here is the breakdown of the confusion:

### 1. The "Default Access Modifier" (The Invisible One)

When people talk about the Default Access Modifier (also called Package-Private),
they are talking about the absence of a keyword.

* **How you write it:** You literally write nothing.
* **Behavior:** Visible only within the same package.
* **Keyword used:** None.

Example:

```java
class Student {
    // No keyword used here! This is "Default Access".
    void study() { 
        System.out.println("Studying...");
    }
}
```

### 2. The `default` Keyword (The Actual Keyword)

The word `default` does exist as a keyword in Java, but it is used for completely
different things:

**Usage A: Interface Methods (Java 8+)** To provide a fallback implementation in an
interface so you don't break existing code

```java
interface Vehicle {
    // Here, 'default' is NOT about access control. 
    // It means "Here is the default code body".
    default void honk() {
        System.out.println("Beep!");
    }
}
```

**Usage B: Switch Statements** To specify what happens if no other case matches.

```java
switch(day) {
    case 1: print("Monday"); break;
    default: print("Weekend"); // The "Else" case
}
```


----------------


## Q - What is Covariant return type?

Covariant Return Type is a feature (introduced in Java 5) that allows an overriding method to return
a subclass (narrower type) of the return type declared in the parent method.

Example:

```java
class Burger {
    // Generic Burger
}

class CheeseBurger extends Burger {
    // Specific Burger
}

class BurgerShop {
    // Parent promises to return a generic Burger
    public Burger order() {
        System.out.println("Here is a standard burger");
        return new Burger();
    }
}

class CheeseBurgerShop extends BurgerShop {
    // OVERRIDING:
    // We changed the return type from 'Burger' to 'CheeseBurger'.
    // This is allowed because CheeseBurger IS-A Burger.
    @Override
    public CheeseBurger order() {
        System.out.println("Here is a cheeseburger");
        return new CheeseBurger();
    }
}

public class Test {
    public static void main(String[] args) {
        CheeseBurgerShop shop = new CheeseBurgerShop();
        
        // No casting needed! We get the specific type directly.
        CheeseBurger cb = shop.order(); 
    }
}
```

Why is this useful?

It saves you from doing annoying type-casting.

This relates directly to the Liskov Substitution Principle:
> The Child can provide more specific guarantees than the Parent, but never less.


----------------


## Q - What if a method in child class is more restricted than a parent class?

It causes a **Compile Time Error**.

This rule exists to preserve the Contract of the Parent Class (related to the **Liskov Substitution Principle**).

The Liskov Substitution Principle (LSP) states:
> Whatever the Parent can do, the Child must also be able to do.

When you override a method, you cannot make the access modifier more restrictive than the parent method.

* ✅ You CAN keep it the same.
* ✅ You CAN make it less restrictive (more visible).
* ❌ You CANNOT make it more restrictive (less visible).


----------------


## Q - Does the finally block execute if there is a return statement inside try or catch?

Consider the following example:

```java
public class MainExample {

    static class Connection {
        public void open() {
            System.out.println("Connection opened");
        }

        public void close() {
            System.out.println("Connection closed");
        }
    }

    private static void method2() {
        Connection connection = new Connection();
        connection.open();

        try {
            // LOGIC
            String str = null;
            str.toString();   // This will throw NullPointerException
            return;           // return inside try
        }
        catch (Exception e) {
            // NOT PRINTING EXCEPTION TRACE (bad practice)
            System.out.println("Exception Handled - Method 2");
            return;           // return inside catch
        }
        finally {
            connection.close();  // will execute?
        }
    }

    public static void main(String[] args) {
        method2();
    }
}
```

Yes. Regardless of whether the try or catch block completes normally, throws an exception,
or executes a `return` statement, the finally block always executes before the method returns.
It is typically used for cleanup tasks like closing connections.


-----------------------


## Q - Is the following program correct?

```java
class Head{
    static public void main(String[] args){
        System.out.println("aa");
    }
}
```

Yes. Notice that `public` and `static` keywords can be in any order.


-----------------------------


## Q - Can you run the code before executing main methods?

Yes, Java allows execution of code before the main method using a **static initializer**.
A **static initializer** runs when the class is loaded and initialized by the JVM, before
any objects are created and before the main method is executed.

**How it works:**

When you run a Java program (e.g., `java MyClass`), the JVM performs the following steps:

* **Load the Class:** It finds `MyClass.class` and loads it into memory.
* **Execute Static Blocks:** During this loading phase, it runs all `static` blocks and initializes static variables.
* **Call Main:** Only after the class is fully loaded does it look for and call `public static void main`.

Example:

```java
public class PreMain {
    
    // 1. This runs FIRST (during class loading)
    static {
        System.out.println("I am running BEFORE main!");
    }

    // 2. This runs SECOND
    public static void main(String[] args) {
        System.out.println("I am running INSIDE main.");
    }
}
```

**Output:**

```text
I am running BEFORE main!
I am running INSIDE main.
```

-----------------------------


## Q - Is it true that main thread doesn't terminate until the child threads are done?

No, `main` does NOT wait for the child thread. Consider the following example:

```java
public class Test06 {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {

            try {
                Thread.sleep(1_000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            System.out.println("t1 - exit");
        });

        t1.start();
        System.out.println("main - exit");
    }
}
```

**Output:**

```text
main - exit
t1 - exit
```

Execution order (important)

1. `t1.start()` → starts a new thread
2. `main` immediately prints:
    ```text
    main - exit
    ```
3. `main` method finishes
4. JVM does NOT exit yet
5. **JVM waits until all non-daemon threads finish**
6. `t1` wakes up after 1 second
7. `t1` prints:
    ```text
    t1 - exit
    ```
8. Now JVM exits


----------------------------------------


## Q - What is hashCode() and how It's related to equals()?

The `hashCode()` method returns an integer that acts as a **category label** or
"bucket address" for an object. It is designed to speed up lookups in
hash-based collections (`HashMap`, `HashSet`, `Hashtable`).

### The Analogy: The Sectioned Library

Imagine a library with 1,000,000 books.

* Without `hashCode`: To find "Harry Potter," you must start at the entrance
  and check every single book one by one until you find it. (**O(N) - Very Slow**)

* With `hashCode`: The library is divided into numbered zones (Zone 1 to Zone 100).
  * You compute the hash: *"Harry Potter"*  **Zone 42**.
  * You walk straight to Zone 42 and ignore the other 99 zones. You only search the small
    pile of books in that specific zone. (**O(1) - Very Fast**)



### How it works in a HashMap (The 3 Steps)

When you call `map.put(key, value)`, the JVM follows this precise sequence:

1. **Generate Hash:** Call `key.hashCode()` to get a unique integer (e.g., `859403`).
2. **Calculate Index:** Convert that hash into a valid array index using modulo (e.g., `859403 % 16 = Index 11`).
3. **Handle Collisions:**
    * If Bucket #11 is empty, store the object there.
    * If Bucket #11 is occupied, use `equals()` to check if the key already exists.
      If not, add it to the chain (Linked List or Tree).
    * **Crucial Java 8+ Detail:** The chain starts as a Linked List. However, if the number of items in
      that single bucket exceeds 8 (the `TREEIFY_THRESHOLD`), the JVM automatically morphs that list into
      a Red-Black Tree. This improves performance from `O(n)` to `O(log n)` during high collisions.


### The Contract: The "Law" of HashCode

If you override `equals()`, you **MUST** override `hashCode()`. Breaking this contract leads to "Lost Objects."

| The Rule                                                                                | The Logic                                                                                                                                                                                                              |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rule 1: If `a.equals(b)` is TRUE, then `a.hashCode() == b.hashCode()` MUST be true.** | **Why?** If two equal keys have different hash codes, they will land in **different buckets**. When you try to `get(key)`, the Map will look in the wrong bucket and return `null`, effectively losing your data.      |
| **Rule 2: If `a.hashCode() == b.hashCode()`, `a.equals(b)` does NOT have to be true.**  | **Why?** This is a **Collision**. Different words (like "Aa" and "BB") can mathematically result in the same hash. They land in the same bucket, and the Map uses `equals()` to differentiate them within that bucket. |
| **Rule 3: Consistency**                                                                 | `hashCode()` must return the same value throughout the object's life (unless the object is modified).                                                                                                                  |

**Summary for Interviews:**

> `hashCode()` determines **where** the object is stored (the bucket). `equals()` determines **what** the
> object is (identity). If you break the link between them, your HashMap essentially
> becomes a black hole where you can put objects in but never get them out.
>


---------------------


## Q - What is marker interface?

A marker interface is an interface with **no methods**. It's like putting a sticker on a class.

The sticker tells the JVM or a framework:
> "This class has a special property — treat it differently."

So the interface **marks** the class for special behavior.

Common marker interfaces in Java:

1. `Serializable`
2. `Cloneable`

**Example 1:** Serializable

Think of `Serializable` as putting a label on a box:

> "This object can be saved and restored!"

Java checks for this label before allowing serialization:

```java
class User implements Serializable {
    String name;
}
```

Without this label, Java refuses to serialize the object.

**Example 2:** Cloneable

If a class wants to allow `clone()`, it must "wear the badge":

```java
class Employee implements Cloneable {
    int id;
}
```

Without this badge, `clone()` throws `CloneNotSupportedException`.

### Resources

* [Marker Interface in Java (Tutorial) e.g. Serialization, Remote](https://www.youtube.com/watch?v=qeGCxKCWFcQ)


----------------------------


## Q - What are Wrapper classes

In Java, when you declare primitive datatypes, then Wrapper classes are responsible
for converting them into objects (Reference types). It was introduced so primitive types
can play nicely with the collections framework.

Wrapper classes have existed since Java 1.0; Java 5 later added autoboxing and unboxing
to make their usage seamless.

Every primitive has a corresponding Wrapper class.

| Primitive Type | Wrapper Class |
|----------------|---------------|
| `int`          | `Integer`     |
| `char`         | `Character`   |
| `double`       | `Double`      |
| `boolean`      | `Boolean`     |
| `byte`         | `Byte`        |

**All wrapper classes are immutable**


-----------------------------



## Q - Do local variables in Java have default values?

Local variables **do not have a default value**. Unlike instance variables (fields), they are not
automatically initialized. You must explicitly initialize them before use; otherwise, you will get
a compile-time error.


-----------------------------


## Q - What is copy constructor?

A copy constructor is a constructor that creates a new object by copying the state of
another object of the same class.

In Java, copy constructors are not built-in; they are user-defined.

Example:

```java
class Employee {
    int id;
    String name;

    // Normal constructor
    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // Copy constructor
    Employee(Employee other) {
        this.id = other.id;
        this.name = other.name;
    }
}
```

Usage:

```java
Employee e1 = new Employee(1, "Alice");
Employee e2 = new Employee(e1); // copy created
```

### Copy Constructor vs clone() — Which is better?

> Copy constructors are generally preferred over `clone()` in Java.
>

Here is the breakdown of why `clone()` is generally hated and Copy Constructors are preferred:

#### 1. The "Constructor Bypass" Problem (Critical)

`clone()` does not call the constructor. It performs a direct memory copy of the object.
This is dangerous because:

* **Validation Logic is Skipped:** If your constructor has logic like `if (age < 0) throw Error`, cloning an
  object bypasses this check. You might end up with an invalid object state.
* **Initialization Logic is Skipped:** Any setup code inside your constructor is ignored.

**Copy Constructor:** It uses the `new` keyword, so it forces the standard object creation
lifecycle, ensuring your object is always valid.

Here is an example:

```java
class User {
    private final int age;
    private final UUID id;

    // Main constructor (single source of truth)
    public User(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        this.age = age;
        this.id = UUID.randomUUID(); // initialization logic
    }

    // Copy constructor
    public User(User other) {
        this(other.age); // ✅ validation + initialization both run
    }

    public int getAge() {
        return age;
    }

    public UUID getId() {
        return id;
    }
}
```

#### 2. The Type Casting Tax

`clone()`: It returns an `Object`. You must manually cast it back to your specific class every time.

```java
// Annoying and ugly
User u2 = (User) u1.clone();
```

**Copy Constructor:** It is strongly typed. No casting required.

```java
// Clean
User u2 = new User(u1);
```


#### 3. The Exception Nightmare

`clone()`: It forces you to handle `CloneNotSupportedException`. This is a **Checked Exception**, meaning you
must wrap it in a try-catch block, even if you know your class implements Cloneable.
It creates noisy, ugly code.

**Copy Constructor:** No exceptions. It just works.


#### 4. The "Marker Interface" Confusion

The design of `clone()` is weird.

* You implement the Cloneable interface...
* ...but `Cloneable` has no methods.
* The actual `clone()` method is in the `Object` class and is `protected`.
* You have to implement an empty interface just to allow a method from a parent class to work.
  It's a very counter-intuitive API design.


-----------------------------


## Q - What object cloning?

Object cloning is the process of creating a copy of an existing object by directly
duplicating its memory state, without invoking any constructors.

Cloning is done using the `clone()` method, usually with the class implementing
the `Cloneable` marker interface.

### Why does Cloneable matter?

`Cloneable` is just a permission badge.

If a class does not implement it, calling `clone()` throws `CloneNotSupportedException`.

### Types of Cloning

#### Shallow Clone (default)

* Creates a new object
* All primitive fields are copied by value, meaning each primitive is duplicated as an
  independent copy inside the new object.
* All reference fields are copied by reference, meaning both objects continue to point
  to the same underlying referenced objects.
* Done using `super.clone()`


Example:

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    return super.clone();   // shallow clone
}
```

If the object contains references, both clones share the same internal objects.


#### Deep Clone

* Creates a new object
* Also creates new copies of all nested objects
* Nothing is shared between original and clone

**Example:**

```java
Emp copy = (Emp) super.clone();
copy.address = new Address(this.address); // deep copy
```

Deep clone must be implemented manually.

### Resources

1. Video 1: https://www.youtube.com/watch?v=b2uFL4BFDYg
1. Video 2: https://www.youtube.com/watch?v=WIh-TVq4ifI
1. Video 3: https://www.youtube.com/watch?v=KWbr7B5LDzs


-----------------------------


## Q - Do we have pointers in Java?

No

-----------------------------


## Q - What is Java String Pool & String Interning?

**String Interning** is a JVM optimization where only one copy of each distinct
string value is stored in a special memory region called the **String Constant Pool**.

Since `String` is the most widely used class in Java, creating a new object
for every single `"Hello"` or `"Error"` would waste massive amounts of RAM.
The Pool solves this by reusing instances.

To fully understand this, you must distinguish between creating a String
with a **Literal** versus the **`new` keyword**.

### 1. String Literal (Automatic Interning)

When you create a string using double quotes, Java automatically checks the pool.

```java
String s1 = "Hello";
String s2 = "Hello"; 

// s1 == s2 is TRUE

```

* **Action:** JVM checks if `"Hello"` exists in the Pool.
* **Result:** It finds the existing `"Hello"` created by `s1`.
* **Outcome:** `s2` points to the exact same memory address as `s1`. No new object is created.

---

### 2. The `new` Keyword (Forcing a New Object)

Using `new` bypasses the pool check for the returned reference.

```java
String s3 = new String("Hello");

// s1 == s3 is FALSE (Different addresses)
// s1.equals(s3) is TRUE (Same value)
```

---

* **Action:** This forces the creation of a **new object on the Heap**, even
  if `"Hello"` already exists in the pool.
* **Interview Tip:** This technically involves two objects:
  1. The **Literal** `"Hello"` (interned in the pool, if not present).
  2. The **New Object** (on the heap, which `s3` refers to).



### 3. Manual Interning (`.intern()`)

You can manually move a heap string into the pool (or retrieve its pool reference) using `.intern()`.

```java
String s3 = new String("Hello"); // Heap object
String s4 = s3.intern();         // Returns the Pool object

// s1 == s4 is TRUE
```

* **Use Case:** Useful when receiving massive amounts of duplicate strings
  from external sources (like a DB or CSV file) where you want to deduplicate memory.

---

### 4. Why is this safe? (Immutability)

The only reason Java can safely share one `"Hello"` object among 100 different variables
is because **Strings are Immutable**.

If `s1` could change its content from `"Hello"` to `"Help"`, it would corrupt `s2` and
every other variable pointing to that shared object. Because they cannot be changed, they
can be safely shared without thread-safety issues.


---------------



## Q - Which class is thread-safe: StringBuilder or StringBuffer?

`StringBuffer` is thread safe.


-----------------------------


## Q - How many times is the finalize() method called in Java?

### What is finalize()?

Imagine you throw an object into the garbage bin (make it eligible for Garbage Collection).

Before the JVM actually destroys it, the JVM may call a special method named `finalize()` to give the
object a chance to clean up.

Think of this as:
> "Hey object, I’m about to delete you. Do you want to do anything before you go?"

But this happens at most once per object.

### Key Idea 1: finalize() may run OR may not run

* JVM decides when GC runs.
* JVM decides if finalize() should run.
* So `finalize()` is NOT guaranteed.

### Key Idea 2: finalize() runs ONCE per object

Even if the object becomes garbage again later, `finalize()` will NOT run a second time.

Let's see a very simple example:

```java
class Demo {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("finalize() called");
    }
}

public class Test {
    public static void main(String[] args) {

        Demo d = new Demo();  // create object

        d = null;             // remove reference → eligible for GC

        System.gc();          // ask JVM to run GC (may run or may not)

        Thread.sleep(1000);   // give JVM time
    }
}
```

What might happen? You might see:

```text
finalize() called
```

Or you might see nothing (GC didn't run).

Either output is normal.

### Why "ONLY ONCE"? (Simple explanation)

Let's add a twist.

In `finalize()`, the object can "save" itself:

```java
class Demo {
    static Demo savedAgain;

    @Override
    protected void finalize() throws Throwable {
        System.out.println("finalize() called");
        savedAgain = this;   // object becomes ALIVE again
    }
}
```

What happens?

* You make the object eligible for GC by doing `d = null`.
* GC runs, sees object is garbage.
* GC calls `finalize()`.
* `finalize()` saves the object back → object becomes alive again.

Now the object is alive again and GC cannot delete it.

Later, if it becomes garbage again, GC will not call `finalize()` again.

Because JVM guarantees:
> finalize() is called only once for each object.
>

**Note:** `finalize()` has been deprecated since Java 9 and is marked for removal.
Alternative is to use `AutoCloseable` or `Cleaner` API.


-----------------------------


## Q - What is Serializable interface? List some real-world use-cases for it.

`Serializable` is a marker interface in Java that indicates an object can be converted
into a byte stream and later reconstructed back into an object.

It is primarily used when objects need to:

* Be persisted to disk
* Be transmitted over a network
* Be stored in caches or HTTP sessions
* Cross JVM boundaries


### Key statement

During serialization, the JVM uses reflection to inspect an object's fields and convert their values
into a byte stream, even though `Serializable` has no methods.

Now let’s prove this with an example.

```java
import java.io.Serializable;

class User implements Serializable {
  int id;
  String name;

  User(int id, String name) {
    this.id = id;
    this.name = name;
  }
}
```

Notice:

* `Serializable` has no methods
* We did not write any serialization logic


#### Serialization code (standard)

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws Exception {

        User user = new User(1, "Alice");

        ObjectOutputStream oos =
            new ObjectOutputStream(new FileOutputStream("user.ser"));

        oos.writeObject(user);
        oos.close();
    }
}
```

#### What the JVM actually does internally (THIS is the reflection part)

When this line runs:

```text
oos.writeObject(user);
```

The JVM internally does something conceptually similar to this (simplified):

```java
Class<?> clazz = user.getClass();          // via reflection
Field[] fields = clazz.getDeclaredFields(); // via reflection

for (Field f : fields) {
    f.setAccessible(true);                 // bypass access checks
    Object value = f.get(user);            // read field value
    writeToStream(value);                  // convert to bytes
}
```

**Note:** Just writing raw values would be insufficient. Java serialization must and does write metadata.


-----------------------------


## Q - What are some use cases of reflection

1. **Dependency Injection Frameworks (Spring Context)**
    * **How:** When you use `@Autowired`, Spring scans your classes, finds the dependencies, and
     injects them into private fields. It does this entirely via Reflection (because it can access
     private members).

2. **Testing Tools (JUnit / Mockito)**
    * **How:** JUnit uses reflection to find all methods annotated with `@Test` and run them.
     Mockito uses it to create "fake" proxy objects that look like your real classes.

3. **Serialization / Deserialization (Jackson / Gson)**
    * **How:** Converting JSON `{"name": "John"}` into a Java object `User`.
     The library inspects the class to match keys to field names.

4. **ORM (Hibernate / JPA)**
    * **How:** Mapping database columns to entity fields and generating SQL queries dynamically based on class definitions.

5. **IDEs and Debuggers (IntelliJ / Eclipse)**
    * **How:** When you type `myObject`. and see a dropdown list of methods (Auto-complete),
     the IDE is using reflection to inspect that object's class to see what methods exist.


-----------------------------


## Q - How many methods are there compare strings in Java?

There are 5 main ways to compare Strings, depending on your goal:

1. `equals()`: Checks if contents are identical (Case-sensitive).
    * **Use case:** Authentication (Password check).

2. `equalsIgnoreCase()`: Checks contents ignoring case.
    * **Use case:** Search functionality (User types `"java"`, finds `Java`).

3. `compareTo()`: Returns an integer (negative, zero, positive) for sorting (Lexicographical order).
    * **Use case:** Sorting a list of names alphabetically.

     | Result              | Meaning        | Order                          |
     |---------------------|----------------|--------------------------------|
     | **Negative** (`-1`) | `a` is smaller | **`a` comes first** (`a`, `b`) |
     | **Zero** (`0`)      | They are equal | Order doesn't change           |
     | **Positive** (`+1`) | `a` is larger  | **`b` comes first** (`b`, `a`) |

4. `compareToIgnoreCase()`: Same as `compareTo()` but case-insensitive.
    * **Use case:** Sorting names where `"Apple"` and `"apple"` should be treated effectively the same.

5. `Objects.equals(s1, s2)`: Null-safe version of `.equals()`.
    * **Use case:** Best practice when you are not sure if `s1` or `s2` is `null`. It avoids `NullPointerException`.


-----------------------------


## Q - Will the following statement adds string to the string pool?

```java
String s1 = "hello";
String s2 = s1 + " world";   // this adds the string to the string pool?
```

The Explanation:

1. **Compile-Time (String Pool):** If you write `"hello" + " world"`, the Java compiler sees that
   both are literals (constants). It combines them into `"hello world"` during compilation and
   places that single string in the pool.

2. **Run-Time (Heap):** In your code, `s1` is a variable. The compiler cannot know for
   sure what `s1` will contain at runtime (technically it can here, but the rule is strict
   for non-final variables). Therefore, JVM executes this as:

    ```java
    new StringBuilder().append(s1).append(" world").toString()
    ```
   **This creates a new object on the Heap**, not the Pool.

### The Nuance: The final Keyword

Follow-up question: "How can you force `s2` into the pool without using `.intern()`?"

Answer: Make `s1` final.

```java
final String s1 = "hello";  // Now it is a Constant
String s2 = s1 + " world";  // Compiler treats s1 as "hello"
```

* Because `s1` is `final`, the compiler treats it as a constant value.
* The expression becomes `"hello" + " world"`.
* The result `"hello world"` is placed in the String Pool.


-----------------------------


## Q - What happens when we concatenate string with different type?

When a String is concatenated with another operand using `+`, Java converts the
other operand to a String.

* For reference types, `toString()` is called (unless the reference is `null`).
* For primitive types, Java does not box them; instead, it uses `String.valueOf()` to convert them to a String.


### What Actually Happens (Under the Hood)

The compiler rewrites string concatenation into something like:

```java
new StringBuilder()
    .append(...)
    .append(...)
    .toString();
```

And each `append()` internally calls `String.valueOf(...)`.


#### Case 1: Primitive Types

```java
String s = "Value: " + 10;
```

What happens:

* `10` is a primitive
* **No boxing occurs (a common misconception is that the primitive type
  is boxed first and then `toString()` is called)**
* Java calls:
    ```java
    String.valueOf(10)
    ``` 
* Result: `"10"`

✔ Important:
> Primitives are NOT boxed during string concatenation.
>


#### Case 2: Reference Types

```java
Object obj = new User();
String s = "User: " + obj;
```

What happens:

* Java calls:
    ```java
    String.valueOf(obj)
    ```
* Which internally does:
    ```java
    obj.toString();
    ```

If `obj == null`:

```java
String s = "User: " + null;
```

Result:

```text
"User: null"
```

No `NullPointerException`


-----------------------------


## Q - What is the difference b/w `equals()` and `equalsIgnoreCase()` method?

The primary difference is case sensitivity.

1. Case Sensitivity:

    * `equals()`: Is case-sensitive. It returns `true` only if the characters
     match exactly, including casing (e.g., `"Java".equals("java")` returns `false`).
    * `equalsIgnoreCase()`: Is case-insensitive. It returns `true` if the characters match regardless
      of casing (e.g., `"Java".equalsIgnoreCase("java")` returns `true`).

2. Parameter Type:

    * `equals(Object anObject)`: Accepts an argument of type `Object`. (It overrides the method
     from the `Object` class).
    * `equalsIgnoreCase(String anotherString)`: Accepts an argument of type `String`. (It is a method
      specific to the `String` class).


-----------------------------


## Q - What is the difference b/w `isEmpty()` and `isBlank()` method of String object?

| Feature / Condition                      | isEmpty()            | isBlank()           |
|:-----------------------------------------|:---------------------|:--------------------|
| **New in Java 11**                       | No                   | Yes                 |
| **String has length of 0** (`""`)        | evaluates to `true`  | evaluates to `true` |
| **String has only whitespace** (`"   "`) | evaluates to `false` | evaluates to `true` |


-----------------------------


## Q - Diff b/w `String`, `StringBuilder` and `StringBuffer`

| Feature         | String                            | StringBuilder                       | StringBuffer                       |
|-----------------|-----------------------------------|-------------------------------------|------------------------------------|
| Mutability      | Immutable                         | Mutable                             | Mutable                            |
| Thread-safety   | Thread-safe (immutable)           | Not thread-safe                     | Thread-safe (synchronized)         |
| Performance     | Slow for frequent modifications   | Fast                                | Slower than StringBuilder          |
| Synchronization | Not required                      | None                                | Yes (methods synchronized)         |
| Use case        | Constant or rarely changed text   | Single-threaded string manipulation | Multi-threaded string manipulation |
| Introduced in   | Java 1.0                          | Java 5                              | Java 1.0                           |
| Memory usage    | Higher due to new object creation | Lower                               | Lower                              |
| String pool     | Yes (literals interned)           | No                                  | No                                 |


-----------------------------


## Q - What is CharSequence?

`CharSequence` is the root interface that defines a readable sequence of characters.


### Key Methods

Every CharSequence must implement:

```java
int length();
char charAt(int index);
CharSequence subSequence(int start, int end);
String toString();
```

These methods allow reading characters, but not modifying them.

### Why was CharSequence introduced?

* To allow methods to accept any type of character data, not just `String`.
* To provide flexibility — code can work with String OR `StringBuilder` OR `CharBuffer`.
* To reduce strict type dependencies and increase polymorphism.


### Simple Example

```java
public void print(CharSequence cs) {
    System.out.println(cs);
}

print("Hello");                 // String
print(new StringBuilder("Hi")); // StringBuilder
print(CharBuffer.wrap("Hey"));  // CharBuffer
```

All of these work because they implement CharSequence.


-----------------------------


## Q - What is serialVersionUID?

`serialVersionUID` is a unique identifier used during Java serialization and deserialization to ensure that the
sender and receiver of a serialized object have compatible class definitions.

It is used in classes that implement:

```java
class Employee implements Serializable { ... }
```

### Why do we need serialVersionUID?

When you serialize an object:

* Java converts the object into a byte stream
* Later, you deserialize it back into an object

But if the **class definition changed** between serialization and deserialization, Java needs a way to know
whether the classes are still compatible.

This is where `serialVersionUID` helps.

### DEFAULT Behavior

If you do not define `serialVersionUID`, Java **generates one automatically** based on:

* class name
* fields
* method signatures

If the class changes (even tiny change), Java generates a **different** serialVersionUID → deserialization fails.

### What happens if serialVersionUID changes?

If mismatched:

```java
java.io.InvalidClassException
```

Meaning:

"Serialized object belongs to version X, but class is version Y."

### What Happens When You Manually Define serialVersionUID

```java
private static final long serialVersionUID = 1L;
```

Now Java uses your version number, not the automatically generated one.

After this, you can

---

#### Add a new field

No error.
New field gets its default value on deserialization.

**Example:**

Old serialized object had no age field.
New class has:

```java
int age;
```

When deserialized → `age = 0`.

---

#### Remove a field

No error.
Extra data in the serialized stream is ignored.

**Example:**
Old object had a mobileNumber field.
New class doesn't.

Deserialization → the old field is ignored.

---

#### Change field order

No issue. Order does not matter.

---

#### Add methods

No issue - methods are not serialized.

---

#### Changes that break compatibility

* Changing the type of field
* Changing the class hierarchy


-----------------------------


## Q - How to prevent serialization of a field?

To prevent a field from being serialized in Java, you have two primary options
depending on the nature of the field.

### 1. The transient Keyword (The Standard Way)

You explicitly mark the field with the `transient` keyword.
This tells the JVM: "Ignore this field when writing the object state to a stream."

```java
class User implements Serializable {
    String username;          // Serialized
    transient String password; // NOT Serialized (ignored)
}
```

When this object is deserialized, the password field will not contain the original value;
it will be initialized to its default value (e.g., `null` for objects, `0` for integers, `false` for booleans).

### 2. The static Modifier (The "Class-Level" Rule)

Static fields are never serialized. Because serialization saves the state of an Object (instance), and
static fields belong to the Class, they are ignored by the serialization process entirely.

```java
class User implements Serializable {
    static String companyName; // NOT Serialized (belongs to class)
}
```

-----------------------------


## Q - Why `Object.clone()` is defined as protected?

1. **Intentional Opt-in:** It prevents "accidental" cloning. A class must consciously decide
    to support cloning by overriding the method and making it `public`.
2. **The Marker Interface:** Even if you make it `public`, you **must** implement `Cloneable`. 
    If you don't, `super.clone()` (the JVM's native engine) will throw `CloneNotSupportedException`.
3. **Shallow vs. Deep:** By default, `Object.clone()` does a **Shallow Copy**. 
    For a **Deep Copy** (like cloning a `List` inside your object), you must manually write 
    that logic inside your override.

### The "Senior" Verdict: Avoid `clone()` entirely

In a real interview, once you explain the technicalities above, you should finish with:

> "However, in modern Java, `clone()` is generally considered **broken**. 
> It's better to use **Copy Constructors** or **Static Factory Methods**."
> 

**Why?**

* `clone()` doesn't call constructors (skips initialization logic).
* It's hard to implement deep copies correctly.
* The `Cloneable` interface is poorly designed (it doesn't actually contain the `clone()` method!).



-----------------------------


## Q - What are the advantages of String being immutable?

1. Thread safety
2. Memory re-use


-----------------------------


## Q - What's the default implementation of `Object.equals()` method?

The default implementation of `equals()` in `Object` performs a reference comparison, meaning:

> Two objects are considered equal only if they refer to the exact same memory location.

In other words:

```text
obj1.equals(obj2)  <=>  obj1 == obj2
```

Both check identity, not content.


-----------------------------



## Q - Does finally block always execute in Java?

Not in the following cases:

* System.exit()
* System crash


-----------------------------


## Q - What are methods provided by the Object class?

`Object` is the root class of all Java classes. Every class implicitly inherits its methods.

| Method                                     | Purpose                                          |
|--------------------------------------------|--------------------------------------------------|
| `boolean equals(Object obj)`               | Compares this object with another for equality   |
| `int hashCode()`                           | Returns a hash code value for the object         |
| `String toString()`                        | Returns a string representation of the object    |
| `Class<?> getClass()`                      | Returns the runtime class of the object          |
| `protected Object clone()`                 | Creates and returns a copy of the object         |
| `protected void finalize()` *(deprecated)* | Called by GC before object reclamation           |
| `void wait()`                              | Causes the current thread to wait until notified |
| `void wait(long timeout)`                  | Waits for the specified time                     |
| `void wait(long timeout, int nanos)`       | Waits with nanosecond precision                  |
| `void notify()`                            | Wakes up a single waiting thread                 |
| `void notifyAll()`                         | Wakes up all waiting threads                     |


----------------


## Q - Can you provide default hashcode() implementation in the interface?

No, you cannot provide a default implementation for methods from the
`Object` class (like `hashCode()`, `toString()`, or `equals()`) inside an interface.

If you try to do this, the compiler will give you an error.

The "Why" behind this rule

It might seem useful to provide a standard `toString()` for all your objects,
but Java forbids it for a very specific architectural reason: "Class Wins."

### 1. The Conflict Resolution Rule

In Java, a class can inherit behavior from two places:

* A Superclass (e.g., Object).
* An Interface (via default methods).

The Rule: If a method exists in both a parent class and an interface, the **parent class's version always wins**.

### 2. The Problem

Since every Java class automatically extends Object, every single class
you ever create already has a version of `hashCode()` inherited from `Object`.

If Java allowed you to write a default `hashCode()` in an interface:

* You implement the interface.
* You call `myObject.hashCode()`.
* Java looks at the rules: "Oh, `Object` class has defined `hashCode()`. Class wins."
* Result: Your interface's default method would never run. It would be "dead code" that
  you thought was working but wasn't.


To prevent this confusion, the Java architects decided to make it a Compile Time Error
instead of just silently ignoring your code.

Example:

```java
interface MyInterface {
    
    // ❌ COMPILE ERROR: 
    // "Default method 'toString' overrides a member of 'java.lang.Object'"
    default String toString() {
        return "Standard Interface String";
    }
}
```

----------------


## Q - What is Comparable interface?

A class implements Comparable when it wants to define its **natural ordering**.

```text
obj1.compareTo(obj2)
```

The `compareTo()` method returns:

1\. Negative value `(obj1 < obj2)`
* Meaning: `obj1` should come before `obj2` when sorting.

2\. Positive value `(obj1 > obj2)`
* Meaning: `obj1` should come after `obj2`.

3\. Zero `(obj1 == obj2)`
* Meaning: both objects are considered equal in sorting order.


-----------------------------


## Q - Give a walk-through of the new features introduced since Java 8?

This is a massive topic. To ace this in an interview, do **not** list every minor
change. Instead, group them by the major **LTS (Long Term Support)** versions that
companies actually use: **Java 11**, **Java 17**, and the new **Java 21**.

Here is the "Executive Summary" of the evolution from Java 8.

---

### Phase 1: Java 9 - 11 (The "Modernization" Era)

*Focus: Removing boilerplate and modernizing APIs.*

#### Modules (Jigsaw)

The Change: Java 9 broke the massive monolithic JDK into small, manageable modules.

* **Key Concept:** Strict encapsulation. You must explicitly declare what packages
  your module exports and what other modules it requires using `module-info.java`.

* **Impact:**
  1. **Security:** Internal JDK APIs (like `sun.misc.Unsafe`) are hidden.
  2. **Scalability:** You can create custom, tiny Java runtimes (using jlink) that only contain
     the modules your app actually needs (e.g., a 30MB JRE instead of 200MB).

#### Collection Factory Methods

The Change: Finally, a clean one-line syntax to create immutable lists, sets, and maps.

* **Old Way:** `Arrays.asList("a", "b")` (Mutable wrapper, allows nulls) or
  `Collections.unmodifiableList(...)` (Verbose).
* New Way:
    ```java
    List<String> list = List.of("a", "b", "c");
    Set<String> set = Set.of("a", "b", "c");
    Map<String, Integer> map = Map.of("a", 1, "b", 2);
    ```

* **Note:** These collections are Immutable. Calling `.add()` throws `UnsupportedOperationException`.
  They also reject null values.

#### Local Variable Type Inference (`var`)

**The Change:** You don't need to repeat the type name on the left side.

* **Java 8:** `Map<String, List<User>> users = new HashMap<>();`
* **Java 11:** `var users = new HashMap<String, List<User>>();`
* *Note:* Still strongly typed! The compiler just infers it.

#### New HttpClient (Standardized)

**The Change:** Finally, a built-in, non-blocking HTTP client. No need for
Apache `HttpClient` or `OkHttp` for simple tasks.

```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder().uri(URI.create("https://api.com")).build();
HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
```

#### String Methods (Life Savers)

* `isBlank()`: Checks if a string is empty OR just whitespace.
* `lines()`: Returns a Stream of lines from a multi-line string.
* `strip()`: Unicode-aware `trim()`.
* `repeat(n)`: Repeats the string n times.

#### Running Single-File Source Code

You can now run a file without compiling it first!

* **Terminal:** `java HelloWorld.java` (No `javac` needed).

---

### Phase 2: Java 12 - 17 (The "Syntactic Sugar" Era)

*Focus: Developer productivity and reducing code noise.*

#### Records (Data Classes)

Immutable data carriers without boilerplate (`getters`, `equals`, `hashCode`, `toString`).

```java
// Java 17
public record User(String name, int id) {}

```

#### Text Blocks (Multi-line Strings)

No more `\n` and `+` concatenation for JSON/SQL.

```java
String json = """
              {
                "name": "John",
                "age": 30
              }
              """;

```

#### Switch Expressions

Arrow syntax, no fall-through, can return values.

```java
var result = switch(day) {
    case "MONDAY", "FRIDAY" -> "Work";
    case "SUNDAY" -> "Sleep";
    default -> "Unknown";
};

```

#### Pattern Matching for `instanceof`

Smart casting.

```java
if (obj instanceof String s) {
    System.out.println(s.length()); // 's' is already cast to String
}

```

#### Sealed Classes

Control exactly who can extend your class (critical for domain modeling).

```java
public sealed interface Shape permits Circle, Square {}

```

#### Helpful NullPointerExceptions

* **Old:** `NullPointerException at line 45` (Where? Who?)
* **New:** `Cannot invoke "String.length()" because "user.name" is null`.

---

### Phase 3: Java 18 - 21 (The "Concurrency Revolution")

*Focus: High-throughput concurrency and simplification.*

#### Virtual Threads (Project Loom) - **The Game Changer**

**The Problem:** Java threads map 1:1 to OS threads. OS threads are heavy (2MB RAM).
You can only have ~5,000 active threads before the server crashes.

**The Solution:** **Virtual Threads** are managed by the JVM, not the OS.
They are essentially "free" (bytes of RAM). You can have **millions** of them.

* **Impact:** You don't need complex "Reactive Programming" (WebFlux) anymore.
  You can write simple, blocking code that handles millions of connections.

```java
// Creates a lightweight virtual thread
Thread.startVirtualThread(() -> {
    System.out.println("Running in a virtual thread!");
});
```

#### Structured Concurrency

**The Problem:** In traditional concurrency, if you spawn 3 threads to do a task and
one fails, the others keep running (leaking resources), and handling errors across them is a nightmare.

**The Solution:** Structured Concurrency treats multiple related tasks running in different
threads as a single unit of work.

* **Impact:** If one sub-task fails, the others are automatically cancelled (cleaned up).
  It brings the simplicity of single-threaded error handling to multi-threaded code.
* Key API: `StructuredTaskScope`

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
Supplier<String> user  = scope.fork(() -> findUser());
Supplier<Integer> order = scope.fork(() -> fetchOrder());

    scope.join().throwIfFailed(); // Wait for both, or fail if one fails
// Both results are ready here
}
```

#### Sequenced Collections

Java finally unified how we access the "first" and "last" elements of a list, set, or deque.

* **Old:** `list.get(0)`, `deque.getFirst()`, `sortedSet.first()`. (Inconsistent).
* **New:** `collection.getFirst()`, `collection.getLast()`, `collection.addFirst()`. (Uniform).

#### Record Patterns

Deconstructs records directly in `instanceof` or `switch`.

```java
if (obj instanceof Point(int x, int y)) {
    System.out.println(x + y); // Access x and y directly!
}
```


#### Foreign Function & Memory API

Foreign Function & Memory API (Java 21)

The Change: A safe, standard way to access memory outside of the Java heap (off-heap) and call native code (C libraries).

* **The Old Way:** JNI (Java Native Interface). It was brittle, difficult to write, and could
  easily crash the entire JVM.
* **The New Way:** The FFM API replaces JNI. It is pure Java API (no native wrapper code needed),
  safer, and much faster.
* **Use Case:** High-performance applications interacting with hardware, heavy AI/ML
  libraries (TensorFlow/PyTorch), or processing massive data without Garbage Collection overhead.


------------------


## Q - What is shutdown hook?

A Shutdown Hook is a special thread that you register with the Java Virtual Machine (JVM).
The JVM promises to run this thread just before it shuts down.

### How to implement it?

You use the `Runtime` class to add a new Thread.

```java
public class ShutdownExample {
    public static void main(String[] args) {
        
        // 1. Register the Shutdown Hook
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("🚨 SHUTDOWN HOOK: Saving data to disk...");
            System.out.println("🚨 SHUTDOWN HOOK: Closing DB connections...");
            System.out.println("✅ Cleanup Complete. Bye!");
        }));

        System.out.println("Application is running... (Press Ctrl+C to stop)");

        // 2. Simulate heavy work
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread is finishing normally.");
    }
}
```

### When does it run?

The Shutdown Hook runs in these scenarios:

1. **Normal Exit**: The last non-daemon thread finishes.
2. **System.exit()**: You call `System.exit(0)`.
3. **User Interrupt**: You press `Ctrl+C` in the terminal.
4. OS Signal: The OS sends a `SIGTERM` (standard kill signal).

### When does it NOT run?

If the JVM is killed violently, the hook is skipped.

1. `Runtime.halt()`: This is the violent version of `System.exit()`.
2. `kill -9` (Force Kill): The OS rips the process from memory immediately.
3. Power Failure: Obviously.



----------------



# Level 2: OOP & Object Model


## Q - What are core principles of OOP?

To remember the core principles of Object-Oriented Programming (OOP), you can use the acronym `A PIE`:

1. Abstraction
1. Polymorphism
1. Inheritance
1. Encapsulation

Let's start with Polymorphism:

Doing the same thing in different ways is called polymorhphism. There are two types of polymorhphism:

1. Runtime polymorphism
1. Compile time polymorphism

### Runtime Polymorphism

Consider an `Animal` class with an `eat()` method. Next, create a `Dog` class by extending `Animal` class
and override the `eat()` method. Now create an object of type `Animal` and `Dog` and assign it to base class reference.

```java
Animal a1 = new Animal();
a1.eat();

a1 = new Dog();
a1.eat();
```

At runtime, the JVM decides which version of `eat()` method should be called (i.e. from `Animal` or `Dog` class).
This is called **Runtime polymorhphism**.


### Compile time Polymorphism

Method overloading is called **Compile time polymorhphism**.

Method overloading allows a class to have more than one method with the same name, but with different
parameters (different type, number, or both). The correct method to call is determined by the compiler based on the method
signature (the number and types of parameters).

Video Polymorphism: https://www.youtube.com/watch?v=jhDUxynEQRI&t=340s


### Abstraction

Abstraction in object-oriented programming (OOP) is the concept of hiding the complex
implementation details and exposing only the essential features of an object or a system. This
helps in reducing programming complexity and effort, enhancing code readability, and providing
a clear and concise interface for the end user or other developers.

Example:

Suppose we want to implement `Set`. There are multiple implementations possible here:

1. Balanced Binary Tree
2. Hashtable

We start by creating a interface called `Set` with two methods `add()` and `remove()`, and leave upto the child class to define the implementation.

Video abstraction: https://www.youtube.com/watch?v=L1-zCdrx8Lk


### Encapsulation

Encapsulation is the ability of an object to hide parts of its state and behavior from the outside world.
To encapculate something means to make it `private`, and thus accessible only from within if the methods if
its own class. There is a little less restrictive mode called `protected` that makes a member of a class available to
subclass as well.

Encapsulation helps us to change implementation without of the class without affecting the users of the class.

### Diff b/w Abstraction and Encapsulation

Consider the following analogy:

Engine is a piece of complex device. The car abstracts the complexity of the engine behind a simple interface pedals.
This is abstraction in action. To protect th engine from tampering, the engine is sealed inside a metal hood.
This is encapsulation.

-----------------------------


## Q - What is association, aggregation and composition?

### Association

Association is the general relationship where one class knows about or interacts with another class.

or

Association is a general HAS-A relationship, where one object is connected to another,
without implying ownership or lifecycle control.

Association often means a class has a field referencing another object -
but not always (it can also be through a method).

### Two forms of association

1. Aggregation
2. Composition

### Aggregation (weak ownership)

* Has-a relationship
* Class stores a field referencing another object
* The contained object can live without the container
* The contained object can be shared across containers
* UML: empty diamond

Example:

```java
class Player {
    private String name;
}

class Team {
    private List<Player> players;

    public Team(List<Player> players) {
        this.players = players;
    }
}
```

Why is this aggregation, not composition?

1. **The Team does NOT own the lifecycle of the Player**. Players already exist before being added to the team.

2. **Players can exist without the Team**.  If the Team object is deleted, Players continue to exist.

3. **The same Player can belong to multiple teams (theoretically)**. Example: National Team, Club Team → shared object.

4. **Team only stores a reference to Player**, not creating or destroying players.

###  Composition (strong ownership)

* Stronger has-a relationship
* Whole–part relationship
* Class stores a field referencing another object
* The part cannot exist without the whole
* The part is owned by the whole
* UML: filled diamond

**Example:**

```java
class OrderItem {
    private String productId;
    private int quantity;
}

class Order {
    private List<OrderItem> items = new ArrayList<>();

    public void addItem(OrderItem item) {
        items.add(item);
    }
}
```

Why is this composition?

* `OrderItem` cannot exist without its `Order`
* When the `Order` is deleted/cancelled, its items are deleted too
* `OrderItem` is not shared across multiple orders

The following diagram summarizes the relationship:

```text
Association (has-a)
   ├── Aggregation (weak has-a)
   └── Composition (strong has-a)
```

1. Video: https://www.youtube.com/watch?v=j84w5VM9GT8&t=1267s


-----------------------------


## Q - Is runtime polymorphism is applicable for fields also?

No, Runtime Polymorphism does NOT apply to fields (variables). It only applies to methods.

In Java, fields are accessed based on the Reference Type (the class name on the left side),
whereas methods are accessed based on the Actual Object (the new class on the right side).

**The Rule**

* **Methods (Overriding):** Resolved at Runtime (Dynamic Binding). Java looks at the actual object in memory.
* **Fields (Hiding):** Resolved at Compile Time (Static Binding). Java looks at the reference type you are holding.

Example:

```java
class Parent {
    String value = "Parent Field";

    void show() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {
    String value = "Child Field"; // Hides Parent's 'value'

    @Override
    void show() {
        System.out.println("Child Method");
    }
}

public class FieldTest {
    public static void main(String[] args) {
        System.out.println("--- Case 1: Parent Reference, Child Object ---");
        Parent p = new Child();

        // 1. Field Access -> STATIC BINDING (Looks at 'Parent' type)
        System.out.println("p.value:    " + p.value);
        // Output: "Parent Field" 

        // 2. Method Call -> DYNAMIC BINDING (Looks at actual 'Child' object)
        p.show();
        // Output: "Child Method"


        System.out.println("\n--- Case 2: Child Reference, Child Object ---");
        Child c = (Child) p; // Downcasting the same object to Child reference

        // 1. Field Access -> STATIC BINDING (Looks at 'Child' type)
        System.out.println("c.value:    " + c.value);
        // Output: "Child Field"

        // 2. Method Call -> DYNAMIC BINDING (Still looks at actual 'Child' object)
        c.show();
        // Output: "Child Method"


        System.out.println("\n--- Case 3: The 'Magic' of Casting ---");
        // You can access the HIDDEN parent field by casting the reference temporarily
        System.out.println("((Parent) c).value: " + ((Parent) c).value);
        // Output: "Parent Field"
    }
}
```


-----------------------------


## Q - Define Singleton class

A Singleton class is a design pattern in object-oriented programming in which only one
instance of the class can ever exist during the lifetime of an application.

**Example 1:**

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

**Example 2:**

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


## Q - How to make a class immutable?

Rules to make object Immutable:

1. Declare the class as final so it can't be extended.
2. Make all the fields private so that direct access is not allowed.
3. Don't provide setter methods for variables.
4. Make all the fields final so that a field's value can be assigned only once.
5. Initialize all fields using a constructor method performing deep copy.
6. Perform deep copy in getter of mutable fields

Here is an example of Immutable class:

Pay close attention to how we handle the `Date` object (which is mutable),
versus the `String` (which is already immutable).

```java
import java.util.Date;

// Rule 1: Class is final (Cannot be extended)
public final class Student {

    // Rule 2 & 4: Fields are private and final
    private final int id;
    private final String name;
    private final Date dateOfBirth; // Mutable object! Danger!

    // Rule 5: Constructor performs Deep Copy for mutable fields
    public Student(int id, String name, Date dateOfBirth) {
        this.id = id;
        this.name = name;

        // DEEP COPY: We create a NEW Date object.
        // If we just did "this.dateOfBirth = dateOfBirth", the caller 
        // could change the date later and break our immutability.
        this.dateOfBirth = new Date(dateOfBirth.getTime());
    }

    // Rule 3: No Setters provided.

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    // Rule 6: Getter performs Deep Copy (Cloning)
    public Date getDateOfBirth() {
        // DEEP COPY: Return a clone, not the original reference.
        // If we returned "this.dateOfBirth", the caller could use 
        // student.getDateOfBirth().setTime(...) to change the internal state.
        return new Date(dateOfBirth.getTime());
    }
}
```

Video: https://www.youtube.com/watch?v=PYJrFi4Hzsg


-----------------------------



  ## Q - In Java, what is the exact difference between a variable declared as final and an object that is immutable?

`final` means once a variable is assigned a reference it can't be changed but
**immutable** means the state of the object can't be changed once its created.


-----------------------------


## Q - Why Java is not completely Object-Oriented?

Because of primitive types like `int`, `char`, `float` etc.


-----------------------------


## Q - Why can't we override private and static methods?

### Why you cannot override private methods

Private methods are NOT visible to subclasses. If a subclass cannot see a method, it cannot override it.

### Why you cannot override static methods

Because static methods are bound at Compile Time, while overriding is a Runtime phenomenon.

Here is the detailed breakdown:

#### 1. The Binding Difference

* **Instance Methods (Overriding)**: Use **Dynamic Binding**. The JVM waits until the code is
  actually running to check "What kind of object is this really?" (e.g., is it a Dog or a Cat?) before deciding which method to run.

* **Static Methods (Hiding)**: Use **Static Binding**. The Compiler decides which method to call
  **before the program even runs** based solely on the **Reference Type** (the class name you wrote)

#### 2. They belong to the Class, not the Object

Overriding is all about Polymorphism (objects acting differently).
Since static methods belong to the class definition itself, they don't care about the object instance.

#### 3. What actually happens? (Method Hiding)

If you try to "override" a static method, Java doesn't give you an error, but it does something different
called Method Hiding.

* **Overriding:** The child's method replaces the parent's method everywhere.
* **Hiding:** The child's method only exists if you look at the child directly.
  If you look at the parent reference, you still see the parent's method.



-----------------------------



## Q - What is Dynamic Method Dispatch?

Dynamic Method Dispatch is the mechanism by which the JVM decides at runtime **whether
to invoke a superclass method or its overriding subclass implementation, based on the
actual object type**, not the reference type.

**Key rule:**
> Method resolution is based on the object, not the reference.
>


-----------------------------

# Level 3: Exception Handling


## Q - Explain the hierarchy of exceptions in Java?

```text
java.lang.Throwable
      |
      +-- java.lang.Error (Unchecked - System Failures)
      |     |
      |     +-- OutOfMemoryError
      |     +-- StackOverflowError
      |     +-- VirtualMachineError
      |
      +-- java.lang.Exception
            |
            +-- (Checked Exceptions - Must handle/declare)
            |     |
            |     +-- java.io.IOException
            |     |     +-- FileNotFoundException
            |     |
            |     +-- java.sql.SQLException
            |     +-- java.lang.ClassNotFoundException
            |
            +-- java.lang.RuntimeException (Unchecked - Logic Errors)
                  |
                  +-- NullPointerException
                  +-- ArithmeticException
                  +-- ArrayIndexOutOfBoundsException
                  +-- ClassCastException
                  +-- IllegalArgumentException
```

1\. The Root: `Throwable`

Everything starts with the `java.lang.Throwable` class. If a class does not extend `Throwable` (directly or indirectly),
it cannot be used with the `throw` keyword or the `try-catch` block.

It splits into two main branches: `Error` and `Exception`.


2\. The **"Fatal"** Branch: `Error`

**What it is:** Serious system-level problems that your application cannot reasonably recover from.

**Responsibility:** These are usually issues with the JVM itself or hardware resources. You should generally
not try to catch these.

Examples:

* `OutOfMemoryError` (Heap is full).
* `StackOverflowError` (Infinite recursion).
* `VirtualMachineError`.

3\. The **"Recoverable"** Branch: `Exception`

**What it is:** Conditions that your application might reasonably want to catch and handle.

**Responsibility:** This is where developers spend 99% of their time.

This branch is further divided into two critical categories:

1. Checked Exceptions
2. Unchecked Exceptions (Runtime Exceptions)


### Checked Exceptions (Compile-Time)

* **Parent:** Directly extends `Exception` (but not `RuntimeException`).
* **The Rule:** The Compiler checks these. You are forced to handle them (using `try-catch`) or declare them
  in your method signature (using `throws`).
* **Philosophy:** These represent "External Failures" that are often out of your control (e.g., the file isn't there, the
  internet is down). Java wants to ensure you have a "Plan B"
* Examples:
  * `IOException` (File handling).
  * `SQLException` (Database issues).
  * `ClassNotFoundException`.

### Unchecked Exceptions (Runtime)

* **Parent:** Extends `RuntimeException`.
* **The Rule:** The compiler does not check these. You can compile your code without handling them.
* **Philosophy:** These represent "Programming Logic Errors." These are usually your fault as a
  developer (e.g., you didn't check for null, you divided by zero). You shouldn't try to catch these;
  you should fix your code so they don't happen.
* Examples:
  * `NullPointerException` (NPE).
  * `ArithmeticException` (Dividing by zero).
  * `ArrayIndexOutOfBoundsException`.


-------------------


## Q - What is Exception chaining?

Exception chaining is a mechanism in Java where one exception is wrapped inside another exception.
This allows a method to translate a low-level exception into a higher-level exception while
preserving the original cause.

In simple words:
> You throw a new exception but also attach the original exception so nothing is lost.

Java supports this through the `Throwable(Throwable cause)` constructor and `getCause()` method.

### Why is Exception Chaining needed?

1\. Convert low-level exceptions to meaningful high-level ones
* Example: Wrap `SQLException` in a `UserNotFoundException`.

2\. Preserve root cause for debugging
* When reading logs, you can see both the high-level error and the original cause.

3\. Avoid losing important context
* If you throw a new exception without chaining, the original stack trace is lost.

### Real-World Example (ELI5)

Imagine this situation:
* _The database throws an `SQLException`._
* Your service should not expose SQL details to callers.
* So you wrap the low-level exception into a clean business exception.

### Practical Example

```java
public User findUser(int id) {
    try {
        return userRepository.getUser(id);
    } catch (SQLException e) {
        throw new RuntimeException("Failed to fetch user with id: " + id, e);
    }
}
```

Here:

* The NEW exception is `RuntimeException`
* The ORIGINAL exception is `SQLException`
* The ORIGINAL exception becomes the "cause"

If you print the stack trace (i.e. `e.printStackTrace()`):

```text
RuntimeException: Failed to fetch user with id: 10
    at findUser(...)
Caused by: java.sql.SQLException: Connection failure
    at ...
```


-----------------------------


## Q - What is the exact difference between ClassNotFoundException and NoClassDefFoundError?

This is one of the most classic "gotcha" questions in Java interviews.
They sound identical, but the **root cause** is completely different.

Here is the breakdown:

### 1. ClassNotFoundException (The "Typo")

* **What it is:** A **Checked Exception**. You (the developer) explicitly asked the JVM
  to load a class by its string name, and the JVM said, "I can't find anything with that name."
* **When it happens:** When using **Reflection** or loading dynamic classes.
* **Common Causes:**
  * `Class.forName("com.mysql.jdbc.Drivr")` <- Typo in the string?
  * `ClassLoader.loadClass("MissingClass")`
  * Missing JAR file in the classpath.


**Code Example:**

```java
try {
    // You try to load a class using a String
    Class.forName("com.fake.MissingClass"); 
} catch (ClassNotFoundException e) {
    // The JVM politely tells you it failed
    System.out.println("Typo! That class doesn't exist.");
}
```

### 2. NoClassDefFoundError (The "Ghost")

* **What it is:** An **Error** (Critical Failure). This is much nastier. It means the
  class **was present** when you compiled your code, but it is **missing** now
  that you are trying to run it.

* **When it happens:** At Runtime, usually during linking or static initialization.

* **Common Causes:**
  * **The "JAR Hell":** You compiled your code with `library-v1.jar` (which has `CoolClass.class`), but
    you deployed it to the server with `library-v2.jar` (which deleted `CoolClass`).
  * **Static Block Failure:** If a class has a `static { ... }` block that throws an exception, the
    class fails to load. Any future attempt to use that class triggers this error.


**Code Example:**

```java
public class GhostDemo {
    public static void main(String[] args) {
        // This line compiles fine because 'Worker' exists right now.
        // BUT, if you delete Worker.class before running this...
        Worker w = new Worker(); 
        
        // BOOM! Java crashes with NoClassDefFoundError.
        // "I swear I saw this class when I compiled! Where did it go??"
    }
}

```

### Summary Table (Memorize This)

| Feature     | `ClassNotFoundException`                          | `NoClassDefFoundError`                                                      |
|-------------|---------------------------------------------------|-----------------------------------------------------------------------------|
| **Type**    | **Exception** (Checked)                           | **Error** (Unchecked / Fatal)                                               |
| **Trigger** | Explicit loading (`Class.forName`, `ClassLoader`) | Implicit loading (variable declaration, `new` keyword)                      |
| **Meaning** | "I cannot find the class name you gave me."       | "I expected this class to be here (it was at compile time), but it's gone!" |
| **Fix**     | Check the string spelling or classpath.           | Check for mismatched JAR versions or static initializer errors.             |



-----------------------------


## Q - What is AutoCloseable interface?

`AutoCloseable` is a functional interface introduced in Java 7 that allows an object to be
used in the try-with-resources statement.

Its single method, void `close() throws Exception`, is called automatically when the `try` block exits
(whether normally or due to an exception).

### 1. The Core Purpose: Try-With-Resources

Before Java 7, you had to close resources (files, sockets, DB connections) manually in
a finally block. This was verbose and error-prone.

`AutoCloseable` automates this cleanup.

The Interface Definition:

```java
public interface AutoCloseable {
    void close() throws Exception;
}
```

### 2. Code Example

Here is how you implement it and use it.

```java
// 1. Create a Custom Resource
class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("Resource closed automatically!");
    }
    
    public void doWork() {
        System.out.println("Working...");
    }
}

// 2. Use it in Try-With-Resources
public class Main {
    public static void main(String[] args) {
        // Notice the parenthesis after 'try'
        try (MyResource res = new MyResource()) {
            res.doWork();
        } catch (Exception e) {
            e.printStackTrace();
        }
        // 'close()' is guaranteed to run here.
    }
}
```

### 3. Senior Engineer Nuance: Exception Suppression

**The Problem (Old finally way):** If your code throws an exception (e.g., `RuntimeException`) AND
your `finally` block throws an exception (e.g., `IOException` while closing), the original exception is lost.
The caller only sees the closing error, which hides the real bug.

The Solution (`AutoCloseable` way): If both the `try` block and the `close()` method throw exceptions:

1. The try block exception is **propagated** (this is the one you want to see).
2. The `close()` exception is **suppressed** and attached to the main exception.
3. You can retrieve it using **mainException.getSuppressed()**.


-----------------------------


# Level 4: Collections Framework

## Q - What is diff b/w Vector and ArrayList?

`Vector` is thread-safe but `ArrayList` is not.

While the primary difference is thread safety, there are 4 key distinctions you should know for an interview.

| Feature           | Vector                                                                                                 | ArrayList                                                                  |
|-------------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **Thread Safety** | **Synchronized.** It is thread-safe.                                                                   | **Not Synchronized.** It is NOT thread-safe.                               |
| **Performance**   | **Slower.** Every operation (`add`, `get`) must acquire a lock, even in a single-threaded environment. | **Faster.** No synchronization overhead.                                   |
| **Growth Rate**   | Doubles in size (**100% increase**) when full.                                                         | Increases by **~50%** `(oldCapacity * 1.5)` when full.                     |
| **Legacy Status** | Legacy class (Java 1.0). Retained for backward compatibility.                                          | Part of Collections Framework (Java 1.2). Standard for modern development. |


-----------------------------


## Q - Diff b/w Hashtable and HashMap

* `Hashtable` is thread-safe but `HashMap` is not.
* `HashMap` allows key with `null` value but `Hashtable` doesn't.

Note that `Hashtable` uses method-level synchronization which is a bottleneck.
If you need a thread-safe map, use `ConcurrentHashMap` (which is much faster than `Hashtable` because it
uses segment locking/CAS instead of locking the entire object).


-----------------------------


## Q - When would you use parallelStream()

### Parallel Stream — Three Examples Explained (Good vs Bad vs Dangerous)

### Example 1 — ✅ GOOD use of parallelStream()

```java
List<Integer> numbers = getOneMillionIntegers();

long count = numbers.parallelStream()
                    .filter(n -> isPrime(n)) // heavy CPU work
                    .count();
```

#### Why this is GOOD

**What the code is doing**

* You have 1 million numbers
* For each number, you run `isPrime(n)`
* Checking primality requires many CPU operations


**Why parallelism helps here**

* Each number is independent
* No shared variables
* No waiting on I/O
* Heavy computation per element


**What happens internally (ELI5)**

* Java splits the list into chunks
* Multiple CPU cores work at the same time
* Each core checks different numbers
* Results are combined at the end


#### Final judgment

* ✅ Correct use of parallelStream
* ✅ CPU-bound
* ✅ Large dataset
* ✅ Independent work


**Interview line:**

> Parallel streams are effective here because the workload is CPU-intensive, independent, and
> large enough to amortize parallel overhead.
>

### Example 2 — ❌ BAD use of parallelStream()

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, ... 1000);

int sum = numbers.parallelStream()
                 .mapToInt(n -> n)
                 .sum();
```

#### Why this is BAD

**What the code is doing**

* Just adding numbers
* Operation per element is extremely cheap

**Why parallelism hurts here**

ParallelStream introduces overhead:

* Splitting the list
* Creating Fork/Join tasks
* Scheduling threads
* Merging partial sums
* But the actual work:

```text
n -> n
```

takes almost no time.

**What happens internally (ELI5)**

You hired 8 workers to:

* Add very small numbers
* Spent more time organizing workers than doing work

#### Final judgment

* ❌ Computation too small
* ❌ Dataset too small
* ❌ Parallel overhead > useful work

**Interview line:**

> This is inefficient because the overhead of parallel execution outweighs the
> trivial computation being performed.


### Example 3 — 💀 DANGEROUS use of parallelStream()

```java
List<String> userIds = Arrays.asList("101", "102", ... "200");

userIds.parallelStream()
       .map(id -> database.getUser(id)) // BLOCKING I/O
       .collect(Collectors.toList());
```

#### Why this is DANGEROUS (not just slow)

**What the code is doing**

* Each element performs a blocking database call
* DB call may take seconds

**Critical hidden detail**

`parallelStream()` uses:

```text
ForkJoinPool.commonPool
```

This pool:

* Is shared by the entire JVM
* Has limited threads (≈ CPU cores)
* Is designed for CPU work, not blocking I/O

**What happens internally (ELI5)**

* All ForkJoin threads make DB calls
* Threads block waiting for DB
* No threads left for:
  * other parallel streams
  * CompletableFutures
  * CPU work

Result:

* ❌ Thread starvation
* ❌ App slowdown
* ❌ Unpredictable production failures

#### Final judgment

* 💀 Blocking I/O in shared ForkJoinPool
* 💀 Can break unrelated parts of the application


**Interview line:**

> Using `parallelStream()` for blocking I/O is dangerous because it blocks the shared
> ForkJoinPool, starving unrelated parallel tasks across the JVM.
>

### One-page Comparison (Interview Gold)

| Example            | Verdict      | Reason                                  |
|--------------------|--------------|-----------------------------------------|
| Prime number check | ✅ GOOD       | Heavy CPU work, independent, large data |
| Summing 1–1000     | ❌ BAD        | Parallel overhead dominates             |
| Database calls     | 💀 DANGEROUS | Blocks shared ForkJoinPool              |


### Final Rule to Say in Interview (Memorize This)

> Use parallelStream() only for large, CPU-bound, stateless operations.
> Avoid it for small workloads and never use it for blocking I/O.
>


-----------------------------


## Q - Explain collection framework hierarchy?

**1\. Top-Level Hierarchy**

```text
Iterable (interface)
   ↓
Collection (interface)
Map (interface)   ← separate hierarchy (NOT a subtype of Collection)
```

**2\. COLLECTION HIERARCHY**

```text
Collection (interface)
   ├── List (interface)
   ├── Set (interface)
   └── Queue (interface)
```

Now let's expand each branch.

**A. LIST Hierarchy (Ordered, allows duplicates)**

```text
List (interface)
   ├── ArrayList (implementation)
   ├── LinkedList (implementation)
   ├── Vector (implementation)
   │       └── Stack (implementation)
   └── CopyOnWriteArrayList (implementation)
```

Examples:

```java
List<String> list1 = new ArrayList<>();
list1.add("A");

List<Integer> list2 = new LinkedList<>();
list2.add(10);

Stack<String> stack = new Stack<>();
stack.push("top");
```

**B. SET Hierarchy (No duplicates)**

```text
Set (interface)
   ├── HashSet (implementation)
   │       └── LinkedHashSet (implementation)
   ├── SortedSet (interface)
   │       └── NavigableSet (interface)
   │               └── TreeSet (implementation)
   ├── EnumSet (implementation)
   └── CopyOnWriteArraySet (implementation)
```

Examples:

```java
Set<String> set1 = new HashSet<>();        // no order
Set<String> set2 = new LinkedHashSet<>();  // insertion order
Set<Integer> set3 = new TreeSet<>();       // sorted
```

**C. QUEUE Hierarchy**

```text
Queue (interface)
   ├── Deque (interface)
   │       ├── ArrayDeque (implementation)
   │       └── LinkedList (implementation)
   └── PriorityQueue (implementation)
```

Examples:

```java
Queue<Integer> q1 = new PriorityQueue<>();     // min-heap
Deque<String> q2 = new ArrayDeque<>();         // stack + queue
Deque<Integer> q3 = new LinkedList<>();        // also queue
```

**3\. MAP HIERARCHY (KEY–VALUE PAIRS)**

Maps are not a part of the Collection hierarchy.

```text
Map (interface)
   ├── HashMap (implementation)
   │       └── LinkedHashMap (implementation)
   ├── SortedMap (interface)
   │       └── NavigableMap (interface)
   │               └── TreeMap (implementation)
   ├── Hashtable (implementation)
   │       └── Properties (implementation)
   └── ConcurrentMap (interface)
           ├── ConcurrentHashMap (implementation)
           └── ConcurrentSkipListMap (implementation)
```

Examples:

```java
Map<String, Integer> m1 = new HashMap<>();       // general-purpose
Map<String, Integer> m2 = new LinkedHashMap<>(); // insertion order
Map<Integer, String> m3 = new TreeMap<>();       // sorted keys
Map<String, String> p = new Properties();        // config properties
ConcurrentMap<String, Integer> m4 = new ConcurrentHashMap<>(); // thread-safe
```

**4\. CONCURRENT COLLECTIONS (java.util.concurrent)**

```text
BlockingQueue (interface)
   ├── ArrayBlockingQueue (implementation)
   ├── LinkedBlockingQueue (implementation)
   ├── PriorityBlockingQueue (implementation)
   ├── DelayQueue (implementation)
   └── SynchronousQueue (implementation)

ConcurrentLinkedQueue (implementation)
ConcurrentLinkedDeque (implementation)
```

Examples:

```java
BlockingQueue<String> bq = new ArrayBlockingQueue<>(10);
ConcurrentLinkedQueue<Integer> cq = new ConcurrentLinkedQueue<>();
```

5\. Full Condensed Diagram with Interface/Implementation Labels

```text
Iterable (I)
   ↓
Collection (I)
   ├── List (I)
   │      ├── ArrayList (C)
   │      ├── LinkedList (C)
   │      ├── Vector (C)
   │      │       └── Stack (C)
   │      └── CopyOnWriteArrayList (C)
   │
   ├── Set (I)
   │      ├── HashSet (C)
   │      │       └── LinkedHashSet (C)
   │      ├── SortedSet (I)
   │      │       └── NavigableSet (I)
   │      │               └── TreeSet (C)
   │      ├── EnumSet (C)
   │      └── CopyOnWriteArraySet (C)
   │
   └── Queue (I)
          ├── Deque (I)
          │       ├── ArrayDeque (C)
          │       └── LinkedList (C)
          └── PriorityQueue (C)

Map (I)
   ├── HashMap (C)
   │       └── LinkedHashMap (C)
   ├── SortedMap (I)
   │       └── NavigableMap (I)
   │               └── TreeMap (C)
   ├── Hashtable (C)
   │       └── Properties (C)
   └── ConcurrentMap (I)
           ├── ConcurrentHashMap (C)
           └── ConcurrentSkipListMap (C)
```

Legend:
* I = Interface
* C = Concrete Implementation


----------------------


## Q - Explain the evolution from SortedSet (Java 1.2) to NavigableSet (Java 6). Why was a new interface introduced instead of extending SortedSet, given that TreeSet already existed?

### 1. SortedSet

**What is SortedSet?**

SortedSet is a Set that maintains its elements in sorted order, either by natural ordering or
by a provided Comparator.

**Java version**
* Introduced in Java 1.2 (as part of the Java Collections Framework)

**What SortedSet Guarantees**

`SortedSet` defines ordering guarantees only.

* `first()` - lowest element
* `last()` - highest element
* `headSet(E toElement)` - elements strictly less than `toElement`
* `tailSet(E fromElement)` - elements greater than or equal to `fromElement`
* `subSet(E fromElement, E toElement)` - range view

These allow:

* Maintaining sorted order
* Creating range-based views


#### What SortedSet Does Not Guarantee

SortedSet does **not** define:

* Closest smaller element
* Closest larger element
* Reverse traversal
* Inclusive/exclusive boundary control
* Remove-and-return operations

Even if an implementation can do these things, the **interface does not promise them**.


### 2. TreeSet Existed Before NavigableSet

**Important clarification**
> Yes, `TreeSet` already existed when `NavigableSet` was introduced.
>

**Timeline**

* **Java 1.2:**
  * `SortedSet` introduced
  * `TreeSet` introduced
  * `TreeSet` implemented SortedSet
  * Internally backed by a Red-Black Tree

A Red-Black Tree already supports:

* Predecessor / successor
* Ordered traversal
* Efficient boundary lookups

So **the underlying data structure already had the capability**.


### 3. The Problem Before Java 6

Before Java 6, developers commonly wrote patterns like:

```java
set.headSet(x).last();
```

This worked, but it had problems:

* Multiple operations instead of one
* Verbose and error-prone
* Intent not clearly expressed
* No interface-level guarantee
* Not portable across different `SortedSet` implementations

Most importantly:
> These navigation operations were not part of the `SortedSet` contract.


### 4. NavigableSet

**What is NavigableSet?**
> `NavigableSet` extends `SortedSet` by formally defining navigation operations over a sorted set.
>

**Java version**
* Introduced in Java 6


#### What NavigableSet Adds

Navigation methods:

* `lower(e)` - greatest element `< e`
* `floor(e)` - greatest element `≤ e`
* `ceiling(e)` - smallest element `≥ e`
* `higher(e)` - smallest element `> e`

Traversal and mutation:

* descendingSet()
* pollFirst(), pollLast()

Precise range control:

* subSet(from, boolean, to, boolean)

These operations are now:

* Explicit
* Guaranteed
* Single-operation semantics


### 5. Why NavigableSet Was Introduced (Despite Existing Capability)

The key reason (very important)

> NavigableSet was introduced not because the data structure changed, but because
> the API contract was insufficient.
>
>

Specifically:

* `SortedSet` did not express navigation semantics
* Adding methods to `SortedSet` would break backward compatibility
* Java chose to extend the API safely via a new interface

#### What Changed in Java 6

* `NavigableSet` was added
* `TreeSet` was updated to implement `NavigableSet`
* No new data structure was introduced
* Existing behavior remained unchanged

This preserved:

* Backward compatibility
* API clarity
* Future extensibility

### 6. Summary Table (Version-Accurate)

| Aspect                          | SortedSet    | NavigableSet                  |
|---------------------------------|--------------|-------------------------------|
| Introduced in                   | Java 1.2     | Java 6                        |
| Guarantees                      | Sorted order | Sorted order + navigation     |
| Closest element lookup          | ❌            | ✅                             |
| Reverse traversal               | ❌            | ✅                             |
| Polling boundaries              | ❌            | ✅                             |
| TreeSet present at introduction | Yes          | Yes (updated to implement it) |


### Final Interview-Ready Answer (Concise)

`SortedSet`, introduced in Java 1.2, guarantees only sorted order and basic range views.
Its primary implementation, `TreeSet`, already existed and was backed by a Red-Black Tree that
supported navigation internally. However, these capabilities were not part of the interface contract.
Java 6 introduced `NavigableSet` to formally define and guarantee navigation operations
such as predecessor, successor, reverse traversal, and precise boundary control, without breaking
backward compatibility. `TreeSet` was then updated to implement `NavigableSet`.

**One Sentence to Remember**
> The data structure already had the ability; NavigableSet gave it a formal, portable contract.
>


----------------------


## Q - What are Fail Fast and Fail Safe Iterators?

### Fail-Fast Iterators

Fail-fast iterators immediately throw a `ConcurrentModificationException` if the
underlying collection is structurally modified during iteration(except through the
iterator's own `remove()` method).

Examples:

* `ArrayList`
* `HashMap`
* `HashSet`

Key points:

* Works on the original collection
* Detects concurrent modification using `modCount`
* Not thread-safe


### Fail-Safe Iterators

Fail-safe iterators do not throw `ConcurrentModificationException` because they iterate
over a snapshot or a copy of the collection.

Examples:

* `CopyOnWriteArrayList`
* `CopyOnWriteArraySet`
* Iterators of `ConcurrentHashMap`

Key points:

* Operate on a snapshot
* Reflect state at iteration start
* Thread-safe but may not show latest changes


-----------------------------


## Q - How to create immutable collections in Java?

In modern Java, you have a few ways to handle this depending on whether 
you want a **read-only view** of an existing list or a truly **immutable** collection. 
Here is a breakdown of the most common methods:

---

### 1. Using the "List.of()" Factory Method (Java 9+)

This is the cleanest and most common way to create a small, fixed-size 
immutable list from scratch. These collections are truly immutable; attempting
to change them will throw an `UnsupportedOperationException`.

```java
List<String> fruits = List.of("Apple", "Banana", "Cherry");

```

### 2. Using Stream Collectors (Java 10+)

If you are processing data through a stream and want the result 
to be immutable, use the collector you mentioned:

```java
List<String> immutableList = items.stream()
    .filter(s -> s.startsWith("A"))
    .collect(Collectors.toUnmodifiableList());

```

### 3. Creating a Copy (Java 10+)

If you already have a mutable list and want to create an immutable snapshot of it:

```java
List<String> copy = List.copyOf(existingList);

```

### 4. The "Unmodifiable View" (The Older Way)

Before Java 9, we used `Collections.unmodifiableList()`. It is important 
to note that this is a **wrapper**. If the underlying original list changes, 
the "unmodifiable" view will also change.

```java
List<String> mutable = new ArrayList<>();
List<String> view = Collections.unmodifiableList(mutable);

mutable.add("New Item"); 
// 'view' now contains "New Item" too! It's not truly immutable.
```

---

### Comparison of Methods

| Method                            | Java Version | Truly Immutable?     | Allows Nulls? |
|-----------------------------------|--------------|----------------------|---------------|
| `List.of(...)`                    | 9+           | **Yes**              | No            |
| `List.copyOf(...)`                | 10+          | **Yes**              | No            |
| `Collectors.toUnmodifiableList()` | 10+          | **Yes**              | No            |
| `Collections.unmodifiableList()`  | 2+           | **No** (it's a view) | Yes           |

Similar methods exists for to handle immutable **Maps** or **Sets** as well.


----------------


# Level 5: Concurrency & Multithreading

## Q - What are different thread states?

![thread states](../images/thread-lifecycle.png)

* when `sleep()` is called, it goes to `TIMED_WAITING` **but the lock is not released**
* when `wait()` / `join()` is called, thread goes to `WAITING` or `TIMED_WAITING` and **releases the lock**
* when thread is waiting for lock it goes to blocked state

-----------------------------


## Q - What is daemon thread?

A Daemon Thread is a low-priority thread that runs in the background to provide services
to other threads (User Threads).

The most important thing to remember is the JVM's exit behavior:

> The JVM waits for all User Threads (non-daemon threads) to finish.
However, it does not wait for Daemon Threads to finish. It abruptly kills them.

Example: Thread which does the Garbage Collection in background is a Daemon thread

The `setDaemon()` method is used to create a Daemon thread in Java.

```java
Thread cleanup = new Thread(() -> {
    while(true) {
        System.out.println("Cleaning...");
        // This loop will be killed abruptly when main() finishes
    }
});

cleanup.setDaemon(true); // Must happen before start()
cleanup.start();
```

-----------------------------


## Q - What is BlockingQueue?

A `BlockingQueue` is a thread-safe queue that automatically coordinates producer and consumer threads
by handling waiting and notification when the queue is empty or full.

### Why was BlockingQueue introduced?

Although Java already provided `synchronized`, `wait()`, and `notify()`, writing correct and reusable
producer–consumer logic using these low-level primitives was complex and error-prone.

BlockingQueue was introduced to:

* Eliminate manual synchronization
* Avoid direct use of `wait()` / `notify()`
* Handle thread waiting and signaling correctly
* Simplify producer–consumer coordination
* Provide a reusable, well-tested abstraction

In short, it encodes **correct concurrency patterns** so developers don't have to reimplement them.

### How BlockingQueue solves the problem

`BlockingQueue` automatically:

* Makes producers wait when the queue is full
* Makes consumers wait when the queue is empty
* Ensures thread safety
* Handles signaling between threads internally

This removes the need for explicit locks and condition handling.


### Core BlockingQueue methods (important)

#### put() — blocking insert

* Inserts an element
* Waits if the queue is full

```java
queue.put(item);
```

#### take() — blocking retrieval

* Removes and returns an element
* Waits if the queue is empty

```java
queue.take();
```

#### offer() — non-blocking insert

* Attempts to insert an element
* Returns immediately
* Returns `false` if the queue is full

```java
boolean added = queue.offer(item);
```

#### poll() — non-blocking retrieval

* Attempts to retrieve an element
* Returns immediately
* Returns `null` if the queue is empty

```java
Integer value = queue.poll();
```

| Method    | Waits? | On failure      |
|-----------|--------|-----------------|
| `put()`   | Yes    | Waits           |
| `take()`  | Yes    | Waits           |
| `offer()` | No     | Returns `false` |
| `poll()`  | No     | Returns `null`  |


### Types of BlockingQueue

Here is the breakdown of the 5 most important `BlockingQueue` implementations in `java.util.concurrent`.

#### 1. ArrayBlockingQueue

**What it is**

* A bounded blocking queue backed by an array
* Fixed size, defined at creation

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
```

**Key characteristics**

* FIFO order
* Fixed capacity
* Single lock for producers and consumers
* Optional fairness policy

**Why it exists**

* To provide strict capacity control and predictable memory usage.

**Use cases**

* Producer–consumer systems with back-pressure
* Systems where memory usage must be capped
* Rate-limited pipelines

---

#### 2. LinkedBlockingQueue

**What it is**

* A linked-node based blocking queue
* Can be bounded or unbounded

```java
BlockingQueue<Integer> queue = new LinkedBlockingQueue<>();
```

**Key characteristics**

* FIFO order
* Separate locks for put and take (better concurrency)
* Higher throughput than array-based queues


To allow higher concurrency between producers and consumers.

**Use cases**

* General-purpose producer–consumer problems
* Task queues
* Default queue used in many thread-pool configurations

---

#### 3. PriorityBlockingQueue

**What it is**

* A **priority-based**, unbounded blocking queue
* Elements are ordered by priority, not insertion order

```java
BlockingQueue<Task> queue = new PriorityBlockingQueue<>();
```

**Key characteristics**

* Uses Comparable or Comparator
* No capacity limit
* FIFO is NOT guaranteed

**Why it exists**

* To process high-priority tasks first.

**Use cases**

* Task schedulers
* Job prioritization systems
* Event processing where priority matters

---

#### 4. DelayQueue

**What it is**

* A blocking queue where elements become available **after a delay**
* Elements must implement Delayed

```java
DelayQueue<DelayedTask> queue = new DelayQueue<>();
```

**Key characteristics**

* Time-based availability
* Unbounded
* Elements retrieved only after delay expires

**Why it exists**

* To support time-based scheduling without manual timers.

**Use cases**

* Retry mechanisms
* Cache expiration
* Scheduled task execution

---

#### 5. SynchronousQueue

**What it is**

* A blocking queue with zero capacity
* No storage — direct handoff between threads

```java
BlockingQueue<Integer> queue = new SynchronousQueue<>();
```

**Key characteristics**

* Each put() waits for a take()
* No buffering
* High throughput under load

**Why it exists**

To enable direct thread-to-thread handoff without queuing.

**Use cases**

* ThreadPoolExecutor (cached thread pools)
* Task handoff scenarios
* Low-latency systems


| BlockingQueue         | Capacity  | Ordering   | Primary Use Case               |
|-----------------------|-----------|------------|--------------------------------|
| ArrayBlockingQueue    | Bounded   | FIFO       | Strict capacity control        |
| LinkedBlockingQueue   | Optional  | FIFO       | General-purpose concurrency    |
| PriorityBlockingQueue | Unbounded | Priority   | Priority-based task processing |
| DelayQueue            | Unbounded | Time-based | Scheduled / delayed tasks      |
| SynchronousQueue      | Zero      | None       | Direct thread handoff          |


**Example usage of BlockingQueue with Producer-Consumer:**

```java
public class PizzaShop {

    public static void main(String[] args) {
        // 1. Create a Queue with a FIXED capacity of 3
        BlockingQueue<String> counter = new ArrayBlockingQueue<>(3);

        // --- The Chef Thread (Producer) ---
        Thread chef = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    String pizza = "Pizza #" + i;

                    System.out.println("👨‍🍳 Chef is baking " + pizza);
                    // put() will BLOCK here if the queue is full (size == 3)
                    counter.put(pizza);
                    System.out.println("✅ Chef placed " + pizza + " on counter. [Count: " + counter.size() + "]");

                    Thread.sleep(200); // Baking takes a little time
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        // --- The Customer Thread (Consumer) ---
        Thread customer = new Thread(() -> {
            try {
                // Let the chef get a head start to fill the counter
                Thread.sleep(1000);

                while (true) {
                    // take() will BLOCK here if the queue is empty (size == 0)
                    String pizza = counter.take();
                    System.out.println("😋 Customer bought " + pizza + ". [Count: " + counter.size() + "]");

                    Thread.sleep(1000); // Eating takes longer than baking!
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        chef.start();
        customer.start();
    }
}
```

**How it works:**

* The example models a classic producer–consumer problem using `ArrayBlockingQueue`.
* The chef is the producer, baking pizzas and placing them on a counter with a fixed capacity of 3.
* The customer is the consumer, taking pizzas from the counter and eating them.
* When the counter (queue) is full, the chef's call to `put()` blocks automatically until the customer removes a pizza.
* When the counter is empty, the customer’s call to `take()` blocks automatically until the chef produces a pizza.
* This shows how `BlockingQueue` handles synchronization internally, without using manual `wait()` / `notify()`.
* The example demonstrates smooth, thread-safe communication between producer and consumer, with natural backpressure
  when one becomes faster than the other.


-----------------------------


## Q - List diff types of Executorservice

1. Fixed thread pool executor
1. Single thread executor
1. Cached thread pool executor
1. Scheduled thread pool executor
1. Work stealing thread pool executor


-----------------------------


## Q - What are the motivations for ExecutorService?

The `ExecutorService` framework was introduced (in Java 5) to solve three major problems
with manual `new Thread()` management:

### 1. Resource Management (The "Thread Explosion" Problem)

Your note about creating 1,000 threads is spot on.

* **The Cost:** In Java, a Thread maps 1:1 to an OS Thread. Each thread needs
  its own **Stack Memory** (usually 1MB by default).
* **The Crash:** If you create 1,000 threads, you just reserved ~1GB of RAM *just for stacks*,
  before you even run a single line of code.
* **The Fix:** The ExecutorService uses a **Worker Pool**. The threads don't die after a task;
  they go back to the "bench" and wait for the next job.

### 2. Abstraction (The "Producer-Consumer" Problem)

Before `ExecutorService`, if you wanted to pass tasks to a background thread safely, you had
to write your own synchronized queue.

* **The Pain:** You had to write `wait()`, `notify()`, and handle strict synchronization to
  prevent race conditions. It was incredibly easy to write deadlock-prone code.
* **The Fix:** The ExecutorService *is* a pre-built Producer-Consumer pattern.
  You (the Producer) just `submit()`, and the internal `BlockingQueue` handles the hand-off to
  the Threads (the Consumers) safely.

### 3. Returning Results (The "Void" Problem)

This is often the most appreciated feature for day-to-day coding.

* **The Pain:** `Runnable.run()` is `void`. If your thread calculated a
  value (like "Process Payment"), it had no standard way to say "Here is the confirmation ID."
  You had to store it in a shared variable and hope the main thread read it at the right time.

* **The Fix:** `Callable<T>` returns a value, and the `Future<T>` acts like a "Claim Check" for
  your dry cleaning. You hold the ticket (Future), and when the work is done, you trade the
  ticket for the result.

### Summary Table

| Feature             | `new Thread()` (The Old Way)               | `ExecutorService` (The New Way) |
|---------------------|--------------------------------------------|---------------------------------|
| **Creation Cost**   | Expensive (New stack every time)           | Cheap (Reuses existing threads) |
| **Overload Risk**   | High (Can crash JVM with too many threads) | Low (Bounded by pool size)      |
| **Code Complexity** | High (Manual sync/lifecycle)               | Low (Just `submit()`)           |
| **Result Handling** | Difficult (Void return type)               | Easy (`Future` object)          |


-----------------------------

## Q - How do you properly shut down an ExecutorService?

### Why shutdown is required

An `ExecutorService` manages non-daemon threads.

If you do not shut it down:

* JVM will not exit
* Threads keep running or waiting
* Resources leak (memory, threads)

So shutdown is **mandatory** in production code.

### shutdown() — Graceful shutdown

**What it does:**

* Stops accepting new tasks
* Allows already submitted tasks to finish
* Does not interrupt running threads

```java
executor.shutdown();
```

**ELI5**

> "Finish your current work, but don't take new work."

**When to use**

* Normal application shutdown
* When task completion is important


### shutdownNow() — Immediate shutdown

**What it does**

* Attempts to stop everything immediately
* Interrupts running threads
* Returns a list of tasks that never started

```java
List<Runnable> pending = executor.shutdownNow();
```

**Important detail**

* Interrupt is a request, not a guarantee
* Tasks must handle interruption properly

**ELI5**
> "Drop what you’re doing and stop now."

**When to use**

* Emergency shutdown
* Application failure scenarios


### awaitTermination() — Wait for shutdown to complete

**What it does**

* Blocks the calling thread
* Waits until (one of them satisfies):
  * All tasks finish
  * Timeout expires
  * Thread is interrupted

```java
executor.awaitTermination(10, TimeUnit.SECONDS);
```

**Return value**

* `true` → executor terminated
* `false` → timeout occurred


**ELI5**

>"Wait until everyone is done or time runs out."


### Proper shutdown pattern (INTERVIEW GOLD)

```java
executor.shutdown(); // graceful

try {
    if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
        executor.shutdownNow(); // force
    }
} catch (InterruptedException e) {
    executor.shutdownNow();
    Thread.currentThread().interrupt();
}
```

**Why this is correct**

* Tries graceful shutdown first
* Escalates only if needed
* Handles interruption correctly


-----------------------------


## Q - What's the diff b/w process and threads?

**A Process** is an independent instance of a program in execution, possessing its own
private memory address space - partitioned into the Code Section, Data Section, Heap, and
Stack - along with ownership of system resources like open files and sockets.

**A Thread** is the smallest unit of execution managed by the OS.
It exists within a process and shares the process's
resources (Heap memory, File handles, Code segment) while maintaining
its own private execution context (Stack, Registers, and Program Counter).

### Process — Components

#### What a process owns

🧠 Process Virtual Address Space

* Code section - Program instructions (read-only)
* Data section - Global & static variables
* Heap - Dynamically allocated memory
* Stack area (important) - Space where multiple thread stacks live

🧾 OS Metadata

* Process Control Block (PCB)
  Stores:
  * PID
  * Process state
  * Memory mappings (page tables)
  * Open file table
  * List of threads
  * Scheduling info


### Thread — Components (core focus)

#### What a thread owns (per thread)
🧠 Execution State

1. Stack
  * Method call frames
  * Local variables
  * Return addresses

2. Registers
  * General purpose registers
  * Stack pointer
  * Instruction pointer (PC)

3. Thread Control Block (TCB) ← IMPORTANT


### Thread Control Block (TCB)

The TCB is the OS data structure that describes a thread.

**What the TCB contains**

| Field               | What it stores              |
|---------------------|-----------------------------|
| Thread ID           | Unique identifier           |
| Thread state        | Running / Ready / Blocked   |
| Program Counter     | Next instruction to execute |
| Stack pointer       | Location of thread’s stack  |
| Register snapshot   | Saved CPU registers         |
| Priority            | Scheduling priority         |
| Parent process      | Which process owns it       |
| Scheduling metadata | Time slice, CPU affinity    |

ELI5

> The TCB is the resume file of a thread.
> When the OS pauses a thread, it saves everything needed to continue later.
>
>

### What Threads Share (inside the same process)

All threads inside one process share:

* Code section
* Data section (static variables)
* Heap (objects)
* Open files & sockets

This sharing is why threads exist.


### Why Threads Are Lightweight (this is critical)

Threads are lightweight because they reuse almost everything.

#### 1. No separate address space

* Process creation → new virtual memory
* Thread creation → reuse existing memory

#### 2. Cheaper context switching

Process switch requires:

* Switching page tables
* Flushing TLB
* Memory remapping

Thread switch requires only:

* Saving/restoring registers
* Switching stack pointer

Much less OS work.


#### 3. Minimal metadata

* PCB = large, complex
* TCB = small and simple


#### 4. Fast communication

* Processes → IPC (pipes, sockets)
* Threads → shared variables (heap)


### Interview-perfect closing line (memorize)

Threads are lightweight because they share the process's memory and resources, requiring only a stack,
registers, and a Thread Control Block, making creation and context switching much cheaper than processes.

---

![threads vs process](../images/threads-vs-process.png)

### Resources:

* https://www.scaler.com/topics/course/free-operating-system-course/video/1443/


-----------------------------


## Q - What is an atomic operation?

Atomicity means an operation is indivisible — it either happens completely or not
at all, and no other thread can observe it in an intermediate state.

-----------------------------

## Q - Which read and write operations are atomic in Java?

Atomic Read/Write Operations

1. Reads and writes of all primitive types except `long` and `double`
  * `int`, `boolean`, `char`, `byte`, `short`, `float`
  * Atomic for both read and write

2. Reads and writes of `long` and `double` declared as `volatile`
  * Guarantees atomicity plus visibility

3. Reads and writes of object references
  * Reading or writing the reference value itself (i.e., assigning or accessing a reference) is
    atomic; object creation and initialization are not.

4. Reads and writes of volatile variables (any type)
  * Atomic read/write with visibility and ordering guarantees

5. Reads and writes via classes in `java.util.concurrent.atomic`
  * Examples: `AtomicInteger.get()`, `AtomicInteger.set()`, `AtomicReference.get()`

6. Monitor enter and exit (`synchronized`)
  * Lock acquisition and release are atomic operations


### Resources

* https://www.oreilly.com/library/view/the-java-r-language/9780133260335/ch17lev1sec7.html

-----------------------------

## Q - What is deadlock?

Deadlock is a situation where two or more threads are stuck forever, because each
one is waiting for the other to release something.


### Resources:

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/quiz/4476614#notes

-----------------------------

## Q - Explain synchronized keyword

The `synchronized` keyword provides **Mutual Exclusion and Visibility for critical sections of code**.
It ensures that only one thread can execute a protected block of code at a time, preventing race conditions.

### Resources

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/lecture/11200008#notes

-----------------------------

## Q - Explain synchronization problem

The synchronization problem arises in concurrent systems when multiple threads or processes
access shared mutable resources without proper coordination, leading to incorrect, inconsistent,
or unpredictable results.

In essence, it is the problem of controlling concurrent access to shared data so that data
integrity and correctness are preserved.

### Resources

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/lecture/11199990#notes

-----------------------------

## Q - Explain different ways of inter-thread communication

Inter-thread communication refers to mechanisms that allow threads to coordinate execution, share data
safely, and signal events without busy-waiting or race conditions.

Below are the primary, interview-relevant mechanisms in Java, grouped by abstraction level.

### 1. wait(), notify(), notifyAll() (Intrinsic Locks)

These methods enable threads to communicate via object monitors.

**How it works**

* A thread calls `wait()` to release the monitor and suspend execution.
* Another thread calls `notify()` / `notifyAll()` to wake waiting threads.
* Must be used **inside a synchronized block/method**.

**Typical Use Case**

Producer–Consumer coordination.

```java
synchronized (lock) {
    while (!condition) {
        lock.wait();
    }
    // proceed
}
```

Pros:
* Low-level, powerful

Cons:
* Error-prone (missed signals, spurious wakeups)


### 2. volatile Variables (Visibility-Based Communication)

Used when one thread needs to **signal state changes** to others.

**How it works**

* Guarantees **visibility and ordering**
* Does not provide mutual exclusion

```java
volatile boolean stopped = false;
```

**Use Case**
* Simple flags (stop signals, readiness indicators)


### 3. Lock and Condition (java.util.concurrent.locks)

A more flexible alternative to `synchronized` + `wait/notify`.

**How it works**

* `Condition.await()` ≈ `wait()`
* `Condition.signal()` / `signalAll()` ≈ `notify()`

```java
lock.lock();
try {
    condition.await();
} finally {
    lock.unlock();
}
```

**Advantages:**

* Multiple conditions per lock
* Better control and readability


### 4. Blocking Queues (`BlockingQueue`)

High-level, built-in inter-thread communication.

**How it works**

* `put()` blocks if full
* `take()` blocks if empty

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
```

**Best for**

* Producer–Consumer patterns
* Eliminates manual synchronization


### 5. Semaphores

Used to **control access to a limited number of resources**.

**How it works**

* `acquire()` blocks if permits unavailable
* `release()` signals availability


```java
Semaphore semaphore = new Semaphore(3);
```

**Use Case**

* Connection pools
* Rate limiting


### 6. Latches and Barriers

**CountDownLatch**

* One-time synchronization point
* Threads wait until count reaches zero

**CyclicBarrier**

* Reusable barrier
* All threads wait until everyone arrives


### 7. Atomic Variables

Used for **lock-free communication** using CAS (Compare-And-Swap).

```java
AtomicInteger counter = new AtomicInteger();
counter.incrementAndGet();
```

**Use Case**

* Counters, sequence numbers
* Non-blocking coordination


### 8. Thread.join()

Allows one thread to **wait for another thread to complete**.

```java
t.join();
```

**Use Case**

* Dependency sequencing

### Summary Table (Interview Gold)

| Mechanism          | Communication Style   | Blocking | Level  |
|--------------------|-----------------------|----------|--------|
| `wait/notify`      | Condition signaling   | Yes      | Low    |
| `volatile`         | Visibility signaling  | No       | Low    |
| `Lock + Condition` | Advanced signaling    | Yes      | Medium |
| `BlockingQueue`    | Data passing          | Yes      | High   |
| `Semaphore`        | Resource permits      | Yes      | Medium |
| `CountDownLatch`   | One-time coordination | Yes      | High   |
| `CyclicBarrier`    | Group synchronization | Yes      | High   |
| Atomic variables   | Lock-free signaling   | No       | Medium |
| `join()`           | Completion dependency | Yes      | Low    |


### Resources

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/lecture/11199990#notes


-----------------------------


## Q - What are some key points to remember when using virtual threads

**Key Points:**

### 1. The Golden Rule: Throughput, Not Latency

* **Throughput (YES):** Virtual Threads allow you to handle **Millions** of concurrent connections
  instead of thousands. This massively increases system throughput (requests per second).

* **Latency (NO):** They do not execute code faster. In fact, a single request might be micro-seconds
  slower due to the overhead of mounting/unmounting from the carrier thread.

* **Key Phrase:** "Virtual threads scale concurrency, not speed."


### 2. CPU-Bound Tasks = No Benefit

* **Why:** Virtual threads rely on yielding (unmounting) when they hit a blocking I/O
  operation (like waiting for a DB query).

* **The Trap:** If you run a heavy calculation (CPU-bound), the virtual thread never yields.
  It hogs the underlying OS Carrier Thread, blocking other virtual threads from running.

* **Advice:** Stick to Platform Threads for heavy computation (e.g., video processing, encryption).


### 3. The "Pinning" Problem (Critical Interview Topic)

This is the most common "gotcha" in Virtual Threads.

* **The Issue:** If you use a synchronized block or a native method, the virtual thread becomes
  pinned to the carrier thread. Even if it hits blocking I/O, it cannot unmount.

* **The Fix:** Use `ReentrantLock` instead of `synchronized` where possible in new code, although the
  JDK team is working on fixing this limitation.


### 4. Do NOT Pool Virtual Threads

* **Old Habit:** With Platform threads, we used `ExecutorService` pools because creating threads was
  expensive (2MB memory + OS calls).

* **New Rule:** Virtual threads are cheap (bytes of memory + no OS call). Create a new one for every task.

* **Code:** Use `Executors.newVirtualThreadPerTaskExecutor()`, never `newFixedThreadPool()`.

### 5. ThreadLocal Explosion

* **The Danger:** In the old world, we had 200 threads, so 200 `ThreadLocal` variables were fine.

* **The New World:** If you have 1 million virtual threads, 1 million `ThreadLocal` instances can
  instantly cause an `OutOfMemoryError`.

* **Advice:** Use `ScopedValues` (Preview feature) instead of `ThreadLocal` for passing context.


### Resources

* https://marcelclasses.udemy.com/course/java-multithreading-concurrency-performance-optimization/learn/lecture/11199990#notes


-----------------------------


## Q - Explain the evolution of concurrency API in Java

![alt text](../images/evolution-of-concurrency-API-java.png)


-----------------------------


## Q - What is CopyOnWriteArrayList and Why is it named CopyOnWriteArrayList, why don't they use something like Collections.synchronizedList()?

### CopyOnWriteArrayList

Whenever a modification operation is performed on the Array list, the existing array is copied internally,
the modification operation is performed on the new copy and then the new array is returned.
The old array will be discarded. Hence, the name, Copy on write.

Modification operations include add, addAll, remove, removeAll, addIf, removeIf, subList etc.


### Collections.synchronizedList

`Collections.synchronizedList()` provides a synchronized (thread-safe) list backed by the specified list.

Example:

```java
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
```

Here is the breakdown of what this is, how it works, and the dangerous trap hidden inside it.

**1\. How it Works (The Wrapper)**

It acts as a Guardian or a wrapper around your regular, non-thread-safe `ArrayList`.

* **Mechanism**: It holds an internal reference to your list.
* **The Lock**: Every single method (`add`, `get`, `remove`, `size`) is wrapped in a `synchronized` block
  using the list object itself as the lock (mutex).
* **Behavior**: Only one thread can access the list at a time. If Thread A is calling `get(0)`,
  Thread B must wait until A is finished before it can call `add("New")`.

**2\. The "Trap": Iteration is NOT Synchronized**

This is the most common interview question about this topic.

While individual methods like `add()` are thread-safe, iterating over the list is not atomic.
If one thread is iterating through the list and another thread modifies it (adds/removes an element) at the
same time, the iterator will crash with a `ConcurrentModificationException`.

**The Fix**: You must manually synchronize the entire iteration block.

```java
List<String> syncList = Collections.synchronizedList(new ArrayList<>());

// BAD CODE (Will crash in multi-threaded env)
for (String s : syncList) {
    System.out.println(s);
}

// GOOD CODE (Manually Synchronized)
synchronized (syncList) {
    for (String s : syncList) {
        System.out.println(s);
    }
}
```


-----------------------------


## Q - Which threads are guaranteed to be created when a Java program starts?

The only thread that is guaranteed to be created when a Java program starts is the `main` thread.

**Explanation (Interview-Safe)**

* The JVM must create the main (non-daemon) thread to invoke:
    ```java
    public static void main(String[] args)
    ```
* **This is the only thread mandated by the Java Language Specification**.

**All other threads—such as:**

* Garbage Collector threads
* JIT compiler threads
* Signal dispatcher
* Reference handler / finalizer threads

are **JVM implementation–dependent**:

* They may exist
* They may be multiple
* They may start lazily or dynamically

Therefore, they are **not guaranteed**.


-----------------------------


## Q - Explain stack and heap memory regions in the context of threads?

### 1. Stack Memory (Thread-specific)

The stack stores the execution state of a thread.

**Ownership**

* One stack per thread
* Not shared
* Exists inside the process address space

**What is allocated on the stack?**

* Method call frames
* Local variables (primitives)
* References to objects (not the objects themselves)

```java
void foo() {
    int x = 10;          // stack
    User u = new User(); // reference on stack
}
```

* `x` → stack
* `u` (reference) → stack
* `new User()` → heap

**Why stack is thread-safe**

Each thread has its own stack, so local variables are isolated by default.


### 2. Heap Memory (Shared across threads)

The heap stores objects and shared data that can be accessed by multiple threads.

**Ownership**

* Shared by all threads in a process
* Managed by the Garbage Collector

**What is allocated on the heap?**

**Objects:** Anything created with `new`:

```java
new Object();
new String("abc");
new ArrayList<>();
```

**Instance variables:** Instance variables live inside objects, and objects live on the heap:

```java
class User {
    int age; // heap (inside object)
}
```

**Static variables**

```java
class Counter {
    static int count = 0;
}
```

* The value of `count` → heap
* Shared across all threads
* Needs synchronization if mutable


### 3. Metaspace (not in heap)

**What is Metaspace?**

Metaspace stores class metadata, not variable values.

What goes into Metaspace?

* Class structure
* Method bytecode
* Field names and types
* Constant pool
* Annotations

Example:

```java
class A {
    static int x;
    int y;
}
```

**Metaspace:**

* `Class A` has a static field `x` and an instance field `y`

**Heap:**

* Actual value of `x` and objects containing `y`


### Key clarification (interview-critical)

* ❌ Static variables are NOT stored in Metaspace
* ✅ Static variable values are stored on the heap
* ✅ Only metadata is in Metaspace


### 4. Why this matters for threads

* Stack → isolated → safe
* Heap → shared → needs synchronization
* Metaspace → read-mostly → safe

This explains:

* Why local variables don't need locks
* Why shared objects do
* Why static fields cause race conditions


### Interview-perfect closing line (memorize)

In a multithreaded application, each thread has its own stack for execution state, all threads
share the heap where objects and static variable values reside, and class metadata is stored
separately in Metaspace.


-----------------------------



## Q - Explain how AtomicInteger works?

### 1. The Core Concept: "Optimistic Locking"

* **Synchronized (Pessimistic):** "I assume someone else is going to mess with this
  variable, so I will lock the door before I even touch it. No one enters
  until I'm done." (Safe, but slow due to blocking/context switching).
* **AtomicInteger (Optimistic):** "I assume no one is messing with it. I'll read the
  value, do my math, and then quickly try to update it. If someone *did* change it
  while I wasn't looking, I'll just retry." (Fast, non-blocking).

---

### 2. The Hardware Magic: CAS (Compare-And-Swap)

`AtomicInteger` does not use Java locks. It uses a specific **CPU instruction** called
**CAS** (Compare-And-Swap).

**The Logic:**
The CPU says: *"I will update this memory location ONLY if it currently holds the value I expect."*

It takes 3 parameters:

1. **V (Value):** The memory address where the variable lives.
2. **E (Expected):** The value I *think* is there (what I read 1ms ago).
3. **N (New):** The new value I want to write.

**The CPU Operation:**

```text
if (V == E) {
    V = N; // Update successful! Return true.
} else {
    // Someone else changed V before I could!
    // Do NOT update. Return false.
}

```

*Crucially, this `if-then-update` happens as a **single, indivisible hardware step**.*

---

### 3. The "Retry Loop" (Spin Lock)

This is what happens inside `incrementAndGet()` when two threads compete.

**Scenario:** `count = 10`.

* **Thread A:** Wants to increment. Reads `10`. Calculates `11`.
* **Thread B:** Wants to increment. Reads `10`. Calculates `11`.

**The Race:**

1. **Thread A** reaches the CPU first.
  * **CAS(V=Addr, E=10, N=11)**
  * Is current value 10? **YES.**
  * Update memory to **11**. Return `true`.


2. **Thread B** reaches the CPU 1 nanosecond later.
  * **CAS(V=Addr, E=10, N=11)**
  * Is current value 10? **NO.** (It is now 11).
  * **FAIL.** Return `false`.



**What does Thread B do?**
It does **NOT** sleep or block. It enters a `while` loop:

1. "Damn, I failed. Let me read the *new* value." (Reads 11).
2. "Okay, let me try to increment 11 to 12."
3. **CAS(V=Addr, E=11, N=12)**.
4. Success!

---

### 4. The Code (Under the Hood)

If you open the source code of `AtomicInteger` (Java 8 version for clarity), you see this pattern
using the infamous `Unsafe` class:

```java
public final int getAndIncrement() {
    int current;
    int next;
    do {
        // 1. Read the current value from volatile memory
        current = get(); 
        
        // 2. Calculate new value (local CPU register)
        next = current + 1; 
        
        // 3. ATTEMPT the update atomically
        // "If memory at 'valueOffset' is still 'current', set it to 'next'"
    } while (!compareAndSet(current, next)); // If false, LOOP AGAIN!
    
    return current;
}

```

### 5. Pros & Cons (Interview Gold)

| Feature             | Synchronized             | AtomicInteger (CAS)           |
|---------------------|--------------------------|-------------------------------|
| **Mechanism**       | OS Mutex / Monitor Lock  | CPU Instruction (lock-free)   |
| **Thread State**    | BLOCKED (Context Switch) | RUNNABLE (Spinning)           |
| **Low Contention**  | Slow (Lock overhead)     | **Extremely Fast**            |
| **High Contention** | Better (Threads sleep)   | **Dangerous** (CPU spin burn) |

**Senior Insight:**

>
> CAS is fantastic for low-to-medium contention. But if you have 1000 threads fighting for the
> same AtomicInteger, they will spin in that `while` loop, burning 100% CPU without doing useful
> work. In that specific extreme case, `LongAdder` or a lock might actually be better.
>


-----------------------------


## Q - What is Spurious Wakeup?

A spurious wakeup occurs when a thread waiting on `wait()` or `await()` wakes up without any
corresponding `notify`, `notifyAll`, or `signal` call. It happens due to JVM and OS-level scheduling
and synchronization optimizations, which is why waiting conditions must always be checked in a loop.


-----------------------------


## Q - What is class level lock?

A class-level lock is a lock associated with the `java.lang.Class` object,
not with any instance of the class.

Every loaded class in Java has exactly one `java.lang.Class` object, and therefore
it has **exactly one class-level lock**.

So when you synchronize on:

```java
synchronized (MyClass.class)
```

OR use a static synchronized method:

```java
static synchronized void doSomething() { }
```

You are acquiring the same single lock - regardless of how many instances exist.

Here is an example:

```java
class Counter {
    private static int count = 0;

    public void increment() {
        synchronized (Counter.class) {
            count++;
        }
    }
}
```

or equivalent:

```java
class Counter {
    private static int count = 0;

    public static synchronized void increment() {
        count++;
    }
}
```

Now:

* Regardless of how many `Counter` objects you create
* Regardless of which object a thread uses

Only one thread in the entire JVM can execute `increment()` at a time.

## Q - How threads communicate using wait() and notify()?

Here is an example:

```java
package org.example.sec02;

class Resource {
    private int data;
    private boolean hasData;

    public synchronized void put(int i) {
        while (hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }

        data = i;
        hasData = true;
        System.out.println("Data produced: " + data);
        notify();
    }

    public synchronized void get() {
        while (!hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }

        System.out.println("Data consumed: " + data);
        hasData = false;
        notify();
    }
}

class Producer implements Runnable {
    private Resource resource;

    public Producer(Resource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            resource.put(i);
        }
    }
}

class Consumer implements Runnable {
    private Resource resource;

    public Consumer(Resource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            resource.get();
        }
    }
}

public class Example14 {
    public static void main(String[] args) {
        Resource resource = new Resource();
        Producer producer = new Producer(resource);
        Consumer consumer = new Consumer(resource);

        Thread t1 = new Thread(producer);
        Thread t2 = new Thread(consumer);

        t1.start();
        t2.start();
    }
}
```

### The problem we are solving using wait() and notify()

In multithreaded programs, threads often need to coordinate.

Example:

* One thread produces data
* Another thread consumes data

Rules:

* Producer must wait if data already exists
* Consumer must wait if data does not exist
* Threads must not waste CPU by busy-waiting

Java provides `wait()` and `notify()` to solve this coordination problem.

Core idea (in simple words)
> wait() makes a thread pause and release the lock until another thread signals it.
notify() or notifyAll() are used to signal waiting threads that the state has changed.

### Very important rules (must know)

* `wait()`, `notify()`, and `notifyAll()` must be called inside a synchronized context
* They work on the same object's lock (monitor)
* A thread must own the lock before calling them
* `wait()` releases the lock
* `notify()` and `notifyAll()` do NOT release the lock immediately
* Woken threads must re-acquire the same lock before continuing

### The shared resource

```java
class Resource {
    private int data;
    private boolean hasData;
```

* `data` → shared value
* `hasData` → state flag
  * `true` → producer must wait
  * `false` → consumer must wait


### Producer logic (`put()`)

```java
public synchronized void put(int i) {
```

#### Step 1: Check the condition

```java
while (hasData) {
    wait();
}
```

* If data already exists, **producer must wait**
* `wait()`:
  * releases the object's lock
  * puts the producer thread into **WAITING state**

`while` is used (not `if`) because:

* Threads can wake up spuriously
* Multiple threads may wake up
* Condition must always be re-checked

#### Step 2: Produce data

```java
data = i;
hasData = true;
System.out.println("Data produced: " + data);
```

Now data is available.


#### Step 3: Notify waiting threads

```java
notify();
```

What `notify()` does (very important)
> `notify()` **wakes ONE arbitrary** thread that is waiting **on the same object lock**.

Key points:

* The awakened thread does not run immediately
* It must first re-acquire the same lock
* Which thread wakes up is not guaranteed

### Consumer logic (`get()`)

```java
public synchronized void get() {
```

#### Step 1: Check the condition

```java
while (!hasData) {
    wait();
}
```

* If no data exists, consumer waits
* Lock is released so producer can run

#### Step 2: Consume data

```java
System.out.println("Data consumed: " + data);
hasData = false;
```

Data is now consumed.

#### Step 3: Notify waiting threads

```java
notify();
```

Again:
> `notify()` wakes **one thread waiting on the same object's lock**, typically the producer.


### `notify()` vs `notifyAll()` (critical distinction)

notify()

* Wakes one arbitrary thread
* Thread must be waiting on the same object
* Risky when multiple threads are waiting

notifyAll()

* Wakes ALL threads waiting on the same object lock
* All woken threads:
  * move from WAITING → BLOCKED
  * compete to re-acquire the lock
  * re-check the condition in while

```java
notifyAll(); // wakes all threads waiting on this object's monitor
```

### Limitations of `wait()` / `notify()`

* No fairness guarantee
* Single waiting queue per object
* Easy to misuse
* Hard to scale with many threads

### Thread starvation using `wait()` and `notify()`

The following is an example of thread starvation using `wait()` and `notify()`/`notifyAll()`.

```java
class Resource {
    private int data;
    private boolean hasData;

    public synchronized void put(int i) {
        while (hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }

        data = i;
        hasData = true;
        System.out.println("Data produced: " + data);
        notifyAll();
    }

    public synchronized void get() {
        while (!hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }

        System.out.println("Data consumed: (" + Thread.currentThread().getName() + ") "  + data);
        hasData = false;
        notifyAll();
    }
}

class Producer implements Runnable {
    private Resource resource;

    public Producer(Resource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            resource.put(i);
        }
    }
}

class Consumer implements Runnable {
    private Resource resource;

    public Consumer(Resource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            resource.get();
        }
    }
}

public class Example15 {
    public static void main(String[] args) {
        Resource resource = new Resource();
        Producer producer = new Producer(resource);
        Consumer consumer = new Consumer(resource);

        Thread t1 = new Thread(producer);
        Thread t2 = new Thread(consumer, "c1");
        Thread t3 = new Thread(consumer, "c2");

        t1.start();
        t2.start();
        t3.start();
    }
}
```


**Output:**

```text
Data produced: 0
Data consumed: (c2) 0
Data produced: 1
Data consumed: (c2) 1
Data produced: 2
Data consumed: (c2) 2
Data produced: 3
Data consumed: (c2) 3
Data produced: 4
Data consumed: (c2) 4
Data produced: 5
Data consumed: (c2) 5
Data produced: 6
Data consumed: (c2) 6
Data produced: 7
Data consumed: (c2) 7
Data produced: 8
Data consumed: (c2) 8
Data produced: 9
Data consumed: (c2) 9
Data produced: 10
Data consumed: (c2) 10
Data produced: 11
Data consumed: (c2) 11
Data produced: 12
Data consumed: (c2) 12
Data produced: 13
Data consumed: (c2) 13
Data produced: 14
Data consumed: (c2) 14
Data produced: 15
Data consumed: (c2) 15
Data produced: 16
Data consumed: (c2) 16
Data produced: 17
Data consumed: (c2) 17
Data produced: 18
Data consumed: (c2) 18
Data produced: 19
Data consumed: (c2) 19
Data produced: 20
Data consumed: (c2) 20
Data produced: 21
Data consumed: (c2) 21
Data produced: 22
Data consumed: (c2) 22
Data produced: 23
Data consumed: (c2) 23
Data produced: 24
Data consumed: (c2) 24
Data produced: 25
Data consumed: (c2) 25
Data produced: 26
Data consumed: (c2) 26
Data produced: 27
Data consumed: (c2) 27
Data produced: 28
Data consumed: (c2) 28
Data produced: 29
Data consumed: (c2) 29
Data produced: 30
Data consumed: (c2) 30
Data produced: 31
Data consumed: (c2) 31
Data produced: 32
Data consumed: (c2) 32
Data produced: 33
Data consumed: (c2) 33
Data produced: 34
Data consumed: (c2) 34
Data produced: 35
Data consumed: (c2) 35
Data produced: 36
Data consumed: (c2) 36
Data produced: 37
Data consumed: (c2) 37
Data produced: 38
Data consumed: (c2) 38
Data produced: 39
Data consumed: (c2) 39
Data produced: 40
Data consumed: (c2) 40
Data produced: 41
Data consumed: (c2) 41
Data produced: 42
Data consumed: (c2) 42
Data produced: 43
Data consumed: (c2) 43
Data produced: 44
Data consumed: (c2) 44
Data produced: 45
Data consumed: (c2) 45
Data produced: 46
Data consumed: (c2) 46
Data produced: 47
Data consumed: (c2) 47
Data produced: 48
Data consumed: (c2) 48
Data produced: 49
Data consumed: (c2) 49
```

Notice that not even once the thread `c1` is invoked to consume the items.

This behavior is expected, and it reveals two very important concurrency concepts:

* `notify()` / `notifyAll()` does NOT guarantee fairness
* Sleeping while holding a lock causes thread starvation

### What you are observing

You have:
* 1 Producer
* 2 Consumers (`c1`, `c2`)
* Shared monitor: `Resource`

But only `c2` consumes all items.

This feels wrong at first, but the JVM is behaving correctly.

### Important concept: `notifyAll()` ≠ fairness

Even though you use:

```java
notifyAll();
```

This only means:
> "Wake everyone up and let them compete"

It does NOT mean:

* Round-robin scheduling
* Equal chance
* Alternating threads

The JVM scheduler is **not fair by default**.

So what's the solution?

To fix this, you must stop using `synchronized` and switch to `ReentrantLock` with fairness enabled.

This forces the lock to act like a polite queue (FIFO - First In, First Out). If c1 has been waiting longer,
c1 will get the lock next, guaranteed.

### Revised Code (Fair Version) using ReentrantLock

```java
class Resource {
    private int data;
    private boolean hasData;
    private final Lock lock = new ReentrantLock(true);
    private final Condition condition = lock.newCondition();

    public void put(int i) {
        // Lock the door
        lock.lock();
        try {
            while (hasData) {
                try {
                    // Sit on the Single Bench
                    condition.await();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }

            data = i;
            hasData = true;
            System.out.println("Data produced: " + data);
            condition.signalAll(); // Wake up everyone on the Single Bench
        } finally {
            lock.unlock();
        }
    }

    public void get() {
        // Lock the door
        lock.lock();
        try {
            while (!hasData) {
                try {
                    // Sit on the Single Bench
                    condition.await();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }

            System.out.println("Data consumed: (" + Thread.currentThread().getName() + ") " + data);
            hasData = false;
            condition.signalAll(); // Wake up everyone on the Single Bench
        } finally {
            lock.unlock();
        }
    }
}

class Producer implements Runnable {
    private final Resource resource1;

    public Producer(Resource resource1) {
        this.resource1 = resource1;
    }

    @Override
    public void run() {
        while (true){
            resource1.put((int)(Math.random() * 4));
        }
    }
}

class Consumer implements Runnable {
    private final Resource resource1;

    public Consumer(Resource resource1) {
        this.resource1 = resource1;
    }

    @Override
    public void run() {
        while (true){
            resource1.get();
        }
    }
}

public class Example14 {
    public static void main(String[] args) {
        Resource resource1 = new Resource();
        Producer producer1 = new Producer(resource1);
        Consumer consumer1 = new Consumer(resource1);

        Thread t1 = new Thread(producer1);
        Thread t2 = new Thread(consumer1, "c1");
        Thread t3 = new Thread(consumer1, "c2");
        Thread t4 = new Thread(consumer1, "c3");

        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```

### How threads communicate using Lock and Condition

This example demonstrates thread coordination between one producer and multiple
consumers using `ReentrantLock` and `Condition`.

### What problem this code solves

We have:

* One producer thread that generates data
* Multiple consumer threads that consume data
* A shared resource that can hold only one value at a time

Rules:

* Producer must wait if data already exists
* Consumers must wait if no data exists
* Only one thread may access the resource at a time
* No thread should waste CPU by busy waiting

### Why Lock and Condition are used together

#### `Lock` → mutual exclusion (who can enter)

```java
private final Lock lock = new ReentrantLock(true);
```

`Lock` replaces `synchronized`.

It provides:

* Explicit lock/unlock control
* Optional fairness (true → FIFO lock acquisition)
* More flexibility than synchronized

Without `Lock`:

* Multiple threads could read/write data simultaneously
* Race conditions would occur

👉 **Lock ensures only one thread enters the critical section at a time**.

#### `Condition` → thread coordination (who should wait and wake)

```java
private final Condition condition = lock.newCondition();
```

`Condition` replaces `wait()`/`notify()`.

It provides:

* A waiting queue associated with a lock
* Ability for threads to:
  * `await()` → wait without holding the lock
  * `signal()` / `signalAll()` → wake waiting threads


Without Condition:

* Threads would have no way to sleep efficiently
* They would have to busy-wait or poll

👉 **Condition enables threads to wait until a specific condition is met**.

### Why both are required (important)

| Concern              | Solved by             |
|----------------------|-----------------------|
| Mutual exclusion     | `Lock`                |
| Waiting & signaling  | `Condition`           |
| Fair scheduling      | `ReentrantLock(true)` |
| Fine-grained control | `Lock + Condition`    |

👉 `Condition` cannot exist without a `Lock`.

👉 A `Lock` alone cannot coordinate thread waiting.

They are designed to be used **together**.

### Step-by-step flow (ELI5)

#### Producer (`put()`)

1. Acquires the lock (enters the room)
2. If data already exists:
  * Calls `condition.await()`
  * Releases the lock and waits
3. Produces data
4. Calls `condition.signalAll()`
  * Wakes all waiting consumers
5. Releases the lock

#### Consumer (`get()`)

1. Acquires the lock
2. If no data exists:
  * Calls `condition.await()`
  * Releases the lock and waits
3. Consumes data
4. Calls `condition.signalAll()`
  * Wakes producer and other consumers
5. Releases the lock

### Why while is used instead of if

```java
while (!hasData) {
    condition.await();
}
```

Because:

* Threads can wake up spuriously
* Multiple threads may wake up at once
* Condition must be re-checked after waking

This is **mandatory best practice**.

### Why signalAll() is used instead of signal()

Because:

* There are multiple consumers
* `signal()` may wake the wrong thread
* `signalAll()` ensures no missed wakeups

Fair lock + `signalAll()` prevents starvation.

### Why fairness (new ReentrantLock(true)) matters

```java
new ReentrantLock(true)
```

This ensures:

* Threads acquire the lock in FIFO order
* Consumers get fair turns
* No thread monopolizes the resource

This solves the starvation issue you observed earlier.


-----------------------------


## Q - What is CAS (Compare-And-Swap)?

CAS is a low-level **CPU instruction** that allows for concurrency without 
using heavy locks (synchronized). It is the foundation of **Optimistic Locking** in Java.

It powers classes like `AtomicInteger`, `AtomicReference`, and `ConcurrentHashMap`.

### 1. The Mechanics (The 3 Operands)

The CPU instruction takes three arguments:

1. **V (Memory Address):** Where the variable lives.
2. **E (Expected Value):** What you *think* is currently there.
3. **N (New Value):** What you want to write.

### 2. The Atomic Operation

The CPU performs this logic as a **single, indivisible step**:

```text
if (Value_at_Memory_V == Expected_E) {
    Update Memory_V to New_N;
    return true; // Success
} else {
    return false; // Failed, someone else updated it first
}

```

### 3. The Retry Loop (Spin Lock)

Because CAS is optimistic, it might fail if another thread modified the variable 
while we were calculating. Therefore, Java wraps the CAS instruction in a `while` loop 
to keep trying until it succeeds.

**Example: How `AtomicInteger` increments:**

```java
public int incrementAndGet() {
    int current, next;
    do {
        current = get();       // 1. Read latest value (e.g., 10)
        next = current + 1;    // 2. Calculate new value (e.g., 11)
        
        // 3. ATTEMPT to swap 10 -> 11 atomically.
        // If 'current' changed in the meantime, CAS returns false.
        // The loop forces us to go back to Step 1 and try again.
    } while (!compareAndSet(current, next)); 
    
    return next;
}
```

### 4. Pros & Cons (Summary)

| Feature          | CAS (Optimistic)                                       | Locking (Pessimistic)                   |
|------------------|--------------------------------------------------------|-----------------------------------------|
| **Best For**     | **Low to Medium Contention**                           | **Complex Logic / High Contention**     |
| **Mechanism**    | Busy-Spinning (While Loop)                             | Context Switching (Sleep/Wake)          |
| **Advantage**    | Extremely fast (No OS overhead)                        | Saves CPU (Thread sleeps while waiting) |
| **Disadvantage** | **High CPU Usage** if threads fight over one variable. | Slow due to thread scheduling overhead. |


-----------------------------


## Q - What is Structured Concurrency (Java 21 Preview)?

Structured Concurrency is a modern concurrency model introduced as a preview 
feature in **Java 21** (Project Loom).

It enforces a simple rule:

> Tasks started together should complete together — and be treated as a single logical unit.
> 

It brings structure to concurrent code the same way structured programming brought order to `goto`.

---

### The Core Problem It Solves

Traditional Java concurrency (ExecutorService, CompletableFuture) allows you to:

* Spawn background tasks
* Forget to cancel them
* Leak threads
* Lose exceptions
* Create orphaned work

**Example (problematic):**

```java
ExecutorService executor = Executors.newFixedThreadPool(2);

Future<User> user = executor.submit(() -> fetchUser());
Future<Order> order = executor.submit(() -> fetchOrder());

// What if fetchUser fails?
// What if fetchOrder hangs?
// Who cancels whom?
```

There is **no lifecycle boundary** tying these tasks together.

---

### The Structured Concurrency Principle

A parent task:

* Starts child tasks
* Waits for them
* Cancels them if one fails
* Collects results
* Ensures no task escapes the scope

Just like method calls:

* A method cannot outlive its caller
* A child thread should not outlive its parent scope

---

###  The API: `StructuredTaskScope`

Structured Concurrency is built around:

```java
StructuredTaskScope
```

**Example:**

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {

    Future<User> user = scope.fork(() -> fetchUser());
    Future<Order> order = scope.fork(() -> fetchOrder());

    scope.join();           // wait for both
    scope.throwIfFailed();  // propagate error

    return new Response(user.resultNow(), order.resultNow());
}
```


### What Just Happened?

Inside that scope:

1. Two tasks started.
2. If one fails → the other is automatically cancelled.
3. Exceptions propagate cleanly.
4. All tasks finish before leaving the block.
5. No orphan threads remain.

This is lifecycle containment.

---

###  Why This Is Powerful

Without structured concurrency:

* Threads can leak
* Exceptions get lost
* Cancellation logic becomes manual
* Observability becomes harder

With structured concurrency:

* Failure is centralized
* Cancellation is automatic
* Code reads like synchronous logic
* Resource cleanup is deterministic

---

###  Relationship with Virtual Threads

Structured concurrency  uses virtual threads by default

---

### Two Common Policies

#### 1 ShutdownOnFailure

If one task fails → cancel others.

#### 2 ShutdownOnSuccess

Return as soon as one succeeds → cancel rest.

Useful for:

* Fastest-response-wins queries
* Redundant service calls

---

### 🆚 Compared to CompletableFuture

| CompletableFuture        | Structured Concurrency |
| ------------------------ | ---------------------- |
| Unstructured graph       | Scoped block           |
| Manual cancellation      | Automatic              |
| Harder error handling    | Centralized            |
| Tasks may outlive parent | Tasks bound to scope   |

Structured Concurrency is about:

> Managing task lifecycles, not just async execution.
>


-----------------------------


## Q - How does ConcurrentHashMap work internally? (Java 7 vs Java 8)

This is the most asked concurrency collection question.

**Java 7 (Segment Locking - The "Old" Way):**

* It divided the map into **16 Segments**.
* Each Segment had its own `ReentrantLock`.
* **Result:** 16 threads could write simultaneously (one per segment).
* **Downside:** Complexity. 16 locks is still a limit.

**Java 8+ (CAS + Synchronized - The "Modern" Way):**

* **No Segments.** It looks like a standard HashMap (Array of Buckets).
* **Reading (`get`):** Completely lock-free (using `volatile` reads).
* **Writing (`put`):**
    1. **Empty Bucket:** Uses **CAS (Compare-And-Swap)** to insert the new node. 
         This is lock-free and incredibly fast.
    2. **Occupied Bucket (Collision):** Uses `synchronized` **only on that specific Node (Head of chain)**.

* **Result:** Millions of threads can write simultaneously as long as they touch different buckets.


-----------------------------


## Q - What were the limitations of the Future interface in Java 5, and how does CompletableFuture address them?

The `Future` interface, introduced in Java 5, provided a way to hold a reference to an 
asynchronous calculation. However, it had significant architectural limitations that made 
building complex, non-blocking systems difficult. `CompletableFuture` (Java 8) was designed 
specifically to resolve these issues.

Here are the technical limitations of `Future` and how `CompletableFuture` addresses them.

### 1. The Blocking Problem (`get()`)

* **Issue with Future:** The only way to retrieve the result from a `Future` is to call the `.get()` method. 
   This method is **blocking**. The thread calling `.get()` is forced to wait (idle) until the 
   computation completes. This negates the benefits of asynchronous programming because the 
   calling thread cannot perform other useful work while waiting.
* **CompletableFuture Solution:** It introduces a **push-based** model using callbacks. 
   Instead of asking for the result, you provide a function (using methods like `thenApply` or `thenAccept`) 
   that the system automatically executes once the result is available. The main thread remains unblocked.


### 2. Lack of Composition (Chaining)

* **Issue with Future:** You cannot create a pipeline of asynchronous steps. If you want to take the 
   result of `Future A` and pass it as input to `Future B`, you must manually block on `A.get()`, retrieve 
   the value, and then submit `B`. This creates a dependency where you essentially turn asynchronous code 
   back into synchronous code.
* **CompletableFuture Solution:** It supports functional composition. Methods like `thenCompose()` allow you
   to chain two asynchronous operations together. The output of the first stage is automatically piped as 
   the input to the next stage without any blocking code in between.


### 3. Combining Multiple Futures

* **Issue with Future:** There is no native API to manage multiple futures simultaneously. 
   If you spawn 5 independent tasks and want to run a final task only when **all** of them are finished, 
   you have to manually loop through the list and call `get()` on each one.
* **CompletableFuture Solution:** It provides combinator methods:
    * `CompletableFuture.allOf(f1, f2, ...)`: Returns a new future that completes when **all** input 
        futures complete.
    * `CompletableFuture.anyOf(f1, f2, ...)`: Returns a new future that completes as soon 
        as **any one** of the input futures completes.


### 4. No Exception Handling

* **Issue with Future:** If an exception occurs inside the asynchronous task, there is no way to 
   handle it gracefully within the future object itself. The exception is trapped until you 
   call `get()`, at which point it is thrown as an `ExecutionException`, requiring 
   verbose `try-catch` blocks in the consumer code.
* **CompletableFuture Solution:** It treats exception handling as a step in the pipeline. 
   Methods like `exceptionally()` and `handle()` allow you to catch errors occurring in any 
   previous stage and provide a recovery value (fallback) so the pipeline can continue processing
   without crashing.


### 5. Cannot Be Manually Completed

* **Issue with Future:** A `Future` represents the result of a background task submitted to 
   an `ExecutorService`. It is passive; you cannot externally force a value into it. 
   You are entirely dependent on the task finishing naturally.
* **CompletableFuture Solution:** As the name implies, it can be explicitly "completed." 
   You can call `.complete(value)` from any thread to immediately set the result of the future. 
   This is critical for scenarios like timeouts or bridging legacy callback-based APIs into a 
   modern `Future` workflow.


### Summary Table

| Limitation in Future                      | Solution in CompletableFuture                            |
|-------------------------------------------|----------------------------------------------------------|
| **Blocking retrieval** via `get()`        | **Non-blocking callbacks** via `thenApply`, `thenAccept` |
| **No Chaining** (dependent tasks)         | **Composition** via `thenCompose`                        |
| **Manual coordination** of multiple tasks | **Combinators** via `allOf`, `anyOf`, `thenCombine`      |
| **Verbose exception handling**            | **Fluent handling** via `exceptionally`, `handle`        |
| **Passive** (cannot set value)            | **Active** (can call `.complete(value)`)                 |


-----------------------------


# Level 6: Modern Java (Java 8 to Java 21)

## Q - What is functional interface.

A Functional Interface is an interface that contains **exactly one abstract method**.
It can have:

* Any number of default methods
* Any number of static methods
* Any number of private methods

But it must have **only one abstract method**.

Functional Interfaces are the foundation of lambda expressions and method references in Java.

### Examples of Functional Interfaces in Java

1. Runnable - `run()`
2. Callable - `call()`
3. Comparator
4. Function - `apply()`
5. Supplier - `get()`
6. Predicate - `test()`
7. Consumer - `accept()`


-----------------------------


## Q - Can you tell few functional interface which is already there before java 8?

* Runnable
* Callable
* Comparator (interviewer might ask about equals() method inside comparator)

-----------------------------


## Q - What are all functional interface introduced in java 8?

* Function
* Predicate
* Consumer
* Supplier

-----------------------------


## Q - What is lambda expression?

A lambda expression is a compact syntax for implementing the single abstract method
of a functional interface.


-----------------------------


## Q - What is Stream in java 8?

A Stream is a sequence of data elements that supports functional-style operations such as
filtering, mapping, and reducing - without modifying the original data source.

It allows you to process data in a declarative, pipeline-based, and efficient way.

Streams are not collections. They do NOT store data - they process data.


-----------------------------


## Q - How Java resolves method conflicts from multiple interfaces?

Java resolves method conflicts from multiple interfaces using well-defined rules
introduced primarily with default methods (Java 8).


### 1. No Conflict for Abstract Methods (Pre–Java 8)

If multiple interfaces declare the same abstract method:

```java
interface A {
    void foo();
}

interface B {
    void foo();
}

class C implements A, B {
    public void foo() { }
}
```

✔ No conflict

* Only one implementation is required
* Method signatures are identical


### 2. Class Always Wins over Interface

If a class (or superclass) provides a concrete implementation:

```java
class Base {
    public void foo() {}
}

interface A {
    default void foo() {}
}

class C extends Base implements A {}
```

✔ No conflict

* **Class method wins**
* Interface default method is ignored

> Rule: Class > Interface
>
>


### 3. Conflict Between Default Methods (Diamond Problem)

If two interfaces define the **same default method**:

```java
interface A {
    default void foo() {}
}

interface B {
    default void foo() {}
}

class C implements A, B {
    // Compile-time error unless overridden
}
```

❌ Compile-time error

**Resolution:**

The **implementing class must override the method**:

```java
class C implements A, B {
    @Override
    public void foo() {
        A.super.foo(); // optional
    }
}
```

> Java forces the class to explicitly resolve ambiguity.
>


### 4. Interface Inheritance: Most Specific Default Wins

If one interface extends another:

```java
interface A {
    default void foo() {}
}

interface B extends A {
    default void foo() {}
}

class C implements B {}
```

✔ No conflict

* B's default method is used
* More specific interface wins

> Rule: Subinterface > Superinterface


### 5. Abstract vs Default Method

If one interface provides a default method and another declares it abstract:

```java
interface A {
    default void foo() {}
}

interface B {
    void foo();
}

class C implements A, B {
    public void foo() {}
}
```

✔ Class must implement `foo()`
* Abstract declaration forces implementation


### 6. Static Methods in Interfaces

```java
interface A {
    static void foo() {}
}

interface B {
    static void foo() {}
}
```



✔ No conflict
* Static methods are **not inherited**
* Must be called using interface name

### Conflict Resolution Priority (Memory Aid)

```text
Class
  ↓
Subinterface
  ↓
Interface
```

### Interview-Ready One-Liner

> Java resolves multiple interface method conflicts by giving priority to
> class implementations, requiring explicit overrides for conflicting default methods, and selecting
> the most specific interface implementation when inheritance is involved.


-----------------------------


## Q - Why default methods were introduced in interfaces?

The primary reason default methods were introduced in Java 8 was Backward Compatibility.

### The Problem (Before Java 8)

In previous versions of Java, if you modified an interface (e.g., added a new method),
you broke **every single class** that implemented that interface. All those classes would fail to
compile until they implemented the new method.

### The Real-World Scenario

When Java 8 introduced Streams (`.stream()`), the architects wanted to add
the `stream()` method to the standard Collection interface so that every
`ArrayList`, `HashSet`, etc., could use it.

* Without Default Methods: Every custom Collection library (like Apache Commons, Guava, or your own `MyCustomList`)
  would have broken instantly upon upgrading to Java 8.
* With Default Methods: The `Collection` interface could provide a default implementation
  of `stream()`, so existing classes continued to work without any changes.

### Secondary Benefit: "Optional" Methods

Before Java 8, interfaces were strict: if an interface had 10 methods, you had to write code for
all 10, even if you only needed one.

#### The Classic "Mouse Listener" Problem

Imagine you are writing a UI app and want to detect a mouse click. You use the `MouseListener` interface.

**The Interface (Standard Java):**

```java
interface MouseListener {
    void mouseClicked(MouseEvent e);  // You want this
    void mousePressed(MouseEvent e);  // You don't care
    void mouseReleased(MouseEvent e); // You don't care
    void mouseEntered(MouseEvent e);  // You don't care
    void mouseExited(MouseEvent e);   // You don't care
}
```

#### 1. The "Old Way" (Painful)

Because the interface rules were strict, your class became filled with "dummy" empty methods
just to satisfy the compiler.

```java
// Java 7: I just want 'clicked', but I forced to write 4 empty methods!
class MyButtonHandler implements MouseListener {
    public void mouseClicked(MouseEvent e) {
        System.out.println("Button Clicked!");
    }

    // --- Useless Boilerplate Below ---
    public void mousePressed(MouseEvent e) {}
    public void mouseReleased(MouseEvent e) {}
    public void mouseEntered(MouseEvent e) {}
    public void mouseExited(MouseEvent e) {}
}
```

#### 2. The "New Way" (With Default Methods)

With Java 8, the interface creator can mark those less-common methods as default with an empty body `{}`.
This tells the compiler: "If the class doesn't implement this, just do nothing. Don't throw an error."

**The Modern Interface:**

```java
interface MouseListener {
    void mouseClicked(MouseEvent e); // Abstract: You MUST implement this

    // Default: You CAN implement these, but you don't have to.
    default void mousePressed(MouseEvent e) {} 
    default void mouseReleased(MouseEvent e) {}
    default void mouseEntered(MouseEvent e) {}
    default void mouseExited(MouseEvent e) {}
}
```

**Your Clean Code:**

```java
// Java 8+: Look how clean this is!
class MyButtonHandler implements MouseListener {
    @Override
    public void mouseClicked(MouseEvent e) {
        System.out.println("Button Clicked!");
    }
    // No other methods required. The defaults (empty bodies) are used automatically.
}
```


------------------


## Q - What are sealed classes?

Sealed Classes (introduced in Java 17) allow a class or interface to strictly restrict which
other classes may extend or implement it.

In simple terms: A parent class decides exactly who its children are.

### 1. The Syntax

You use the `sealed` keyword to define the class and the `permits` keyword to list the allowed subclasses.

```java
// 1. Parent restricts children to ONLY Circle and Square
public sealed class Shape permits Circle, Square { }

// 2. Child 1: Must be final, sealed, or non-sealed
public final class Circle extends Shape { }

// 3. Child 2: Can be non-sealed to open inheritance back up
public non-sealed class Square extends Shape { }
```


### 2. The Three Rules for Subclasses

Every class that extends a sealed class must specify how it relates to inheritance.
It must choose exactly one of these three modifiers:

* `final`: "I am the end of the line." No one can extend this class.
* `sealed`: "I have specific children." It continues the restriction.
* `non-sealed`: "I am open." It breaks the seal and allows anyone to extend it from this point down.


### 3. Why use them? (The "Killer Feature")

The biggest advantage is Exhaustive Pattern Matching in switch statements.

Because the compiler knows exactly which subclasses exist, it can ensure you cover every possible case.
You do not need a default block.

```java
// Compile-time safety!
String result = switch (shape) {
    case Circle c -> "It is a circle with radius " + c.radius();
    case Square s -> "It is a square with side " + s.side();
    // No 'default' needed because Java knows no other shapes exist!
};
```


-----------------------------


## Q - Why static methods inside interface were introduced in Java?

The primary reason static methods were introduced in Java 8 interfaces was to allow
utility methods to live directly inside the interface, eliminating the need for separate helper classes.

Here is the breakdown of why this matters:

**1\. The "Utility Class" Problem (Before Java 8)**

Before Java 8, interfaces could only define contracts (abstract methods).
If you wanted to provide helpful tools or common logic related to that interface,
you had to create a separate "plural" class.

* Interface: Collection
* Helper Class: Collections (Full of static methods like sort, reverse, etc.)

This was messy because you had to keep track of two files for one concept.

2\. The Solution (Java 8+)

By allowing static methods in interfaces means you can call
methods directly using the interface name without creating an object.

This allows API designers to put the helper tools inside the interface itself, keeping everything in one place.

* Old Way: `Collections.sort(list);` (Uses the helper class)
* New Way: `Comparator.comparing(...)` (Uses the interface directly)

3\. Why this is efficient:

* Interfaces cannot have constructors or static blocks.
* Static methods in interfaces are effectively just "global functions"
  namespaced under the interface name. They do not hold state, making them
  cheap in terms of memory and performance.

### Important Distinction: No Inheritance

Unlike `default` methods, static methods in interfaces are NOT inherited.

* You cannot call them on an instance variable (`obj.staticMethod()`).
* You cannot call them on a subclass (`ChildClass.staticMethod()`).
* You must call them using the specific interface name (`MyInterface.staticMethod()`).

Example:

```java
// 1. The Interface
interface UserValidator {
    
    // Abstract Method (Contract for the class to implement)
    boolean hasPermission(String username);

    // Static Utility Method (Helper Tool)
    // You can call this WITHOUT creating an instance of a class!
    static boolean isValidEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }
}

// 2. The Implementation Class
class EmployeeValidator implements UserValidator {
    
    @Override
    public boolean hasPermission(String username) {
        // Simple logic for demo
        return "admin".equals(username);
    }
}

// 3. The Main Class
public class Main {
    public static void main(String[] args) {
        
        // --- USAGE 1: Using the Static Method ---
        // NOTICE: We call it directly on the Interface Name.
        // We do NOT need to create an object.
        boolean isEmailValid = UserValidator.isValidEmail("test@example.com");
        System.out.println("Is Email Valid? " + isEmailValid);


        // --- USAGE 2: Using the Abstract Method ---
        EmployeeValidator emp = new EmployeeValidator();
        System.out.println("Is Admin? " + emp.hasPermission("admin"));


        // --- ❌ COMMON MISTAKE (Will Not Compile) ---
        // Static methods are NOT inherited by the implementing class.
        // emp.isValidEmail("test@example.com");  // ERROR
        // EmployeeValidator.isValidEmail("..."); // ERROR
    }
}
```


-----------------------------


## Q - What is Predicate joining?

Predicate Joining (often called Predicate Chaining) is a technique in Java 8 used
to combine multiple Predicate conditions into a single, complex logical test.

It allows you to take small, simple logic units and "glue" them together using
logical operators like `AND`, `OR`, and `NOT`.

### How it works

The Predicate interface contains default methods that let you join them:

* `p1.and(p2)` - Returns a predicate that is true only if both are true.
* `p1.or(p2)` - Returns a predicate that is true if either is true.
* `p1.negate()` - Returns the opposite (inverse) of the predicate.

### Code Example

Imagine you want to filter a list of names. You have two rules:

* Name must be longer than 3 characters.
* Name must start with "A".

Instead of writing one giant if statement, you can define them separately and join them.

Example:

```java
import java.util.function.Predicate;

public class PredicateJoinExample {
    public static void main(String[] args) {
        
        // 1. Define Simple Predicates
        Predicate<String> isLongEnough = s -> s.length() > 3;
        Predicate<String> startsWithA  = s -> s.startsWith("A");

        // 2. JOIN them using .and()
        // Logic: (length > 3) && (startsWith "A")
        Predicate<String> validName = isLongEnough.and(startsWithA);

        // 3. JOIN them using .or()
        // Logic: (length > 3) || (startsWith "A")
        Predicate<String> looseRule = isLongEnough.or(startsWithA);

        // 4. Test it
        System.out.println(validName.test("Anna")); // True (Matches both)
        System.out.println(validName.test("Bob"));  // False (Too short, no 'A')
        
        // 5. Negate (Reverse)
        // Logic: !(length > 3)
        Predicate<String> isShort = isLongEnough.negate();
        System.out.println(isShort.test("Bob"));    // True
    }
}
```


-----------------------------



## Q - What is Functional joining?

Functional Joining (or Function Chaining) is a feature of the `Function<T, R>` interface
in Java 8. It allows you to combine multiple functions into a single processing pipeline.

This is widely used to create complex data transformations from small, reusable steps.

### The Methods

There are two default methods used for chaining:

* `andThen(after)`: Runs the current function first, and then uses its result as input for the next function.
* `compose(before)`: Runs the other function first, and then uses its result as input for the current function.

### Code Example

Imagine a data pipeline: Input → Multiply by 2 → Add 10 → Result.

Example:

```java
import java.util.function.Function;

public class FunctionJoinExample {
    public static void main(String[] args) {

        // 1. Define separate, simple functions
        Function<Integer, Integer> multiplyBy2 = i -> i * 2;
        Function<Integer, Integer> addTen      = i -> i + 10;

        // 2. JOIN using .andThen() (Standard Chaining)
        // Order: multiplyBy2 runs FIRST -> addTen runs SECOND
        // Input 5: (5 * 2 = 10) -> (10 + 10 = 20)
        Function<Integer, Integer> pipeline = multiplyBy2.andThen(addTen);
        
        System.out.println("andThen Result: " + pipeline.apply(5)); // Output: 20


        // 3. JOIN using .compose() (Reverse Chaining)
        // Order: addTen runs FIRST -> multiplyBy2 runs SECOND
        // Input 5: (5 + 10 = 15) -> (15 * 2 = 30)
        Function<Integer, Integer> reversePipeline = multiplyBy2.compose(addTen);
        
        System.out.println("compose Result: " + reversePipeline.apply(5)); // Output: 30
    }
}
```

### Visualizing andThen vs compose

| Method  | 	Syntax         | 	Execution Order | 	Math Equivalent |
|---------|-----------------|------------------|------------------|
| andThen | 	f1.andThen(f2) | 	f1 → f2         | 	f2(f1(x))       |
| compose | 	f1.compose(f2) | 	f2 → f1         | 	f1(f2(x))       |


-----------------------------


## Q - What is Consumer chaining?

Consumer Chaining is the ability to combine multiple Consumer operations
so they run one after another on the same input.

Since a `Consumer` returns `void`, you cannot pass a result from one to the
next (like you do with `Function`). Instead, you use chaining to perform a
**sequence of independent side effects** (actions) on the same object.

The Method: `andThen()`

The `Consumer` interface has a default method called `andThen`.

* **Syntax:** `firstConsumer.andThen(secondConsumer)`
* **Behavior:** It runs the first consumer, then immediately runs the second consumer using the same input.

Example:

```java
import java.util.function.Consumer;

class Product {
    String name = "Phone";
    
    @Override
    public String toString() { return name; }
}

public class ConsumerChainExample {
    public static void main(String[] args) {
        
        // 1. Define the separate actions (Consumers)
        Consumer<Product> paintProduct = p -> {
            System.out.println("1. Painting " + p.name + " Black");
            p.name = "Black " + p.name; // Modifying the object
        };

        Consumer<Product> packageProduct = p -> {
            System.out.println("2. Packaging " + p.name + " into box");
        };

        Consumer<Product> shipProduct = p -> {
            System.out.println("3. Shipping " + p.name);
        };

        // 2. CHAIN them together
        // Order: Paint -> Package -> Ship
        Consumer<Product> assemblyLine = paintProduct
                                            .andThen(packageProduct)
                                            .andThen(shipProduct);

        // 3. Run the chain
        assemblyLine.accept(new Product());
    }
}
```

## Q - How to use chaining with Supplier?

Supplier can't be  chained as it takes no input.


-----------------------------


## Q - Difference between Optional.of() and Optional.ofNullable()?

The difference lies in how they handle `null` values.

The Short Answer:

* `Optional.of(value)`: Use this when you are 100% sure the value is NOT null. If it is null,
  it crashes immediately (NPE).
* `Optional.ofNullable(value)`: Use this when the value might be null. If it is null, it returns
  an empty Optional instead of crashing.

It feels redundant because `ofNullable()` handles everything, right?

But `Optional.of()` has a very specific purpose: Defensive Programming.

It is used to say: **"If this value is null, it is a BUG, not a valid state."**

1\. The "Silent Failure" Problem

If you always use `ofNullable()`, you might accidentally hide serious bugs.

**Imagine this scenario:** You are building a checkout system. You load a tax rate configuration from a file.
This configuration must exist for the app to work.

Using `ofNullable()` (Bad Logic):

```java
// Logic: Load tax rate. If config is missing (null), wrap it safely.
Optional<Double> taxRate = Optional.ofNullable(getTaxConfig()); 

// Later in code...
double totalTax = price * taxRate.orElse(0.0); 

// RESULT: The customer pays $0 tax. No error is thrown. 
// You lose money, and you don't know why.
```

2\. The `Optional.of()` Solution (Fail Fast)

If you use `Optional.of()`, you force the program to crash immediately at the source of the error,
rather than letting a `null` flow through your system as an "Empty Optional" and causing weird logic errors later.

Using `Optional.of()` (Good Logic):

```java
// Logic: This MUST exist. If it's null, crash NOW so I can fix the config.
Optional<Double> taxRate = Optional.of(getTaxConfig()); 

// RESULT: Immediate NullPointerException. 
// You see the log, realize the config file is missing, and fix it.
```

Summary: When to use what?

* `Optional.ofNullable()`: "I don't know if the user entered a middle name. If not, that's fine." (Valid business logic).
* `Optional.of()`: "I just created this object 2 lines ago. It SHOULD be there. If it's null, something is terrifyingly wrong." (Logic assertion).


----------------



# Level 7: JVM Architecture & Internals


## Q - What is JIT?

JIT (Just-In-Time) Compiler is a component of the JVM that optimizes performance. While the Interpreter 
executes bytecode line-by-line, the JIT identifies frequently used methods (Hotspots) and compiles them 
into native machine code on the fly. This allows Java to run nearly as fast as C++ after a warm-up period.


-----------------------------


## Q - What is class Loader?

* Part of the JVM that dynamically loads Java classes into the JVM memory (Metaspace).
* **Lazy Loading:** It does not load all classes at startup; it loads them only when the application needs them.
* **Delegation Hierarchy:** When asked to load a class, a ClassLoader first delegates the request to its Parent. 
  It only tries to load it itself if the Parent cannot find it.
* **Visibility:** A child ClassLoader can see classes loaded by the parent, but the parent cannot see classes 
  loaded by the child.

In simple terms: ClassLoader = Reads `.class` (bytecode) files and makes them usable by the JVM.


-----------------------------


## Q - What are different types of classloaders?


**Bootstrap ClassLoader**

* **What it loads:** The bare minimum to run Java. Specifically, the `java.base` module.
* **Classes:** `java.lang.String`, `java.util.List`, `java.lang.System` etc.

**Platform ClassLoader (Java 9+; replaces Extension loader)**

* Loads platform modules / non-core JDK modules (`java.sql`, `java.xml`, etc.).
* Parent is the Bootstrap loader.

**Application (System) ClassLoader**

* Loads classes from the application classpath (`-cp`, `CLASSPATH`, `target/classes`, or the module path when 
  running modular apps).
* Parent is Platform loader.

**Custom Class Loaders**

* Load encrypted classes
* Load classes from a database or network
* Hot-reload modules
* Plugin systems (e.g., IDEs, servers)


-----------------------------


## Q - What are the diff memory area allocated by JVM?

* Heap Space
    * **What:** Where all Objects live (e.g., `new Employee()`).
    * **Scope:** Shared by all threads (Global).
    * **Cleanup:** Managed by Garbage Collector.
* Stack
    * **What:** Stores method calls (Stack Frames), local variables, and partial results.
    * **Scope:** One per thread (Thread-safe).
    * **Cleanup:** Automatically cleaned when the method finishes.

* Method area (permgen/metaspace)
    * **What:** It stores:
        * Class metadata (structure of the class)
        * Method metadata
        * Field metadata (name, type, modifiers)
        * Method bytecode
        * Runtime constant pool
    * **Note:** In modern Java, this uses native memory (outside the Heap).

* PC Register (Program Counter)
    * **What:** Holds the address of the current instruction being executed.
    * **Analogy:** The "bookmark" telling the CPU which line to read next.

* Native Method Stack
    * **What:** Used for native code (C/C++ libraries) called via JNI.


-----------------------------


## Q - Can you explain the architectural change from PermGen to Metaspace in Java 8? Specifically, where are Class definitions and static variables stored in the modern memory model, and what happens at the OS and JVM level if Metaspace reaches its limit?

This is a classic "evolution of Java" interview question. To answer this effectively, we need
to look at how the JVM’s memory model changed significantly between Java 7 and Java 8.

Here is the ground-level breakdown of the transition from **PermGen** (Permanent Generation)
to **Metaspace**, and exactly where your data lives now.

---

### 1. The Old World: PermGen (Java 7 and older)

In older versions of Java, the JVM had a memory area called **PermGen**. It was a special part
of the Heap (technically separate, but contiguous) where the JVM stored "permanent" data that
the running program didn't modify often.

* **What lived there:** Class definitions (metadata), **Static variables**, and the String Constant Pool.
* **The Problem:** PermGen had a **fixed maximum size**. If you loaded too many classes or had
  too many huge static maps, you would crash with the infamous `java.lang.OutOfMemoryError: PermGen space`.
  Tuning this size (`-XX:MaxPermSize`) was a headache for developers.

### 2. The New World: Metaspace (Java 8+)

In Java 8, Oracle completely removed PermGen. It was replaced by a new memory area called **Metaspace**.

#### Key Difference: Location

* **PermGen** lived inside the JVM's pre-allocated memory.
* **Metaspace** lives in **Native Memory** (OS Memory). This means it is **not** part of the
  Java Heap. It is allocated directly from the RAM available on the server, outside the
  JVM's specific constraints.

### 3. Answering Your Specific Questions

#### "Where do Class Definitions live?"

**Answer: Metaspace (Native Memory).**
The metadata that describes a class (methods, bytecode, field descriptions) lives here.
Because Metaspace uses native memory, it can grow dynamically as long as the underlying OS has RAM available.

#### "Where do Static Variables live?"

**Answer: The Heap.**
This is a critical distinction and a common interview trap.

* In Java 7, static variables were in PermGen.
* **In Java 8+, Static Variables (and the String Pool) were moved to the main Heap.**
  * Specifically, they are stored within the `java.lang.Class` object representing that class,
    which resides in the Heap.
  * This allows the Garbage Collector to clean them up more easily if the ClassLoader dies.

---

### Comparison: PermGen vs. Metaspace

| Feature                | PermGen (Java 7)            | Metaspace (Java 8+)                       |
|------------------------|-----------------------------|-------------------------------------------|
| **Location**           | Inside JVM Memory           | **Native Memory** (OS RAM)                |
| **Size**               | Fixed (limited by defaults) | Dynamic (Auto-grows)                      |
| **Class Metadata**     | Stored here                 | Stored here                               |
| **Static Variables**   | Stored here                 | **Moved to Heap**                         |
| **String Pool**        | Stored here (mostly)        | **Moved to Heap**                         |
| **Garbage Collection** | Special/Inefficient         | Triggered when usage hits high-water mark |

---

### 4. What happens if Metaspace fills up?

Even though Metaspace is "dynamic," it is not infinite. Here is the chain of events:

1. **Default Behavior:** If you do not set a limit, Metaspace will grow and consume as much of your
   system's physical RAM as it needs to store class metadata. This can technically crash
   the *entire OS* if it eats all the RAM.
2. **Setting a Limit:** Most production environments set a cap using the flag `-XX:MaxMetaspaceSize`.
3. **The "High-Water Mark":** When Metaspace usage grows near the limit, the JVM
   triggers a **Garbage Collection (GC)**. It attempts to unload unused classes and
   their ClassLoaders to free up space.
4. **The Crash:** If the GC runs and *cannot* free up enough space for new class metadata, the JVM throws:
   `java.lang.OutOfMemoryError: Metaspace`

#### Common Causes of Metaspace OOM:

* **Leaking ClassLoaders:** Frequent hot-deployments in servers (like Tomcat) where old
  versions of the application (and their classes) aren't fully unloaded.
* **Dynamic Class Generation:** Frameworks like Hibernate, Spring, or Mockito generate proxy
  classes on the fly. If they generate too many without cleaning up, Metaspace fills up.

#### Summary for the Interview

* **PermGen** is gone.
* **Class Metadata** is in **Metaspace** (Native Memory).
* **Static Variables** are in the **Heap**.
* If Metaspace fills (hits the `MaxMetaspaceSize` cap), the JVM tries to GC dead classes.
  If it fails, you get an OOM Error.


-------------------


## Q - Explain JVM Architecture?

![jvm-architecture](../images/jvm-architecture.png)
<br>
SRC: https://www.geeksforgeeks.org/java/how-jvm-works-jvm-architecture/

Think of the JVM as a factory that takes Java bytecode and safely runs it on your computer.

### 1. JVM Language Class (.class file)

**What it is (ELI5)**

This is the instruction manual written in a language the JVM understands (bytecode).

* You write Java
* Compiler converts it to `.class`
* JVM reads this file


### 2. Class Loader — “The Librarian”

**What it does**

The **Class Loader**:

* Finds `.class` files
* Loads them into JVM memory
* Verifies they are safe
* Links them for execution

**ELI5**

📚 A librarian who fetches books before reading starts.


**Important detail (simple)**

There are multiple loaders, but conceptually:

* Bootstrap → core Java (String, Object)
* Application → your code

### 3. JVM Memory (Big Box in Diagram)

#### 3.1 Method Area — "Class Blueprint Shelf"

**What it stores**

* Class structure
* Method code
* Static variables
* Constant pool


**ELI5**

🏗️ Blueprints of buildings, not the buildings themselves.


**Key points**

* Shared by all threads
* One copy per class
* In Java 8+, implemented as **Metaspace**

---

#### 3.2 Heap — "Big Toy Box"

**What it stores**

* Objects created using `new`
* Arrays

**ELI5**

🧸 All toys go into one big box everyone can access.

**Key points**

* Shared by all threads
* Garbage Collector cleans it
* Largest memory area

---

#### 3.3 Stack — "Each Thread's Notebook"

**What it stores**

* Method calls
* Local variables
* Partial results

**ELI5**

📓 Each thread gets its own notebook.


**Key points**

* One stack per thread
* Very fast
* Automatically cleaned

---

#### 3.4 PC Register — "Bookmark"

**What it stores**

* Address of the next instruction to execute

**ELI5**

🔖 A bookmark telling JVM where you stopped reading.

**Key points**

* One per thread
* Very small
* Crucial for multithreading


---

#### 3.5 Native Method Stack — "Foreign Language Notes"

**What it stores**

* Calls to non-Java code (C/C++)

**ELI5**

🗒️ Notes written in another language.


### 4. Execution Engine — "The Brain"

This is where code actually runs.

####  4.1 Interpreter — "Reads Slowly"
What it does

* Reads bytecode line by line
* Executes immediately

**ELI5**

👶 Reads instructions one step at a time.

**Pros / Cons**

* ✔ Fast startup
* ❌ Slower execution

---

#### 4.2 JIT Compiler — "Learns and Gets Faster"

**What it does**

* Detects frequently used code
* Converts it to machine code
* Optimizes execution

**ELI5**

🧠 Memorizes common steps to move faster next time.

**Result**

🔥 Java programs get faster as they run.

---

#### 4.3 Garbage Collector — "Cleaner"

**What it does**

* Finds unused objects
* Frees heap memory

**ELI5**

🧹 Cleans toys no one is playing with.


### 5. Native Method Interface (JNI) — "Translator"

**What it does**

* Connects Java code to native libraries

**ELI5**

🌍 A translator between Java and C/C++.


### 6 Native Method Libraries — "External Helpers"

**What they are**

* OS-level libraries
* Written in C/C++

**ELI5**

🧰 Outside helpers that Java can call when needed.


### How Everything Works Together (Story)

1. `.class` file is given to JVM
2. **Class Loader** loads it
3. Code & metadata go to **Method Area**
4. Objects go to **Heap**
5. Method calls go to **Stack**
6. **Execution Engine** runs the code
7. **Garbage Collector** cleans memory
8. Program finishes 🎉


-----------------------------



## Q - Explain the JVM heap structure shown in this diagram and describe the role of each memory region.

```text
Heap
 ├── Young Generation
 │    ├── Eden
 │    ├── Survivor S0
 │    └── Survivor S1
 └── Old Generation
```

Below is an ELI5, step-by-step explanation of each heap area, using a single simple story so the behavior is
intuitive rather than abstract.

### Big Picture (ELI5)

Imagine the JVM heap as a **school system** for objects.

* **Young Generation** = Kindergarten + Primary school
* **Old Generation** = College / Working professionals

---

* Objects start young.
* Most don't live long.
* Only the survivors grow old.

### 1. Young Generation

This is where all new objects are born.

#### 1.1 Eden Space (Birthplace)

**What it is (ELI5):**

Eden is the nursery. Every new baby object is born here.

**Example:**

```java
User u = new User();
Order o = new Order();
```

Both `u` and `o` are created in Eden.

**What usually happens:**

* Eden fills up very fast
* JVM says: "Let me clean this place"

This triggers **Minor GC**.

**Reality check:**

* Most objects die here
* They are never copied anywhere else

**ELI5 analogy:**

Most toys kids ask for are forgotten in 5 minutes.

---

#### 1.2 Survivor Space S0 (First Survival Test)

**What it is:**

Objects that **did not die** during Minor GC are moved here.

**Example:**

```java
List<String> cache = new ArrayList<>();
```

If `cache` is still referenced after GC:

* It survives
* It moves from **Eden → S0**

**Important rule:**

* Eden is emptied completely after Minor GC
* Only alive objects are copied

**ELI5 analogy:**

Kids who didn’t quit school move to Grade 1.

---

#### 1.3 Survivor Space S1 (Second Survival Test)

**What it is:**
Objects keep hopping between S0 and S1 while aging.

**How it works:**

* Next Minor GC happens
* Objects in S0 that are still alive → moved to S1
* Their **age increases by 1**

**Age example:**

```text
GC #1 → age = 1
GC #2 → age = 2
GC #3 → age = 3
```

**ELI5 analogy:**

Students move class to class every year.


### 2. Promotion to Old Generation

**When does promotion happen?**

An object is promoted when:

* It reaches a certain age (default ~15 GCs), OR
* Survivor space is full, OR
* Object is too large

**Example:**

```java
static Map<String, Config> appConfig;
```

This object:

* Lives for the entire application lifetime
* Quickly promoted to Old Generation

**ELI5 analogy:**

Student graduates and starts working.


### 3. Old Generation (Tenured)

**What it is:**

Home for **long-living objects**.

**Typical objects here:**

* Singletons
* Caches
* Session data
* Large collections

**GC behavior:**

* Collected by Major GC / Full GC
* Happens rarely
* Much slower and more expensive

**ELI5 analogy:**

Adults change houses rarely—but moving is painful.


### 4. Why Two Survivor Spaces?

**Simple reason:**

To avoid fragmentation and keep copying clean.

**Rule:**

* JVM always copies from **one survivor → the other**
* One is empty at any time

**ELI5 analogy:**

You move students from Classroom A to Classroom B every year, never mixing old desks.


### 5. End-to-End Example Flow

```java
public void process() {
    Order o = new Order();          // Eden
    List<Item> items = new ArrayList<>(); // Eden
}
```

**Step-by-step:**

1. Objects created → **Eden**
2. Eden fills → Minor GC
3. `Order` still referenced → moved to `S0`
4. Next GC → `Order` moves to S1, age++
5. After many GCs → `Order` promoted to **Old Gen**
6. If reference removed → collected in Major GC


### Resources

* [Garbage collection in Java, with Animation and discussion of G1 GC](https://www.youtube.com/watch?v=UnaNQgzw4zY)


-----------------------------


## Q - Explain WeakHashMap

This is a great specific question. `WeakHashMap` is the "magic self-cleaning map."

To understand it, let's look at the **Memory Leak** problem it solves.

### The Problem: The "Sticky" Metadata

Imagine you are using a third-party library that gives you `Socket` objects.
You want to associate some metadata (like a "User ID") with each socket, but you
cannot modify the `Socket` class itself.

**The Naive Approach (Standard HashMap):**

```java
// Strong Reference Map
Map<Socket, String> metadata = new HashMap<>();

// You store metadata
metadata.put(clientSocket, "User-123");
```

**The Leak:**

1. The connection closes.
2. Your application stops using `clientSocket` (sets it to `null`).
3. **Garbage Collector runs:** It wants to delete the `Socket` object.
4. **BUT IT CAN'T.** Why? Because your `metadata` HashMap is still holding
   a **Strong Reference** to that `Socket` as a key.
5. **Result:** The `Socket` stays in memory forever (Memory Leak).

---

### The Solution: `WeakHashMap`

`WeakHashMap` wraps the **Key** (the Socket) in a `WeakReference`.

```java
// Weak Reference Map
Map<Socket, String> metadata = new WeakHashMap<>();

metadata.put(clientSocket, "User-123");
```

**The Magic:**

1. The connection closes.
2. Your application stops using `clientSocket` (removes the strong reference).
3. **Garbage Collector runs:** It sees the `Socket` is *only* held by the `WeakHashMap`.
4. **GC Action:** Since it's a `WeakReference`, the GC says "I don't care about this map
   entry," and **deletes the Socket object**.
5. **Cleanup:** The `WeakHashMap` notices the key is gone and automatically removes the entire
   entry (Key & Value) from the map.

### When to use this? (The "Metadata" Use Case)

You use `WeakHashMap` when you want to attach extra information to an
object, but **the lifespan of that information should be tied to the lifespan of the object itself.**

* **Example 1: Caching Expensive Computations:**
  `WeakHashMap<BigImage, Thumbnail>`
  If the `BigImage` is no longer used by the app, we don't need the `Thumbnail` anymore. Let them both disappear.
* **Example 2: ThreadLocal Storage:**
  Internally, `ThreadLocal` uses a similar weak-reference mechanism so that when a Thread dies, its
  local variables are cleaned up.


-----------------------------


## Q - Explain SoftReference

This is the **"Smart Cache"** reference.

If `StrongReference` is "Do not delete this under any circumstances," and `WeakReference` is
"Delete this as soon as you see it," then **`SoftReference`** is:

> **Keep this in memory as long as you can. But if you are about to run
> out of RAM (throw an OutOfMemoryError), then delete this first to save the application.**

### 1. The Use Case: Building an In-Memory Cache

Imagine you are building a **Photo Gallery App** (like Google Photos).

* **Problem:** Loading a 10MB image from the hard drive takes 500ms. It's slow.
* **Goal:** You want to keep the decoded images in RAM so scrolling is instant.
* **Risk:** If you keep *every* image the user scrolls past, you will run out of RAM in 2 minutes and crash.

**The Solution: SoftReference**
You wrap your heavy images in `SoftReference`.

```java
// Strong Reference (The "Cache Map" itself)
Map<String, SoftReference<Image>> cache = new HashMap<>();

// 1. Wrap the heavy object
Image bigImage = loadFromDisk("vacation.jpg");
SoftReference<Image> softRef = new SoftReference<>(bigImage);

// 2. Store it
cache.put("vacation.jpg", softRef);

// 3. REMOVE the strong reference (Critical!)
bigImage = null; 
// Now, the ONLY thing holding the image is the SoftReference.
```

### 2. The Mechanics (Ground Level)

How does the Garbage Collector (GC) decide when to clear it?

It uses a formula based on **Free Memory** vs. **Time Since Last Access**.

* **Scenario A (Plenty of RAM):**
  The GC runs. It sees your `SoftReference`. It checks free memory. "Oh, we have 2GB free. No need to panic."
  * **Result:** The image stays in memory.
  * **User Benefit:** When they scroll back, the image loads instantly.


* **Scenario B (Low RAM):**
  The user opens a 4K video editor in the background. Free memory drops to 10MB. The GC runs and
  panics. "I need to allocate memory for this new video, but I'm full!"
  * **Action:** The GC looks for `SoftReference` objects. It effectively says, *"Sorry, I need this space."*
  * **Result:** It clears the reference (sets it to null) and reclaims the 10MB image memory. The app **does not crash**.



### 3. The Coding Pattern (The "Check-Check-Reload")

Because a `SoftReference` can disappear at any moment, you **must** check if it's still there before using it.

```java
public Image getImage(String key) {
    // 1. Get the wrapper
    SoftReference<Image> ref = cache.get(key);
    
    // 2. Try to get the real object
    Image img = (ref != null) ? ref.get() : null;

    // 3. CHECK: Did the GC delete it?
    if (img == null) {
        // Yes, it was cleared to save memory.
        // We must reload it from disk (slower, but safe).
        img = loadFromDisk(key);
        
        // Put it back in the cache
        cache.put(key, new SoftReference<>(img));
    }
    
    return img;
}

```

### Summary

* **Strong:** "I need this. Keep it or crash."
* **Soft:** "I'd *like* to keep this (Cache). But delete it if you need space."
* **Weak:** "I only care about this if someone else does (Metadata)."


-----------------------------



# Level 8: Garbage Collection & Performance Tuning


## Q - How to manually trigger the garbage collection process?

Call `System.gc()`


----------------


## Q - Explain Minor GC vs Major GC vs Full GC

First: one mental picture (lock this in). Think of the Heap as a house:

```text
House (Heap)
 ├── Kids Room (Young Generation)
 └── Storage Room (Old Generation)
```

Garbage Collection is **cleaning**.

### 1. Minor GC — "Clean the kids' room"

**What it is (ELI5)**

> Minor GC cleans only the Young Generation.

That means:

* Eden
* Survivor spaces (S0, S1)

It **does NOT touch Old Generation**.

#### When does Minor GC happen?

When Eden gets full.

**Code example**

```java
public static void main(String[] args) {
    while (true) {
        new Object(); // lots of short-lived objects
    }
}
```

What happens?

* Objects keep filling Eden
* Eden fills up
* JVM says: "Time to clean kids' room"

👉 Minor GC happens

#### What does Minor GC actually do?

Step by step:

1. Stop-the-world (very short)
2. JVM starts from GC roots
3. Live objects copied:
    * Eden → Survivor
4. Dead objects are ignored
5. Eden is cleared


#### Why Minor GC is fast

* Young Gen is small
* Most objects are dead
* Copying few live objects is cheap

#### Interview line (memorize)

> Minor GC collects only the Young Generation and is fast because most objects die young.
> 
> 

### 2. Major GC — "Clean the storage room"

**What it is (ELI5)**

> Major GC cleans the Old Generation.

This means:

* Long-lived objects
* Caches
* Large objects

#### When does Major GC happen?

When:

* Old Gen fills up
* Promotion from Young → Old fails

**Code example**

```java
static List<byte[]> cache = new ArrayList<>();

public static void main(String[] args) {
    while (true) {
        cache.add(new byte[1_000_000]); // long-lived objects
    }
}
```

**What happens?**

* Objects go to Old Gen
* Old Gen fills up
* JVM says: "Need to clean storage room"

👉 Major GC happens


#### Why Major GC is slower

* Old Gen is large
* Objects live longer
* More references to traverse


### Important interview clarification

⚠️ Major GC ≠ Full GC (always)


### 3. Full GC — "Clean the entire house"

**What it is (ELI5)**

> Full GC cleans EVERYTHING.

It includes:

* Young Generation
* Old Generation
* Metaspace (class metadata)


#### When does Full GC happen?

Common causes:

* **Promotion failure** 
    
    Surviving objects cannot be promoted to Old Generation, and Old Gen cleanup (Major GC / Mixed GC) fails to free enough space.

* **Allocation failure**
    
    Eden allocation fails → Young GC runs → promotion pressure occurs → Old Gen cleanup fails → then Full GC is triggered. <br> 
    ⚠️ Allocation failure by itself does NOT immediately cause Full GC.

* **Metaspace full**

    JVM cannot allocate class metadata; triggers Full GC to attempt class unloading.

* **Explicit System.gc() (sometimes)**

    Depends on GC and JVM flags; may trigger Full GC or concurrent GC.

**Code example**

```java
static List<byte[]> cache = new ArrayList<>();

public static void main(String[] args) {
    while (true) {
        cache.add(new byte[1_000_000]); // long-lived objects
    }
}
```

**What happens?**

* Young fills → Minor GC
* Promotion fails
* Old fills
* JVM panics: “Clean EVERYTHING”

👉 Full GC

#### Why Full GC is dangerous

* Long Stop-the-World
* Application freezes
* SLA violations


#### Interview killer line

> Full GC pauses the entire application and should be avoided in latency-sensitive systems.
>

### Side-by-side comparison (ELI5)

| GC Type  | Cleans      | Speed     | STW     | Risk   |
|----------|-------------|-----------|---------|--------|
| Minor GC | Young Gen   | Fast      | Short   | Low    |
| Major GC | Old Gen     | Slow      | Longer  | Medium |
| Full GC  | Entire Heap | Very Slow | Longest | High   |


## Q - What is Stop-The-World(STW) problem?

**What STW really means (no jargon)**
> STW means: the JVM temporarily pauses ALL your application code so it can safely check memory.
>

That's it.

### Why does JVM need STW at all?

Because **your program is changing memory constantly**.

Imagine GC running while this happens:

```text
obj1.ref = obj2;
obj1.ref = null;
```

GC would get **wrong answers** if memory keeps changing.

So JVM says:
> "Everyone stop touching memory for a moment."
>

### What exactly is stopped?

* Your main() logic
* Your web requests
* Your background threads
* Everything except GC threads


### Tiny code example

```java
public static void main(String[] args) {
    while (true) {
        new Object();
    }
}
```

When Eden fills:

* JVM pauses this loop
* Runs GC
* Resumes loop

That pause is **STW**.


### ELI5 analogy

🧹 Cleaning a room

* Kids running around → chaos
* Ask kids to stop → clean safely
* Kids resume play

STW = "Everyone freeze for 5 ms"


### Important truth (interview gold)

> STW is unavoidable, but modern GC tries to make it short.

* Minor GC → short STW
* Full GC → long STW (bad)


## Q - What is Allocation Failure?

What it means (plain English)

> JVM tried to create a new object, but there was no space in Young Generation (Eden).
>

So JVM says:

> "I can't allocate memory. I must run minor GC."
>

That situation is called **Allocation Failure**.

### Simple code example

```java
public static void main(String[] args) {
    while (true) {
        new Object(); // keep allocating
    }
}
```

### Step-by-step what JVM does

**Step 1: Object creation**
* `new Object()` goes to Eden

**Step 2: Eden fills up**
* No space left

**Step 3: Allocation Failure occurs**
* JVM cannot allocate new object

**Step 4: JVM runs Minor GC**
* Tries to free space in Young Gen

👉 This is normal and expected


## Q - What is Promotion Failure?

JVM tried to move surviving objects from Young Gen to Old Gen, but Old Gen had no space. 
That's a Promotion Failure.

**Promotion Failure can lead to Full GC**. Consider the following example:

```java
static List<Object> list = new ArrayList<>();

public static void main(String[] args) {
    while (true) {
        list.add(new Object());
    }
}
```

Key facts:

* list is static → GC Root
* Every object added is long-lived
* Nothing ever becomes unreachable


### Step 1: Objects are created in Eden

Each iteration:

```java
new Object();
```


JVM does:
* Allocate object in Eden
* Eden fills quickly


### Step 2: Eden becomes full → Allocation Failure

JVM tries to allocate a new object but:

```text
Eden: ❌ no space
```

This is an **Allocation Failure**.

So JVM says:
> "Let me run a Minor GC."



### Step 3: Minor GC starts (STW)

During Minor GC, JVM does:

1. Stop-the-world
2. Find GC roots
    * list (static)
3. Traverse references

What JVM discovers:

```text
list → object1
list → object2
list → object3
...
```

👉 Every object is reachable <br>
👉 Nothing is dead


### Step 4: JVM tries to evacuate live objects

Minor GC uses copying.

JVM tries to move live objects:

```text
Eden → Survivor (S0 / S1)
```

But here’s the problem 👇

* Too many objects survived
* Survivor space is small by design

So JVM says:
> "Survivor is too small — I must promote objects to Old Gen."
>


### Step 5: JVM attempts promotion

Promotion means:

```text
Young Gen → Old Gen
```

JVM checks:

```text
Old Gen free space ?
```

Two possibilities:


**Case A: Old Gen has space ✅ (normal)**

* Objects are promoted
* Eden is cleared
* Program continues

No problem.


**Case B: Old Gen does NOT have space ❌**

Why Old Gen is full:

* Objects have been accumulating for a long time
* list keeps references forever
* Nothing ever got deleted

So JVM sees:

```text
Old Gen: ❌ insufficient space
```

### Step 6: Promotion Failure occurs (THIS IS THE MOMENT)

> Promotion Failure = JVM tried to move surviving Young objects to Old Gen, but Old Gen had no space
>

This is a hard failure.

At this exact point:

* Young GC could not free space
* Promotion could not happen

JVM has **no safe place** to put live objects.


### Step 7: Old Generation cleanup attempt

Because the prior Young/Minor GC and promotion attempt did not reclaim enough memory to accommodate 
surviving objects, the JVM detects Old Generation pressure and attempts to free space in the Old Generation.

* **Classic collectors (Serial / Parallel GC)**
 
    → Run a Major GC to clean the Old Generation.

* **G1 GC**
 
    → Run one or more Mixed GCs, collecting Young regions along with selected Old regions that contain a 
    high amount of garbage.


### Step 8: JVM escalates → Full GC

Only if Major GC / Mixed GC cannot free enough space, then:

JVM now says:

> "I've tried everything cheap.
I must clean the entire heap."

So it triggers:
👉 Full GC

Full GC:

* Scans Young Gen
* Scans Old Gen
* Tries to compact
* Tries to free anything


### Step 8: Why Full GC still fails here

In your code:

* list still holds references
* All objects are still reachable
* Nothing can be freed

So after Full GC:

* Still no space
* JVM throws:
    ```text
    OutOfMemoryError: Java heap space
    ```

### One-sentence interview answer

> Promotion failure happens when a Minor GC cannot free enough space because surviving objects need to be 
> promoted, but the Old Generation does not have sufficient free space, forcing a Full GC.
>



----------------



## Q - Explain working of GC Roots?

GC Roots are the starting points from which the Garbage Collector decides what is alive.

If an object is reachable from a GC Root → it is alive. 

If not → it is garbage.

The 3 main GC Roots:

1. All live thread stacks
2. Classes (specifically those loaded by the System ClassLoader, which hold the static fields)
3. JNI / native references


**Goal (what we are showing)**

We want to see:

* Where GC roots come from
* How GC starts from them
* How it walks references
* How it decides what stays and what goes


### Example 1: Single thread, single object

Code

```java
public class Demo {
    public static void main(String[] args) {
        Object o = new Object();
        // GC could run here
    }
}
```

#### What memory looks like while main is running

**Thread stack (main thread)**

```text
Stack (main thread)
-------------------
o  ───► Object@1
```

**Heap**

```text
Heap
----
Object@1
```

#### How GC works here (step by step)

1. JVM pauses the program
2. JVM looks at active threads
3. Finds the main thread
4. Looks at its stack
5. Sees variable `o`
6. Follows `o` to `Object@1`
7. Marks `Object@1` as alive

That's it.

Nothing else is checked.

---

### Example 2: Multiple method calls (stack frames)

Code

```java
public class Demo {
    public static void main(String[] args) {
        foo();
    }

    static void foo() {
        bar();
    }

    static void bar() {
        Object o = new Object();
        // GC could run here
    }
}
```

**What the stack looks like**

```text
Stack (main thread)
-------------------
bar()
  o  ───► Object@1
foo()
main()
```

#### GC root traversal

GC does this:

1. JVM pauses program
2. JVM finds the main thread
3. JVM walks every stack frame
   * `bar()` → sees `o`
   * `foo()` → nothing
   * `main()` → nothing

4. Follows `o` to `Object@1`
5. Marks it alive

**Yes — GC goes all the way down the stack**, frame by frame.

---

### Example 3: When stack root disappears

Code

```java
static void foo() {
    Object o = new Object();
}

public static void main(String[] args) {
    foo();
    // GC could run here
}
```

**After foo() returns**

```text
Stack (main thread)
-------------------
main()
```


**Heap**

```text
Object@1
```

#### GC traversal now

1. JVM pauses program
2. JVM scans thread stack
3. No reference to `Object@1`
4. No other roots exist
5. `Object@1` is not reachable
6. It is garbage

---


### Example 4: Static variable (class root)

Code

```java
class Store {
    static Object shared;
}

public class Demo {
    public static void main(String[] args) {
        Store.shared = new Object();
        // GC could run here
    }
}
```

**Memory - Class area (static data)**

```text
Store.shared ───► Object@2
```

**Heap**

```text
Object@2
```

#### GC traversal

1. JVM pauses program
2. JVM scans thread stacks
3. JVM scans static variables
4. Finds `Store.shared`
5. Follows it to `Object@2`
6. Marks `Object@2` alive

Even if no thread variable points to it, it stays.

---


### Example 5: Multiple threads

Code

```java
public class Demo {
    public static void main(String[] args) {
        new Thread(() -> {
            Object a = new Object();
            sleep();
        }).start();

        new Thread(() -> {
            Object b = new Object();
            sleep();
        }).start();
    }
}
```

**Memory**

```text
Thread-1 stack
--------------
a ───► Object@A

Thread-2 stack
--------------
b ───► Object@B
```

#### GC traversal

GC does:

1. Pause everything
2. Scan Thread-1 stack
    * finds `a` → Object@A
3. Scan Thread-2 stack
    * finds `b` → Object@B
4. Both objects are alive

If a thread ends, its stack disappears, and so do its roots.

---

### Example 6: Following references (walking the graph)

Code

```java
public class Demo {
    public static void main(String[] args) {
        Object a = new Object();
        Object b = new Object();
        a = b;
        // GC could run here
    }
}
```

**Memory**

```text
Stack
-----
a ───► Object@B
b ───► Object@B

Heap
----
Object@A   (no references)
Object@B
```

#### GC traversal

1. Start from stack
2. Follow `a` → Object@B
3. Follow `b` → Object@B
4. Object@B is alive
5. Object@A is unreachable → garbage

GC does not care that Object@A was created first.

### The single rule GC follows (memorize this)

> GC starts from known references and follows pointers.
Anything it can reach stays.
Anything it cannot reach goes.

That's all.



----------------



## Q - Explain the working of G1 Garbage Collector

### What is G1 GC?

G1 GC divides the heap into small regions and incrementally cleans the most garbage-heavy regions to avoid long pauses.

Instead of this (old collectors):

```text
[ Young Gen ][ Old Gen ]
```

G1 uses this:

```text
[ R1 ][ R2 ][ R3 ][ R4 ][ R5 ] ...
```

Each region:

* Same size (1–32 MB)
* Can act as Eden, Survivor, Old, or Free
* Role can change over time

Important:

> Young / Old are logical roles, not fixed memory areas.
> 


### Code example (we will use this throughout)

```java
class Demo {
    static Object root; // static → GC root

    public static void main(String[] args) {
        Object a = new Object(); // A
        Object b = new Object(); // B
        Object c = new Object(); // C
        Object d = new Object(); // D

        root = a;
        a = b;
    }
}
```

### Phase 1: Object allocation (NO GC yet)

Assume objects land like this:

```text
R1: A
R3: B, C, D
R2: empty
R4: empty
```

References created by code:

```text
GC Root → A (R1)
A (R1) → B (R3)
```

### Phase 2: Remembered Set creation (during normal execution)

When this line runs:

```java
a = b;
```

The JVM notices:

```text
Reference from R1 → R3
```

So it records:

```text
Remembered Set of R3:
  ← R1
```

Key point:

> Remembered sets are created during normal execution, not during GC.


### Phase 3: GC starts (Stop-the-World)

GC always does two distinct jobs:

1. Decide which objects are alive
2. Decide which regions to clean


#### Step 1: GC Root traversal (liveness)

GC looks ONLY at GC roots:

* Static fields
* Thread stacks

Here:

```text
root → A
A → B
```

Alive objects:

```text
A (R1)
B (R3)
```

Dead objects:

```text
C (R3)
D (R3)
```

#### Step 2: Region accounting (THIS IS CRITICAL)

GC now summarizes per region.

**R1**

* Contains: A
* Alive: A

```text
R1: 100% alive
```

**R2**

* Contains: nothing

```text
R2: empty (ignored)
```

**R3**

* Contains: B, C, D
* Alive: B
* Dead: C, D

```text
R3: 33% alive, 67% garbage
```

**R4**

* Contains: nothing

```text
R4: empty (ignored)
```

This accounting is **not guessed** - it comes directly from marking.


#### Step 3: Region selection (why G1 is called "Garbage First")

GC asks:

> "Which region gives me the most memory back for the least work?"

Comparison:

```text
R1 → 0% garbage
R3 → ~67% garbage ✅
```

So:
> R3 is selected for cleanup
> 


#### Step 4: Safety check using remembered sets

Before deleting anything in R3, GC must ensure:

> "Is anything outside R3 still pointing into R3?"

GC looks at:

```text
Remembered Set of R3 → { R1 }
```

**So GC checks only R1, not the entire heap**.

GC finds:

```text
A (R1) → B (R3)
```

So:

* B must be kept
* C and D can be deleted


#### Step 5: Cleanup result

After cleanup:

```text
R3: B
```

Memory reclaimed safely.


### Now let's place this into the FULL G1 FLOW

#### Young GC (baseline behavior)

Trigger:

```text
Eden regions fill up
```

Action:

* Stop the world (short)
* Collect Young regions only
* Promote survivors to Old regions

This is equivalent to Minor GC.


#### When Old Gen pressure appears

Symptoms:

* Promotions increase
* Old regions accumulate
* JVM detects rising Old Gen usage

Young GC does not stop.

Instead, JVM does additional work:

* Clean selected Old regions (garbage-heavy ones)
* Use remembered sets to do this safely

This combined operation is called **Mixed GC**. 

```text
Mixed GC = Young GC + selected Old Gen cleanup
```

### Full escalation chain (memorize this)

```text
Eden fills
→ Young GC

Old Gen pressure
→ Mixed GC (Young GC + selected Old)

Mixed GC succeeds
→ continue

Mixed GC fails repeatedly
→ Full GC (last resort)

Full GC fails
→ OutOfMemoryError
```

### When does G1 move to Full GC?

> Only when repeated Mixed GCs fail to reclaim enough Old Gen space.

Common reasons:

* Almost all objects are alive
* Heavy static references
* Memory leak
* Extreme fragmentation


### Why classic collectors were slower

Serial / Parallel GC:

* Old Gen = one large block
* No regions
* No remembered sets

So:
> "To clean Old Gen, scan all of it."

Result:

* Long pauses
* Poor latency


### Final interview-ready summary (perfect answer)

> G1 divides the heap into regions, marks live objects starting from GC roots, computes garbage per region, and 
> performs Mixed GCs—Young GC plus selected garbage-heavy Old regions—using remembered sets for safety, escalating
> to Full GC only if Mixed GCs cannot reclaim enough space.
>


----------------


## Q - Java has automatic Garbage Collection, which is supposed to manage memory for us. However, Memory Leaks are still a very real problem in Java applications.

> Can you explain how a memory leak technically occurs in Java, even when the Garbage Collector
> is working perfectly? Please provide one specific coding example (a pattern) where this happens silently.
>

In C++, a memory leak means you forgot to call delete. In Java, a memory leak means 
you are holding onto an object you no longer need.

The Garbage Collector (GC) is smart, but it follows one strict rule: 
> "If a live object references it, I cannot touch it."
>

**The Classic Example: The Static Cache**

```java
public class LeakyApp {
    // This list lives forever because it is STATIC
    private static final List<Object> cache = new ArrayList<>();

    public void processData() {
        Object hugeData = new byte[10_000_000]; // 10MB
        cache.add(hugeData); // Added to cache
        
        // ... do work ...
        
        // BUG: We forgot to remove it!
        // The method ends, but 'cache' still holds the reference.
        // GC sees 'cache' is reachable, so it keeps 'hugeData' in memory forever.
    }
}
```

**Fix:** Use `WeakHashMap` or explicitly remove objects when done.



-----------------------------



## Q - Explain Serial GC

### 1. What is Serial GC?

**The Concept:**
Serial Garbage Collector is the simplest, oldest, and most basic implementation of garbage collection in Java.

* **"Serial"** means **sequential**. It does one thing at a time.
* It uses a **single thread** to handle all garbage collection tasks.
* It was the default collector in Java 5 and 6 for client-side machines (like desktops) because
  it assumes you don't have powerful multi-core CPUs.

---

### 2. How it Works: The "Stop-The-World" Event

This is the most critical concept to understand for Serial GC.

Imagine your Java application is a busy restaurant.

* **The Application Threads:** These are the chefs cooking food and serving customers.
* **The Garbage Collector:** This is the cleaner.

**In Serial GC:**

1. The restaurant gets messy (Heap fills up).
2. **Stop-The-World:** The manager blows a whistle. **EVERYONE STOPS.** The chefs stop cooking. 
   The waiters freeze. No one moves. The restaurant is effectively "dead" to the outside world.
3. **The Single Cleaner:** One cleaner walks in.
    * He goes to the kitchen (Young Gen) and cleans it.
    * He goes to the dining area (Old Gen) and cleans it.


4. **Resume:** The cleaner leaves. The manager blows the whistle again. Everyone starts moving 
   exactly where they left off.

**Technical Translation:**
When GC triggers, the JVM pauses **all** application threads. It spawns **one single GC thread**. 
That thread scans the heap, marks dead objects, sweeps them away, and compacts the memory. 
Only after it finishes do the application threads resume.

---

### 3. The Two Components (Young vs. Old)

Serial GC divides the Heap into two main physical areas (Generations). It handles them differently.

**Important:** Serial GC is not just one algorithm; it is a pair of collectors that work together.
When you enable `-XX:+UseSerialGC`, the JVM activates:

| Generation    | Component Name           | Algorithm                     |
|---------------|--------------------------|-------------------------------|
| **Young Gen** | **DefNew** (Default New) | **Serial Mark-Copy**          |
| **Old Gen**   | **TenuredGeneration**    | **Serial Mark-Sweep-Compact** |

#### A. Young Generation (DefNew)

* **What lives here:** Newly created objects (e.g., `new String("hello")`, `new Customer()`).
* **The Algorithm: Serial Mark-Copy**.
    * It reserves a separate "Survivor Space".
    * It pauses the app, finds the live objects in Eden, and copies them to the Survivor space.
    * It wipes the rest of Eden clean.

* **Why?** Most new objects die young (like temp variables in a loop). 
 Copying the few survivors is faster than scanning all the dead ones.

#### B. Old Generation (TenuredGeneration)

* **What lives here:** Objects that survived many Minor GCs (long-lived data like Caches, DB connections).
* **The Algorithm: Serial Mark-Sweep-Compact**.
    1. **Mark:** The single thread scans the whole Old Gen to find live objects.
    2. **Sweep:** It identifies the empty spaces between live objects.
    3. **Compact:** This is the heavy lifting. It moves live objects together to the beginning of 
    the memory block so that there is one large chunk of free space at the end.

* **Why compact?** To ensure we have a large contiguous chunk of free space for future allocations.

---

### 4. Pros and Cons (Interview Material)

| Feature             | Description                                                                                                                                  |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **Simplicity**      | No complex thread synchronization overhead. It is very efficient for small amounts of data.                                                  |
| **Low Overhead**    | It doesn't use extra CPU for managing multiple GC threads.                                                                                   |
| **The Dealbreaker** | **Long Pauses.** Because one thread does everything, if you have a large Heap (e.g., 2GB+), the "Stop-The-World" pause can last for seconds. |

### 5. When should you use it?

You might think "Never," but that's wrong. It is still useful in specific cases:

1. **Small Heaps:** If your heap is under 100MB (e.g., a tiny microservice or AWS Lambda function), 
   Serial GC is faster than G1GC because it lacks the "management overhead."
2. **Single Core Environments:** If your Docker container is limited to 1 CPU core, using 
   Parallel GC (multi-threaded) is useless because the OS has to "time-slice" the threads on 
   one core anyway, which is slower.

**Command to enable:** `-XX:+UseSerialGC`



----------------


## Q - Explain Parallel GC

This is the natural evolution of Serial GC. It was the default garbage collector 
for a long time (up until Java 9) because it solves the biggest problem of Serial GC: **Speed.**

### 1. The Core Concept: "Strength in Numbers"

If Serial GC is one janitor cleaning a messy building, **Parallel GC is a whole cleaning crew.**

* **Serial GC:** 1 CPU core doing all the work.
* **Parallel GC:** Uses **all available CPU cores** to perform the garbage collection.

**Key Technical Difference:**
It is still a "Stop-The-World" collector. Your application threads **still freeze** completely. 
However, because multiple threads are working together, the freeze time is much shorter.

---

### 2. How it Works (Under the Hood)

Parallel GC uses a "Divide and Conquer" approach. It splits the heap into smaller 
chunks and assigns them to different threads.

#### The "Stop-The-World" Sequence:

1. **Trigger:** The Heap (Young or Old) gets full.
2. **Pause:** The JVM pauses all application threads.
3. **Spawn Threads:** The JVM wakes up a team of GC threads (usually equal to the number of CPU cores you have).
4. **Parallel Mark:**
    * Thread A scans the top-left corner of the heap.
    * Thread B scans the top-right.
    * Thread C scans the bottom-left, etc.
    * They all identify live objects **simultaneously**.

5. **Parallel Compact (The Hard Part):**
   * They work together to slide live objects to the start of the memory block.
   * *Note:* This is complex because if GC Thread A moves an object, GC Thread B needs to know 
    where it went to update references. This requires some synchronization overhead, but 
    it's still faster than doing it alone.


6. **Resume:** Once all threads report "Done," the application resumes.

---

### 3. The Two Components (Young vs. Old)

Just like Serial GC, Parallel GC treats Young and Old generations differently, but now with multi-threading.
When you enable `-XX:+UseParallelGC`, the JVM activates:

| Generation    | Component Name                     | Algorithm                       |
|---------------|------------------------------------|---------------------------------|
| **Young Gen** | **PSYoungGen** (Parallel Scavenge) | **Parallel Mark-Copy**          |
| **Old Gen**   | **ParallelOld**                    | **Parallel Mark-Sweep-Compact** |

#### A. Young Generation (PSYoungGen)

* **Goal:** Speed. New objects die fast.
* **Algorithm: Parallel Mark-Copy**.
    * Multiple threads scan Eden simultaneously.
    * They coordinate to copy surviving objects into the Survivor Space.
* **Why it's fast:** Copying is CPU-intensive. By splitting the work across 8 or 16 cores, we 
   can clear Eden much faster than Serial GC.

#### B. Old Generation (ParallelOld)

* **Goal:** Space efficiency.
* **Algorithm: Parallel Mark-Sweep-Compact.**
    1. **Mark:** All threads scan the Old Gen to find live objects.
    2. **Summary:** They calculate where each live object *should* go to make the memory compact.
    3. **Compact:** They work together to slide objects to the start of the heap.

* **Why compact?** To eliminate fragmentation (Swiss Cheese memory) so we can allocate large objects later.

---

### 4. The "Throughput" Focus (Important for Interviews)

Parallel GC is often called the **"Throughput Collector."**

* **Throughput = (Time spent running app) / (Total time)**
* Parallel GC cares about getting the *most work done* over a long period.
* **Example:** It might pause for 1 second every hour. That is a long pause, but it's 
  very efficient because it cleaned a huge amount of memory in that 1 second.

### 5. Pros and Cons

| Feature             | Description                                                                                                                                                                         |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **High Throughput** | Best for batch processing, number crunching, or backend jobs where raw speed matters more than responsiveness.                                                                      |
| **Scalable**        | It scales well with hardware. If you add more CPU cores, GC gets faster.                                                                                                            |
| **The Downside**    | **Pauses are still unpredictable.** If you have a massive heap (e.g., 64GB), even with 16 threads, scanning and compacting it takes time. You might see "GC Pauses" of 3-5 seconds. |

### 6. Summary Comparison

* **Serial:** 1 Thread. Slow pause. Good for tiny heaps/single core.
* **Parallel:** N Threads. Fast pause (for medium heaps). Good for batch jobs.

**Command to enable:** `-XX:+UseParallelGC`


----------------


## Q - Explain Concurrent Mark Sweep(CMS) GC

Now we enter the era of **"Low Latency."**

**CMS (Concurrent Mark Sweep)** was designed to solve one specific problem: **Long Stop-The-World pauses.**

If Parallel GC is a "Cleaning Crew" that shuts down the building to 
clean, **CMS is a "Janitor" who cleans quietly in the background while people are still working.**

### 1. The Core Concept: "Concurrent"

This is the most important word in modern GC.

* **Parallel:** Multiple GC threads working together **while the app is paused**.
* **Concurrent:** The GC thread works **while the application is running**.

**The Trade-off:**

* **Parallel GC:** Pauses for 5 seconds, but uses 0% CPU while the app runs.
* **CMS:** Pauses for only 0.1 seconds, but steals some CPU (e.g., 20%) from your app 
  constantly to do background work.

---

### 2. The Two Components (Young vs. Old)

**Crucial Detail:** CMS is strictly an Old Generation collector. **It cannot handle the 
Young Generation alone**. When you enable -XX:+UseConcMarkSweepGC, the JVM activates this specific pair:

| Generation    | Component Name                  | Algorithm                              |
|---------------|---------------------------------|----------------------------------------|
| **Young Gen** | **ParNew** (Parallel New)       | **Parallel Mark-Copy**                 |
| **Old Gen**   | **CMS** (Concurrent Mark Sweep) | **Concurrent Mark-Sweep** (No Compact) |

* **Young Gen (ParNew):** Works exactly like Parallel GC's Young Gen (it is Stop-The-World and uses multiple threads).
    * **The Difference:** It has extra overhead ("hooks") to handle object promotion and header 
      updates safely, because the Old Gen is being managed by a background concurrent thread (CMS).
    * **Performance:** slightly slower than `PSYoungGen` because of this overhead, but necessary for CMS integration.

* **Old Gen (CMS):** This is where the magic happens. It cleans the Old Gen without stopping the application.

---

### 3. How it Works: The 4 Phases

CMS is more complex than Parallel GC. It breaks the job into 
four distinct phases to minimize pausing.

#### Phase 1: Initial Mark (Stop-The-World)

* **Action:** The JVM pauses the application.
* **Task:** The GC scans **only the "Root" objects** (static variables, thread stacks). It just 
  marks the starting points.
* **Duration:** extremely fast (milliseconds).
* **App Status:** **Frozen.**

#### Phase 2: Concurrent Mark (App Running)

* **Action:** The application resumes.
* **Task:** The GC thread follows all the references from those Roots to find all live objects in the heap.
* **Challenge:** Since the app is running, it might change references *while* the GC is 
  looking at them. (e.g., "I just marked Object A as live, but the app just deleted the reference to it!").
* **App Status:** **Running** (but slightly slower due to CPU sharing).

#### Phase 3: Remark (Stop-The-World)

* **Action:** The JVM pauses the application again.
* **Task:** The GC fixes the mistakes from Phase 2. It looks for objects that were 
  modified *during* the Concurrent Mark phase (the "dirty cards").
* **Duration:** Short (but longer than Initial Mark).
* **App Status:** **Frozen.**

#### Phase 4: Concurrent Sweep (App Running)

* **Action:** The application resumes.
* **Task:** The GC goes through the heap and reclaims the memory of dead objects.
* **App Status:** **Running.**

---

### 4. The Fatal Flaw: "Fragmentation" (The Swiss Cheese Problem)

You might notice something missing. **CMS does NOT Compact.**

* **Parallel GC:** Moves live objects together to create big empty spaces.
* **CMS:** Just marks dead space as "free" in a list (Free List).

**The Result:**
Imagine your memory is a row of parking spots.

* Parallel GC moves all cars to the left. You have a huge empty lot on the right.
* CMS just removes cars where they are. You have empty spots scattered everywhere (Swiss Cheese).

**The Crash:**
If you try to park a **Bus** (allocate a large object) and there are only small "Car" spots 
available scattered around, allocation fails.

### 5. The "Concurrent Mode Failure"

When fragmentation gets too bad, or if the Old Gen fills up faster than the 
background thread can clean it, CMS panics.

1. **The Panic:** "I have no space for this object!"
2. **The Fallback:** It triggers a **Full Serial GC** (using the **Serial Old** / `TenuredGeneration`) collector.
3. **The Result:** A massive Stop-The-World pause (often 10+ seconds) to fully compact 
   the heap using a single thread.

### 6. Summary for Interview

* **Goal:** Minimize pause times.
* **Method:** Does marking and sweeping concurrently (while app runs).
* **Pros:** Very short pauses (great for user experience).
* **Cons:** High CPU usage. **Memory Fragmentation.**
* **Status:** **Removed in Java 14.** (Replaced by G1GC and ZGC).

**Why learn it?**
Many legacy systems still run on Java 8 with CMS. Knowing *why* it failed leads 
perfectly into why **G1GC** was invented (to solve fragmentation).


----------------


## Q - Explain G1 GC


### G1GC (Garbage First) – The "Predictable" Collector

**The Problem it Solves:**

* **Parallel GC** freezes the application for too long on large heaps (scanning 64GB takes seconds).
* **CMS** cleans in the background but leaves memory "fragmented" (Swiss Cheese), eventually leading to crashes.

**The G1GC Solution:**
G1GC completely changes the layout of the Heap to solve both. It is designed 
for **Large Heaps (6GB+)** with a focus on **Low Latency** (short pauses).

---

### 1. The Architecture: "Regions"

Instead of three massive, contiguous blocks (Eden, Survivor, Old), G1GC chops the 
entire Heap into equal-sized chunks called "Regions"** (1MB - 32MB each).

Unlike Serial, Parallel, and CMS, which use two separate engines (one for Young, one for Old), 
G1GC is a single unified engine.

| Generation    | Component Name  | Algorithm                                              |
|---------------|-----------------|--------------------------------------------------------|
| **Young Gen** | G1CollectedHeap | **Parallel Evacuation (Copying)**                      |
| **Old Gen**   | G1CollectedHeap | **Concurrent Marking + Parallel Evacuation (Copying)** |


* **Virtual Roles:** A region is not permanently fixed.
    * A region usually starts as **Free**.
    * It becomes an **Eden** region when you allocate objects.
    * After a GC, it might become a **Survivor** or **Old** region.
    * Crucially, these regions **do not** have to be next to each other in memory.

---

### 2. The Lifecycle (How it Runs)

G1GC operates in a loop consisting of three distinct phases.

#### Phase A: Young Only Phase (Normal Mode)

* **Trigger:** The set of Eden regions is full.
* **Action:** A standard **Stop-The-World (STW)** pause.
* **Algorithm: Parallel Evacuation (Copying).**
* **What happens:**
    * G1GC pauses the app.
    * It picks all Eden regions and all Survivor regions.
    * It copies (evacuates) live objects into new **Survivor** or **Old regions**.
    * **Key Detail:** Because it copies objects to new regions, it is **compacting** memory by definition.

* **Result:** Eden is empty. The application resumes.

#### Phase B: The Concurrent Marking Cycle (The Proactive Trigger)

* **Trigger:** This does *not* wait for the Old Gen to be full. It starts 
  when the **Total Heap Occupancy** hits a threshold called **IHOP** (Initiating Heap Occupancy Percent).
* **Default:** **45% full.**
* **Action:** While the application is **running** (concurrently), G1GC scans the Old regions.
* **Goal:** To calculate the "Liveness" of each Old region.
    * *Region X:* 95% live data. (Expensive to clean).
    * *Region Y:* 5% live data (95% garbage). (Cheap to clean).

* **Result:** G1GC now has a list of "Candidate Regions" (mostly garbage) that are worth cleaning.

#### Phase C: The Mixed GC (The "Magic")

* **Trigger:** Occurs *after* the Concurrent Marking is done.
* **Action:** The next time Eden fills up, G1GC switches from a "Young Only" GC to a **"Mixed" GC**.
* **Why "Mixed"?** Because it cleans:
    1. **ALL** Young Regions (Eden + Survivor).
    2. **PLUS** a calculated number of **Candidate Old Regions** (the ones with the most garbage).

* **The "Garbage First" Logic:** It prioritizes the Old regions that are mostly garbage because they 
  give the highest return on investment (reclaiming the most space for the least work).

---

### 3. The Killer Feature: "Predictable Pauses"

This is what makes G1GC the "Gold Standard" for production.

You give the JVM a target: ` -XX:MaxGCPauseMillis=200` (Don't pause for more than 200ms).

* **During a Mixed GC:** G1GC calculates:
> *"I have 200ms. Cleaning the Young Gen will take 100ms. That leaves me 100ms to 
> clean Old regions. Based on my stats, I can clean exactly **4 Old Regions** in that time."*
>

* It adds those 4 specific Old regions to the cleanup list, cleans 
  them, and **stops** exactly when the time is up.

**Contrast with Parallel GC:** Parallel GC would try to clean the *entire* Old Gen, taking 
5 seconds regardless of your target.

---

### 4. The Failure Mode: "Evacuation Failure"

Just like CMS has "Concurrent Mode Failure," G1 has a failure mode.

* **The Problem:** **Evacuation Failure**.
    * G1 tries to copy live objects from Region A to Region B.
    * But there are **no free regions left** in the heap.

* **The Fallback:** It triggers a **Full GC**.
    * *Historical Note:* Before Java 10, this fallback was **Single Threaded (Serial Old)**.
    * *Modern Java (10+):* The fallback is **Parallel**, so it’s not as catastrophic, but still a long pause.


---

### 5. Summary for the Interview

If asked to explain G1GC, use this structure:

1. **Layout:** "G1GC divides the heap into thousands of small **Regions**. It is a single component
    handling both generations."
2. **Algorithm:** "It uses **Parallel Evacuation** (Copying) for both Young and Old generations. 
    This means it is always compacting; it never suffers from the fragmentation issues of CMS."
3. **Strategy:** "It marks concurrently (while app runs) to find which Old regions are mostly garbage."
4. **Mixed GC:** "It then performs 'Mixed GCs' where it cleans the Young Gen + the 'Garbage First' Old regions
    to meet a strict **pause time target** (e.g., 200ms)."



----------------


## Q - Can you compare the different Garbage Collectors in Java and explain when to use each one?

### The Ultimate Java GC Cheat Sheet

| Collector       | Young Gen *(Component & Algo)*                | Old Gen *(Component & Algo)*                               | Goal & Characteristic                                                                                  | When to Use?                                                                              | Default In                         | JVM Flag                  |
|-----------------|-----------------------------------------------|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|------------------------------------|---------------------------|
| **Serial GC**   | **DefNew**<br>Serial Mark-Copy                | **Tenured**<br>Serial Mark-Sweep-Compact                   | **Minimal Overhead**<br>Single-threaded. Stops the world for everything.                               | • Small Heaps (<100MB)<br>• Single Core CPUs<br>• AWS Lambda / Microservices              | Client Class (Java 5–8)            | `-XX:+UseSerialGC`        |
| **Parallel GC** | **PSYoungGen**<br>Parallel Mark-Copy          | **ParallelOld**<br>Parallel Mark-Compact                   | **Max Throughput**<br>Multi-threaded STW. Finishes work fast over pause times.                         | • Batch Processing <br/>• Video Encoding<br>• Number Crunching<br>• Logging/Audit Systems | Java 6, 7, 8 (Server Class)        | `-XX:+UseParallelGC`      |
| **CMS GC**      | **ParNew**<br>Parallel Mark-Copy              | **CMS**<br>Concurrent Mark-Sweep                           | **Low Latency (Legacy)**<br>Concurrent Old Gen cleaning.<br>Does **NOT** compact (fragmentation risk). | • Do Not Use (Deprecated)<br>• Legacy Java 8 apps                                         | Removed in Java 14                 | `-XX:+UseConcMarkSweepGC` |
| **G1 GC**       | **G1 (Young Regions)**<br>Parallel Evacuation | **G1 (Old Regions)**<br>Concurrent Mark + Mixed Evacuation | **Predictable Latency**<br>Balanced throughput & pause time. Compacting.                               | • Standard choice<br>• Web Servers (Spring Boot)<br>• Large Heaps (4GB–32GB)              | Java 9+ (11, 17, 21)               | `-XX:+UseG1GC`            |
| **ZGC**         | **ZGC**<br>Colored Pointers                   | **ZGC**<br>Load Barriers                                   | **Ultra-Low Latency**<br>Pauses <1ms regardless of heap size (even multi-TB).                          | • Massive Heaps (>32GB)<br>• Real-time Systems<br>• Gaming / Trading                      | Java 15+ (Production ready in 17+) | `-XX:+UseZGC`             |


### Notes (Accuracy Improvements)

* ZGC is production-ready since **Java 17**, not “future only”.
* CMS was removed in **Java 14**.
* G1 is default since **Java 9**.
* Parallel GC was default before Java 9 (server class machines).


---


## Q - What is a heap dump? Why do we use it? Have you ever taken a heap dump?

This is a quintessential production troubleshooting question.

Here is the ground-level breakdown of **Heap Dumps**, why they are 
the "Black Box" of Java debugging, and exactly how to answer the "Have you ever taken one?" part.

### 1. What is a Heap Dump? (The "Crime Scene Photo")

Think of your running Java application as a busy city.
A **Heap Dump** is like freezing time and taking a high-resolution 3D photo of the entire city.

* **It contains:** Every single object currently in memory (Strings, User objects, HashMaps, etc.).
* **It shows:**
    * What the object is (Class).
    * What data it holds (Values).
    * **Crucially:** Who is holding onto it (References).

* **Format:** usually a binary file with a `.hprof` extension.

### 2. Why do we use it?

We rarely take heap dumps when things are going well because they are 
heavy (if you have a 16GB heap, the file is ~16GB, and writing it pauses the app).

We use them for two main reasons:

#### A. The `OutOfMemoryError` (OOM)

Your application crashes with `java.lang.OutOfMemoryError: Java heap space`.

* **The Mystery:** "Why did we run out of RAM? Did we have a massive spike in users? Or is there a bug?"
* **The Heap Dump:** Shows you exactly what was consuming that 16GB of RAM at the moment of death. 
  Usually, it's one specific `List` or `Map` that grew uncontrollably.

#### B. Memory Leaks

The application starts fast but gets slower and slower over 3 days until it crashes.

* **The Theory:** "Somewhere, we are creating objects but never deleting them."
* **The Heap Dump:** You take one dump on Day 1 and another on Day 3. You compare them. 
  If `UserSession` objects jumped from 1,000 to 1,000,000, you found your leak.

---

### 3. "Have you ever taken a heap dump?" (The Interview Answer)

**Do not just say "Yes."** You need to describe the *process* to show you’ve actually battled production issues.

Here is a Senior Developer level answer:

> "Yes, absolutely. I’ve dealt with a few memory leaks in production. 
> Typically, I use two approaches depending on the urgency."
> 

#### Scenario A: The Proactive Setup (Best Practice)

> "In our production scripts, we always pass the flag `-XX:+HeapDumpOnOutOfMemoryError`.
> This is critical because when the JVM crashes at 3 AM, it automatically generates a 
> snapshot right before it dies. I can then analyze that file (`java_pid.hprof`) the next 
> morning to see exactly what killed the application."

#### Scenario B: The Manual Inspection (Debugging a Slow App)

> "If an app is running slowly but hasn't crashed yet, I use the command line tool **`jmap`**."
> 
> *Command:* `jmap -dump:live,format=b,file=heap_dump.hprof <PID>`
> 
> *"I verify the Process ID (PID) using `jps`, then trigger the dump. 
> I usually add the `:live` option so it only dumps objects that are currently 
> referenced, which makes the file smaller and easier to read."*
> 

---

### 4. How do you analyze it? (The "Eclipse MAT" Tool)

You cannot open a 10GB file in Notepad. You need a tool. The industry 
standard is **Eclipse MAT (Memory Analyzer Tool)**.

If asked **"How do you read it?"**:

1. **Load the Dump:** Open the `.hprof` file in Eclipse MAT.
2. **The Histogram:** I look at the "Histogram" view first. It lists classes by the number of instances.
    * *Normal:* `String`, `char[]`, `Integer` are at the top.
    * *Suspicious:* `com.mycompany.OrderProcessor` has 5 million instances.

3. **The Dominator Tree:** This is the most powerful view. It tells you **"Who is keeping these objects alive?"**
    * *Example:* You see 5 million `Order` objects. The Dominator Tree shows they 
      are all being held inside a `static HashMap` in your `CacheManager` class.
    * *Conclusion:* "Ah, we forgot to clear the cache! That's the leak."


### Summary for the Interview

1. **Definition:** A snapshot of memory at a specific point in time.
2. **Usage:** To debug OOM errors and find Memory Leaks.
3. **Tools:**
    * **Capture:** `jmap` (command line) or `-XX:+HeapDumpOnOutOfMemoryError` (automatic).
    * **Analyze:** **Eclipse MAT** or **VisualVM**.
Ready for the next question?
4. **Key Insight:** "I look for the 'Dominator Tree' to see which large collection is holding onto 
   memory it shouldn't be."


---


## Q - What is memory management in Java?

Here is the short, interview-ready version of **Java Memory Management**.

In older languages (C/C++), you had to do this manually (malloc to create, free to delete). 
If you forgot to delete, your app crashed (Memory Leak). In Java, Memory Management is automatic. 
You create objects, and Java's "Garbage Collector" deletes them when you're done.

**Key Components:**

1. **Stack Memory (Thread Execution):**
    * **What it holds:** Local variables (`int`, `boolean`) and **references** to objects.
    * **Lifecycle:** Automatically created when a method starts and cleared when it ends.
    * **Speed:** Very fast.


2. **Heap Memory (Object Storage):**
    * **What it holds:** The actual **Objects** (e.g., `new Employee()`).
    * **Lifecycle:** Managed by the **Garbage Collector**. Objects live here as long as 
    they are referenced by the Stack.
    * **Speed:** Slower than Stack, but much larger.


**How it works:**
You create an object (`new Object()`). It goes into the **Heap**. A reference to it 
goes onto the **Stack**. When the Stack reference is removed (method ends), the object in 
the Heap becomes "Garbage" and is eventually cleaned up by the Garbage Collector.


----------------


## Q - What are the types of Heap memory?

When we talk about "Types of Heap Memory" in an interview, we are specifically 
referring to the **Generational Layout**.

Java divides the Heap into two main areas based on the **age** of the objects (how long they have survived).

Here is the breakdown of the 3 specific spaces inside the Heap:

### 1. Young Generation (The Nursery)

This is where **new objects are born**. It is small and designed for speed because 
most objects die very quickly (e.g., temporary variables in a loop).

It is further divided into three sub-spaces:

* **Eden Space:**
    * **Role:** Every time you write `new Object()`, it goes here first.
    * **Behavior:** When it fills up, a **Minor GC** happens. Most objects here are 
    dead and get wiped out immediately.


* **Survivor Space 0 (S0 / From):**
    * **Role:** Stores objects that survived a Minor GC in Eden.


* **Survivor Space 1 (S1 / To):**
    * **Role:** The destination for the next Minor GC. (Remember the "Copying" mechanism? 
    Objects ping-pong between S0 and S1).


### 2. Old Generation (The Retirement Home)

Also called **Tenured Space**.

* **Role:** This stores long-lived objects (e.g., Caches, Database connections, Spring Beans) 
  that have survived many rounds of garbage collection in the Young Generation.
* **Size:** It is much larger than the Young Generation.
* **Behavior:** Garbage Collection here (Major GC) is rare but slow because the space is huge.

---

#### Summary Table for Interview

| Memory Type   | Sub-Type             | What lives there?              | GC Type         |
|---------------|----------------------|--------------------------------|-----------------|
| **Young Gen** | **Eden**             | Newly created objects.         | Minor GC (Fast) |
| **Young Gen** | **Survivor (S0/S1)** | Objects that survived 1+ GCs.  | Minor GC        |
| **Old Gen**   | **Tenured**          | Objects that survived ~15 GCs. | Major GC (Slow) |

**Note:** In Java 8+, **Metaspace** is *not* part of the Heap. It is separate Native Memory. 
So if asked "Types of **Heap**," stick strictly to Young and Old.


----------------


## Q - How do you optimize JVM memory?

This is a very broad question, so to answer it effectively in an interview, 
you need a **strategy**, not just a list of flags.

The "Ground Level" approach to optimization is: **Measure First, Tune Second.** 
You cannot optimize what you cannot see.

Here is the step-by-step process senior engineers use to optimize JVM memory.

---

### Step 1: Right-Sizing the Heap ( The Foundation)

The most common problem is simply having the wrong heap size.

* **The Flags:**
    * `-Xms`: Initial Heap Size (Start).
    * `-Xmx`: Maximum Heap Size (Ceiling).

* **The Optimization Trick:** **Set them to the SAME value.**
    * *Why?* If `Xms` is 1GB and `Xmx` is 4GB, the JVM has to constantly ask the OS 
     for more memory as the app grows, and give it back when it shrinks. This resizing takes CPU time.
    * *Best Practice:* `java -Xms4g -Xmx4g ...`
    * This forces the JVM to allocate all 4GB at startup, eliminating resizing overhead.


### Step 2: Choosing the Right Collector

You don't bring a Ferrari to a mud race. Choosing the GC depends on your goal.

* **Web Server / API (Low Latency):** Use **G1GC** (Default in Java 9+).
    * *Flag:* `-XX:+UseG1GC`
    * *Why:* You want to avoid long pauses so users don't see timeouts.


* **Batch Processing / Number Crunching (Throughput):** Use **Parallel GC**.
    * *Flag:* `-XX:+UseParallelGC`
    * *Why:* You don't care if the app freezes for 5 seconds as long as the job 
    * finishes 10 minutes faster overall.

### Step 3: Tuning the "Pause Goal" (The Magic Knob)

If you are using **G1GC**, this is the single most important optimization you can make.

* **The Flag:** `-XX:MaxGCPauseMillis=200`
* **How it works:** You tell the JVM, *"I don't care how you do it, but do NOT stop my 
  app for more than 200 milliseconds."*
* **The Trade-off:**
    * If you set this too low (e.g., 50ms), GC will run very frequently (bad throughput).
    * If you set it too high (e.g., 1000ms), users will notice the lag.
    * *Standard Start:* 200ms is a safe default.


### Step 4: Handling "Metaspace" (The Hidden Memory)

Since Java 8, class metadata is stored in native memory (Metaspace). If you have a memory 
leak here, it can crash your *entire server*, not just the JVM.

* **The Flag:** `-XX:MaxMetaspaceSize=256m`
* **Optimization:** Always set a cap. If you don't, a buggy app (generating infinite dynamic classes) 
  will eat all the RAM on the physical machine until the OS kills the process.

#### Step 5: Enable GC Logging (The Black Box)

You cannot optimize if you don't know what's happening. Always enable logging in production.

* **The Flags (Java 9+):** `-Xlog:gc*:file=gc.log:time,uptimemillis`
* **Why:** This writes a file showing exactly when GC ran and how long it took. 
  You can upload this file to tools like **GCViewer** or **GCeasy.io** to visualize your memory health.

---

#### Summary for the Interview

"I approach optimization in three phases:

1. **Baseline:** I set `-Xms` and `-Xmx` to the same value (e.g., 70-80% of container memory) to 
  prevent resizing overhead.
2. **Selection:** I choose the right collector. For most REST APIs, I use **G1GC** because it 
   offers predictable pause times.
3. **Tuning:** I set a realistic pause target (`-XX:MaxGCPauseMillis`) and monitor the GC logs. 
   If I see frequent 'Full GCs', I know I need to increase the heap size or investigate a memory leak."

This shows you understand the *process*, not just the syntax.

----------------



