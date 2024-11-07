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
