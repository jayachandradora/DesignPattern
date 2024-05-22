# Observer Design Pattern: Overview, Use Cases, and Example

The Observer design pattern is a behavioral pattern that defines a one-to-many dependency between objects, so that when one object changes state, all its dependents are notified and updated automatically. Here's an overview of the Observer pattern, along with some use cases and an example:

## Observer Design Pattern

- **Intent:** Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

- **Key Components:**
  1. **Subject:** Represents the object being observed. It maintains a list of observers and provides methods to register, remove, and notify observers.
  2. **Observer:** Defines an interface for objects that should be notified of changes in the subject's state.
  3. **Concrete Subject:** Implements the subject interface and maintains the state that observers are interested in. It sends notifications to observers when its state changes.
  4. **Concrete Observer:** Implements the observer interface and defines the actions to be taken when notified by the subject.

## Use Cases

1. **UI Components in GUI Applications:**
   - Use the Observer pattern to implement UI components that need to react to changes in underlying data or state. For example, in a weather application, the temperature display component can be an observer of the weather data subject.
   
2. **Event Handling in Event-Driven Systems:**
   - Implement event handling mechanisms using the Observer pattern. For instance, in a messaging system, subscribers can register as observers to receive notifications when new messages are published.

3. **Model-View-Controller (MVC) Architecture:**
   - Apply the Observer pattern to implement the Model-View-Controller architecture. The model acts as the subject, and views register as observers to update their presentation when the model's state changes.

## Example - 1

We'll extend the previous product availability example to send notifications to a list of registered users when the product availability changes. We'll have a User class representing a user who can be notified

```Java

import java.util.ArrayList;
import java.util.List;

// Observer interface
interface Observer {
    void update(String productName, boolean available);
}

// Concrete observer class
class NotificationManager implements Observer {
    private List<User> users;

    NotificationManager() {
        users = new ArrayList<>();
    }

    public void registerUser(User user) {
        users.add(user);
    }

    public void unregisterUser(User user) {
        users.remove(user);
    }

    @Override
    public void update(String productName, boolean available) {
        for (User user : users) {
            user.receiveNotification(productName, available);
        }
    }
}

// User class
class User {
    private String name;

    User(String name) {
        this.name = name;
    }

    public void receiveNotification(String productName, boolean available) {
        if (available) {
            System.out.println("Notification to user " + name + ": " + productName + " is now available. You can purchase it!");
        } else {
            System.out.println("Notification to user " + name + ": " + productName + " is currently out of stock.");
        }
    }
}

// Product class
class Product {
    private boolean available;
    private String productName;
    private NotificationManager notificationManager;

    Product(String productName) {
        this.productName = productName;
        this.notificationManager = new NotificationManager();
    }

    public void registerUser(User user) {
        notificationManager.registerUser(user);
    }

    public void unregisterUser(User user) {
        notificationManager.unregisterUser(user);
    }

    public void setAvailable(boolean available) {
        this.available = available;
        notificationManager.update(productName, available);
    }
}

public class Main {
    public static void main(String[] args) {
        // Create product
        Product product = new Product("Smartphone");

        // Create users
        User user1 = new User("Alice");
        User user2 = new User("Bob");

        // Register users with product
        product.registerUser(user1);
        product.registerUser(user2);

        // Update product availability
        product.setAvailable(true);

        // Unregister one user
        product.unregisterUser(user2);

        // Update product availability again
        product.setAvailable(false);
    }
}

```
## Details of above example 

In the provided example, we're implementing the Observer Design Pattern without explicitly defining a "Subject" interface. However, we can still identify the components of the pattern:

1.  **Concrete Observer:** NotificationManager class serves as the concrete observer in this example. It implements the Observer interface and is responsible for receiving notifications about changes in the product availability and then forwarding those notifications to the registered users.

2.  **Concrete Subject:** In this implementation, the Product class acts as the concrete subject. It maintains the state of the product (whether it's available or not) and notifies the observer (NotificationManager) about changes in availability. It also provides methods for registering and unregistering users who are interested in receiving notifications.

3.  **Observer Interface:** The Observer interface defines the contract that concrete observers must implement. In this example, it has a single method update(String productName, boolean available) which concrete observers (NotificationManager in this case) implement to receive updates about product availability changes.

###  In summary:

*  Concrete Observer: NotificationManager
*  Concrete Subject: Product
*  Observer Interface: Observer

## Example - 2

Let's consider a stock market application where users can subscribe to receive notifications when the price of a particular stock changes. We'll implement this using the Observer pattern:

```java

import java.util.ArrayList;
import java.util.List;

// Subject interface
interface Subject {
    void registerObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Concrete Subject
class StockMarket implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String stockName;
    private double price;

    public void setStockData(String stockName, double price) {
        this.stockName = stockName;
        this.price = price;
        notifyObservers();
    }

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(stockName, price);
        }
    }
}

// Observer interface
interface Observer {
    void update(String stockName, double price);
}

// Concrete Observer
class StockObserver implements Observer {
    private String observerName;

    public StockObserver(String observerName) {
        this.observerName = observerName;
    }

    @Override
    public void update(String stockName, double price) {
        System.out.println(observerName + " received notification: " + stockName + " price updated to " + price);
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        StockMarket stockMarket = new StockMarket();
        
        // Create observers
        Observer user1 = new StockObserver("User1");
        Observer user2 = new StockObserver("User2");

        // Register observers
        stockMarket.registerObserver(user1);
        stockMarket.registerObserver(user2);

        // Set stock data (notify observers)
        stockMarket.setStockData("GOOG", 1500.00);
        stockMarket.setStockData("AAPL", 300.00);
    }
}

```
