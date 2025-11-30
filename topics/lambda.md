<!-- TOC -->
  * [What is target type?](#what-is-target-type)
    * [Context 1: Assignment Context](#context-1-assignment-context)
    * [Context 2: Variable Initialization Context](#context-2-variable-initialization-context)
    * [Context 3: Return Context](#context-3-return-context)
    * [Context 4: Method Argument Context](#context-4-method-argument-context)
  * [Example of an illegal, standalone lambda](#example-of-an-illegal-standalone-lambda)
  * [Generic Functional Interfaces](#generic-functional-interfaces)
  * [Passing lambdas as arguments](#passing-lambdas-as-arguments)
  * [Lambda Expressions and Exceptions](#lambda-expressions-and-exceptions)
  * [Lambda Expressions and Variable Capture](#lambda-expressions-and-variable-capture)
    * [1. Lambdas Can Access Enclosing Variables](#1-lambdas-can-access-enclosing-variables)
    * [2. Local variables are different — "Variable Capture"](#2-local-variables-are-different--variable-capture)
    * [3. What is "effectively final"?](#3-what-is-effectively-final)
    * [4. Lambdas CANNOT modify captured local variables](#4-lambdas-cannot-modify-captured-local-variables)
    * [5. But lambdas CAN modify instance or static state](#5-but-lambdas-can-modify-instance-or-static-state)
    * [6. Lambdas do NOT define their own `this`](#6-lambdas-do-not-define-their-own-this)
  * [Method References](#method-references)
    * [Types of Method References](#types-of-method-references)
    * [Reference to a static method](#reference-to-a-static-method)
    * [Reference to an instance method of a specific object](#reference-to-an-instance-method-of-a-specific-object)
    * [Unbound instance method reference — the object will be provided as the first argument](#unbound-instance-method-reference--the-object-will-be-provided-as-the-first-argument)
    * [When to use method references?](#when-to-use-method-references)
<!-- TOC -->

The key to understanding Java's implementation of lambda expressions are two constructs.

* The first is the lambda expression, itself. 
* The second is the functional interface. 

Let's begin with a simple definition of each.

A _lambda expression_ is, essentially, an anonymous (that is, unnamed) method. However,
this method is not executed on its own. Instead, it is used to implement a method defined by
a functional interface. Thus, a lambda expression results in a form of anonymous class.
Lambda expressions are also commonly referred to as _closures_.

A _functional interface_ is an interface that contains one and only one abstract method.
Normally, this method specifies the intended purpose of the interface. Thus, a functional
interface typically represents a single action. For example, the standard interface `Runnable` is a
functional interface because it defines only one method: `run()`. Therefore, `run()` defines the
action of `Runnable`. Furthermore, a functional interface defines the target type of a lambda
expression.

## What is target type?

A target type is the type that the lambda expression will be converted into.

Because a lambda by itself has **no type**.

Example lambda:

```java
() -> System.out.println("hi")
```

* This is just _a block of behavior_.
* It is **not** a class
* It is **not** an object
* It is **not** a method

Until Java knows **what functional interface you want**, it cannot assign a type to it.

That functional interface is called the **target type**.

As mentioned earlier, a lambda expression is not executed on its own. Rather, it forms
the implementation of the abstract method defined by the functional interface that specifies
its target type. As a result, a lambda expression can be specified only in a context in which a
target type is defined. One of these contexts is created when a lambda expression is assigned
to a functional interface reference. Other target type contexts include variable initialization,
return statements, and method arguments, to name a few.

Here are some examples:

### Context 1: Assignment Context

This is when you assign a value to an existing variable reference.

```java
Socket2 socket;
socket = () -> System.out.println("hi");
```

### Context 2: Variable Initialization Context

This happens when you're declaring and initializing a variable at the same time.

```java
Socket2 socket2 = () -> System.out.println("hi");
```

### Context 3: Return Context

This happens when you're returning a value from a method.

```java
Runnable getTask() {
    return () -> System.out.println("task");
}
```

### Context 4: Method Argument Context

This happens when you're passing a value to a method.

```java
void execute(Socket2 s) { s.send(); }

execute(() -> System.out.println("sent"));
```

## Example of an illegal, standalone lambda

```java
() -> System.out.println("hi");  // ERROR — no target type
```

Java doesn't know what's the target type here. So this is a compilation error.


## Generic Functional Interfaces

A lambda expression, itself, cannot specify type parameters.

Unlike a method, you cannot write:

```text
<T> (T x) -> x     // ILLEGAL in Java
```

Lambdas do not have their own `<T>` or `<K, V>`.

They rely entirely on the target type (the functional interface) to provide any type information.

Here is an exmaple if generic funtional interface:

```java
package org.example.sec02;

interface SAM_1<T>{
    T process(T t);
}

public class Example10 {
    public static void main(String[] args) {
        SAM_1<String> ob = (s) -> s.toUpperCase();
        SAM_1<Integer> ob2 = (s) -> s * 10;

        System.out.println(ob.process("aaa"));
        System.out.println(ob2.process(10));
    }
}
```

## Passing lambdas as arguments

Here is an example how lamdbas can be used to quickly change object ordering:

```java

class Employee implements Comparable<Employee> {
    private final String name;
    private final int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    @Override
    public int compareTo(Employee other) {
        return Integer.compare(this.id, other.id);
    }

    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }

    @Override
    public String toString() {
        return "Employee{name='" + name + "', id=" + id + "}";
    }

}

public class Example10 {
    public static void main(String[] args) {
        Employee e1 = new Employee("aa", 1);
        Employee e2 = new Employee("bb", 2);
        Employee e3 = new Employee("cc", 3);

        List<Employee> list = new ArrayList<>(List.of(e2, e1, e3));

        System.out.println(list);
        list.sort(null);
        System.out.println(list);
        list.sort((o1, o2) -> o2.getName().compareTo(o1.getName()));
        System.out.println(list);
    }
}
```

## Lambda Expressions and Exceptions

If the functional interface method declares checked exceptions, then (and only then) the lambda may throw them.
If the method does not declare them, the lambda CANNOT throw checked exceptions.

```java
@FunctionalInterface
interface FileProcessor {
    void process() throws IOException;
}

public class Example10 {
    public static void main(String[] args) {
        FileProcessor fp = () -> {
            throw new IOException("File error");  // OK
        };
    }
}
```

## Lambda Expressions and Variable Capture

### 1. Lambdas Can Access Enclosing Variables

A lambda can freely use:

* instance variables
* static variables
* methods of the enclosing class
* `this` (refers to the outer class, NOT the lambda)

Example:

```java
class Demo {
    int count = 10;

    void show() {
        Runnable r = () -> {
            System.out.println(count);  // OK
            System.out.println(this.count); // OK
        };
        r.run();
    }
}
```

### 2. Local variables are different — "Variable Capture"

A lambda may access local variables from the method where it is written, but ONLY if they are effectively `final`.

Example:

```java
void test() {
    int x = 10;  // effectively final

    Runnable r = () -> System.out.println(x); // OK
}
```

Here, `x` is effectively final because it never changes.


### 3. What is "effectively final"?

Effectively final = `a variable whose value never changes after assignment`.

So this is allowed:

```java
int x = 10;
Runnable r = () -> System.out.println(x);
```

But this is NOT:

```java
int x = 10;
x = 20;              // not effectively final now!

Runnable r = () -> System.out.println(x);  // ERROR
```

### 4. Lambdas CANNOT modify captured local variables

This is ILLEGAL:

````java
int x = 10;

Runnable r = () -> {
    x++;           // ERROR – cannot modify captured variable
};
````

Because modifying it makes it **not effectively final**.


### 5. But lambdas CAN modify instance or static state

Allowed:

```java
class Demo {
    int count = 0;

    void test() {
        Runnable r = () -> {
            count++;          // OK
        };
        r.run();
    }
}
```

Because instance variables are not subject to the "effectively final" rule.

### 6. Lambdas do NOT define their own `this`

Example:

```java
class Demo {
    void test() {
        Runnable r = () -> {
            System.out.println(this); // this = Demo instance
        };
        r.run();
    }
}
```

## Method References

A method reference is a way to refer to an existing method by name, without calling it.

It is simply a shortcut for a lambda expression.

Instead of writing:

```java
x -> MyClass.myStaticMethod(x)
```

you can write:

```java
MyClass::myStaticMethod
```

You are referring to the method, not invoking it.

It is important to note that a method reference is valid only if the referenced method’s signature conforms to the
functional interface's abstract method signature.

This means the referenced method must have:

1. The same number of parameters
2. Compatible parameter types
3. A compatible return type
4. Checked exceptions compatible with the functional interface’s throws clause

If ANY of these do not match, the method reference is illegal.

### Types of Method References

Java defines 4 types of method references:

1. Reference to a static method
2. Reference to an instance method of a specific object
3. Unbound instance method reference — the object will be provided as the first argument.
4. Reference to a constructor

### Reference to a static method

```java
ClassName::staticMethodName
```

Example:

```java
interface IntOperation {
    int apply(int x);
}

class MathUtils {
    public static int square(int x) {
        return x * 2;   // (or x * x, this is just an example)
    }
}

public class Example10 {
    public static void main(String[] args) {
        IntOperation intOperation = MathUtils::square;
        System.out.println(intOperation.apply(10));
    }
}
```

Note that method references are just shortcut, we can achieve the same thing as above using lambda: 

```java
IntOperation op = (n) -> MathUtils.square(n);
```

### Reference to an instance method of a specific object

```java
instanceReference::methodName
```

Meaning: Call this specific object’s method whenever the functional interface method is invoked.

Example:

```java
interface Printer {
    void print();
}

class Message {
    void show() {
        System.out.println("Hello from Message.show()");
    }
}


public class Example10 {
    public static void main(String[] args) {
        Message msg = new Message();
        Printer p = msg::show;
        p.print();
    }
}
```

### Unbound instance method reference — the object will be provided as the first argument

```java
ClassName::instanceMethodName
```

Consider the following interface:

```java
interface StringOp{
    String uppercase(String s);
}

StringOp op2 = String::toUpperCase;
op2.uppercase("hello world");
```

`String::toUpperCase` is an unbound instance method reference. 

Meaning: The object on which `toUpperCase()` is called will come from the first argument of the 
functional interface method.

Example:

```java
interface StringOp {
    String uppercase(String s);
}

public class Example10 {
    public static void main(String[] args) {
        StringOp op = a -> a.toUpperCase();
        StringOp op2 = String::toUpperCase;
        System.out.println(op2.uppercase("hello"));
    }
}
```

When Java sees this method reference:

```java
String::toUpperCase
```

it automatically rewrites it into a lambda of the form:

```java
(String s) -> s.toUpperCase()
```

#### When to use method references?

A method reference only makes sense when an existing method already performs the logic you want.


### Reference to a constructor

A constructor reference is a way to refer to a class's constructor without invoking it.

It works exactly like a method reference but targets `new`.

```java
ClassName::new
```

This creates an instance of a functional interface whose abstract method returns an object of that class.

#### Essence of Constructor Reference

When Java sees:

```java
ClassName::new
```

it rewrites it into a lambda equivalent to:

```java
(args...) -> new ClassName(args...)
```

That's the entire concept.


