# DesignPattern
DesignPattern

## 1. State.java

public interface State {<br>
	public abstract void doAction();<br>
}<br>

## 2. ACStartState.java

/** An Implementation to perform START/ON Action */<br>
public class ACStartState implements State {<br>
	@Override<br>
	public void doAction() {<br>
		System.out.println("AC is turned ON");<br>
	}<br>
}<br>

## 3. ACStopState.java

/** An Implementation to perform STOP/OFF Action */<br>
public class ACStopState implements State {<br>
	@Override<br>
	public void doAction() {<br>
		System.out.println("AC is turned OFF");<br>
	}<br>
}<br>

## 4. ACContext.java

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

## 5.ACRemoteTest.java

/** <br>
 * Client Program which makes use of<br>
 * State design pattern<br>
 */<br>
public class ACRemoteTest {<br>
 
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
	}<br>
}
