# Stack vs Heap in Java

## 1. Java Memory Overview

The JVM uses different runtime memory areas to execute Java programs.

Two important areas are:

- **Stack:** Stores method execution data, including stack frames and local variables.
- **Heap:** Stores objects and arrays created during program execution.

## 2. Stack Memory

Each thread has its own Java Stack.

Whenever a method is called, a new **Stack Frame** is created.

A stack frame contains:
- Local variables
- Method execution data
- Operand stack
- Return-related information

When the method finishes, its stack frame is removed.

### Example

```java
public class StackDemo {

    public static void main(String[] args) {
        int age = 25;
        calculate();
    }

    static void calculate() {
        int result = 10 + 20;
        System.out.println(result);
    }
}
```

**Execution:**
1. `main()` starts and gets a stack frame.
2. `age` is a local variable in that frame.
3. `calculate()` is called, creating another stack frame.
4. `calculate()` finishes, and its frame is removed.
5. `main()` finishes, and its frame is removed.

## 3. Heap Memory

Heap memory is shared among threads.

It stores objects and arrays created during program execution.

### Example

```java
class Employee {
    String name;
}

public class HeapDemo {

    public static void main(String[] args) {

        Employee emp = new Employee();
        emp.name = "Shaunak";

        System.out.println(emp.name);
    }
}
```

**Conceptual memory representation:**

- `emp` is a local reference in the `main()` stack frame.
- The `Employee` object is stored in the Heap.
- The reference points to the object.

## 4. Multiple References to One Object

```java
Employee emp1 = new Employee();

Employee emp2 = emp1;

emp1 = null;
```

Important:
- `emp1` and `emp2` initially point to the same object.
- Setting `emp1 = null` does not remove the object.
- `emp2` still references the object.
- When no reachable references remain, the object becomes eligible for garbage collection.

**Remember:** Eligible for GC does not mean immediately deleted.

## 5. Stack vs Heap

| Stack | Heap |
|---|---|
| Each thread has its own stack | Shared among threads |
| Stores method stack frames | Stores objects and arrays |
| Method calls create frames | Object creation uses heap memory |
| Frames are removed as methods return | Objects are reclaimed by GC |
| Limited in size | Usually much larger |
| StackOverflowError can occur | OutOfMemoryError can occur |

## 6. StackOverflowError

Occurs when a thread's stack cannot grow further.

Common cause: uncontrolled recursion.

```java
public class StackOverflowDemo {

    static void call() {
        call();
    }

    public static void main(String[] args) {
        call();
    }
}
```

The method keeps calling itself, creating more stack frames until the stack limit is reached.

## 7. OutOfMemoryError

Occurs when the JVM cannot allocate required memory.

Example:

```java
import java.util.ArrayList;
import java.util.List;

public class HeapMemoryDemo {

    public static void main(String[] args) {

        List<byte[]> data = new ArrayList<>();

        while (true) {
            data.add(new byte[1024 * 1024]);
        }
    }
}
```

The list retains references to allocated arrays, preventing them from becoming eligible for garbage collection. Eventually, heap allocation can fail.

## 8. Real-World Backend Example

Consider a Spring Boot request:

```text
Controller
    |
    v
Service
    |
    v
Repository
    |
    v
Database
```

- Controller, service and repository method calls use stack frames.
- Objects such as request DTOs, entities and result lists are generally allocated on the Heap.
- When methods return, their stack frames are removed.
- Objects that are no longer reachable can eventually be reclaimed by GC.

If an application unnecessarily retains large collections or objects, heap usage can grow and eventually cause an OutOfMemoryError.

## 9. Interview Questions

**Q1. What is Stack memory in Java?**

Stack memory stores method execution data in stack frames. Each thread has its own Java Stack.

**Q2. What is Heap memory?**

Heap is the shared runtime memory area where objects and arrays are generally allocated.

**Q3. Is Heap shared among threads?**

Yes. Heap memory is shared among application threads.

**Q4. What happens when a method finishes?**

Its stack frame is removed from the thread's stack.

**Q5. What happens when an object has no reachable references?**

It becomes eligible for garbage collection, but collection is not guaranteed to happen immediately.

**Q6. Difference between StackOverflowError and OutOfMemoryError?**

StackOverflowError occurs when a thread's stack is exhausted. OutOfMemoryError occurs when the JVM cannot allocate required memory.

**Q7. Can two references point to the same object?**

Yes. Assigning one reference to another copies the reference, not the object.

## 10. Quick Revision

Remember these five points:

1. Stack is per-thread; Heap is shared.
2. Method calls create stack frames.
3. Objects and arrays are generally allocated on Heap.
4. Returning from a method removes its stack frame.
5. Unreachable objects become eligible for garbage collection.

## 11. Practice Task

Create an `Employee` class with:

- `id`
- `name`
- `salary`

Inside `main()`:

1. Create an Employee object.
2. Assign it to `emp1`.
3. Assign `emp1` to `emp2`.
4. Set `emp1 = null`.
5. Print the employee name using `emp2`.
6. Explain which reference is on Stack and where the object is conceptually stored.

---

**Interview explanation formula:**

Definition → Purpose → Working → Real-world example.

Do not memorize every sentence. Understand the memory flow and explain it in your own words.