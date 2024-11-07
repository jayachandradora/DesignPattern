# SingleTon Enum Caching Design Pattern

Using the **Enum Singleton** pattern to implement a real-time scenario, such as a **Cache Manager** or a **Database Connection Pool**, is a common and effective practice. The Enum Singleton design pattern ensures that the class is thread-safe, guarantees only one instance, and prevents issues with serialization or reflection-based attacks.

### Scenario: **Cache Manager Singleton using Enum**

Imagine we want to implement a **Cache Manager** that stores data and allows for basic operations like adding, retrieving, and clearing cache. This Cache Manager should be a **Singleton** to ensure there’s only one cache instance throughout the application.

By using **Enum** for Singleton, we can ensure that the Cache Manager behaves consistently and safely in a multithreaded environment.

Here’s how we can implement this:

### **1. Cache Manager Singleton with Enum**

```java
import java.util.HashMap;
import java.util.Map;

public enum CacheManager {
    
    // Singleton instance
    INSTANCE;
    
    // The cache is stored in a simple HashMap
    private final Map<String, Object> cache = new HashMap<>();
    
    // Method to add an item to the cache
    public void put(String key, Object value) {
        cache.put(key, value);
    }

    // Method to get an item from the cache
    public Object get(String key) {
        return cache.get(key);
    }

    // Method to clear the cache
    public void clear() {
        cache.clear();
    }

    // Optional: Method to check if a key exists in cache
    public boolean containsKey(String key) {
        return cache.containsKey(key);
    }
    
    // Optional: Method to get the cache size
    public int size() {
        return cache.size();
    }
}

```

### **2. Usage Example:**

```java
public class CacheManagerExample {

    public static void main(String[] args) {
        // Accessing the CacheManager singleton instance
        CacheManager cache = CacheManager.INSTANCE;

        // Adding data to the cache
        cache.put("user123", "John Doe");
        cache.put("user456", "Jane Smith");

        // Retrieving data from the cache
        String user123 = (String) cache.get("user123");
        System.out.println("User 123: " + user123); // Output: User 123: John Doe

        // Checking if a key exists in cache
        System.out.println("Is user456 in cache? " + cache.containsKey("user456")); // Output: true

        // Cache size
        System.out.println("Cache size: " + cache.size()); // Output: 2

        // Clearing the cache
        cache.clear();
        System.out.println("Cache size after clear: " + cache.size()); // Output: 0
    }
}
```

### **3. Key Points of the Implementation:**

1. **Singleton Pattern via Enum**: By using the `Enum` type for the singleton (`INSTANCE`), we ensure:
   - Only one instance of `CacheManager` exists.
   - Thread safety is guaranteed because enum instances are created only once, even in multi-threaded environments.
   - The enum type handles serialization automatically, preserving the singleton property during deserialization.
   
2. **Cache Operations**: The `CacheManager` offers basic operations to add, retrieve, and clear cache items. In a real-world scenario, you could expand this to support time-to-live (TTL) for cache entries, eviction policies, or integration with external caches.

3. **Thread Safety**: Because enum instances are inherently thread-safe, this approach ensures that even if multiple threads access the cache concurrently, there won’t be any issues with synchronization.

4. **No Need for Explicit Synchronization**: Traditional Singleton patterns often require synchronization mechanisms (like `synchronized` blocks or `volatile` variables) to ensure thread-safety. With the enum-based Singleton, Java ensures the creation and access to the singleton instance are thread-safe without needing extra synchronization code.

### **4. Handling Cache Size & Memory Management (Optional Enhancements):**

You could extend this basic implementation by adding features like cache eviction, maximum size, and expiration time. For example, you could create a simple **Least Recently Used (LRU) Cache** or implement a **time-to-live (TTL)** feature using `ScheduledExecutorService` to clear expired cache entries.

### Example: Cache with Expiry (TTL)

Here’s an enhanced version of the `CacheManager` with **TTL support** (time-to-live):

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;

public enum CacheManager {

    INSTANCE;

    private final Map<String, CacheItem> cache = new HashMap<>();
    
    // Method to put an item in the cache with TTL (time-to-live in seconds)
    public void put(String key, Object value, long ttl) {
        long expiryTime = System.currentTimeMillis() + TimeUnit.SECONDS.toMillis(ttl);
        cache.put(key, new CacheItem(value, expiryTime));
    }

    // Method to get an item from the cache
    public Object get(String key) {
        CacheItem cacheItem = cache.get(key);
        if (cacheItem != null && System.currentTimeMillis() < cacheItem.expiryTime) {
            return cacheItem.value;
        }
        // Cache expired or not present
        cache.remove(key);
        return null;
    }

    // Method to clear the cache
    public void clear() {
        cache.clear();
    }

    // Internal CacheItem class that holds the value and expiry time
    private static class CacheItem {
        private final Object value;
        private final long expiryTime;

        public CacheItem(Object value, long expiryTime) {
            this.value = value;
            this.expiryTime = expiryTime;
        }
    }
}
```

### Usage Example for Cache with Expiry:

```java
public class CacheWithTTLExample {

    public static void main(String[] args) {
        // Accessing the CacheManager singleton instance
        CacheManager cache = CacheManager.INSTANCE;

        // Adding items to the cache with a TTL of 5 seconds
        cache.put("user123", "John Doe", 5);
        cache.put("user456", "Jane Smith", 5);

        // Retrieving data from the cache before expiry
        System.out.println("User 123: " + cache.get("user123")); // Output: John Doe

        // Simulating a delay
        try {
            Thread.sleep(6000); // Sleep for 6 seconds
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // Trying to retrieve data from the cache after expiry
        System.out.println("User 123 after TTL: " + cache.get("user123")); // Output: null (expired)

        // The second cache item should still be valid as its TTL hasn't expired yet
        System.out.println("User 456: " + cache.get("user456")); // Output: Jane Smith
    }
}
```

### **5. Real-World Use Case (Database Connection Pool)**

You could also use the Enum Singleton pattern for managing a **Database Connection Pool**.

Here's an abstract example of what it might look like:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public enum DatabaseConnectionPool {

    INSTANCE;

    private Connection connection;

    // Initialize connection pool (simplified)
    DatabaseConnectionPool() {
        try {
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Provide access to the connection
    public Connection getConnection() {
        return connection;
    }

    // Example of closing the connection (in a real case, you'd handle a pool of connections)
    public void closeConnection() throws SQLException {
        if (connection != null && !connection.isClosed()) {
            connection.close();
        }
    }
}
```

### Usage for Database Connection Singleton:

```java
public class DatabaseConnectionExample {

    public static void main(String[] args) {
        // Accessing the singleton Database connection
        DatabaseConnectionPool dbPool = DatabaseConnectionPool.INSTANCE;

        // Getting the connection from the pool
        Connection connection = dbPool.getConnection();
        System.out.println("Database Connection: " + connection);

        // Closing the connection (for demonstration, in a real scenario we'd handle connection reuse)
        try {
            dbPool.closeConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### Conclusion:
The **Enum Singleton** pattern is ideal for scenarios like **Cache Managers** or **Database Connection Pools**, where you need a single, consistent instance throughout the lifetime of the application. By using **Java Enum** to implement Singleton, you avoid issues like synchronization, reflection-based attacks, and deserialization problems, while ensuring thread-safety and simplicity. 

In the examples above, the Cache Manager ensures that only one instance of the cache is used, handles optional time-to-live (TTL) for cache items, and ensures thread-safe access. Similarly, the Database Connection Pool pattern could be adapted for managing multiple connections to a database while ensuring a single instance of the pool.
