# Proxy Design Pattern

The Proxy Design Pattern provides a surrogate or placeholder for another object to control access to it. This pattern involves creating a new class (the proxy) that acts as an intermediary between the client and the real object. The proxy receives client requests, performs some pre-processing (such as validation, caching, logging, or security checks), and then forwards the request to the real object if appropriate.

###    Components:

**Proxy:** Acts as the intermediary, implementing the same interface as the subject.

**Subject:** The actual object being accessed.

**Client:** Uses the proxy to interact with the subject.

### Use Cases:

**Lazy Loading:** Delay creating the subject until it's actually needed. This improves performance for resource-intensive objects. 

**Access Control:** Enforce access rules based on user permissions. The proxy can verify user credentials before granting access to the subject.

**Caching:** Improve performance by storing frequently accessed data in the proxy.

**Remote Proxies:** Provide a local interface for accessing remote objects located on different servers.

**Security:** Add security checks to the proxy to intercept and potentially modify requests before they reach the subject.

### How Proxy Design Pattern Works:

**Client:** The client interacts with the proxy object, thinking it's the real object.

**Proxy:** The proxy object receives requests from the client and performs additional tasks such as validation, caching, or access control before forwarding the request to the real object.

**Real Subject:** The real object that the proxy represents. The proxy forwards requests to the real subject if necessary.

**Example:**
Let's consider a simple example of a proxy for an expensive image loading operation. The proxy will only load the image from disk when requested by the client.
Proxy is a structural design pattern that provides an object that acts as a substitute for a real service object used by a client. A proxy receives client requests, does some work (access control, caching, etc.) and then passes the request to a service object.

```java
// Image interface
interface Image {
    void display();
}

// RealImage class representing the actual image
class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading image: " + fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying image: " + fileName);
    }
}

// ProxyImage class representing the proxy for the image
class ProxyImage implements Image {
    private String fileName;
    private RealImage realImage;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}

// Client code
public class ProxyDemo {
    public static void main(String[] args) {
        // Client requests image display through proxy
        Image image = new ProxyImage("sample.jpg");

        // Image will be loaded only when requested by the client
        image.display();

        // Image will not be loaded again if already loaded
        image.display();
    }
}

```

### Use Cases:

1.  **Remote Proxy:** Provides a local representative for an object in a different address space, often on a remote server.

2.  **Virtual Proxy:** Creates expensive objects on demand, such as large images or files, only when they are required.

3.  **Protection Proxy:** Controls access to sensitive objects by checking permissions or performing authentication before allowing access.

4  **Logging Proxy:** Adds logging functionality to methods of the real object without modifying its code.

5.  **Caching Proxy:** Stores the results of expensive operations and returns the cached result when the same operation is requested again, instead of recalculating it.

Overall, the Proxy Design Pattern is useful for providing a level of indirection to control access to objects, add functionality, or improve performance without changing the client's code.

```java
package proxy;
interface DatabaseExecuter {
  public void executeDatabase(String query) throws Exception;
}
```

```ruby
class DatabaseExecuterImpl implements DatabaseExecuter {

  @Override
  public void executeDatabase(String query) throws Exception {
    System.out.println("Going to execute Query: " + query);
  }
  
}
```

```ruby
class DatabaseExecuterProxy implements DatabaseExecuter {
  boolean ifAdmin;
  DatabaseExecuterImpl dbExecuter;
  
  public DatabaseExecuterProxy(String name, String passwd) {
    if(name == "Admin" && passwd == "Admin@123") {
      ifAdmin = true;
    }
    dbExecuter = new DatabaseExecuterImpl();
  }

  @Override
  public void executeDatabase(String query) throws Exception {
    if(ifAdmin) {
      dbExecuter.executeDatabase(query);
    } else {
      if(query.equals("DELETE")) {
        throw new Exception("DELETE not allowed for non-admin user");
      } else {
        dbExecuter.executeDatabase(query);
      }
    }
  }
}
```

```ruby
public class ProxyPatternExample {

  public static void main(String[] args) throws Exception {
  
    DatabaseExecuter nonAdminExecuter = new DatabaseExecuterProxy("NonAdmin", "Admin@123");
    nonAdminExecuter.executeDatabase("DELEE");
    
    DatabaseExecuter nonAdminExecuterDELETE = new DatabaseExecuterProxy("NonAdmin", "Admin@123");
    nonAdminExecuterDELETE.executeDatabase("DELETE");
    
    DatabaseExecuter adminExecuter = new DatabaseExecuterProxy("Admin", "Admin@123");
    adminExecuter.executeDatabase("DELETE");
  }
}
```
