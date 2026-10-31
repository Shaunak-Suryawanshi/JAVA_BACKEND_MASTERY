# Java Strings, StringBuilder, Immutability & Object Class

## 1. What is a String?

A `String` is an object in Java that represents a sequence of characters.

```java
String name = "Java";
```

- `String` is a class, not a primitive data type.
- Strings are immutable.
- Java provides the `String` class in `java.lang`.
- String literals can be reused through the String Pool.

## 2. String Pool

The String Pool is a special area managed within the Java heap where Java reuses interned string values.

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2); // true
```

Both variables refer to the same pooled String object.

### Using `new`

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);      // false
System.out.println(s1.equals(s2)); // true
```

`new String()` explicitly creates a separate String object.

### `==` vs `equals()`

| Operator / Method | Meaning |
|---|---|
| `==` | Compares references (object identity) |
| `equals()` | Compares String content |

**Interview point:** Use `equals()` when comparing String values.

## 3. String Immutability

Immutable means an object's state cannot be changed after it is created.

```java
String name = "Java";
name.concat(" Backend");

System.out.println(name); // Java
```

`concat()` creates a new String. It does not modify the original.

To keep the result:

```java
name = name.concat(" Backend");
System.out.println(name); // Java Backend
```

### Why is String immutable?

1. **Security:** Strings are commonly used for paths, usernames, URLs and configuration values.
2. **String Pool:** Shared String objects can be safely reused.
3. **Thread safety:** Multiple threads can read an immutable String without changing its state.
4. **Hashing:** Its hash code can be safely cached because its content does not change.

## 4. String vs StringBuilder vs StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Mutable? | No | Yes | Yes |
| Thread-safe? | Safe for concurrent reads | No | Synchronized methods |
| Performance for repeated changes | Often inefficient | Usually faster | Usually slower than StringBuilder |
| Best use | Fixed text | Repeated modifications in one thread | Shared mutable text with synchronized operations |

### StringBuilder example

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Backend");
sb.insert(0, "Learn ");
sb.replace(0, 5, "Master");

System.out.println(sb);
```

Important methods:

- `append()` – adds text at the end.
- `insert()` – inserts text at an index.
- `replace()` – replaces a range.
- `delete()` – removes a range.
- `reverse()` – reverses the sequence.
- `toString()` – converts it into a String.

**Interview point:** Use StringBuilder when repeatedly modifying text, such as building a response or constructing a large string inside a loop.

## 5. Important String Methods

```java
String text = "Java Backend";
```

| Method | Purpose |
|---|---|
| `length()` | Returns the number of characters |
| `charAt(index)` | Returns the character at an index |
| `toUpperCase()` | Converts to uppercase |
| `toLowerCase()` | Converts to lowercase |
| `contains("Java")` | Checks whether text exists |
| `startsWith("Java")` | Checks the beginning |
| `endsWith("Backend")` | Checks the ending |
| `substring(0, 4)` | Extracts characters from index 0 to 3 |
| `replace("Java", "Python")` | Replaces matching text |
| `trim()` | Removes leading/trailing characters with values up to U+0020 |
| `isEmpty()` | Checks whether length is zero |
| `split(" ")` | Splits text into an array |
| `String.join("-", "A", "B")` | Joins values with a delimiter |

**Remember:**
- Java indexes start at `0`.
- `substring(start, end)` includes `start` but excludes `end`.
- `isEmpty()` does not consider whitespace-only text empty.
- `trim()` is not a complete Unicode whitespace solution.
- `strip()` and `isBlank()` were introduced in Java 11, so they are not available in Java 8.
- `String.join()` is available from Java 8.

## 6. String in Java 8 Internals

In Java 8, String internally uses a private final `char[]` to store characters.

Conceptually:

```java
public final class String {
    private final char[] value;
}
```

This is a simplified illustration, not the complete source code.

- `private` prevents direct external access.
- `final` prevents reassignment of the array reference.
- String's implementation does not expose a way to mutate its internal character data.

Newer Java versions changed the internal representation, so remember this explanation specifically for Java 8.

## 7. Object Class

`Object` is the root class of Java's class hierarchy. Every Java class directly or indirectly inherits from it.

Important methods:

| Method | Purpose |
|---|---|
| `toString()` | Returns a textual representation of an object |
| `equals()` | Checks logical equality |
| `hashCode()` | Returns an integer hash value |
| `getClass()` | Returns the runtime class |
| `wait()` | Makes a thread wait |
| `notify()` | Wakes one waiting thread |
| `notifyAll()` | Wakes all waiting threads |

### toString()

```java
class Employee {
    int id = 101;

    @Override
    public String toString() {
        return "Employee ID: " + id;
    }
}
```

Without overriding `toString()`, the default output usually contains the class name and a hash-based representation.

### equals() and hashCode()

These methods are especially important for Collections and HashMap.

Contract:

1. If two objects are equal according to `equals()`, they must return the same `hashCode()`.
2. Two objects having the same hash code are not necessarily equal.
3. If an object's equality-relevant state does not change, its hash code should remain consistent during the application's execution.

We will study these deeply in the Collections and HashMap topic.

## 8. Practical Java Example

Save as `StringPractice.java`:

```java
public class StringPractice {

    public static void main(String[] args) {

        // String Pool
        String s1 = "Java";
        String s2 = "Java";

        System.out.println(s1 == s2); // true

        // new String creates a separate object
        String s3 = new String("Java");

        System.out.println(s1 == s3);      // false
        System.out.println(s1.equals(s3)); // true

        // Immutability
        String language = "Java";
        language.concat(" Backend");

        System.out.println(language); // Java

        // StringBuilder
        StringBuilder sb = new StringBuilder("Java");
        sb.append(" Backend");

        System.out.println(sb); // Java Backend

        // String methods
        String text = "Java Backend Development";

        System.out.println(text.length());
        System.out.println(text.toUpperCase());
        System.out.println(text.substring(0, 4));
        System.out.println(text.contains("Backend"));
    }
}
```

## 9. Interview Questions

**Q1. Is String a primitive or reference type?**

String is a reference type because it is a class in Java.

**Q2. Why is String immutable?**

For security, String Pool reuse, safe concurrent reads and reliable hashing.

**Q3. What is the difference between `==` and `equals()`?**

`==` compares references, while `equals()` compares content for String objects.

**Q4. What is the String Pool?**

It is an area within the Java heap where Java reuses interned String objects.

**Q5. What happens when you concatenate a String?**

A new String is created; the original String is not modified.

**Q6. Difference between StringBuilder and StringBuffer?**

Both are mutable. StringBuilder is not synchronized; StringBuffer synchronizes its methods.

**Q7. Why is StringBuilder preferred in loops?**

It avoids creating a new String object for every intermediate modification.

**Q8. Can we modify a String object?**

No. We can assign a new String to the same variable, but cannot change the original object's content.

**Q9. What is the parent class of all Java classes?**

`java.lang.Object`.

**Q10. Why should equals() and hashCode() be overridden together?**

Because hash-based collections rely on both methods. Equal objects must have the same hash code.

## 10. Quick Revision

- String = immutable sequence of characters.
- String is a reference type.
- String Pool = reuses interned Strings in the heap.
- `==` = reference comparison.
- `equals()` = content comparison for String.
- String concatenation creates a new String.
- StringBuilder = mutable, not synchronized.
- StringBuffer = mutable, synchronized methods.
- Use StringBuilder for repeated modifications in one thread.
- Java 8 String internally uses `char[]`.
- Object is the root class of Java's class hierarchy.
- `equals()` and `hashCode()` are important for HashMap and other collections.

---

**Next topic:** Java Collections Framework — List, Set, Queue and Map, followed by HashMap internals, `equals()` and `hashCode()`, and Generics.