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

/** This Contaxt class performed Action based on state  */<br>
public class ACContext implements State {<br>
 
	private State state;<br>
	public void setState(State state) {<br>
		this.state = state;<br>
	}<br>
	
	public State getState() {<br>
		return state;<br>
	}<br>
	
	@Override<br>
	public void doAction() {<br>
		state.doAction();<br>
	}<br>
}<br>

## 5.ACRemoteTest.java

/** <br>
 * Client Program which makes use of<br>
 * State design pattern<br>
 */<br>
public class ACRemoteTest {<br>
 
	public static void main(String[] args) {<br>
		
		//Create Context Object<br>
		ACContext acContext = new ACContext();<br><br>
		
		//Create State Object<br>
		State AcStartState = new ACStartState();<br><br>
		
		//Now setting state to start AC<br>
		acContext.setState(AcStartState);<br><br>
		
		//Now Perform Action<br>
		acContext.doAction();<br><br>
		
		System.out.println("-------------------------------");<br>
		
		//Now setting State to stop AC<br>
		State AcStopState = new ACStopState();<br>
		acContext.setState(AcStopState); <br>
		
		//Now Perform Action <br>
		acContext.doAction();<br>
	}
}
