# Chain of Responsibility

The **Chain of Responsibility** is a behavioral design pattern that allows a request to be passed along a chain of handlers, where each handler can process the request or pass it to the next handler in the chain. This pattern decouples the sender of the request from the receiver, allowing multiple objects to process the request without the sender needing to know which object will handle it.

### Key Components:
1. **Handler**: Defines an interface for handling requests, and a reference to the next handler in the chain.
2. **ConcreteHandler**: Implements the handler interface and processes the request, either handling it or passing it to the next handler in the chain.
3. **Client**: Initiates the request, which is then passed along the chain.
4. **Chain**: A sequence of handlers where each handler either processes the request or forwards it to the next handler in the chain.

### Basic Structure:

1. **Handler Interface**:
   - Defines a method for processing the request.
   - Each handler has a reference to the next handler in the chain.

2. **Concrete Handlers**:
   - Each handler checks if it can process the request. If so, it handles it. Otherwise, it forwards the request to the next handler.

3. **Client**:
   - Creates the chain of handlers and initiates the request.

### Example of Chain of Responsibility Pattern in Java

Let's look at an example where a customer service system processes requests based on the urgency level (low, medium, high).

#### Example: Customer Support System

```java
// Handler Interface
abstract class SupportHandler {
    protected SupportHandler nextHandler;

    public void setNextHandler(SupportHandler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public abstract void handleRequest(String issueLevel);
}

// Concrete Handler for Low-Level Issues
class LowLevelSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueLevel) {
        if ("low".equalsIgnoreCase(issueLevel)) {
            System.out.println("Low-Level Support: Handling the issue.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issueLevel);
        }
    }
}

// Concrete Handler for Medium-Level Issues
class MediumLevelSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueLevel) {
        if ("medium".equalsIgnoreCase(issueLevel)) {
            System.out.println("Medium-Level Support: Handling the issue.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issueLevel);
        }
    }
}

// Concrete Handler for High-Level Issues
class HighLevelSupport extends SupportHandler {
    @Override
    public void handleRequest(String issueLevel) {
        if ("high".equalsIgnoreCase(issueLevel)) {
            System.out.println("High-Level Support: Handling the issue.");
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issueLevel);
        }
    }
}

// Client Code
public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        // Create the chain of responsibility
        SupportHandler lowLevel = new LowLevelSupport();
        SupportHandler mediumLevel = new MediumLevelSupport();
        SupportHandler highLevel = new HighLevelSupport();

        // Set up the chain of handlers
        lowLevel.setNextHandler(mediumLevel);
        mediumLevel.setNextHandler(highLevel);

        // Test with different issue levels
        System.out.println("Request with low level issue:");
        lowLevel.handleRequest("low");

        System.out.println("\nRequest with medium level issue:");
        lowLevel.handleRequest("medium");

        System.out.println("\nRequest with high level issue:");
        lowLevel.handleRequest("high");

        System.out.println("\nRequest with unknown issue level:");
        lowLevel.handleRequest("unknown");
    }
}
```

### Output:
```
Request with low level issue:
Low-Level Support: Handling the issue.

Request with medium level issue:
Medium-Level Support: Handling the issue.

Request with high level issue:
High-Level Support: Handling the issue.

Request with unknown issue level:
```

### Key Components in the Example:
- **SupportHandler** is the base class with the `handleRequest` method.
- **LowLevelSupport**, **MediumLevelSupport**, and **HighLevelSupport** are the concrete handlers that process specific issue levels or pass the request to the next handler if they can't handle it.
- **Client** creates the chain and invokes the `handleRequest` method on the first handler (`lowLevel`), which forwards the request along the chain.

---

### Use Cases for the Chain of Responsibility Pattern:

1. **Event Handling in GUI Frameworks**:
   - In GUI applications, events like mouse clicks or key presses often need to be handled in a flexible way. Different components (buttons, text fields, menus) can act as handlers in the event chain.
   - **Example**: In a desktop application, a mouse event might first be handled by a button, then passed to a form, and finally to the main window if none of the previous components can process it.

2. **Middleware in Web Servers**:
   - Web servers (e.g., Apache, Nginx) often use chains of responsibility for processing requests. A request can pass through various middleware handlers like authentication, logging, caching, etc.
   - **Example**: A web request goes through authentication middleware, logging middleware, and data transformation middleware before reaching the final endpoint.

3. **Authorization/Authentication Systems**:
   - Multiple security checks can be handled in a chain: first verifying the user’s credentials, then checking their permissions, and finally auditing the action.
   - **Example**: A user login system might pass the request through multiple security handlers—authentication (verify username/password), authorization (check user role), and logging (audit the login attempt).

4. **Error Handling**:
   - Multiple error handlers can be part of a chain, and each handler may be responsible for dealing with specific types of errors. If one handler cannot deal with the error, it passes the error to the next handler.
   - **Example**: An error handling chain might include database connection errors, file I/O errors, and network errors, each of which is handled by a different handler.

5. **Logging and Monitoring**:
   - In a complex system, you might want to handle logging at different levels. For example, logging might be passed through handlers that log to a console, file, and remote server in a chain.
   - **Example**: In a logging system, different handlers might log messages at different severity levels (info, debug, error) to different destinations (local files, cloud storage, etc.).

6. **Command Processing**:
   - In a system with many command types, commands can be passed through a series of handlers that can process them according to their types. This is often used in command-line applications or chatbots.
   - **Example**: A chatbot might receive a command (e.g., "get weather", "set reminder") and pass it through different handlers for natural language processing, execution, and response formatting.

7. **Financial Transaction Validation**:
   - In a banking system, a transaction request might go through several handlers to ensure its validity: checking for sufficient funds, ensuring no fraud, verifying the transaction type, and so on.
   - **Example**: A banking system processes a withdrawal request where the transaction is checked for sufficient funds, proper account verification, fraud detection, and compliance with financial regulations in sequence.

8. **Customer Support Systems**:
   - As seen in the example above, different levels of customer support can be represented as handlers in a chain. Low-level requests are handled by lower-tier agents, while complex issues are passed up the chain to higher-tier support agents.

---

### Advantages of the Chain of Responsibility Pattern:

1. **Decoupling**: The sender does not need to know which handler will process the request. The request is simply passed along the chain.
2. **Flexibility**: New handlers can be added to the chain without modifying existing code. You can dynamically change the chain of responsibility based on the context.
3. **Responsibility Distribution**: The pattern allows you to distribute the responsibility of handling a request among different handlers, each dealing with a specific aspect of the request.
4. **Chain Reusability**: Handlers can be reused across different chains or contexts, promoting code reusability.

---

### Disadvantages of the Chain of Responsibility Pattern:

1. **Performance**: If the chain is long or complex, it may result in performance overhead as requests are passed through multiple handlers.
2. **Difficult Debugging**: It might be harder to debug issues because the flow of control passes through a series of handlers, and the handler that caused the issue may not be immediately apparent.
3. **Hard to Track**: If the chain becomes too long or complex, tracking which handler is responsible for which actions might get challenging.

---

### Conclusion:

The **Chain of Responsibility** pattern is perfect for scenarios where multiple objects might handle a request, but the exact handler is unknown at the time of request initiation. It allows for flexible, extensible handling of requests while keeping sender and receiver decoupled. The pattern is widely used in event handling, middleware, validation systems, error handling, and various other applications where requests must pass through a series of handlers.

Sure! Let’s look at another example where we can apply the **Chain of Responsibility** pattern. This time, let's create a **purchase approval system** in an e-commerce platform.

### Example: E-commerce Purchase Approval System

In this example, we have a system where a purchase request needs to be approved based on the amount. There are different levels of approval based on the purchase amount:

- **Junior Manager** can approve purchases up to $100.
- **Senior Manager** can approve purchases up to $1,000.
- **Executive** can approve purchases over $1,000.

The request will be passed through a chain of approvers, and each handler (manager) will either approve the purchase or pass it to the next level.

### Code Implementation:

```java
// Handler Interface
abstract class PurchaseApprover {
    protected PurchaseApprover nextApprover;

    public void setNextApprover(PurchaseApprover nextApprover) {
        this.nextApprover = nextApprover;
    }

    public abstract void approvePurchase(PurchaseRequest request);
}

// Concrete Handler for Junior Manager
class JuniorManager extends PurchaseApprover {
    @Override
    public void approvePurchase(PurchaseRequest request) {
        if (request.getAmount() <= 100) {
            System.out.println("Junior Manager approves the purchase of " + request.getAmount());
        } else if (nextApprover != null) {
            nextApprover.approvePurchase(request);
        }
    }
}

// Concrete Handler for Senior Manager
class SeniorManager extends PurchaseApprover {
    @Override
    public void approvePurchase(PurchaseRequest request) {
        if (request.getAmount() <= 1000) {
            System.out.println("Senior Manager approves the purchase of " + request.getAmount());
        } else if (nextApprover != null) {
            nextApprover.approvePurchase(request);
        }
    }
}

// Concrete Handler for Executive
class Executive extends PurchaseApprover {
    @Override
    public void approvePurchase(PurchaseRequest request) {
        if (request.getAmount() > 1000) {
            System.out.println("Executive approves the purchase of " + request.getAmount());
        } else if (nextApprover != null) {
            nextApprover.approvePurchase(request);
        }
    }
}

// PurchaseRequest class
class PurchaseRequest {
    private double amount;

    public PurchaseRequest(double amount) {
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}

// Client Code
public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        // Create the handlers (approvers)
        PurchaseApprover juniorManager = new JuniorManager();
        PurchaseApprover seniorManager = new SeniorManager();
        PurchaseApprover executive = new Executive();

        // Set up the chain of responsibility
        juniorManager.setNextApprover(seniorManager);
        seniorManager.setNextApprover(executive);

        // Create purchase requests
        PurchaseRequest request1 = new PurchaseRequest(50); // Small purchase
        PurchaseRequest request2 = new PurchaseRequest(500); // Medium purchase
        PurchaseRequest request3 = new PurchaseRequest(1500); // Large purchase

        // Process the purchase requests
        System.out.println("Processing request for $50:");
        juniorManager.approvePurchase(request1);  // Handled by Junior Manager

        System.out.println("\nProcessing request for $500:");
        juniorManager.approvePurchase(request2);  // Handled by Senior Manager

        System.out.println("\nProcessing request for $1500:");
        juniorManager.approvePurchase(request3);  // Handled by Executive
    }
}
```

### Output:

```
Processing request for $50:
Junior Manager approves the purchase of 50.0

Processing request for $500:
Senior Manager approves the purchase of 500.0

Processing request for $1500:
Executive approves the purchase of 1500.0
```

### Breakdown of the Example:

1. **Purchase Approver**: 
   - The base class `PurchaseApprover` defines the common interface for all handlers, with the `approvePurchase` method.
   - It also holds a reference to the next handler in the chain (`nextApprover`), allowing it to pass the request along the chain if it can't handle it.

2. **Concrete Handlers**:
   - **JuniorManager**: Handles requests where the purchase amount is $100 or less. If the amount exceeds this, it passes the request to the next handler.
   - **SeniorManager**: Handles requests with amounts up to $1,000. If the amount exceeds this, it passes the request to the next handler.
   - **Executive**: Handles requests with amounts over $1,000.

3. **PurchaseRequest**: This class represents the purchase request and holds the amount of the purchase.

4. **Client Code**:
   - The `ChainOfResponsibilityExample` class sets up the chain of approvers.
   - It then creates `PurchaseRequest` objects with different amounts and passes them through the chain starting with the `juniorManager`.
   
---

### Use Cases for the Chain of Responsibility Pattern in the Example:

1. **Delegating Responsibilities**:
   - The pattern allows for a clear delegation of responsibilities. Each handler is responsible for approving purchases within a certain range, while others take over when the amount is out of their range.
   - This approach avoids the need for multiple `if-else` conditions or switch statements and provides more flexibility to extend or modify the system.

2. **Extensibility**:
   - Adding new levels of approvers is easy. If you wanted to add a **"Director"** or **"VP"** level of approval, you could simply create a new handler and insert it into the chain.
   - For example, you could add another handler to approve purchases over $10,000.

3. **Flexible Request Handling**:
   - The request can pass through multiple handlers, each of which decides whether to process the request or pass it along.
   - This allows for complex decision-making processes where requests can be processed at different levels of an organization.

4. **Decoupling**:
   - The client code does not need to know the specific handler (approver) that will process the request. The client just initiates the request and the chain takes care of the rest.

5. **Error Handling**:
   - If no handler can process the request (e.g., a handler for a purchase amount that doesn't fit within any range), the request can either be logged as an error or passed to a final "default handler" that takes care of unprocessed requests.

---

### Advantages of Using Chain of Responsibility in This Example:

1. **Simplified Client Code**:
   - The client code only interacts with the first handler in the chain. It does not need to worry about the details of which handler will process the request.

2. **Open/Closed Principle**:
   - The pattern allows you to add new handlers without modifying the existing code. If you need to introduce more levels of approval, you can do so without affecting the existing handlers.

3. **Responsibility Segmentation**:
   - Each handler is responsible for a specific range of purchase amounts, making it clear which parts of the system handle what.

4. **Dynamic Request Processing**:
   - The request is processed dynamically based on its characteristics (purchase amount in this case). Each handler decides whether it can process the request or pass it to the next one.

---

### Disadvantages:

1. **Long Chains**:
   - If the chain becomes too long, it could introduce overhead as requests are passed down through multiple handlers before being processed.
   
2. **Difficulty in Tracing Requests**:
   - If there is an issue with a request, it may be harder to trace exactly where it was handled, especially if the chain is complex.

3. **Potential for Unhandled Requests**:
   - If no handler in the chain is able to process the request, it might end up being unhandled unless a final fallback handler is provided.

---

### Conclusion:
The **Chain of Responsibility** pattern is a great solution for scenarios where requests need to be handled by multiple handlers, each with specific criteria. It provides a flexible, extensible way of delegating responsibility for handling requests while decoupling the request initiator from the actual handlers. This pattern is commonly used in systems with varying levels of processing or approval, like the **purchase approval system** demonstrated here.
