## TOC

- [Generics](#generics)
- [A Simple Generics Example](#a-simple-generics-example)
- [Type Erasure](#type-erasure)
- [Generics Work Only with Reference Types](#generics-work-only-with-reference-types)
- [Generic Types Differ Based on Their Type Arguments](#generic-types-differ-based-on-their-type-arguments)
- [How Generics Improve Type Safety](#how-generics-improve-type-safety)
- [A Generic Class with Two Type Parameters](#a-generic-class-with-two-type-parameters)
- [Bounded Types](#bounded-types)
- [Using Wildcard Arguments](#using-wildcard-arguments)
- [Bounded Wildcards](#bounded-wildcards)
- [Creating a Generic Method](#creating-a-generic-method)

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




