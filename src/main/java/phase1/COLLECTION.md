# Java HashMap Internals — Java 8

## 1. What is HashMap?

HashMap stores data in key-value pairs.

```java
Map<Integer, String> map = new HashMap<>();
map.put(101, "Rahul");
map.put(102, "Amit");
```

- Keys must be unique.
- Values can be duplicated.
- One null key is allowed.
- Multiple null values are allowed.
- No guaranteed insertion order.
- HashMap is not thread-safe.

## 2. How HashMap Works Internally

HashMap uses:
- Hashing
- Buckets
- `hashCode()`
- `equals()`
- Linked Lists
- Red-Black Trees (Java 8)

Basic process:

1. Calculate the key's hash.
2. Calculate the bucket index.
3. Find the bucket.
4. Use `equals()` to check whether the key already exists.
5. Update the value if the key exists; otherwise, insert a new entry.

### Bucket index calculation

Java 8 uses:

```java
index = (n - 1) & hash;
```

Where:
- `n` = table capacity
- `hash` = spread hash value

Java 8 also spreads the hash using:

```java
h ^ (h >>> 16)
```

## 3. Collision

A collision occurs when multiple keys map to the same bucket.

Java 8 handles collisions using:

- Linked List initially.
- Red-Black Tree when treeification conditions are met.

## 4. equals() and hashCode()

### equals()

Compares objects based on logical equality.

### hashCode()

Returns an integer hash value used by hash-based collections.

### Important Contract

1. Equal objects must have the same hash code.
2. Same hash codes do not guarantee equal objects.
3. Whenever you override `equals()`, also override `hashCode()`.

### Example

```java
import java.util.Objects;

class Employee {

    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }

        Employee employee = (Employee) obj;

        return id == employee.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

## 5. HashMap Capacity

Capacity means the number of buckets in the internal table.

Default initial capacity in Java 8:

```text
16
```

The table is allocated lazily when entries are first inserted.

## 6. Load Factor

Load factor determines how full a HashMap can become before resizing.

Default load factor:

```text
0.75
```

Formula:

```text
Threshold = Capacity × Load Factor
```

Example:

```text
Capacity = 16
Load Factor = 0.75

Threshold = 16 × 0.75
Threshold = 12
```

## 7. Resizing

When the number of entries exceeds the threshold:

- HashMap increases its capacity.
- Capacity generally doubles.
- Entries are redistributed into the new table.

Example:

| Capacity | Threshold |
|---:|---:|
| 16 | 12 |
| 32 | 24 |
| 64 | 48 |
| 128 | 96 |

## 8. Treeification

Treeification converts a collision-heavy linked list into a Red-Black Tree.

Java 8 implementation details:

| Condition | Value |
|---|---:|
| Treeification threshold | 8 nodes |
| Minimum table capacity | 64 |
| Untreeification threshold | 6 nodes |

If table capacity is below 64, HashMap generally resizes instead of treeifying.

## 9. Time Complexity

| Operation | Average |
|---|---|
| put() | O(1) |
| get() | O(1) |
| remove() | O(1) |
| containsKey() | O(1) |

Collision-heavy buckets can affect performance.

## 10. Practical Example

```java
import java.util.HashMap;
import java.util.Map;

public class Main {

    public static void main(String[] args) {

        Map<Integer, String> employees = new HashMap<>();

        employees.put(101, "Rahul");
        employees.put(102, "Amit");
        employees.put(103, "Priya");

        System.out.println(employees.get(101));

        System.out.println(employees.size());

        System.out.println(employees.containsKey(102));
    }
}
```

Output:

```text
Rahul
3
true
```

## 11. Important Interview Questions

1. How does HashMap work internally?
2. What is hashing?
3. What is a collision?
4. Why does HashMap use `hashCode()` and `equals()`?
5. What happens if only `equals()` is overridden?
6. What is the default HashMap capacity?
7. What is the default load factor?
8. What is resizing?
9. What is treeification?
10. Why is HashMap not thread-safe?

## Quick Revision

- HashMap stores key-value pairs.
- Default capacity = 16.
- Default load factor = 0.75.
- Default threshold = 12.
- Capacity generally doubles during resizing.
- Java 8 uses linked lists and Red-Black Trees for collision handling.
- `hashCode()` helps locate a bucket.
- `equals()` checks logical equality.
- Equal objects must have equal hash codes.
- HashMap is not thread-safe.

**Next topic:** Java Generics — type safety, generic classes, generic methods, and bounded types.
