# Java Generics — Java 8

## 1. What are Generics?

Generics allow classes, interfaces and methods to work with different data types while providing compile-time type safety.

### Why do we need Generics?

Without Generics:
- Different data types can accidentally be inserted into a collection.
- Typecasting is required.
- Incorrect typecasting can cause runtime errors.

With Generics:
- Type safety is provided.
- Unwanted data types are rejected at compile time.
- Typecasting is generally unnecessary when retrieving values.

### Without Generics

```java
ArrayList list = new ArrayList();

list.add("Java");
list.add(100);

String value = (String) list.get(1); // Runtime error
```

### With Generics

```java
ArrayList<String> list = new ArrayList<>();

list.add("Java");
// list.add(100); // Compile-time error

String value = list.get(0);
```

## 2. Generic Type Naming Conventions

| Symbol | Meaning |
|---|---|
| T | Type |
| E | Element |
| K | Key |
| V | Value |
| N | Number |

These are naming conventions, not Java keywords.

Example:

```java
List<String> names;
Map<Integer, String> employees;
```

## 3. Generic Class

A generic class works with different data types.

```java
class Box<T> {

    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}
```

Usage:

```java
Box<String> stringBox = new Box<>();
stringBox.setValue("Java");

Box<Integer> integerBox = new Box<>();
integerBox.setValue(100);
```

Here, T is replaced by the specified reference type.

## 4. Generic Method

A generic method declares its own type parameter.

```java
public static <T> void printData(T data) {

    System.out.println(data);
}
```

Usage:

```java
printData("Java");
printData(100);
printData(10.5);
```

Important syntax:

```java
public static <T> void methodName(T value)
```

`<T>` declares the method's type parameter.

## 5. Bounded Type Parameters

Bounded types restrict which types can be used.

Syntax:

```java
<T extends Number>
```

This accepts Number and its subclasses.

Example:

```java
public static <T extends Number> void printNumber(T number) {

    System.out.println(number);
}
```

Allowed:
- Integer
- Double
- Float
- Long

Not allowed:
- String
- Boolean

Remember: `extends` can be used to specify bounds involving both classes and interfaces.

## 6. Wildcards (?)

A wildcard represents an unknown type.

Example:

```java
public static void printList(List<?> list) {

    for (Object item : list) {
        System.out.println(item);
    }
}
```

It accepts:
- List<String>
- List<Integer>
- List<Double>

A wildcard is useful when the exact element type does not matter.

## 7. Generic Invariance

Generics are invariant.

Even though Integer extends Number:

```java
List<Integer> integers = new ArrayList<>();

// Not allowed:
List<Number> numbers = integers;
```

Why?

If this assignment were allowed, a Double could be inserted into the Integer list, breaking type safety.

Therefore:

`List<Integer>` is NOT a subtype of `List<Number>`.

## 8. Upper-Bounded Wildcard (? extends)

Syntax:

```java
List<? extends Number>
```

Meaning:

Accept a list of Number or any subclass of Number.

Examples:
- List<Integer>
- List<Double>
- List<Float>

Example:

```java
public static double calculateSum(List<? extends Number> numbers) {

    double sum = 0;

    for (Number number : numbers) {
        sum += number.doubleValue();
    }

    return sum;
}
```

Important:
- Reading elements as Number is allowed.
- Adding Integer, Double or other specific values is not allowed.
- Adding null is allowed, but normally not useful.

Reason: Java does not know the exact element type of the list.

## 9. Lower-Bounded Wildcard (? super)

Syntax:

```java
List<? super Integer>
```

Meaning:

Accept Integer or any of its supertypes.

Examples:
- List<Integer>
- List<Number>
- List<Object>

Example:

```java
public static void addNumbers(List<? super Integer> numbers) {

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
}
```

Important:
- Adding Integer values is allowed.
- When reading an element, the compiler guarantees only Object.

## 10. PECS Principle

PECS stands for:

**Producer Extends, Consumer Super.**

### Producer Extends

Use `? extends T` when a collection produces values that your method reads.

```java
public static void readNumbers(List<? extends Number> numbers) {

    for (Number number : numbers) {
        System.out.println(number);
    }
}
```

### Consumer Super

Use `? super T` when a collection consumes values that your method adds.

```java
public static void addIntegers(List<? super Integer> numbers) {

    numbers.add(100);
    numbers.add(200);
}
```

Remember:

- Producer = extends
- Consumer = super

## 11. Practical Example — Copying Data

```java
import java.util.*;

public class Main {

    public static <T> void copyData(
            List<? super T> destination,
            List<? extends T> source) {

        for (T item : source) {
            destination.add(item);
        }
    }

    public static void main(String[] args) {

        List<Integer> source = Arrays.asList(10, 20, 30);

        List<Number> destination = new ArrayList<>();

        copyData(destination, source);

        System.out.println(destination);
    }
}
```

Output:

```text
[10, 20, 30]
```

Explanation:

- Source produces T values: `? extends T`.
- Destination consumes T values: `? super T`.

## 12. Type Erasure

Java implements Generics using a mechanism called type erasure.

During compilation:
- Generic type information is largely removed.
- The compiler performs type checks.
- Necessary casts are inserted.
- Bounded type parameters are generally replaced by their bound.

Example:

```java
List<String> names = new ArrayList<>();
```

At runtime, the generic type argument String is generally not retained as the collection's runtime type.

## 13. Important Rules

| Rule | Explanation |
|---|---|
| Generics use reference types | Integer, String, Double |
| Primitive types are not allowed | int, double, boolean |
| Generic class | `class Box<T>` |
| Generic method | `<T> void method(T value)` |
| Bounded type | `<T extends Number>` |
| Unknown type | `<?>` |
| Upper bound | `<? extends Number>` |
| Lower bound | `<? super Integer>` |
| PECS | Producer Extends, Consumer Super |

Use wrapper classes instead of primitive types:

| Primitive | Wrapper |
|---|---|
| int | Integer |
| double | Double |
| float | Float |
| boolean | Boolean |
| long | Long |
| char | Character |

## 14. Important Interview Questions

1. What are Generics in Java?
2. Why do we need Generics?
3. What is compile-time type safety?
4. Difference between generic class and generic method?
5. What are T, E, K and V?
6. What is a bounded type parameter?
7. What is a wildcard?
8. What is the difference between `List<Object>` and `List<?>`?
9. Why are Generics invariant?
10. What is `? extends`?
11. What is `? super`?
12. What is PECS?
13. Why can't primitive types be used in Generics?
14. What is type erasure?
15. Can you add elements to `List<? extends Number>`?
16. Can you add Integer values to `List<? super Integer>`?

## Quick Revision

- Generics provide compile-time type safety.
- Generic class: `Box<T>`.
- Generic method: `<T> void printData(T data)`.
- Bounded type: `<T extends Number>`.
- Wildcard: `?`.
- Upper bound: `? extends T`.
- Lower bound: `? super T`.
- PECS = Producer Extends, Consumer Super.
- Generics are invariant.
- Java uses type erasure.
- Generics cannot use primitive types directly.

**Next Topic: Exception Handling in Java.**