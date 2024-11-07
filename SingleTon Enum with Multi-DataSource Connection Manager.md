# SingleTon Enum with Multi-DataSource Connection Manager
Implementing a **multi-database connection manager** (e.g., for PostgreSQL, Cassandra, SQL, and NoSQL databases) using the **Enum Singleton** pattern is an excellent design choice. This ensures that the connection management is thread-safe, efficient, and that only one instance of the connection manager exists across your application.

### Steps to Implement Multi-DataSource Connection Manager:

1. **Create an Enum Singleton for DataSource Management**: 
   The enum will hold different database connections, each for PostgreSQL, Cassandra, and a NoSQL database (e.g., MongoDB). We'll use a strategy pattern to manage the connection to various databases.

2. **Use a Common Interface for Data Sources**: 
   The connection manager will interact with all data sources via a common interface, allowing for flexibility and easy addition of new data sources.

3. **Connection Pooling**: 
   For relational databases like PostgreSQL, you can use a connection pool (e.g., HikariCP or Apache DBCP), while for NoSQL, you'll use the respective client libraries (e.g., Cassandra’s `DataStax` or MongoDB’s Java client).

4. **Abstract Connection Handling**: 
   Each database type (SQL, NoSQL, etc.) will implement the same interface but with different configurations and connection management logic.

### Code Implementation:

#### 1. Define a Common Interface for Connections:

```java
public interface DataSourceConnection {
    void connect();
    void disconnect();
    // Other common methods can be added here (e.g., transaction management)
}
```

#### 2. Implement Connection Classes for PostgreSQL, Cassandra, and NoSQL:

```java
// PostgreSQL Connection Implementation
public class PostgresConnection implements DataSourceConnection {
    @Override
    public void connect() {
        // Logic to connect to PostgreSQL
        System.out.println("Connecting to PostgreSQL Database...");
    }

    @Override
    public void disconnect() {
        // Logic to disconnect from PostgreSQL
        System.out.println("Disconnecting from PostgreSQL Database...");
    }
}

// Cassandra Connection Implementation
public class CassandraConnection implements DataSourceConnection {
    @Override
    public void connect() {
        // Logic to connect to Cassandra
        System.out.println("Connecting to Cassandra Database...");
    }

    @Override
    public void disconnect() {
        // Logic to disconnect from Cassandra
        System.out.println("Disconnecting from Cassandra Database...");
    }
}

// NoSQL (MongoDB) Connection Implementation
public class MongoDbConnection implements DataSourceConnection {
    @Override
    public void connect() {
        // Logic to connect to MongoDB
        System.out.println("Connecting to MongoDB Database...");
    }

    @Override
    public void disconnect() {
        // Logic to disconnect from MongoDB
        System.out.println("Disconnecting from MongoDB Database...");
    }
}
```

#### 3. Define the Enum Singleton for Multi-DataSource Management:

Here, we'll use the **Enum Singleton** pattern to manage connections to different databases (PostgreSQL, Cassandra, and MongoDB). The enum will initialize and provide access to the correct data source based on the required type.

```java
public enum MultiDataSourceManager {
    
    INSTANCE;

    // Enum holds different database connection managers
    private final DataSourceConnection postgresConnection;
    private final DataSourceConnection cassandraConnection;
    private final DataSourceConnection mongoConnection;

    // Enum constructor initializes connections for all databases
    MultiDataSourceManager() {
        // Initialize connections for each database
        this.postgresConnection = new PostgresConnection();
        this.cassandraConnection = new CassandraConnection();
        this.mongoConnection = new MongoDbConnection();
    }

    // Get the appropriate connection for PostgreSQL
    public DataSourceConnection getPostgresConnection() {
        return postgresConnection;
    }

    // Get the appropriate connection for Cassandra
    public DataSourceConnection getCassandraConnection() {
        return cassandraConnection;
    }

    // Get the appropriate connection for MongoDB
    public DataSourceConnection getMongoConnection() {
        return mongoConnection;
    }

    // Example: Generic method to disconnect from all databases
    public void disconnectAll() {
        postgresConnection.disconnect();
        cassandraConnection.disconnect();
        mongoConnection.disconnect();
    }

    // Example: Generic method to connect to all databases
    public void connectAll() {
        postgresConnection.connect();
        cassandraConnection.connect();
        mongoConnection.connect();
    }
}
```

#### 4. Usage Example:

```java
public class MultiDataSourceExample {

    public static void main(String[] args) {
        // Access the MultiDataSourceManager singleton instance
        MultiDataSourceManager manager = MultiDataSourceManager.INSTANCE;

        // Connect to all databases
        manager.connectAll();

        // Get connections for specific databases
        DataSourceConnection postgres = manager.getPostgresConnection();
        postgres.connect(); // PostgreSQL-specific connect logic
        
        DataSourceConnection cassandra = manager.getCassandraConnection();
        cassandra.connect(); // Cassandra-specific connect logic
        
        DataSourceConnection mongo = manager.getMongoConnection();
        mongo.connect(); // MongoDB-specific connect logic
        
        // Disconnect all databases at once
        manager.disconnectAll();
    }
}
```

### 5. Optional Enhancements:

#### a. **Connection Pooling for SQL Databases**:
   You can use a **connection pool** (e.g., HikariCP or Apache DBCP) for databases like PostgreSQL to manage multiple connections efficiently.

   Example of PostgreSQL connection pooling:

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class PostgresConnection implements DataSourceConnection {
    private HikariDataSource dataSource;

    @Override
    public void connect() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("user");
        config.setPassword("password");
        dataSource = new HikariDataSource(config);
        System.out.println("Connecting to PostgreSQL Database with Pooling...");
    }

    @Override
    public void disconnect() {
        if (dataSource != null) {
            dataSource.close();
        }
        System.out.println("Disconnecting from PostgreSQL Database...");
    }
}
```

#### b. **Connection Pooling for NoSQL Databases**:
   For NoSQL (e.g., MongoDB), connection pooling is managed automatically by the MongoDB driver.

   ```java
   public class MongoDbConnection implements DataSourceConnection {
       private MongoClient mongoClient;

       @Override
       public void connect() {
           MongoClientURI uri = new MongoClientURI("mongodb://localhost:27017");
           mongoClient = new MongoClient(uri);
           System.out.println("Connecting to MongoDB with connection pooling...");
       }

       @Override
       public void disconnect() {
           if (mongoClient != null) {
               mongoClient.close();
           }
           System.out.println("Disconnecting from MongoDB Database...");
       }
   }
   ```

### Key Benefits of this Approach:

1. **Thread Safety**: The **Enum Singleton** guarantees that only one instance of the `MultiDataSourceManager` is created, making it thread-safe and easily accessible across your application.
   
2. **Flexible**: This design can be easily extended to handle other types of databases by simply adding new connection implementations and updating the enum.
   
3. **Centralized Management**: You have a single entry point for managing multiple data sources, making the code easier to maintain and extend.

4. **Optimized for Performance**: With connection pooling for SQL databases and automatic management for NoSQL databases, this design provides good performance in terms of both connection management and resource utilization.

### Conclusion:

The **Enum Singleton** pattern is an excellent choice for managing multiple database connections, ensuring that only one instance of the connection manager exists across the application while also providing a simple and efficient way to handle connections to PostgreSQL, Cassandra, and MongoDB (or any other databases). By leveraging this approach, you can easily manage different types of databases with centralized connection management and without dealing with the complexity of explicit synchronization or resource leaks.

## Connection Pooling with multiple data sources (e.g., PostgreSQL, Cassandra, MongoDB)

To implement **connection pooling** with multiple data sources (e.g., PostgreSQL, Cassandra, MongoDB) using the **Enum Singleton pattern** and ensure that each data source maintains a pool of **10 connections**, we can take the following approach:

1. **Connection Pooling Concept**: 
   - For **SQL databases** like PostgreSQL, we will use **HikariCP** or **Apache DBCP** to create and manage a connection pool with a fixed number of connections.
   - For **NoSQL databases** like **Cassandra** and **MongoDB**, we'll manage multiple connections manually or use the respective libraries that manage connection pooling automatically.

2. **The Plan**:
   - Create a connection pool for each database type (PostgreSQL, Cassandra, MongoDB).
   - Use a fixed pool size of **10 connections** for each database.
   - For each data source, the connection manager will handle obtaining and releasing connections from the pool.
   
### 1. **PostgreSQL Connection Pooling (HikariCP)**

We'll configure **HikariCP** to manage a pool of 10 connections for PostgreSQL.

### 2. **Cassandra Connection Pooling (DataStax Java Driver)**

Cassandra's driver (`DataStax` Java driver) manages pooling automatically, but we'll configure it to use a fixed number of connections.

### 3. **MongoDB Connection Pooling**

MongoDB's Java client also supports automatic connection pooling, but we will configure it to use a fixed pool size.

---

### Full Implementation with Connection Pools

#### **1. PostgreSQL Connection Pool (Using HikariCP)**

For PostgreSQL, we will configure HikariCP to create a pool of 10 connections.

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class PostgresConnection implements DataSourceConnection {
    
    private HikariDataSource dataSource;

    @Override
    public void connect() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("user");
        config.setPassword("password");
        config.setMaximumPoolSize(10);  // Set the pool size to 10 connections
        
        dataSource = new HikariDataSource(config);
        System.out.println("PostgreSQL connection pool created with 10 connections.");
    }

    @Override
    public void disconnect() {
        if (dataSource != null) {
            dataSource.close();
        }
        System.out.println("PostgreSQL connection pool closed.");
    }

    public HikariDataSource getDataSource() {
        return dataSource;
    }
}
```

#### **2. Cassandra Connection Pool (Using DataStax Java Driver)**

For Cassandra, we can configure the **DataStax Java Driver** to manage a pool of 10 connections.

```java
import com.datastax.driver.core.Cluster;
import com.datastax.driver.core.Session;

public class CassandraConnection implements DataSourceConnection {
    
    private Cluster cluster;
    private Session session;

    @Override
    public void connect() {
        // Configure the Cassandra connection pool with 10 connections
        cluster = Cluster.builder()
                         .addContactPoint("localhost")
                         .withPort(9042)
                         .withMaxConnectionsPerHost(10)  // Set the pool size to 10 connections per host
                         .build();
                         
        session = cluster.connect();
        System.out.println("Cassandra connection pool created with 10 connections.");
    }

    @Override
    public void disconnect() {
        if (session != null) {
            session.close();
        }
        if (cluster != null) {
            cluster.close();
        }
        System.out.println("Cassandra connection pool closed.");
    }
    
    public Session getSession() {
        return session;
    }
}
```

#### **3. MongoDB Connection Pool (Using MongoClient)**

MongoDB's Java driver automatically handles connection pooling. We'll set the **maximum connection pool size** to 10 connections.

```java
import com.mongodb.MongoClient;
import com.mongodb.MongoClientURI;

public class MongoDbConnection implements DataSourceConnection {
    
    private MongoClient mongoClient;

    @Override
    public void connect() {
        // Configure the MongoDB connection pool with 10 connections
        MongoClientURI uri = new MongoClientURI("mongodb://localhost:27017", 
            MongoClientOptions.builder()
                             .connectionsPerHost(10)  // Set the pool size to 10 connections per host
                             .build());
                             
        mongoClient = new MongoClient(uri);
        System.out.println("MongoDB connection pool created with 10 connections.");
    }

    @Override
    public void disconnect() {
        if (mongoClient != null) {
            mongoClient.close();
        }
        System.out.println("MongoDB connection pool closed.");
    }

    public MongoClient getMongoClient() {
        return mongoClient;
    }
}
```

#### **4. MultiDataSourceManager Enum Singleton for Connection Pools**

Now we integrate the three connection classes into the **Enum Singleton** pattern to manage the connection pools for PostgreSQL, Cassandra, and MongoDB.

```java
public enum MultiDataSourceManager {
    
    INSTANCE;

    // Enum holds different database connection managers
    private final PostgresConnection postgresConnection;
    private final CassandraConnection cassandraConnection;
    private final MongoDbConnection mongoConnection;

    // Enum constructor initializes connections for all databases
    MultiDataSourceManager() {
        this.postgresConnection = new PostgresConnection();
        this.cassandraConnection = new CassandraConnection();
        this.mongoConnection = new MongoDbConnection();
    }

    // Connect to all databases
    public void connectAll() {
        postgresConnection.connect();
        cassandraConnection.connect();
        mongoConnection.connect();
    }

    // Disconnect from all databases
    public void disconnectAll() {
        postgresConnection.disconnect();
        cassandraConnection.disconnect();
        mongoConnection.disconnect();
    }

    // Get PostgreSQL connection pool
    public HikariDataSource getPostgresDataSource() {
        return postgresConnection.getDataSource();
    }

    // Get Cassandra session
    public Session getCassandraSession() {
        return cassandraConnection.getSession();
    }

    // Get MongoDB client
    public MongoClient getMongoClient() {
        return mongoConnection.getMongoClient();
    }
}
```

#### **5. Usage Example**

Now, we can use the `MultiDataSourceManager` to connect to multiple databases, manage the connection pools, and access them.

```java
public class MultiDataSourceExample {

    public static void main(String[] args) {
        // Access the MultiDataSourceManager singleton instance
        MultiDataSourceManager manager = MultiDataSourceManager.INSTANCE;

        // Connect to all databases (initialize connection pools)
        manager.connectAll();

        // Access PostgreSQL connection pool (HikariCP)
        HikariDataSource postgresDataSource = manager.getPostgresDataSource();
        System.out.println("PostgreSQL Pool Size: " + postgresDataSource.getHikariPoolMXBean().getTotalConnections());

        // Access Cassandra session
        Session cassandraSession = manager.getCassandraSession();
        System.out.println("Cassandra Connection: " + cassandraSession);

        // Access MongoDB client
        MongoClient mongoClient = manager.getMongoClient();
        System.out.println("MongoDB Client: " + mongoClient);

        // Disconnect from all databases
        manager.disconnectAll();
    }
}
```

### Key Points:
- **HikariCP for PostgreSQL**: HikariCP is used to manage a pool of 10 PostgreSQL connections.
- **Cassandra (DataStax Driver)**: We configure the `Cluster` object to use a maximum of 10 connections per host.
- **MongoDB**: MongoDB's Java client automatically manages a connection pool with 10 connections per host.
- **Enum Singleton**: The `MultiDataSourceManager` enum provides a centralized point to manage connections to all databases.

### Conclusion:
By using the **Enum Singleton pattern**, we ensure that the database connections for PostgreSQL, Cassandra, and MongoDB are managed centrally and efficiently. Each database has its own connection pool (with 10 connections in this case), and we provide easy access to these pools via the `MultiDataSourceManager`. This design is thread-safe, scalable, and ensures proper management of database connections across multiple types of data sources in a Java application.
