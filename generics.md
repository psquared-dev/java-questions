## TOC

<!-- TOC -->
  * [TOC](#toc)
  * [Generics](#generics)
  * [A Simple Generics Example](#a-simple-generics-example)
  * [Type Erasure](#type-erasure)
  * [Generics Work Only with Reference Types](#generics-work-only-with-reference-types)
  * [Generic Types Differ Based on Their Type Arguments](#generic-types-differ-based-on-their-type-arguments)
  * [How Generics Improve Type Safety](#how-generics-improve-type-safety)
  * [A Generic Class with Two Type Parameters](#a-generic-class-with-two-type-parameters)
  * [Bounded Types](#bounded-types)
  * [Using Wildcard Arguments](#using-wildcard-arguments)
  * [Bounded Wildcards](#bounded-wildcards)
  * [Creating a Generic Method](#creating-a-generic-method)
  * [Generic Constructors](#generic-constructors)
  * [Generic Interfaces](#generic-interfaces)
  * [Raw Types and Legacy Code](#raw-types-and-legacy-code)
  * [Generic class hierarchies](#generic-class-hierarchies)
  * [A Generic Subclass](#a-generic-subclass)
  * [Run-Time Type Comparisons Within a Generic Hierarchy](#run-time-type-comparisons-within-a-generic-hierarchy)
  * [Casting](#casting)
  * [Overriding Methods in a Generic Class](#overriding-methods-in-a-generic-class)
  * [Type Inference with Generics](#type-inference-with-generics)
  * [Erasure](#erasure)
    * [Source Code (with generics)](#source-code-with-generics)
    * [What the compiler turns it into (after erasure)](#what-the-compiler-turns-it-into-after-erasure)
    * [Bridge Methods](#bridge-methods)
<!-- TOC -->

## Generics

The term _generics_ means _parameterized types_.

Parameterized types are important because they enable you to create classes, interfaces, and methods 
in which the type of data upon which they operate is specified as a parameter. 

Using generics, it is possible to create a single class, for example, that automatically works with different
types of data. A class, interface, or method that operates on a parameterized type is called generic, as in 
generic class or generic method.

## A Simple Generics Example

Here is a simple example of a generic class in Java:

```java
class Gen<T> {
    T ob;

    public Gen(T ob) {
        this.ob = ob;
    }

    T getOb() {
        return ob;
    }

    void showType() {
        System.out.println("Type of T is " + ob.getClass().getName());
    }
}

public class Main01 {
    public static void main(String[] args) {
        Gen<String> aa = new Gen<String>("aa");
        String ob = aa.getOb();
        aa.showType();
    }
}
```

Here, `T` is the name of a type parameter. This name is used as a placeholder for the actual
type that will be passed to `Gen` when an object is created. Thus, `T` is used within `Gen`
whenever the type parameter is needed. Notice that `T` is contained within `< >`. This syntax
can be generalized. Whenever a type parameter is being declared, it is specified within angle
brackets. Because `Gen` uses a type parameter, `Gen` is a generic class, which is also called a
parameterized type.

## Type Erasure

Consider the following declaration:

```java
Gen<Integer> iOb;
```

Look closely at this declaration. First, notice that the type `Integer` is specified within the
angle brackets after `Gen`. In this case, `Integer` is a type argument that is passed to `Gen`'s
type parameter, `T`. This effectively creates a version of `Gen` in which all references to `T` are
translated into references to `Integer`. Thus, for this declaration, `ob` is of type `Integer`,
and the return type of `getOb( )` is of type `Integer`.

It’s necessary to state that the Java compiler does not actually create
different versions of `Gen`, or of any other generic class. Although it’s helpful to think in
these terms, it is not what actually happens. Instead, the compiler removes all generic type
information, substituting the necessary casts, to make your code behave as if a specific
version of `Gen` were created. Thus, there is really only one version of `Gen` that actually exists
in your program. The process of removing generic type information is called **erasure**.

```java
Gen<String> aa = new Gen<String>("aa");
```

Notice that when the `Gen` constructor is called, the type argument `String` is also specified.
This is because the type of the object (in this case `aa`) to which the reference is being
assigned is of type `Gen<String>`. Thus, the reference returned by new must also be of type
`Gen<String>`. If it isn’t, a compile-time error will result. For example, the following
assignment will cause a compile-time error:

```java
iOb = new Gen<Double>(88.0); // Error! Incompatible types
```

Because `aa` is of type `Gen<String>`, it can’t be used to refer to an object of `Gen<Double>`.
This type checking is one of the main benefits of generics because it ensures type safety.


## Generics Work Only with Reference Types

When declaring an instance of a generic type, the type argument passed to the type parameter
must be a reference type. You cannot use a primitive type, such as `int` or `char`. For example,
with `Gen`, it is possible to pass any class type to `T`, but you cannot pass a primitive type to a
type parameter. Therefore, the following declaration is illegal:

```java
Gen<int> intOb = new Gen<int>(53); // Error, can't use primitive type
```

However, you can use the wrapper classes that correspond to the primitive types.

```java
Gen<Integer> obj1 = new Gen<Integer>(1);
```

## Generic Types Differ Based on Their Type Arguments

A key point to understand about generic types is that a reference of one specific version of a
generic type is not type compatible with another version of the same generic type. For example,
assuming the program just shown, the following line of code is in error and will not compile:

```java
obj1 = obj2;    // Wrong!
```

Even though both `obj1` and `obj2` are of type `Gen<T>`, they are references to different types
because their type arguments differ. This is part of the way that generics add type safety and
prevent errors

## How Generics Improve Type Safety

Consider the following example:

```java
class NonGen {
    Object ob;

    public NonGen(Object ob) {
        this.ob = ob;
    }

    Object getOb() {
        return ob;
    }

    void showType() {
        System.out.println("Type of ob is " + ob.getClass().getName());
    }
}

public class Main01 {
    public static void main(String[] args) {
        NonGen iOb;
        iOb = new NonGen(88);
        iOb.showType();

        //  this cast is necessary
        int v  = (Integer) iOb.getOb();

        NonGen strOb = new NonGen("Non-Generics Test");
        strOb.showType();
        String str = (String) strOb.getOb();
        System.out.println("value: " + str);

        // This compiles, but is conceptually wrong!
        iOb = strOb;
        v = (Integer) iOb.getOb(); // run-time error!
    }
}
```

There are several things of interest in this version. First, notice that `NonGen` replaces all
uses of `T` with `Object`. This makes `NonGen` able to store any type of object, as can the generic
version. However, it also prevents the Java compiler from having any real knowledge about the
type of data actually stored in NonGen, which is bad for two reasons. 

1. explicit casts must be employed to retrieve the stored data. 
2. many kinds of type mismatch errors cannot be found until run time.

Conside this line:

```java
int v = (Integer) iOb.getOb();
```

Here, the cast to `Integer` is necessary because the compiler does not know what type of object.

Now, consider the following sequence from near the end of the program:

```java
// This compiles, but is conceptually wrong!
iOb = strOb;
v = (Integer) iOb.getOb(); // run-time error!
```

Here, `strOb` is assigned to `iOb`. However, `strOb` refers to an object that contains a string, not an integer. 
This assignment is syntactically valid but semantically wrong. And the worst part is `iOb` contains a string and 
when we try to cast it to an `Integer`, a run-time error occurs.

So using Objects we are able to create "generic" code, but we lose type safety and have to do explicit casting.
Generics prevent this from occurring. In essence, through generics, **run-time  errors are converted into compile-time 
errors**. This is a major advantage.

## A Generic Class with Two Type Parameters

```java
// A simple generic class with two type
// parameters: T and V.
class TwoGen<T, V> {
    T ob1;
    V ob2;
    // Pass the constructor a reference to
    // an object of type T and an object of type V.
    TwoGen(T o1, V o2) {
        ob1 = o1;
        ob2 = o2;
    }
    // Show types of T and V.
    void showTypes() {
        System.out.println("Type of T is " +
                ob1.getClass().getName());
        System.out.println("Type of V is " +
                ob2.getClass().getName());
    }
    T getOb1() {
        return ob1;
    }
    V getOb2() {
        return ob2;
    }
}

// Demonstrate TwoGen.
class SimpGen {
    public static void main(String[] args) {
        TwoGen<Integer, String> tgObj =
                new TwoGen<Integer, String>(88, "Generics");
        
        // Show the types.
        tgObj.showTypes();
        
        // Obtain and show values.
        int v = tgObj.getOb1();
        System.out.println("value: " + v);
        String str = tgObj.getOb2();
        System.out.println("value: " + str);
    }
}
```

**Output:**

```Bash
Type of T is java.lang.Integer
Type of V is java.lang.String
value: 88
value: Generics
```

It specifies two type parameters: `T` and `V`, separated by a comma. Because it has two type
parameters, two type arguments must be passed to TwoGen when an object is created, as
shown next:

```java
TwoGen<Integer, String> tgObj = new TwoGen<Integer, String>(88, "Generics");
```

In this case, `Integer` is substituted for `T`, and `String` is substituted for `V`.

## Bounded Types

In the preceding examples, the type parameters could be replaced by any class type. This is
fine for many purposes, but sometimes it is useful to limit the types that can be passed to a
type parameter. For example, assume that you want to create a generic class that contains a
method that returns the average of an array of numbers. Furthermore, you want to use the
class to obtain the average of an array of any type of number, including integers, floats, and
doubles.

To create such a class, you might try something like this:

```java
// Stats attempts (unsuccessfully) to
// create a generic class that can compute
// the average of an array of numbers of
// any given type.
//
// The class contains an error!
class Stats<T> {
    T[] nums; // nums is an array of type T
    
    // Pass the constructor a reference to
    // an array of type T.
    Stats(T[] o) {
        nums = o;
    }
    // Return type double in all cases.
    double average() {
        double sum = 0.0;
        for(int i=0; i < nums.length; i++)
            sum += nums[i].doubleValue(); // Error!!!
        return sum / nums.length;
    }
}

public class Main01 {
    public static void main(String[] args) {
        Stats<Integer> iob = new Stats<Integer>(new Integer[]{1, 2, 3, 4, 5});
        System.out.println(iob.average());

        Stats<Double> dob = new Stats<Double>(new Double[]{1.0, 2.0, 3.0, 4.0, 5.0});
        System.out.println(dob.average());
    }
}
```

In Stats, `the average()` method attempts to obtain the double version of each number in
the nums array by calling `doubleValue()`. Because all numeric classes, such as `Integer` and
`Double`, are subclasses of Number, and Number defines the `doubleValue()` method, this
method is available to all numeric wrapper classes. The trouble is that the compiler has no
way to know that you are intending to create `Stats` objects using only numeric types. Thus,
when you try to compile `Stats`, an error is reported that indicates that the `doubleValue()`
method is unknown. To solve this problem, you need some way to tell the compiler that you
intend to pass only numeric types to `T`. Furthermore, you need some way to ensure that only
numeric types are actually passed.

To handle such situations, Java provides **bounded types**. When specifying a type parameter,
you can create an upper bound that declares the superclass from which all type arguments
must be derived. This is accomplished through the use of an extends clause when specifying
the type parameter, as shown here:

```java
<T extends superclass>
```

This specifies that `T` can only be replaced by `superclass`, or **subclasses of superclass**. Thus,
`superclass` defines an inclusive, upper limit.

Here is a corrected version of `Stats` that uses a bounded type parameter:

```java
class Stats<T extends Number> {
    T[] nums; // nums is an array of type T

    // Pass the constructor a reference to
    // an array of type T.
    Stats(T[] o) {
        nums = o;
    }
    // Return type double in all cases.
    double average() {
        double sum = 0.0;
        for(int i=0; i < nums.length; i++)
            sum += nums[i].doubleValue(); // Error!!!
        return sum / nums.length;
    }
}

public class Main01 {
    public static void main(String[] args) {
        Stats<Integer> iob = new Stats<Integer>(new Integer[]{1, 2, 3, 4, 5});
        System.out.println(iob.average());

        Stats<Double> dob = new Stats<Double>(new Double[]{1.0, 2.0, 3.0, 4.0, 5.0});
        System.out.println(dob.average());
    }
}
```

**Output:**

```Bash
3.0
3.0
```

## Using Wildcard Arguments

Type safety prevents mixing types but can be too strict.  In a class like Stats<T extends Number>, we might want to 
compare averages of different numeric types (`Integer`, `Double`, etc.).

At first, you might think of implementing `isSameAvg()` like this:

```java
class Stats<T extends Number> {
    T[] nums; // nums is an array of type T

    // Pass the constructor a reference to
    // an array of type T.
    Stats(T[] o) {
        nums = o;
    }
    // Return type double in all cases.
    double average() {
        double sum = 0.0;
        for(int i=0; i < nums.length; i++)
            sum += nums[i].doubleValue();
        return sum / nums.length;
    }

    boolean isSameAvg(Stats<T> ob) {
        if(average() == ob.average())
            return true;
        return false;
    }
}

public class Main01 {
    public static void main(String[] args) {
        Stats<Integer> iob = new Stats<Integer>(new Integer[]{1, 2, 3, 4, 5});
        System.out.println(iob.average());

        Stats<Double> dob = new Stats<Double>(new Double[]{1.0, 2.0, 3.0, 4.0, 5.0});
        System.out.println(dob.average());

        if(iob.isSameAvg(dob))    // Compile-time error
            System.out.println("Averages are the same.");
        else
            System.out.println("Averages differ.");
    }
}
```

The trouble with this attempt is that it will work only with other `Stats` objects whose type is the
same as the invoking object. For example, if the invoking object is of type `Stats<Integer>`, then
the parameter ob must also be of type `Stats<Integer>`. It can’t be used to compare the average
of an object of type `Stats<Double>` with the average of an object of type `Stats<Short>`, for
example. Therefore, this approach won’t work except in a very narrow context and does not
yield a general (that is, generic) solution.

This is where wildcard arguments come in. A wildcard argument is specified by a question mark (?), and it 
represents an unknown type.

Here is a revised version of `isSameAvg()` that uses a wildcard argument:

```java
class Stats<T extends Number> {
    T[] nums; // nums is an array of type T

    // Pass the constructor a reference to
    // an array of type T.
    Stats(T[] o) {
        nums = o;
    }
    // Return type double in all cases.
    double average() {
        double sum = 0.0;
        for(int i=0; i < nums.length; i++)
            sum += nums[i].doubleValue();
        return sum / nums.length;
    }

    boolean isSameAvg(Stats<?> ob) {
        if(average() == ob.average())
            return true;
        return false;
    }
}

public class Main01 {
    public static void main(String[] args) {
        Stats<Integer> iob = new Stats<Integer>(new Integer[]{1, 2, 3, 4, 5});
        System.out.println(iob.average());

        Stats<Double> dob = new Stats<Double>(new Double[]{1.0, 2.0, 3.0, 4.0, 5.0});
        System.out.println(dob.average());

        if(iob.isSameAvg(dob))    
            System.out.println("Averages are the same.");
        else
            System.out.println("Averages differ.");
    }
}
```

It is important to understand that the wildcard does not affect what type of
`Stats` objects can be created. This is governed by the `extends` clause in the `Stats` declaration.
The wildcard simply matches any valid Stats object.

## Bounded Wildcards
Wildcard arguments can be bounded in much the same way that a type parameter can be
bounded. A bounded wildcard is especially important when you are creating a generic type
that will operate on a class hierarchy. Conside the following class hierarchy:

```java
// Two-dimensional coordinates.
class TwoD {
    int x, y;
    TwoD(int a, int b) {
        x = a;
        y = b;
    }
}
// Three-dimensional coordinates.
class ThreeD extends TwoD {
    int z;
    ThreeD(int a, int b, int c) {
        super(a, b);
        z = c;
    }
}
// Four-dimensional coordinates.
class FourD extends ThreeD {
    int t;
    FourD(int a, int b, int c, int d) {
        super(a, b, c);
        t = d;
    }
}
```

At the top of the hierarchy is `TwoD`, which encapsulates a two-dimensional, `XY`
coordinate. `TwoD` is inherited by `ThreeD`, which adds a third dimension, creating an `XYZ`
coordinate. `ThreeD` is inherited by `FourD`, which adds a fourth dimension (time), yielding a
four-dimensional coordinate.

Shown next is a generic class called `Coords`, which stores an array of coordinates:

```java
// This class holds an array of coordinate objects.
class Coords<T extends TwoD> {
    T[] coords;
    Coords(T[] o) { coords = o; }
}
```

Now, assume that you want to write a method that displays the `X` and `Y` coordinates for
each element in the coords array of a `Coords` object. Because all types of `Coords` objects
have at least two coordinates (`X` and `Y`), this is easy to do using a wildcard, as shown here:

```java
static void showXY(Coords<?> c) {
    System.out.println("X Y Coordinates:");
    for(int i=0; i < c.coords.length; i++)
        System.out.println(c.coords[i].x + " " +
                c.coords[i].y);
    System.out.println();
}
```

Because `Coords` is a bounded generic type that specifies `TwoD` as an upper bound, all
objects that can be used to create a `Coords` object will be arrays of type `TwoD`, or of classes
derived from `TwoD`. Thus, `showXY()` can display the contents of any `Coords` object.

However, what if you want to create a method that displays the `X`, `Y`, and `Z` coordinates of a
`ThreeD` or `FourD` object? The trouble is that not all `Coords` objects will have three coordinates,
because a `Coords<TwoD>` object will only have `X` and `Y`. Therefore, how do you write a method
that displays the `X`, `Y`, and `Z` coordinates for `Coords<ThreeD>` and `Coords<FourD>` objects,
while preventing that method from being used with `Coords<TwoD>` objects? The answer is the
bounded wildcard argument.

A bounded wildcard specifies either an **upper bound or a lower bound** for the type
argument. This enables you to restrict the types of objects upon which a method will
operate. The most common bounded wildcard is the upper bound, which is created using
an `extends` clause in much the same way it is used to create a bounded type.

Here is a method that uses an upper-bounded wildcard to display the `X`, `Y`, and `Z`:

```java
static void showXYZ(Coords<? extends ThreeD> c) {
    System.out.println("X Y Z Coordinates:");
    for(int i=0; i < c.coords.length; i++)
        System.out.println(c.coords[i].x + " " +
                c.coords[i].y + " " +
                c.coords[i].z);
    System.out.println();
}
```

Notice that an `extends` clause has been added to the wildcard in the declaration of
parameter `c`. It states that the `?` can match any type as long as it is `ThreeD`, or a class derived
from `ThreeD`.


## Creating a Generic Method

It is possible to create a generic method that is enclosed within a non-generic class. Consider
the following example:

```java
class GenMethDemo{
    public static <T extends Comparable<T>, V extends T>  boolean isIn(T x, V[] y){
        for (int i = 0; i < y.length; i++) {
            if(x.equals(y[i]))
                return true;
        }

        return false;
    }
}

public class Main01 {
    public static void main(String[] args) {
        // you can explicitly specify the type arguments, although usually they are inferred
        System.out.println(GenMethDemo.<Integer, Integer>isIn(1, new Integer[]{1,2,3}));

        // same as above, type arguments inferred
        System.out.println(GenMethDemo.isIn(1, new Integer[]{1,2,3})); 
    }
}
```

Let’s examine `isIn()` closely. First, notice how it is declared by this line:

```java
static <T extends Comparable<T>, V extends T> boolean isIn(T x, V[] y) 
```

The type parameters are declared before the return type of the method. Also note that
`T extends Comparable<T>`. `Comparable` is an interface declared in `java.lang`. A class that
implements `Comparable` defines objects that can be ordered. Thus, requiring an upper
bound of `Comparable` ensures that `isIn()` can be used only with objects that are capable of
being compared. `Comparable` is generic, and its type parameter specifies the type of objects
that it compares. (Shortly, you will see how to create a generic interface.) Next, notice that
the type `V` is upper-bounded by `T`. Thus, `V` must either be the same as type `T`, or a subclass
of `T`. This relationship enforces that `isIn()` can be called only with arguments that are
compatible with each other. Also notice that `isIn()` is static, enabling it to be called
independently of any object. Understand, though, that generic methods can be either static
or non-static. There is no restriction in this regard.

Note that when we specify the type arguments explicitly. There is nothing gained by specifying the type arguments:

```java
Integer[] nums = { 1, 2, 3, 4, 5 };
GenMethDemo.<Integer, Integer>isIn(2, nums);
```

Furthermore, JDK 8 improved type inference as it relates to methods. As a result, today there are fewer
cases in which explicit type arguments are needed.


## Generic Constructors

It is possible for constructors to be generic, even if their class is not. For example, consider
the following short program:

```java
class Gencons{
    private double val;

    // This is a generic constructor.
    <T extends Number> Gencons(T arg){
        val = arg.doubleValue();
    }

    void showVal(){
        System.out.println("val: " + val);
    }
}

public class Main01 {
    public static void main(String[] args) {
        Gencons gencons1 = new <Integer>Gencons(100);
        Gencons gencons2 = new Gencons(100);  // same as above, type argument inferred
        Gencons gencons3 = new <Double>Gencons(1.2);
    }
}
```

Note that `Gencons` is not a generic class. So we can't create references as follows:

```java
Gencons<Integer> obj1 = new Gencons(1);
```

## Generic Interfaces

Generic interfaces are specified just like generic classes. Here is an example. It creates an interface
called `MinMax` that declares the methods `min( )` and `max( )`, which are expected to return
the minimum and maximum value of some set of objects.

```java
// A generic interface example.
// A Min/Max interface.
interface MinMax<T extends Comparable<T>> {
    T min();
    T max();
}
// Now, implement MinMax
class MyClass<T extends Comparable<T>> implements MinMax<T> {
    T[] vals;
    MyClass(T[] o) { vals = o; }
    // Return the minimum value in vals.
    public T min() {
        T v = vals[0];
        for(int i=1; i < vals.length; i++)
            if(vals[i].compareTo(v) < 0) v = vals[i];
        return v;
    }
    // Return the maximum value in vals.
    public T max() {
        T v = vals[0];
        for(int i=1; i < vals.length; i++)
            if(vals[i].compareTo(v) > 0) v = vals[i];
        return v;
    }
}
class GenIFDemo {
    public static void main(String[] args) {
        Integer[] inums = {3, 6, 2, 8, 6 };
        Character[] chs = {'b', 'r', 'p', 'w' };
        MyClass<Integer> iob = new MyClass<Integer>(inums);
        MyClass<Character> cob = new MyClass<Character>(chs);
        System.out.println("Max value in inums: " + iob.max());
        System.out.println("Min value in inums: " + iob.min());
        System.out.println("Max value in chs: " + cob.max());
        System.out.println("Min value in chs: " + cob.min());
    }
}
```

Although most aspects of this program should be easy to understand, a couple of key
points need to be made. First, notice that MinMax is declared like this:

```java
interface MinMax<T extends Comparable<T>> {
```

In general, a generic interface is declared in the same way as is a generic class. In this case,
the type parameter is `T`, and its upper bound is `Comparable`. As explained earlier, `Comparable`
is an interface defined by `java.lang` that specifies how objects are compared. Its type parameter
specifies the type of the objects being compared.

Next, `MinMax` is implemented by `MyClass`. Notice the declaration of `MyClass`,
shown here:

```java
class MyClass<T extends Comparable<T>> implements MinMax<T> {
```

Pay special attention to the way that the type parameter `T` is declared by `MyClass` and then
passed to `MinMax`. Because `MinMax` requires a type that implements `Comparable`, the
implementing class (`MyClass` in this case) must specify the same bound. Furthermore, once
this bound has been established, there is no need to specify it again in the `implements` clause.
In fact, it would be wrong to do so.

```java
// This is wrong!
class MyClass<T extends Comparable<T>>
        implements MinMax<T extends Comparable<T>> {
```

Once the type parameter has been established, it is simply passed to the interface without
further modification.

In general, if a class implements a generic interface, then that class must also be generic,
at least to the extent that it takes a type parameter that is passed to the interface. For example,
the following attempt to declare `MyClass` is in error:

```java
class MyClass implements MinMax<T> { // Wrong!
```

Because `MyClass` does not declare a type parameter, there is no way to pass one to `MinMax`.
In this case, the identifier T is simply unknown, and the compiler reports an error. Of course,
if a class implements a specific type of generic interface, such as shown here:

```java
class MyClass implements MinMax<Integer> { // OK
```

then the implementing class does not need to be generic.


## Raw Types and Legacy Code

Before JDK 5, Java had no generics, so older code used plain classes (like `List`) without type parameters.
When generics were added, Java needed a way for old and new code to work together.

✅ Solution: Allow using a generic class without type arguments → called a raw type.
This lets legacy (pre-generics) code still compile and run with newer generic code.

⚠️ Drawback: Using raw types disables type safety — the compiler can’t check for type errors,
so runtime errors (like `ClassCastException`) can happen.

```java
// Demonstrate a raw type.
class Gen<T> {
    T ob; // declare an object of type T

    // Pass the constructor a reference to
    // an object of type T.
    Gen(T o) {
        ob = o;
    }

    // Return ob.
    T getOb() {
        return ob;
    }
}

public class Main01 {
    public static void main(String[] args) {
        // Create a Gen object for Integers.
        Gen<Integer> iOb = new Gen<Integer>(88);

        // Create a Gen object for Strings.
        Gen<String> strOb = new Gen<String>("Generics Test");

        // Create a raw-type Gen object and give it
        // a Double value.
        Gen raw = new Gen(Double.valueOf(98.6));

        // Cast here is necessary because type is unknown.
        double d = (Double) raw.getOb();
        System.out.println("value: " + d);

        // The use of a raw type can lead to run-time
        // exceptions. Here are some examples.
        // The following cast causes a run-time error!
        // int i = (Integer) raw.getOb(); // run-time error
        // This assignment overrides type safety.
        strOb = raw; // OK, but potentially wrong
        // String str = strOb.getOb(); // run-time error

        // This assignment also overrides type safety.
        raw = iOb; // OK, but potentially wrong
        // d = (Double) raw.getOb(); // run-time error
    }
}
```

This program contains several interesting things. First, a raw type of the generic `Gen` class
is created by the following declaration:

```java
Gen raw = new Gen(Double.valueOf(98.6));
```

Notice that no type arguments are specified. In essence, this creates a Gen object whose type
`T` is replaced by `Object`.

A raw type is not type safe. Thus, a variable of a raw type can be assigned a reference to
any type of `Gen` object. The reverse is also allowed; a variable of a specific `Gen` type can be
assigned a reference to a raw `Gen` object. However, both operations are potentially unsafe
because the type checking mechanism of generics is circumvented.

Because of the potential for danger inherent in raw types, if you compile with `-Xlint:unchecked`, 
javac displays unchecked warnings when a raw type is used in a way that might jeopardize type safety.

For the above program, the warnings were produced for the following two lines:

```Bash
Gen raw = new Gen(Double.valueOf(98.6));
strOb = raw;
```

In the first line, it is the call to the `Gen` constructor without a type argument that causes the
warning.

The second line produces a warning because `raw` could be pointing to any type. 
Suppose, if `raw` was actually pointing to `Gen<Integer>`, and later on try to do:

```java
String s = strOb.getOb(); // 💥 ClassCastException
```

This would cause a `ClassCastException` at runtime.

At first, you might think that this line should also generate an unchecked warning, but it does not:

```java
raw = iOb; // OK, but potentially wrong
```

It's allowed without warning because assigning a parameterized type to a raw type drops type information intentionally.
Warnings only appear when assigning a raw type to a parameterized type, since that’s potentially unsafe.


## Generic class hierarchies

Generic class can act as a  superclass or be a subclass. The key difference between
generic and non-generic hierarchies is that in a generic hierarchy, any type arguments
needed by a generic superclass must be passed up the hierarchy by all subclasses. This is
similar to the way that constructor arguments must be passed up a hierarchy.

```java
class Gen<T>{
    T ob;

    public Gen(T ob) {
        this.ob = ob;
    }

    T getOb(){
        return ob;
    }
}

class Gen2<T> extends Gen<T>{
    public Gen2(T ob) {
        super(ob);
    }
}
```

Notice how `Gen2` is declared by the following line:

```java
class Gen2<T> extends Gen<T> {
```

The type parameter `T` is specified by `Gen2` and is also passed to `Gen` in the `extends` clause.
This means that whatever type is passed to `Gen2` will also be passed to `Gen`. For example, this
declaration:

```java
Gen2<Integer> num = new Gen2<Integer>(100);
```

passes `Integer` as the type parameter to `Gen`. Thus, the `ob` inside the `Gen` portion of `Gen2`
will be of type `Integer`.

Notice also that `Gen2` does not use the type parameter `T` except to support the `Gen`
superclass. Thus, even if a subclass of a generic superclass would otherwise not need to be
generic, it still must specify the type parameter(s) required by its generic superclass.

Of course, a subclass is free to add its own type parameters, if needed. For example,
here is a variation on the preceding hierarchy in which `Gen2` adds a type parameter of its own:

```java
class Gen<T>{
    T ob;

    public Gen(T ob) {
        this.ob = ob;
    }

    T getOb(){
        return ob;
    }
}

class Gen2<T, V> extends Gen<T>{
    V ob2;
    public Gen2(T ob, V ob2) {
        super(ob);
        this.ob2 = ob2;
    }

    public V getOb2() {
        return ob2;
    }
}

public class Example08 {
    public static void main(String[] args) throws IOException {
        Gen2<String, Integer> aa = new Gen2<>("aa", 1);
        System.out.println(aa.getOb2());
    }
}
```

Notice the declaration of this version of `Gen2`, which is shown here:

```java
class Gen2<T, V> extends Gen<T> {
```

Here, `T` is the type passed to `Gen`, and `V` is the type that is specific to `Gen2`. `V` is used to
declare an object called `ob2`, and as a return type for the method `getOb2()`. In `main()`, a
`Gen2` object is created in which type parameter `T` is `String`, and type parameter `V` is `Integer`.


## A Generic Subclass

It is perfectly acceptable for a non-generic class to be the superclass of a generic subclass.
For example, consider this program:

```java
import java.io.IOException;

class NonGen {
    int num;

    public NonGen(int num) {
        this.num = num;
    }

    public int getNum() {
        return num;
    }
}

class Gen<T> extends NonGen {
    T ob;

    public Gen(T ob, int num) {
        super(num);
        this.ob = ob;
    }

    T getOb() {
        return ob;
    }
}

public class Example08 {
    public static void main(String[] args) throws IOException {
        Gen<String> aa = new Gen<>("aa", 1);
        System.out.println(aa.getOb());
    }
}
```

In the program, notice how `Gen` inherits `NonGen` in the following declaration:

```java
class Gen<T> extends NonGen {
```

Because `NonGen` is not generic, no type argument is specified. Thus, even though `Gen`
declares the type parameter `T`, it is not needed by (nor can it be used by) `NonGen`. Thus,
`NonGen` is inherited by `Gen` in the normal way. No special conditions apply.

## Run-Time Type Comparisons Within a Generic Hierarchy

The `instanceof` determines if an object is an instance of a class. It returns `true` if an
object is of the specified type or can be cast to the specified type. The `instanceof` operator
can be applied to objects of generic classes. The following class demonstrates some of the
type compatibility implications of a generic hierarchy

```java
class Gen<T> {
    T ob;

    Gen(T o) {
        ob = o;
    }

    // Return ob.
    T getOb() {
        return ob;
    }
}

// A subclass of Gen.
class Gen2<T> extends Gen<T> {
    Gen2(T o) {
        super(o);
    }
}

public class Example08 {
    public static void main(String[] args) throws IOException {
        Gen<Integer> iOb = new Gen<Integer>(88);
        Gen2<Integer> iOb2 = new Gen2<Integer>(99);
        Gen2<String> strOb2 = new Gen2<String>("Generics Test");

        if(iOb instanceof Gen<?>){
            System.out.println("iOb instanceof Gen<?>");
        }

        if(iOb instanceof Gen2<?>){
            System.out.println("iOb instanceof Gen2<?>");
        }

        ///

        if(iOb2 instanceof Gen<?>){
            System.out.println("iOb2 instanceof Gen<?>");
        }

        if(iOb2 instanceof Gen2<?>){
            System.out.println("iOb2 instanceof Gen2<?>");
        }

        //

        if(strOb2 instanceof Gen<?>){
            System.out.println("strOb2 instanceof Gen<?>");
        }

        if(strOb2 instanceof Gen2<?>){
            System.out.println("strOb2 instanceof Gen2<?>");
        }
    }
}
```

* `instanceof Gen<?>` simply checks whether the object is an instance of class `Gen` or its subclass.
* `instanceof Gen2<?>` checks whether the object is exactly `Gen2` or a subclass of it.

Thus:

* Objects of `Gen` match `Gen`, not `Gen2`.
* Objects of `Gen2` match both `Gen` and `Gen2`.


## Casting

You can cast one instance of a generic class into another only if the two are otherwise
compatible and their type arguments are the same. For example, assuming the foregoing
program, this cast is legal:

```java
(Gen<Integer>) iOb2 // legal
```
because `iOb2` includes an instance of `Gen<Integer>`. But, this cast:

```java
(Gen<Long>) iOb2 // illegal
```

is not legal because `iOb2` is not an instance of `Gen<Long>`.

Note: **Any subtype can be cast to its parent type as long as the generic type parameters are identical**.


## Overriding Methods in a Generic Class

A method in a generic class can be overridden just like any other method. For example,
consider this program in which the method `getOb()` is overridden.

```java

class Gen<T> {
    T ob;

    Gen(T o) {
        ob = o;
    }

    // Return ob.
    T getOb() {
        return ob;
    }
}

// A subclass of Gen.
class Gen2<T> extends Gen<T> {
    Gen2(T o) {
        super(o);
    }

    @Override
    T getOb() {
        System.out.print("Gen2's getOb(): ");
        return ob;
    }
}

public class Example08 {
    public static void main(String[] args) throws IOException {
        // Create a Gen object for Integers.
        Gen<Integer> iOb = new Gen<Integer>(88);

        // Create a Gen2 object for Integers.
        Gen2<Integer> iOb2 = new Gen2<Integer>(99);

        // Create a Gen2 object for Strings.
        Gen2<String> strOb2 = new Gen2<String>("Generics Test");
        
        System.out.println(iOb.getOb());
        System.out.println(iOb2.getOb());
        System.out.println(strOb2.getOb());
    }
}
```

**Output:**

```bash
88
Gen2's getOb(): 99
Gen2's getOb(): Generics Test
```

As the output confirms, the overridden version of `getOb()` is called for objects of type `Gen2`,
but the superclass version is called for objects of type `Gen`.


## Type Inference with Generics

Beginning with JDK 7, it became possible to shorten the syntax used to create an instance of
a generic type. To begin, consider the following generic class:

```java

class MyClass<T, V>{
    T ob1;
    V ob2;

    public MyClass(T ob1, V ob2) {
        this.ob1 = ob1;
        this.ob2 = ob2;
    }
}

public class Example08 {
    public static void main(String[] args) throws IOException {
        MyClass<Integer, String> aa = new MyClass<>(11, "aa");
    }
}
```

Prior to JDK 7, to create an instance of `MyClass`, you would have needed to use a statement
similar to the following:

```java
MyClass<Integer, String> mcOb = new MyClass<Integer, String>(98, "A String");
```

Here, the type arguments (which are Integer and String) are specified twice: first, when
`mcOb` is declared, and second, when a `MyClass` instance is created via new. Since generics
were introduced by JDK 5, this is the form required by all versions of Java prior to JDK 7.
Although there is nothing wrong, per se, with this form, it is a bit more verbose than it needs
to be. In the new clause, the type of the type arguments can be readily inferred from the type
of `mcOb`; therefore, there is really no reason that they need to be specified a second time.
To address this situation, JDK 7 added a syntactic element that lets you avoid the second
specification.

Today the preceding declaration can be rewritten as shown here:

```java
MyClass<Integer, String> mcOb = new MyClass<>(98, "A String");
```

Type inference can also be applied to parameter passing. For example, if the following
method is added to `MyClass`

```java
boolean isSame(MyClass<T, V> o) {
    if(ob1 == o.ob1 && ob2 == o.ob2) return true;
    else return false;
}
```

then the following call is legal:

```java
if(mcOb.isSame(new MyClass<>(1, "test"))) System.out.println("Same");
```

In this case, the type arguments for the argument passed to `isSame()` can be inferred 
from the parameter's type (and parameter type is determined by `mcOb`. 

## Erasure

1. Generics exist only at compile time.
    The JVM does not know type parameters.

2. During compilation, all generic type parameters are removed (erased).

3. Each type parameter is replaced by:
   * its bound (e.g., `T extends Number` → `Number`)
   * or `Object` if no bound is specified.

4. The compiler inserts casts where needed to preserve type safety.

5. As a result, different generic instantiations share the same runtime class.
(`List<Integer>` and `List<String>` both become `List`.)

Here an example of type erasure in action.

### Source Code (with generics)

```java
class Box<T> {
    T value;

    T getValue() {
        return value;
    }
}

Box<Integer> b = new Box<>();
Integer x = b.getValue();
```

### What the compiler turns it into (after erasure)

```java
class Box {
    Object value;

    Object getValue() {
        return value;
    }
}

Box b = new Box();
Integer x = (Integer) b.getValue();   // compiler inserts cast
```

Explanation

* `T` is erased to `Object` (no bound).
* All generic type information disappears.
* The compiler adds a cast so the program behaves as if generics exist.
* At runtime, `Box<Integer>` and `Box<String>` are both just `Box`.


### Bridge Methods

Consider the following program:

```java
class A {
    Object getVal() { return null; }
}

class B extends A {
    String getVal() { return "Hello"; }
}

public class Example08 {
    public static void main(String[] args){
        A ob = new B();
        System.out.println(ob.getVal());
    }
}
```

Here is what happens behind the scenes.

Imagine you have two construction crews: the **Java Language Crew** and the **JVM (Java Virtual Machine) Crew**.

**Step 1: The Java Language Crew's Rule (Your Code)**

You wrote two methods that look like this:

| Class | Method            | Crew's Opinion                                                                                                                                                 |
|-------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `A`   | `Object getVal()` | This is the original, basic method.                                                                                                                            |
| `B`   | `String getVal()` | This is an override. The Java Language Crew says,<br/> "This is allowed because a `String` is a special type of `Object` (covariant return type). Great job!". |


Result: Your source code is perfectly legal according to the high-level Java rules.

**Step 2: The JVM Crew's Rule (The Problem)**

The JVM Crew only works with very strict, low-level contracts called signatures. They don't care about 
**"covariant return types"** - they just see if the methods are exactly the same.

The JVM's rule for overriding is: The name, the inputs, AND the output MUST match exactly.

| Class | Method Signature (JVM View) |
|-------|-----------------------------|
| `A`   | `getVal()` returns `Object` |
| `B`   | `getVal()` returns `String` |

The JVM Crew looks at these two. They say, "Nope, the outputs are different! These are two separate methods, 
not an override!"


**Step 3: The Broken Contract (The Polymorphism Failure)**
   Now, look at the code that runs:

   ```java
   A ob = new B();      // We hold a B object with an A-shaped hand
   System.out.println(ob.getVal());
   ```

   1. **The Call:** Since your hand is `A`, the JVM is instructed to call the method it knows `A` has: the one that 
returns an `Object`.

   2. **The Search (Without Bridge):** The JVM looks inside the `B` object for a method that exactly matches
   the signature: `getVal()` returns `Object`.

   3. **The Failure:** It finds only the method you wrote: `getVal() returns String`. Since the output type is 
   wrong for the contract it was asked to fulfill, the JVM would ignore it and move up to class `A`.

   4. **Broken Polymorphism:** It would execute **A.getVal()**, which returns `null`. Polymorphism 
   (the idea that the subclass's version runs) is broken!

**Step-by-Step: The Bridge Method Fix**

The Java compiler sees this problem coming and acts as the Construction Manager by 
secretly adding the bridge method to **class B**.

**Step 4: The Compiler Adds the Bridge**

The compiler inserts a hidden, synthetic method into `B`:

```java
// Inside Class B, added by the compiler
Object getVal() {          // Signature matches the JVM's requirement!
    return this.getVal();  // Immediately calls the String getVal() you wrote!
}
```

**Step 5: Polymorphism is Restored**

When the code runs again:

1. **The Call:** The JVM is still looking for the contract: `getVal() returns Object`.

2. **The Search (With Bridge):** It looks in **B** and `FINDS` the new, hidden `Object getVal()` method. Success!

3. **The Execution:** The JVM runs this hidden method. The hidden method's only instruction is to 
call the `String getVal()` method you actually wrote.

4. **The Final Result:** Your String `getVal()` runs and prints `"Hello"`.

The bridge method exists solely to satisfy the strict, low-level signature requirement of the JVM while allowing
the high-level Java language feature of covariant return types to work correctly.

