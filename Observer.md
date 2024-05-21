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

## Example

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
