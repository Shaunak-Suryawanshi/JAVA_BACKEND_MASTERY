# Java 8 Features — Revision Notes

## 1. Lambda Expressions

### Definition
A lambda expression is a concise way to implement a functional interface without creating an anonymous class.

### Syntax
```java
(parameters) -> expression
```

Or:

```java
(parameters) -> {
    // statements
};
```

### Example
```java
Runnable task = () -> System.out.println("Task running");

task.run();
```

### Sorting with Lambda
```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1);

numbers.sort((a, b) -> a.compareTo(b));
```

### Remember
- Introduced in Java 8.
- Reduces boilerplate code.
- Works with functional interfaces.
- Commonly used with Collections and Streams.

---

## 2. Functional Interfaces

### Definition
A functional interface is an interface containing exactly one abstract method (SAM — Single Abstract Method).

It can contain multiple default and static methods.

### Example
```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        Calculator calculator = (a, b) -> a + b;

        System.out.println(calculator.add(10, 20));
    }
}
```

### Built-in Functional Interfaces

Package: `java.util.function`

| Interface | Method | Purpose |
|---|---|---|
| Predicate<T> | test() | Checks a condition; returns boolean |
| Function<T,R> | apply() | Transforms input into output |
| Consumer<T> | accept() | Consumes input; returns void |
| Supplier<T> | get() | Supplies a value without input |

### Examples

**Predicate**
```java
Predicate<Integer> isEven = n -> n % 2 == 0;

System.out.println(isEven.test(10));
```

**Function**
```java
Function<String, Integer> getLength = s -> s.length();

System.out.println(getLength.apply("Java"));
```

**Consumer**
```java
Consumer<String> print = s -> System.out.println(s);

print.accept("Hello");
```

**Supplier**
```java
Supplier<Double> randomValue = () -> Math.random();

System.out.println(randomValue.get());
```

### Remember
- `@FunctionalInterface` is optional but recommended.
- A functional interface can have only one abstract method.
- Lambda expressions can implement functional interfaces.

---

## 3. Method References

### Definition
A method reference is a shorter form of a lambda expression when the lambda simply calls an existing method.

Operator: `::`

### Four Types

**1. Static method reference**
```java
Function<String, Integer> parser = Integer::parseInt;
```

**2. Particular object instance method**
```java
Consumer<String> printer = System.out::println;
```

**3. Arbitrary object instance method**
```java
Function<String, String> upper = String::toUpperCase;
```

**4. Constructor reference**
```java
Supplier<ArrayList<String>> list = ArrayList::new;
```

### Remember
Use method references when they make the lambda simpler and clearer.

---

## 4. Stream API

### Definition
Stream API processes data from collections or other sources using a sequence of operations.

It does not store data itself.

### Stream Pipeline

1. Source
2. Intermediate operations
3. Terminal operation

Example:
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .collect(Collectors.toList());

System.out.println(result);
```

Output:
```text
[4, 16, 36]
```

### Creating Streams
```java
list.stream();

Arrays.stream(array);

Stream.of(1, 2, 3);
```

### Intermediate Operations

These return another stream.

| Method | Purpose |
|---|---|
| filter() | Filters elements |
| map() | Transforms elements |
| flatMap() | Flattens nested streams |
| sorted() | Sorts elements |
| distinct() | Removes duplicates |
| limit() | Limits elements |
| skip() | Skips elements |
| peek() | Helps inspect elements during processing |

### Terminal Operations

These produce a result or side effect.

| Method | Purpose |
|---|---|
| forEach() | Performs an action |
| collect() | Collects results |
| count() | Counts elements |
| reduce() | Combines elements into one result |
| min() | Finds minimum |
| max() | Finds maximum |
| anyMatch() | Checks whether any element matches |
| allMatch() | Checks whether all elements match |
| noneMatch() | Checks whether no elements match |
| findFirst() | Finds first element |
| findAny() | Finds any element |

### Collectors

```java
collect(Collectors.toList());

collect(Collectors.toSet());

collect(Collectors.joining(", "));
```

### reduce()

Combines elements into one result.

```java
int sum = numbers.stream()
        .reduce(0, (a, b) -> a + b);
```

Here:
- `0` is the identity.
- `(a, b) -> a + b` is the accumulator.

### flatMap()

Converts nested streams into one stream.

```java
List<List<Integer>> nested = Arrays.asList(
        Arrays.asList(1, 2),
        Arrays.asList(3, 4)
);

List<Integer> result = nested.stream()
        .flatMap(list -> list.stream())
        .collect(Collectors.toList());
```

Output:
```text
[1, 2, 3, 4]
```

### Important Stream Concepts

- Intermediate operations are lazy.
- Processing starts when a terminal operation executes.
- A stream cannot be reused after a terminal operation.
- Streams do not modify the original collection by default.
- `min()` and `max()` return Optional.
- Short-circuiting operations include `findFirst()`, `findAny()`, `anyMatch()`, `allMatch()`, `noneMatch()` and `limit()`.

### Collection vs Stream

| Collection | Stream |
|---|---|
| Stores data | Processes data |
| Can be traversed multiple times | Cannot be reused after terminal operation |
| Supports adding/removing elements | Does not directly modify the source |
| Data structure | Processing abstraction |

---

## 5. Optional

### Definition
Optional is a container object that may or may not contain a non-null value.

It helps represent potentially missing results and reduces careless null handling.

### Creating Optional

```java
Optional<String> a = Optional.of("Java");

Optional<String> b = Optional.ofNullable(null);

Optional<String> c = Optional.empty();
```

### Important Methods

**isPresent()**
```java
optional.isPresent();
```

Checks whether a value exists.

**ifPresent()**
```java
optional.ifPresent(System.out::println);
```

Executes an action when a value exists.

**orElse()**
```java
String name = optional.orElse("Unknown");
```

Returns the value or a default value. The fallback expression is evaluated eagerly.

**orElseGet()**
```java
String name = optional.orElseGet(() -> "Unknown");
```

Evaluates the fallback supplier only when the Optional is empty.

**orElseThrow()**
```java
String name = optional.orElseThrow(
        () -> new RuntimeException("Value missing")
);
```

**map()**
```java
Optional<Integer> length =
        optional.map(String::length);
```

Transforms the contained value.

**flatMap()**

Transforms a value when the mapping function already returns an Optional, avoiding nested Optional objects.

### Important Difference

| Method | Behavior |
|---|---|
| of() | Rejects null |
| ofNullable() | Accepts null |
| empty() | Creates empty Optional |
| orElse() | Eager fallback |
| orElseGet() | Lazy fallback |

### Java Version Note

`Optional.isEmpty()` was introduced in Java 11. Use `!optional.isPresent()` when working with Java 8.

### Backend Example

A repository method may return:

```java
Optional<Employee> findById(int id);
```

The caller must handle the possibility that the employee does not exist.

### Best Practice
- Mainly use Optional as a return type.
- Avoid using it as a replacement for every nullable field or parameter.
- Avoid calling `get()` without checking whether a value exists.

---

## 6. Comparable vs Comparator

### Comparable

Defines the natural/default ordering of objects.

Package: `java.lang`

Method:
```java
int compareTo(T obj);
```

Example:
```java
class Employee implements Comparable<Employee> {

    int id;

    Employee(int id) {
        this.id = id;
    }

    @Override
    public int compareTo(Employee other) {
        return Integer.compare(this.id, other.id);
    }
}
```

### Comparator

Defines custom sorting logic separately from the class.

Package: `java.util`

Method:
```java
int compare(T obj1, T obj2);
```

Example:
```java
Comparator<Employee> byName =
        (e1, e2) -> e1.name.compareTo(e2.name);
```

### Main Differences

| Comparable | Comparator |
|---|---|
| Natural ordering | Custom ordering |
| compareTo() | compare() |
| Implemented inside the class | Defined separately |
| Generally one natural ordering | Multiple custom orderings |
| java.lang | java.util |

### Java 8 Comparator Methods

**comparing()**
```java
employees.sort(Comparator.comparing(e -> e.name));
```

**comparingDouble()**
```java
employees.sort(
    Comparator.comparingDouble((Employee e) -> e.salary)
);
```

**thenComparing()**
```java
employees.sort(
    Comparator.comparingDouble((Employee e) -> e.salary)
              .thenComparing(e -> e.name)
);
```

**reversed()**
```java
employees.sort(
    Comparator.comparingDouble((Employee e) -> e.salary)
              .reversed()
);
```

**nullsFirst()**
```java
Comparator<String> comparator =
        Comparator.nullsFirst(Comparator.naturalOrder());
```

### compare() Return Values

- Negative: first object comes before second.
- Zero: equal for this comparison.
- Positive: first object comes after second.

Prefer `Integer.compare()` and `Double.compare()` over subtraction to avoid integer overflow.

---

## 7. Java 8 Date and Time API

Package: `java.time`

Java 8 introduced a modern, immutable API for handling dates, times and time zones.

### Important Classes

| Class | Purpose |
|---|---|
| LocalDate | Date only |
| LocalTime | Time only |
| LocalDateTime | Date and time, without timezone |
| ZonedDateTime | Date, time and timezone |
| Instant | Point on the UTC timeline |
| Period | Date-based difference |
| Duration | Time-based difference |
| DateTimeFormatter | Formatting and parsing |

### LocalDate

```java
LocalDate today = LocalDate.now();

LocalDate date = LocalDate.of(2026, 11, 2);

today.plusDays(10);

today.minusMonths(2);
```

### LocalTime

```java
LocalTime now = LocalTime.now();

LocalTime time = LocalTime.of(9, 30);

time.plusHours(2);
```

### LocalDateTime

```java
LocalDateTime now = LocalDateTime.now();

LocalDateTime meeting =
        LocalDateTime.of(2026, 11, 5, 14, 30);
```

### DateTimeFormatter

Formatting:
```java
LocalDate today = LocalDate.now();

DateTimeFormatter formatter =
        DateTimeFormatter.ofPattern("dd-MM-yyyy");

String formatted = today.format(formatter);
```

Parsing:
```java
String date = "25-12-2026";

LocalDate parsedDate =
        LocalDate.parse(date, formatter);
```

### Common Formatting Patterns

| Pattern | Meaning |
|---|---|
| dd | Day |
| MM | Month |
| yyyy | Year |
| HH | Hour (24-hour) |
| mm | Minutes |
| ss | Seconds |

Remember: `MM` = month; `mm` = minutes.

### Period

Used for date-based differences.

```java
LocalDate start = LocalDate.of(2024, 1, 1);
LocalDate end = LocalDate.of(2026, 1, 1);

Period period = Period.between(start, end);

System.out.println(period.getYears());
```

### Duration

Used for time-based differences.

```java
LocalTime start = LocalTime.of(10, 0);
LocalTime end = LocalTime.of(12, 30);

Duration duration = Duration.between(start, end);

System.out.println(duration.toHours());
System.out.println(duration.toMinutes());
```

### Period vs Duration

| Period | Duration |
|---|---|
| Date-based | Time-based |
| Years, months, days | Seconds, nanoseconds |
| Commonly used with LocalDate | Commonly used with LocalTime and Instant |

### Timezone Handling

- LocalDateTime does not contain timezone information.
- ZonedDateTime includes timezone information.
- Instant represents a point on the UTC timeline.

For globally distributed backend applications, Instant is often useful for storing consistent timestamps.

---

# Java 8 Interview Quick Revision

1. What is a lambda expression?
2. What is a functional interface?
3. What is SAM?
4. Difference between Predicate, Function, Consumer and Supplier?
5. What is a method reference?
6. What are the four types of method references?
7. What is Stream API?
8. Difference between intermediate and terminal operations?
9. What is lazy evaluation?
10. Difference between map() and flatMap()?
11. Difference between reduce() and collect()?
12. What is Optional?
13. Difference between orElse() and orElseGet()?
14. Difference between Comparable and Comparator?
15. What does compareTo() return?
16. What is Comparator.comparing()?
17. Difference between LocalDate and LocalDateTime?
18. Difference between Period and Duration?
19. What is DateTimeFormatter?
20. Difference between Instant and ZonedDateTime?

---

# Java 8 Important Packages

| Package | Use |
|---|---|
| java.util.function | Functional interfaces |
| java.util.stream | Stream API |
| java.util | Collections, Comparator, Optional |
| java.time | Date and Time API |
| java.time.format | Date formatting and parsing |

---

# Final Revision Checklist

- [ ] Lambda expressions
- [ ] Functional interfaces
- [ ] Predicate, Function, Consumer, Supplier
- [ ] Method references
- [ ] Stream API — Part 1
- [ ] Stream API — Part 2
- [ ] Optional
- [ ] Comparable vs Comparator
- [ ] Date and Time API
- [ ] Complete practical examples

**Java 8 section completed.**

Next roadmap phase: **JVM Architecture and Memory Management**.

Focus areas: JVM/JRE/JDK, class loading, runtime memory areas, Stack vs Heap, Metaspace, object allocation, garbage collection, memory leaks, OutOfMemoryError and StackOverflowError.