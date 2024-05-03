# State Design Pattern
The main idea of State pattern is to allow the object for changing its behavior without changing its class. Also, by implementing it, the code should remain cleaner without many if/else statements.

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

# Scenario

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

// Other state classes implemented similarly...

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
