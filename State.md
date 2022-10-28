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
