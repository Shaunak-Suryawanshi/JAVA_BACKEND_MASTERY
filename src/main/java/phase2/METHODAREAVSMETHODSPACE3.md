# Method Area vs Metaspace — Java 8

## 1. First, Imagine a School 🏫

Imagine a school:

- Students are objects.
- Every student has their own name and roll number.
- The school has shared information, such as rules, class details and subjects.

Java works in a similar way.

The JVM needs:
- Information about classes.
- Actual objects created from those classes.

## 2. What is Method Area?

**Simple definition:**

Method Area is a shared memory area in the JVM that stores information about loaded classes.

Think of it as a school library containing the blueprints of every class.

It stores information such as:

- Class metadata.
- Method information.
- Field information.
- Runtime constant pool.
- Method bytecode.

### Why do we need it?

Imagine creating 500 Employee objects.

Does Java need to load the Employee class definition 500 times?

No!

The JVM loads the class information and uses it to create multiple objects.

**Remember:** One loaded class definition can be shared by many objects.

## 3. What is Metaspace?

**Simple definition:**

Metaspace is the native-memory area used by the HotSpot JVM to store class metadata.

Think of it this way:

- Method Area = the JVM's logical blueprint library.
- Metaspace = where HotSpot keeps that class metadata.

They are related but are not exactly the same thing.

### Why was Metaspace introduced?

Before Java 8, HotSpot used something called **PermGen (Permanent Generation)** to store much of its class metadata.

PermGen had a configured maximum size.

When it became full, Java applications could throw:

```text
java.lang.OutOfMemoryError: PermGen space
```

Java 8 removed PermGen and introduced Metaspace.

Metaspace uses native memory and can grow as needed, subject to available memory and configured limits.

**Important:** Metaspace is not unlimited.

## 4. Method Area vs Metaspace

| Method Area | Metaspace |
|---|---|
| JVM specification concept | HotSpot implementation |
| Shared runtime memory area | Uses native memory |
| Holds class-level information conceptually | Stores class metadata |
| Specification-defined | Introduced in Java 8 HotSpot |

## 5. Practical Java Example

```java
class Employee {

    int id;
    String name;

    void work() {
        System.out.println("Employee working");
    }
}

public class Main {

    public static void main(String[] args) {

        Employee e1 = new Employee();
        Employee e2 = new Employee();

        e1.id = 101;
        e2.id = 102;

        e1.work();
        e2.work();
    }
}
```

### What happens internally?

1. JVM loads the Employee class.
2. Class metadata becomes available through the Method Area abstraction.
3. HotSpot stores that metadata in Metaspace.
4. Two Employee objects are created in the Heap.
5. `e1` and `e2` are references in the main method's stack frame.
6. Both objects use the same loaded Employee class definition.

### Memory trick

- Metaspace → Class information.
- Heap → Actual objects.
- Stack → Method execution and local variables.

## 6. What Happens When Metaspace Becomes Full?

Imagine a backend application dynamically loading thousands of classes.

Frameworks, proxies and dynamically generated classes can increase Metaspace usage.

If the JVM cannot allocate more Metaspace memory, it may throw:

```text
java.lang.OutOfMemoryError: Metaspace
```

We can configure its maximum size:

```bash
java -XX:MaxMetaspaceSize=256m Main
```

This sets a maximum of 256 MB.

It does not mean Java immediately allocates all 256 MB.

## 7. Real-World Backend Example

Imagine a Spring Boot application.

It loads:
- Controller classes.
- Service classes.
- Repository classes.
- Entity classes.
- Framework-generated proxy classes.

The JVM needs class metadata for these loaded classes.

In HotSpot Java 8+, this metadata is stored in Metaspace.

If class loaders keep loading classes and those classes cannot be unloaded, Metaspace usage can grow.

## 8. Interview Questions

**Q1. What is Method Area?**

A shared JVM runtime area that stores class-level information such as class metadata, method information and runtime constant pools.

**Q2. What is Metaspace?**

In HotSpot Java 8+, Metaspace is native memory used to store class metadata.

**Q3. What replaced PermGen?**

Metaspace replaced PermGen in Java 8 HotSpot.

**Q4. Is Metaspace part of Java Heap?**

No. It uses native memory outside the Java Heap.

**Q5. Is Metaspace unlimited?**

No. It can run out of available native memory or reach its configured maximum.

**Q6. Can many objects share one class definition?**

Yes. Multiple objects can use the same loaded class definition.

## 9. Quick Revision

Remember these three things:

1. Method Area = JVM's logical class-information area.
2. Metaspace = HotSpot's native-memory implementation for class metadata.
3. Heap = where Java objects and arrays are generally allocated.

### One-line memory trick

- Method Area → Blueprint library.
- Metaspace → Place where HotSpot keeps class metadata.
- Heap → Actual objects.

## 10. Practice Questions

Try answering without looking at the notes:

1. Why doesn't Java load the Employee class definition separately for every object?
2. What is the difference between Method Area and Metaspace?
3. Why did Java 8 replace PermGen?
4. Is Metaspace part of Heap?
5. What happens when Metaspace becomes full?

**Interview answer formula:**

Definition → Why we need it → How it works → Real-world example.

Don't memorize the answers word for word. Understand the story and explain it in your own words.