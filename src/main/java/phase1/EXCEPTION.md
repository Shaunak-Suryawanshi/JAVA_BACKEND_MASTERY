# Exception Handling in Java

## 1. What is an Exception?

An exception is an unwanted event that interrupts the normal execution of a program.

Examples:
- Dividing by zero
- Accessing a null reference
- Reading a missing file
- Database connection failure

Exception handling allows us to handle errors and maintain normal program flow where possible.

## 2. Exception Hierarchy

```text
Object
  |
Throwable
  |
  |--- Error
  |      |--- OutOfMemoryError
  |      |--- StackOverflowError
  |
  |--- Exception
         |--- IOException
         |--- SQLException
         |--- RuntimeException
                |--- NullPointerException
                |--- ArithmeticException
                |--- ArrayIndexOutOfBoundsException
                |--- IllegalArgumentException
```

- **Error:** Serious JVM or system-level problem, generally not handled by application code.
- **Exception:** Conditions an application may handle.

## 3. Checked vs Unchecked Exceptions

### Checked Exceptions
- Checked by the compiler.
- Must be caught or declared using `throws`.
- Examples: IOException, SQLException, ClassNotFoundException.

### Unchecked Exceptions
- Occur under RuntimeException.
- Compiler does not require us to catch or declare them.
- Examples: NullPointerException, ArithmeticException, IllegalArgumentException.

## 4. try-catch

Used to handle exceptions.

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}
```

Multiple catch blocks can handle different exception types. Place specific exception types before general types.

## 5. finally

The finally block executes after try-catch during normal program execution, whether an exception occurs or not.

```java
try {
    System.out.println("Try block");
} catch (Exception e) {
    System.out.println("Exception");
} finally {
    System.out.println("Cleanup");
}
```

Common use: resource cleanup.

It may not execute if the JVM terminates abruptly.

## 6. throw

Used to explicitly throw an exception object.

```java
if (age < 18) {
    throw new IllegalArgumentException("Invalid age");
}
```

## 7. throws

Used in a method declaration to declare exceptions that may be passed to the caller.

```java
static void readFile() throws IOException {
    FileReader reader = new FileReader("data.txt");
    reader.close();
}
```

### throw vs throws

| throw | throws |
|---|---|
| Throws an exception object | Declares possible exceptions |
| Used inside method/block | Used in method declaration |
| Followed by an object | Followed by exception class names |

## 8. Custom Exceptions

Developer-created exceptions representing application-specific errors.

### Checked custom exception

```java
public class InsufficientBalanceException extends Exception {

    public InsufficientBalanceException(String message) {
        super(message);
    }
}
```

### Unchecked custom exception

```java
public class InvalidUserException extends RuntimeException {

    public InvalidUserException(String message) {
        super(message);
    }
}
```

Extend `Exception` for checked exceptions and `RuntimeException` for unchecked exceptions.

## 9. Exception Propagation

Exception propagation means an exception travels up the method-call chain until it is handled.

Example:

```text
main()
  |
method1()
  |
method2()
  |
method3()
  |
Exception occurs
  |
Travels back to main()
  |
Handled by catch
```

If no method handles an exception, the thread terminates and Java prints a stack trace.

Checked exceptions must be caught or declared as they propagate.

## 10. Try-with-Resources

Automatically closes resources after the try block finishes.

```java
try (BufferedReader reader =
         new BufferedReader(new FileReader("data.txt"))) {

    System.out.println(reader.readLine());

} catch (IOException e) {
    System.out.println(e.getMessage());
}
```

- Resource must implement AutoCloseable or Closeable.
- Resources close automatically.
- Multiple resources are supported.
- Resources close in reverse declaration order.
- Useful for files, JDBC connections, statements and result sets.

## 11. Exception Handling Best Practices

1. Catch specific exceptions.
2. Never leave catch blocks empty.
3. Use meaningful exception messages.
4. Create custom exceptions for business errors.
5. Use try-with-resources for resource cleanup.
6. Avoid using exceptions for normal program flow.
7. Log exceptions appropriately in real applications.
8. Do not catch Error unless there is a specific, justified reason.

## 12. Backend Application Connection

In backend applications:

- Service methods may throw business exceptions.
- Exceptions can propagate to controllers.
- Spring Boot can use `@RestControllerAdvice` to handle exceptions centrally.
- Errors can be converted into HTTP responses such as 400, 404 or 500.

## 13. Important Interview Questions

1. What is an exception?
2. Difference between Error and Exception?
3. Checked vs unchecked exceptions?
4. Difference between throw and throws?
5. What is finally?
6. Can finally execute without catch?
7. What is a custom exception?
8. What is exception propagation?
9. What happens when an exception is not handled?
10. What is try-with-resources?
11. What is AutoCloseable?
12. Why should we avoid empty catch blocks?
13. How does Spring Boot handle exceptions?

## Quick Revision

- `try` = Code that may cause an exception.
- `catch` = Handles an exception.
- `finally` = Cleanup code.
- `throw` = Explicitly throws an exception.
- `throws` = Declares possible exceptions.
- Custom exception = Application-specific error.
- Propagation = Exception travels up the call chain.
- Try-with-resources = Automatically closes resources.
