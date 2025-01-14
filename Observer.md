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

## Example - 0

A **Publish-Subscribe (Pub/Sub)** service system is a powerful messaging pattern where publishers send messages (events) and subscribers receive notifications about those messages. The **Observer Design Pattern** fits naturally into this model since it defines a relationship where a subject (publisher) maintains a list of observers (subscribers), and when the subject changes state, all observers are notified.

In this case, the **Pub/Sub** system allows for a one-to-many notification mechanism. Publishers (events) publish messages or notifications, and multiple subscribers (listeners) are notified when new messages are published. 

### Real-World Example: Pub/Sub Service System with Observer Pattern

In a **Pub/Sub system**, a **Publisher** sends events (messages), and various **Subscribers** (e.g., email services, logging systems, analytics) are interested in receiving these events. This setup allows for an asynchronous, loosely coupled system.

Let's design a simple Pub/Sub service system using the **Observer Pattern**.

---

### Java Implementation of Pub/Sub System using Observer Pattern

#### Step 1: Define the `Observer` Interface
The `Observer` interface will have an `update()` method that is called when a new message/event is published by the publisher.

```java
// Observer interface
interface Subscriber {
    void update(String message);
}
```

#### Step 2: Define the `Publisher` (Subject)
The `Publisher` is the subject in the observer pattern. It holds a list of subscribers and notifies them when a new message/event occurs.

```java
import java.util.ArrayList;
import java.util.List;

// Publisher class - Subject
class Publisher {
    private List<Subscriber> subscribers;
    private String topic;

    public Publisher(String topic) {
        this.topic = topic;
        this.subscribers = new ArrayList<>();
    }

    // Register a subscriber
    public void subscribe(Subscriber subscriber) {
        subscribers.add(subscriber);
        System.out.println("New subscriber added to topic: " + topic);
    }

    // Unsubscribe a subscriber
    public void unsubscribe(Subscriber subscriber) {
        subscribers.remove(subscriber);
        System.out.println("Subscriber removed from topic: " + topic);
    }

    // Notify all subscribers about a new message
    public void publish(String message) {
        System.out.println("Publishing message on topic " + topic + ": " + message);
        for (Subscriber subscriber : subscribers) {
            subscriber.update(message);
        }
    }

    // Get topic of the publisher
    public String getTopic() {
        return topic;
    }
}
```

#### Step 3: Define Concrete Subscribers (Observers)
Each subscriber (observer) will implement the `Subscriber` interface and handle updates when a new message is published.

```java
// Concrete Subscriber - EmailService
class EmailService implements Subscriber {
    @Override
    public void update(String message) {
        System.out.println("EmailService received message: " + message);
    }
}

// Concrete Subscriber - LoggingService
class LoggingService implements Subscriber {
    @Override
    public void update(String message) {
        System.out.println("LoggingService logs message: " + message);
    }
}

// Concrete Subscriber - AnalyticsService
class AnalyticsService implements Subscriber {
    @Override
    public void update(String message) {
        System.out.println("AnalyticsService analyzes message: " + message);
    }
}
```

#### Step 4: Client Code
The client code sets up the **Publisher** and **Subscribers**, and demonstrates the **Pub/Sub** behavior by publishing messages and observing the notifications.

```java
public class PubSubSystem {
    public static void main(String[] args) {
        // Create Publisher (Topic)
        Publisher newsPublisher = new Publisher("Tech News");

        // Create Subscribers (Observers)
        EmailService emailService = new EmailService();
        LoggingService loggingService = new LoggingService();
        AnalyticsService analyticsService = new AnalyticsService();

        // Subscribe to Publisher
        newsPublisher.subscribe(emailService);
        newsPublisher.subscribe(loggingService);
        newsPublisher.subscribe(analyticsService);

        // Publish new messages (events)
        newsPublisher.publish("New Java version released!");

        // Unsubscribe a service
        newsPublisher.unsubscribe(loggingService);

        // Publish another message after unsubscribing
        newsPublisher.publish("Python reaches new milestone!");
    }
}
```

### Explanation:

1. **Subscriber Interface (`Subscriber`)**:
   - This interface defines the `update()` method that each subscriber (listener) must implement. This method is called whenever the publisher sends out a new message.

2. **Publisher (Subject)**:
   - The `Publisher` class maintains a list of `Subscriber` objects. It provides methods for subscribing (`subscribe()`) and unsubscribing (`unsubscribe()`) to the topic.
   - The `publish()` method is used to send a message to all subscribers.

3. **Concrete Subscribers**:
   - `EmailService`, `LoggingService`, and `AnalyticsService` are concrete implementations of the `Subscriber` interface. Each subscriber takes action when it receives a message via the `update()` method.

4. **Client Code**:
   - The `PubSubSystem` class demonstrates the **Publish-Subscribe** pattern. It sets up a `Publisher` and subscribes multiple `Subscriber` objects to it.
   - When the publisher calls `publish()`, all the subscribed services (observers) are notified with the message.

### Example Output:

```
New subscriber added to topic: Tech News
New subscriber added to topic: Tech News
New subscriber added to topic: Tech News
Publishing message on topic Tech News: New Java version released!
EmailService received message: New Java version released!
LoggingService logs message: New Java version released!
AnalyticsService analyzes message: New Java version released!

Subscriber removed from topic: Tech News
Publishing message on topic Tech News: Python reaches new milestone!
EmailService received message: Python reaches new milestone!
AnalyticsService analyzes message: Python reaches new milestone!
```

### Key Benefits of Using the Observer Pattern in a Pub/Sub System:

1. **Loose Coupling**: The **Publisher** does not need to know about the specific actions each **Subscriber** performs. It just sends messages to all subscribed listeners. Subscribers are independent of the Publisher.

2. **Scalability**: New **Subscribers** can be easily added to the system without modifying the **Publisher** or existing **Subscribers**. You can add more services (like notifications, analytics, etc.) by simply creating new observers and subscribing them to the Publisher.

3. **Asynchronous Communication**: The Pub/Sub pattern enables asynchronous communication between components, where the Publisher and Subscribers do not need to operate in a synchronous manner. They can work independently.

4. **Maintainability**: The code is easier to maintain and extend because the Publisher only manages its state (messages) and not the logic of what happens when an event occurs. Subscribers can evolve independently as long as they adhere to the `Subscriber` interface.

### Conclusion:
The **Observer Pattern** is a perfect fit for **Pub/Sub systems**, as it allows multiple independent subscribers to receive updates from a publisher without tight coupling. It is especially useful in event-driven systems, messaging services, and applications where different components need to react to changes or events occurring in other parts of the system.

By using this pattern, you can create a scalable, flexible, and maintainable system where publishers and subscribers are loosely coupled, and new observers can be added easily.

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
