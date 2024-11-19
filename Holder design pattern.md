# Holder design pattern

The **Holder** design pattern is a structural design pattern that is used to encapsulate a value or an object to provide easy access and manage its lifecycle. In essence, a **Holder** acts as a container that holds a reference to an object, but without exposing the details of its implementation or lifecycle management.

This pattern is not one of the more widely discussed or formalized patterns, but it has practical applications, especially in situations where:

1. **Object Management**: You need to manage a single instance of an object or resource, without exposing the details of its creation, destruction, or internal state.
2. **Simplification**: You want to simplify code by decoupling the object lifecycle from the client using it.
3. **Lazy Initialization**: The object held may be initialized only when needed.

In Java, the **Holder** pattern can be implemented using a class that wraps a value or object and provides methods for accessing it. It is sometimes similar to a **wrapper class** or **container object**, but the intent is specifically to hold and manage an object or a set of values with minimal impact on the client.

### Example of a Holder Pattern in Java:

```java
// A simple Holder class
public class Holder<T> {
    private T value;

    // Constructor to initialize the value
    public Holder(T value) {
        this.value = value;
    }

    // Getter for the value
    public T getValue() {
        return value;
    }

    // Setter for the value
    public void setValue(T value) {
        this.value = value;
    }
}

// Usage of the Holder class
public class Main {
    public static void main(String[] args) {
        // Holding a String object
        Holder<String> stringHolder = new Holder<>("Hello, World!");

        // Accessing the held value
        System.out.println(stringHolder.getValue());  // Output: Hello, World!

        // Holding an Integer object
        Holder<Integer> intHolder = new Holder<>(42);
        System.out.println(intHolder.getValue());  // Output: 42
    }
}
```

### Key Characteristics:
1. **Generic**: The `Holder` class can hold any type of object, which is often achieved using generics (like `T` in the example above).
2. **Encapsulation**: The internal workings of the object are hidden. Clients interact with the `Holder` to access or modify the value.
3. **Simplification**: It provides a simple abstraction, helping reduce the complexity of direct object management.

### Common Use Cases:
- **Lazy Loading**: A `Holder` can hold a resource that is expensive to create (e.g., a connection or large object) and instantiate it only when required.
- **Caching**: It can be used to hold previously computed values to avoid redundant calculations.
- **Thread Safety**: In concurrent environments, the `Holder` can encapsulate thread-safe access to shared resources.
- **Temporary Storage**: If you need a temporary container for objects during a process (e.g., during calculations or transformations), a `Holder` might be used.

### Differences from Similar Patterns:
- **Wrapper**: A Wrapper class may have additional functionality around the held object, whereas a `Holder` typically just holds and exposes the object without altering its behavior.
- **Singleton**: The Singleton pattern ensures only one instance of an object exists in the system, while the `Holder` pattern is more about object containment and management.

In summary, the **Holder** design pattern is useful for encapsulating objects to control their access, lifecycle, or initialization logic, often in a flexible and reusable way.

## The Holder Design Pattern can be combined with the Singleton Design Pattern

The **Holder Design Pattern** can be combined with the **Singleton Design Pattern** to create a pattern where the holder manages a single instance of a resource or object, ensuring that it is lazily instantiated and only created once. This combination is often used to implement thread-safe, efficient, and controlled access to a single instance of an object.

In this case, the **Holder** class holds a reference to a **Singleton** instance, ensuring that only one instance of the Singleton is created and it is accessible in a thread-safe manner.

### Scenario: Singleton + Holder Pattern

In this example, we will combine the **Singleton** design pattern with the **Holder** design pattern. The `Holder` will be used to hold a **Singleton** instance of a class. The Singleton instance will be lazily created, ensuring it is instantiated only when it is needed for the first time.

### Singleton Class (Thread-Safe and Lazy Initialization)
```java
public class Singleton {
    // Private static reference to the single instance of the Singleton
    private static Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {
        // Initialization logic (e.g., expensive or complicated setup)
        System.out.println("Singleton instance created.");
    }

    // Public static method to provide access to the Singleton instance
    public static Singleton getInstance() {
        // Lazy initialization (thread-safe using double-checked locking)
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    // Some business logic
    public void performAction() {
        System.out.println("Singleton is performing an action.");
    }
}
```

### Holder Class to Hold Singleton Instance
```java
// The Holder class is used to hold a reference to the Singleton instance.
public class SingletonHolder {
    // The Holder class is holding the Singleton instance lazily.
    private static final Singleton instance = Singleton.getInstance();

    // Getter method for accessing the Singleton instance
    public static Singleton getSingletonInstance() {
        return instance;
    }
}
```

### Main Class to Demonstrate the Holder and Singleton
```java
public class Main {
    public static void main(String[] args) {
        // Access the Singleton instance via the Holder class
        Singleton singleton = SingletonHolder.getSingletonInstance();
        singleton.performAction(); // Output: Singleton is performing an action.
        
        // Trying to access the Singleton again will return the same instance
        Singleton anotherSingleton = SingletonHolder.getSingletonInstance();
        System.out.println(singleton == anotherSingleton); // Output: true (both are the same instance)
    }
}
```

### Explanation:
1. **Singleton Class**: The `Singleton` class is designed to ensure that only one instance of the class is created throughout the application's lifetime. It uses lazy initialization with double-checked locking to ensure thread safety.
   - The `getInstance()` method guarantees that the instance is created only when it is needed and is created only once, no matter how many threads access it.

2. **Holder Class**: The `SingletonHolder` class is a container for the Singleton instance. In this case, the instance is created and stored statically when the class is loaded. This ensures that the Singleton is initialized only once, and you can access it easily via `SingletonHolder.getSingletonInstance()`.
   - **Eager Initialization**: Here, the instance of `Singleton` is created at the time the `SingletonHolder` class is loaded, ensuring that it’s always available in a thread-safe manner.

3. **Lazy vs Eager Initialization**: In this example, the Singleton instance is eagerly created in the `SingletonHolder` class, but in other scenarios, it can be lazily loaded, depending on your needs. Lazy loading would delay the initialization until the first access to the Singleton instance.

### Benefits of Using the Holder Pattern with Singleton:
- **Thread-Safety**: By combining the Singleton pattern's lazy initialization with the Holder pattern, you ensure that only one instance of the Singleton is created in a thread-safe manner.
- **Simplified Access**: Clients can access the Singleton instance through the Holder class, making the access point clear and centralized.
- **Efficient Resource Management**: The Singleton instance is only created once and reused throughout the application's lifecycle, saving resources and ensuring consistency.

### Conclusion:
By combining the **Singleton** pattern with the **Holder** pattern, we ensure efficient and controlled access to a single instance of a class. The **Holder** acts as a container, holding and managing the lifecycle of the **Singleton** instance, ensuring that it is only created once and is accessible to other parts of the application in a thread-safe and efficient manner.
