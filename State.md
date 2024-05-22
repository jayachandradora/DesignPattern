# State Design Pattern
The main idea of State pattern is to allow the object for changing its behavior without changing its class. Also, by implementing it, the code should remain cleaner without many if/else statements.

The State Design Pattern is a behavioral design pattern that allows an object to alter its behavior when its internal state changes. This pattern is useful when an object's behavior depends on its state, and it must change its behavior at runtime based on that state.

### How State Design Pattern Works:

**Context:** This is the class that contains the state. It delegates the state-specific behavior to the current state object.

**State Interface:** This interface defines a set of methods that encapsulate the behavior associated with a particular state.

**Concrete States:** These are the classes that implement the State interface. Each concrete state provides its own implementation of the behavior associated with that state.

### Example:
Let's consider an example of a TCP connection that can be in different states such as Established, Closed, and Reset. We'll implement the State Design Pattern to represent these states and transition between them.

```java

// State interface
interface TCPState {
    void open();
    void close();
    void reset();
}

// Concrete state classes
class TCPEstablishedState implements TCPState {
    @Override
    public void open() {
        System.out.println("TCP connection is already open.");
    }

    @Override
    public void close() {
        System.out.println("Closing TCP connection.");
    }

    @Override
    public void reset() {
        System.out.println("Resetting TCP connection.");
    }
}

class TCPClosedState implements TCPState {
    @Override
    public void open() {
        System.out.println("Opening TCP connection.");
    }

    @Override
    public void close() {
        System.out.println("TCP connection is already closed.");
    }

    @Override
    public void reset() {
        System.out.println("Cannot reset, TCP connection is closed.");
    }
}

class TCPResetState implements TCPState {
    @Override
    public void open() {
        System.out.println("Cannot open, TCP connection is in reset state.");
    }

    @Override
    public void close() {
        System.out.println("Cannot close, TCP connection is in reset state.");
    }

    @Override
    public void reset() {
        System.out.println("Resetting TCP connection.");
    }
}

// Context class representing the TCP connection
class TCPConnection {
    private TCPState currentState;

    public TCPConnection() {
        currentState = new TCPClosedState(); // Initial state is Closed
    }

    public void setState(TCPState state) {
        currentState = state;
    }

    public void open() {
        currentState.open();
    }

    public void close() {
        currentState.close();
    }

    public void reset() {
        currentState.reset();
    }
}

// Client code
public class StatePatternDemo {
    public static void main(String[] args) {
        TCPConnection tcpConnection = new TCPConnection();

        // Establish connection
        tcpConnection.open();

        // Close connection
        tcpConnection.close();

        // Reset connection
        tcpConnection.reset();
    }
}

```

### Use Cases:

1.	**Object Behavior Depends on State:** Use the State pattern when an object's behavior changes based on its internal state.

2.	**Avoiding Long, Complex Conditional Statements:** Use the State pattern to replace complex conditional statements with a cleaner and more maintainable design.

3.	**Flexibility and Extensibility:** The State pattern makes it easy to add new states or change existing ones without modifying the context class.

4.	**Finite State Machines (FSMs):** Use the State pattern to model finite state machines where an object can be in one of several states and transitions between states are triggered by events or actions.

5.	**Game Development:** In game development, the State pattern can be used to represent the various states of game characters or entities, such as idle, walking, running, attacking, etc.

## 1. State.java
```ruby
public interface State {
	public abstract void doAction();
}
```

## 2. ACStartState.java
```ruby
/** An Implementation to perform START/ON Action */
public class ACStartState implements State {
	@Override
	public void doAction() {
		System.out.println("AC is turned ON");
	}
}
```
## 3. ACStopState.java
```ruby
/** An Implementation to perform STOP/OFF Action */
public class ACStopState implements State {
	@Override
	public void doAction() {
		System.out.println("AC is turned OFF");
	}
}
```
## 4. ACContext.java

```ruby
/** This Contaxt class performed Action based on state  */ 
public class ACContext implements State {
 
	private State state;
	public void setState(State state) {
		this.state = state;
	}
	
	public State getState() {
		return state;
	}
	
	@Override<br>
	public void doAction() {
		state.doAction();
	}
}
```
## 5.ACRemoteTest.java
```ruby
/** Client Program which makes use of State design pattern<br> **/ 
public class ACRemoteTest {
 
	public static void main(String[] args) {
		 
		//Create Context Object
		ACContext acContext = new ACContext();
		
		//Create State Object
		State AcStartState = new ACStartState();
		
		//Now setting state to start AC
		acContext.setState(AcStartState);
		
		//Now Perform Action
		acContext.doAction();
		
		System.out.println("-------------------------------");
		
		//Now setting State to stop AC
		State AcStopState = new ACStopState();
		acContext.setState(AcStopState); 
		
		//Now Perform Action 
		acContext.doAction();
	}
}
```

# Best Real World Use Case

Consider a simple vending machine that can be in different states: NoSelectionState, HasSelectionState, InsertCoinState, and SoldState. The behavior of the vending machine changes depending on its current state.

## Example

```ruby

// Interface representing the states
interface VendingMachineState {
    void selectItem(String item);
    void insertCoin(int amount);
    void dispenseItem();
}

// Concrete state classes
class NoSelectionState implements VendingMachineState {
    private VendingMachine vendingMachine;

    public NoSelectionState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void selectItem(String item) {
        vendingMachine.setItem(item);
        vendingMachine.changeState(new HasSelectionState(vendingMachine));
        System.out.println("Item selected: " + item);
    }

    @Override
    public void insertCoin(int amount) {
        System.out.println("Please select an item first");
    }

    @Override
    public void dispenseItem() {
        System.out.println("Please select an item first");
    }
}

// State representing when an item is selected
class HasSelectionState implements VendingMachineState {
    private VendingMachine vendingMachine;

    public HasSelectionState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void selectItem(String item) {
        System.out.println("Item " + item + " is already selected");
    }

    @Override
    public void insertCoin(int amount) {
        vendingMachine.setAmountInserted(amount);
        vendingMachine.changeState(new InsertCoinState(vendingMachine));
        System.out.println(amount + " cents inserted");
    }

    @Override
    public void dispenseItem() {
        System.out.println("Please insert coins first");
    }
}

// State representing when coins are inserted
class InsertCoinState implements VendingMachineState {
    private VendingMachine vendingMachine;

    public InsertCoinState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void selectItem(String item) {
        System.out.println("Please finish inserting coins first");
    }

    @Override
    public void insertCoin(int amount) {
        int totalAmount = vendingMachine.getAmountInserted() + amount;
        vendingMachine.setAmountInserted(totalAmount);
        System.out.println(amount + " cents inserted. Total: " + totalAmount);
    }

    @Override
    public void dispenseItem() {
        int itemCost = vendingMachine.getItemCost();
        int amountInserted = vendingMachine.getAmountInserted();
        if (amountInserted >= itemCost) {
            vendingMachine.changeState(new SoldState(vendingMachine));
            vendingMachine.dispenseItem();
        } else {
            System.out.println("Please insert more coins to cover the cost");
        }
    }
}

// State representing when an item is dispensed
class SoldState implements VendingMachineState {
    private VendingMachine vendingMachine;

    public SoldState(VendingMachine vendingMachine) {
        this.vendingMachine = vendingMachine;
    }

    @Override
    public void selectItem(String item) {
        System.out.println("Please wait, dispensing the item");
    }

    @Override
    public void insertCoin(int amount) {
        System.out.println("Already dispensing the item");
    }

    @Override
    public void dispenseItem() {
        int amountInserted = vendingMachine.getAmountInserted();
        int itemCost = vendingMachine.getItemCost();
        if (amountInserted >= itemCost) {
            System.out.println("Item dispensed. Enjoy!");
            int change = amountInserted - itemCost;
            if (change > 0) {
                System.out.println("Change returned: " + change + " cents");
            }
            vendingMachine.setAmountInserted(0);
            vendingMachine.changeState(new NoSelectionState(vendingMachine));
        } else {
            System.out.println("Insufficient amount inserted to dispense the item");
        }
    }
}
// Context class (Vending Machine)
class VendingMachine {
    private VendingMachineState state;
    private String item;

    public VendingMachine() {
        this.state = new NoSelectionState(this);
    }

    public void setState(VendingMachineState state) {
        this.state = state;
    }

    public void changeState(VendingMachineState state) {
        setState(state);
    }

    public void selectItem(String item) {
        state.selectItem(item);
    }

    public void insertCoin(int amount) {
        state.insertCoin(amount);
    }

    public void dispenseItem() {
        state.dispenseItem();
    }

    public void setItem(String item) {
        this.item = item;
    }

    // Other methods...
}

// Client code
public class Main {
    public static void main(String[] args) {
        VendingMachine vendingMachine = new VendingMachine();
        vendingMachine.selectItem("Soda");
        vendingMachine.insertCoin(50);
        vendingMachine.dispenseItem();
    }
}

```
In this example, the VendingMachine class represents the context and maintains a reference to the current state. The VendingMachineState interface declares methods that represent the actions that can be performed on the vending machine. Each concrete state class implements these methods according to its behavior.

When a user interacts with the vending machine, the context delegates the request to the current state object, which handles the request appropriately based on its implementation. As the state changes, the context updates its current state accordingly.

This pattern allows for cleaner and more modular code, as each state encapsulates its behavior, making it easier to add new states or modify existing ones without affecting the context or other states.
