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

Yes, Google Pay can be considered an example of the **Proxy Pattern**. The Proxy Pattern involves using an intermediary object (the proxy) to control access to another object. In the case of Google Pay, the **GooglePay** service acts as a proxy that facilitates communication between the user's bank account (which holds the actual money) and the payment system that deducts money. The proxy might perform additional tasks such as logging, authentication, or validation before actually executing the payment.

### Key Components of the Proxy Pattern:

1. **Subject (Real Object)**: The real object is the one doing the actual work. In this case, it could be the `BankAccount` class.
2. **Proxy**: The proxy (e.g., `GooglePay`) controls access to the real object (bank account) and can add additional logic or security before interacting with it.
3. **Client**: The client interacts with the proxy and doesn't need to know the details of the actual object.

### Scenario:
- We have a `BankAccount` class representing a user's bank account with a balance.
- A `User` class represents the individual using the bank account.
- A `GooglePay` class will act as a proxy, where the payment request is processed, and money is deducted from the bank account.

### Java Implementation:

```java
// Step 1: Real Subject - BankAccount class
class BankAccount {
    private String accountHolder;
    private double balance;

    public BankAccount(String accountHolder, double initialBalance) {
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }

    public String getAccountHolder() {
        return accountHolder;
    }

    public double getBalance() {
        return balance;
    }

    public boolean deductMoney(double amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println("Deducted " + amount + " from account. New balance: " + balance);
            return true;
        } else {
            System.out.println("Insufficient funds.");
            return false;
        }
    }
}

// Step 2: Proxy class - GooglePay
class GooglePay {
    private BankAccount bankAccount;

    public GooglePay(BankAccount bankAccount) {
        this.bankAccount = bankAccount;
    }

    public boolean makePayment(double amount) {
        // Perform additional proxy logic like validation, logging, etc.
        System.out.println("Initiating payment through GooglePay...");
        
        if (validatePayment(amount)) {
            // Forward the request to the real object (BankAccount)
            return bankAccount.deductMoney(amount);
        }
        return false;
    }

    private boolean validatePayment(double amount) {
        // Additional validation can be done here (e.g., checking if the user is authenticated)
        System.out.println("Payment validated successfully.");
        return true;
    }
}

// Step 3: Client class - User
class User {
    private String name;
    private GooglePay googlePay;

    public User(String name, BankAccount bankAccount) {
        this.name = name;
        this.googlePay = new GooglePay(bankAccount);
    }

    public void makePayment(double amount) {
        System.out.println(name + " is trying to make a payment of " + amount);
        googlePay.makePayment(amount);
    }
}

// Main class to run the simulation
public class Main {
    public static void main(String[] args) {
        // Create a bank account with an initial balance
        BankAccount userAccount = new BankAccount("John Doe", 500.0);
        
        // Create a user who will use GooglePay as a proxy
        User user = new User("John Doe", userAccount);
        
        // User tries to make a payment
        user.makePayment(200.0); // Should succeed
        user.makePayment(400.0); // Should fail due to insufficient funds
    }
}
```

### Explanation:
1. **BankAccount**: This class represents the real object. It has methods to get the balance and deduct money from the account.
   
2. **GooglePay**: This class acts as a proxy. It contains a reference to the `BankAccount` and handles interactions with the real object. Before making the payment, it performs any validation (such as authentication or checking the amount) and then forwards the request to the `BankAccount`.

3. **User**: The `User` class represents the client that wants to make a payment. It doesn't interact directly with the `BankAccount`. Instead, it interacts with `GooglePay`, which abstracts away the real object.

### Example Output:
```
John Doe is trying to make a payment of 200.0
Initiating payment through GooglePay...
Payment validated successfully.
Deducted 200.0 from account. New balance: 300.0
John Doe is trying to make a payment of 400.0
Initiating payment through GooglePay...
Payment validated successfully.
Insufficient funds.
```

### Benefits of the Proxy Pattern in this Example:
- **Separation of Concerns**: The `GooglePay` class handles the validation, logging, and other logic related to payments, while the `BankAccount` class simply handles the balance.
- **Access Control**: The `GooglePay` proxy ensures that the user can't make payments directly from the `BankAccount`, adding an extra layer of security or validation.
- **Flexibility**: If we need to add additional functionality (like logging or transaction monitoring), it can be done in the `GooglePay` proxy without modifying the `BankAccount` class.

This pattern allows for cleaner and more maintainable code, especially in systems like Google Pay, where there are multiple layers of logic between the user and the financial transactions.


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
