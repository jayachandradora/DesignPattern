# Facade Design Pattern

The Facade design pattern is a structural pattern that provides a simplified interface to a complex subsystem of classes, making it easier to use. Here's an overview of the Facade pattern, along with its key components, characteristics, use cases, and an example:

## Facade Design Pattern

- **Intent:** Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

- **Key Components:**
  1. **Facade:** Provides a simplified interface to the complex subsystem. It delegates client requests to appropriate subsystem objects and coordinates their interactions.
  2. **Subsystem Classes:** Represents the various components and functionality of the subsystem. These classes implement the actual functionality required by the client.

- **Characteristics:**
  - Simplifies client usage by providing a single entry point to access complex subsystem functionality.
  - Encapsulates the complexity of the subsystem behind a well-defined interface.
  - Promotes loose coupling between clients and the subsystem, as clients interact with the facade rather than directly with subsystem classes.

## Use Cases

1. **API Facade for External Services:**
   - Use a facade to abstract the complexities of interacting with external APIs or services. This simplifies the integration process and shields the client from changes in the underlying services.
   
2. **Subsystem Initialization and Configuration:**
   - Use a facade to encapsulate the initialization and configuration of a complex subsystem. This provides a single entry point for clients to configure the subsystem without exposing internal implementation details.
   
3. **Legacy System Integration:**
   - When integrating with legacy systems with complex interfaces, use a facade to provide a modern, simplified interface. This allows clients to interact with the legacy system without dealing with its intricacies.

## Example - 0

Another **great example** of the **Facade Pattern** in action is the **Order Processing System** in an **e-commerce application**. E-commerce systems often have many complex subsystems to handle different aspects of an order, such as inventory management, payment processing, shipping, and customer notification. Instead of exposing all of these individual subsystems to the client (e.g., the website or mobile app), we can use a facade to provide a simplified interface for placing and processing orders.

### Real-World Use Case: E-Commerce Order Processing

In this example, we will model an order processing system that involves:
- **Inventory Management**: Checking and updating product stock.
- **Payment Gateway**: Processing payment for the order.
- **Shipping Service**: Shipping the order.
- **Customer Notification**: Notifying the customer about the order status.

Using the **Facade Pattern**, we will create an `OrderFacade` that hides the complexity of interacting with these subsystems and provides a simple `placeOrder` method to the client.

---

### Java Implementation: E-Commerce Order Processing System

#### Step 1: Subsystems (Inventory, Payment, Shipping, Notification)

```java
// Subsystem 1: Inventory System
class InventorySystem {
    public boolean checkStock(String productId) {
        // In a real system, you'd query the database or an external service
        System.out.println("Checking stock for product " + productId);
        return true; // Assume the product is always in stock
    }

    public void updateStock(String productId) {
        // In a real system, you'd update the stock level in the database
        System.out.println("Updating stock for product " + productId);
    }
}

// Subsystem 2: Payment Gateway
class PaymentGateway {
    public boolean processPayment(String paymentMethod, double amount) {
        // In a real system, you'd integrate with a payment processor
        System.out.println("Processing payment of " + amount + " using " + paymentMethod);
        return true; // Assume payment is always successful
    }
}

// Subsystem 3: Shipping Service
class ShippingService {
    public void shipOrder(String productId, String address) {
        // In a real system, you'd integrate with a shipping API
        System.out.println("Shipping product " + productId + " to " + address);
    }
}

// Subsystem 4: Customer Notification
class NotificationService {
    public void sendOrderConfirmation(String customerEmail) {
        // In a real system, you'd send an email or SMS
        System.out.println("Sending order confirmation to " + customerEmail);
    }
}
```

#### Step 2: Facade Class (OrderFacade)

```java
// Facade Class: OrderFacade
class OrderFacade {
    private InventorySystem inventorySystem;
    private PaymentGateway paymentGateway;
    private ShippingService shippingService;
    private NotificationService notificationService;

    public OrderFacade(InventorySystem inventorySystem, PaymentGateway paymentGateway, 
                       ShippingService shippingService, NotificationService notificationService) {
        this.inventorySystem = inventorySystem;
        this.paymentGateway = paymentGateway;
        this.shippingService = shippingService;
        this.notificationService = notificationService;
    }

    // Simplified method for placing an order
    public void placeOrder(String productId, double amount, String paymentMethod, String customerEmail, String shippingAddress) {
        // Step 1: Check inventory
        if (!inventorySystem.checkStock(productId)) {
            System.out.println("Product out of stock. Order cannot be processed.");
            return;
        }

        // Step 2: Process payment
        if (!paymentGateway.processPayment(paymentMethod, amount)) {
            System.out.println("Payment failed. Order cannot be processed.");
            return;
        }

        // Step 3: Update inventory
        inventorySystem.updateStock(productId);

        // Step 4: Ship the product
        shippingService.shipOrder(productId, shippingAddress);

        // Step 5: Notify the customer
        notificationService.sendOrderConfirmation(customerEmail);

        System.out.println("Order placed successfully!");
    }
}
```

#### Step 3: Client Class (E-Commerce Application)

```java
// Client class: ECommerceApplication
class ECommerceApplication {
    public static void main(String[] args) {
        // Create subsystem objects
        InventorySystem inventorySystem = new InventorySystem();
        PaymentGateway paymentGateway = new PaymentGateway();
        ShippingService shippingService = new ShippingService();
        NotificationService notificationService = new NotificationService();

        // Create the facade object
        OrderFacade orderFacade = new OrderFacade(inventorySystem, paymentGateway, shippingService, notificationService);

        // Place an order
        String productId = "12345";
        double orderAmount = 99.99;
        String paymentMethod = "Credit Card";
        String customerEmail = "customer@example.com";
        String shippingAddress = "123 Main St, Cityville";

        // Place the order
        orderFacade.placeOrder(productId, orderAmount, paymentMethod, customerEmail, shippingAddress);
    }
}
```

### Explanation:

1. **Subsystems**: 
   - **InventorySystem**: Handles checking and updating product stock.
   - **PaymentGateway**: Processes the payment using a specified payment method.
   - **ShippingService**: Ships the product to the provided shipping address.
   - **NotificationService**: Sends a notification (e.g., email) to the customer confirming their order.

2. **Facade Class (`OrderFacade`)**: 
   - The `OrderFacade` class provides a **simplified interface** for placing an order. It coordinates the interaction between the subsystems. The client (e.g., the e-commerce app or website) only interacts with the facade, not the individual subsystems.
   - The facade method `placeOrder()` handles the flow of actions required to process an order: checking inventory, processing payment, updating stock, shipping the product, and sending a confirmation to the customer.

3. **Client (`ECommerceApplication`)**: 
   - The client interacts with the `OrderFacade` to place an order. The client does not need to worry about the underlying subsystems or their complexities. It simply calls `placeOrder()` and passes the necessary parameters.

### Example Output:

```
Checking stock for product 12345
Processing payment of 99.99 using Credit Card
Updating stock for product 12345
Shipping product 12345 to 123 Main St, Cityville
Sending order confirmation to customer@example.com
Order placed successfully!
```

### Key Benefits of the Facade Pattern in This Example:

1. **Simplified Client Interface**: The client (e-commerce application) does not need to know about the individual subsystems. They simply call a single `placeOrder()` method from the `OrderFacade`.
2. **Loose Coupling**: The client is decoupled from the subsystems. The `OrderFacade` takes care of the interactions between different systems (inventory, payment, shipping, notification), so changes in the subsystems do not affect the client.
3. **Readability and Maintainability**: The code is more readable and easier to maintain. If there is a change in the way payments are processed or inventory is checked, only the `OrderFacade` and the relevant subsystem need to be updated, rather than the entire client codebase.
4. **Reduced Complexity**: The facade hides the complexity of dealing with multiple subsystems, making it easier for the client to use the system without understanding the intricacies of each subsystem.

### Conclusion:
The **Facade Pattern** is highly effective in simplifying interactions with complex subsystems, and this **e-commerce order processing system** is a perfect example of how it can be used. The facade provides a unified, high-level interface that makes it easier for the client to place an order, without needing to worry about the underlying complexities of inventory management, payment processing, shipping, and customer notifications.

## Example - 1

Consider a banking system that consists of multiple subsystems such as account management, transaction processing, and customer service. We can use the Facade pattern to create a `BankingFacade` that provides a simplified interface for common banking operations:

```java
// Subsystem classes
class AccountManagement {
    public void createAccount(String accountNumber) {
        System.out.println("Account created: " + accountNumber);
    }
    // Other account management methods...
}

class TransactionProcessing {
    public void deposit(String accountNumber, double amount) {
        System.out.println("Deposit of $" + amount + " to account " + accountNumber + " processed.");
    }
    // Other transaction processing methods...
}

class CustomerService {
    public void inquireBalance(String accountNumber) {
        System.out.println("Balance inquiry for account " + accountNumber + " processed.");
    }
    // Other customer service methods...
}

// Facade class
class BankingFacade {
    private AccountManagement accountManagement;
    private TransactionProcessing transactionProcessing;
    private CustomerService customerService;

    public BankingFacade() {
        accountManagement = new AccountManagement();
        transactionProcessing = new TransactionProcessing();
        customerService = new CustomerService();
    }

    // Simplified interface methods
    public void openAccount(String accountNumber) {
        accountManagement.createAccount(accountNumber);
    }

    public void deposit(String accountNumber, double amount) {
        transactionProcessing.deposit(accountNumber, amount);
    }

    public void checkBalance(String accountNumber) {
        customerService.inquireBalance(accountNumber);
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        BankingFacade bankingFacade = new BankingFacade();
        bankingFacade.openAccount("123456789");
        bankingFacade.deposit("123456789", 1000.00);
        bankingFacade.checkBalance("123456789");
    }
}
```

### Example - 2

Consider a multimedia player application that consists of multiple subsystems such as audio player, video player, and playlist manager. We can use the Facade pattern to create a MultimediaPlayerFacade that provides a unified interface to perform common operations such as play, pause, stop, and skip tracks:

```java

// Subsystem classes
class AudioPlayer {
    public void play() { System.out.println("Audio playing..."); }
    public void pause() { System.out.println("Audio paused."); }
    public void stop() { System.out.println("Audio stopped."); }
}

class VideoPlayer {
    public void play() { System.out.println("Video playing..."); }
    public void pause() { System.out.println("Video paused."); }
    public void stop() { System.out.println("Video stopped."); }
}

class PlaylistManager {
    public void next() { System.out.println("Next track..."); }
    public void previous() { System.out.println("Previous track..."); }
}

// Facade class
class MultimediaPlayerFacade {
    private AudioPlayer audioPlayer;
    private VideoPlayer videoPlayer;
    private PlaylistManager playlistManager;

    public MultimediaPlayerFacade() {
        audioPlayer = new AudioPlayer();
        videoPlayer = new VideoPlayer();
        playlistManager = new PlaylistManager();
    }

    public void play() {
        audioPlayer.play();
        videoPlayer.play();
    }

    public void pause() {
        audioPlayer.pause();
        videoPlayer.pause();
    }

    public void stop() {
        audioPlayer.stop();
        videoPlayer.stop();
    }

    public void nextTrack() {
        playlistManager.next();
    }

    public void previousTrack() {
        playlistManager.previous();
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        MultimediaPlayerFacade player = new MultimediaPlayerFacade();
        player.play();
        player.pause();
        player.nextTrack();
    }
}

```
