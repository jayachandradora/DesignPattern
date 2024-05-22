# DesignPattern

The Prototype Design Pattern is a creational design pattern that focuses on creating objects based on a template or prototype instance. Instead of creating objects from scratch, the pattern allows cloning of existing objects, which serves as a prototype. This approach is particularly useful when the construction of a new instance is more expensive than copying an existing one.

### Example:

Let's consider a scenario where we have a Shape interface representing different geometric shapes like Circle and Rectangle. We'll implement the Prototype Design Pattern to clone these shapes.

```java

// Shape interface
interface Shape extends Cloneable {
    void draw();
    Shape clone();
}

// Concrete implementation of Circle
class Circle implements Shape {
    private String type;

    Circle(String type) {
        this.type = type;
    }

    @Override
    public void draw() {
        System.out.println("Drawing " + type + " Circle");
    }

    @Override
    public Shape clone() {
        return new Circle(type);
    }
}

// Concrete implementation of Rectangle
class Rectangle implements Shape {
    private String type;

    Rectangle(String type) {
        this.type = type;
    }

    @Override
    public void draw() {
        System.out.println("Drawing " + type + " Rectangle");
    }

    @Override
    public Shape clone() {
        return new Rectangle(type);
    }
}

// Client class
public class PrototypeDemo {
    public static void main(String[] args) {
        // Create prototypes
        Circle circlePrototype = new Circle("Red");
        Rectangle rectanglePrototype = new Rectangle("Blue");

        // Clone prototypes
        Shape circleClone = circlePrototype.clone();
        Shape rectangleClone = rectanglePrototype.clone();

        // Draw clones
        circleClone.draw();
        rectangleClone.draw();
    }
}

```
### Use Cases:

1.	**Reducing Object Creation Overhead:** When creating an object involves complex initialization processes or resource-intensive operations, the Prototype pattern allows you to create new instances by copying existing ones, thereby reducing overhead.

2.	**Dynamic Object Creation:** Prototype pattern allows creating new objects at runtime by copying prototypes. This is particularly useful when the type of objects to be created is not known until runtime.

3.	**Managing Immutable Objects:** In scenarios where objects are immutable (i.e., their state cannot be changed once created), the Prototype pattern provides an efficient way to create new instances with different initial states by cloning existing ones.

4.	**Avoiding Subclassing:** Prototype pattern can be used to avoid subclassing of creator classes. Instead of creating subclasses for each variation of an object, you can clone a prototype and modify it as needed.

5.	**Stateful Prototypes:** Prototypes can encapsulate not only the structure but also the internal state of an object. This allows creating new instances with the same state as the prototype, which is useful for maintaining consistent initial states across objects.

Overall, the Prototype Design Pattern is a powerful tool for managing object creation and promoting code reusability by allowing the creation of new objects based on existing ones.


`Prototype comes under a creational design pattern that makes use of cloning objects if object creation complex and time-consuming.`

### Student.java
```ruby
package prototype;
public class Student {
 
	private Integer id;
	private String name;
	
	public Integer getId() {
		return id;
	}
 
	public void setId(Integer id) {
		this.id = id;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
	
	@Override
	public String toString() {
		return "Student [Student id=" + id + ", Student name=" + name + "]";
	}
}
```
### StudentDAO.java creates Cloned Object that makes use of  Prototype Pattern:
```ruby
package prototype;
import java.util.ArrayList;
import java.util.List;
/**
 * Dummy Student DAO implementation
 * In Real Scenario Object can be constructed by calling database.
 */
public class StudentDAO implements Cloneable{

  private static List<Student> studentList;
	
	static {
		studentList = new ArrayList<>();

		Student student1 = new Student();
		student1.setId(10);
		student1.setName("PK");

		Student student2 = new Student();
		student2.setId(20);
		student2.setName("MK");

		studentList.add(student1);
		studentList.add(student2);
	}
	
	/**
	 * In Real Scenario Object can be constructed by calling database.
	 * @return Student List
	 */
	public List<Student> getAllStudents(){
		return studentList;
	}
	
	/**
	 * Creating Clone of an Existing object
	 */
	@Override
	public List<Student> clone() throws CloneNotSupportedException {
		
		List<Student> dummyStudentList = new ArrayList<>();
		for (Student student : studentList) {
			dummyStudentList.add(student);
		}
		return dummyStudentList;
	}
}
```
### PrototypePatternTest.java
```ruby
package prototype;
import java.util.List;
/**
 *Client Program which uses cloned object
 */
public class PrototypePatternTest {
 
	public static void main(String[] args) throws CloneNotSupportedException  {
		
		StudentDAO studentDAO = new StudentDAO();
		
		//Getting Original copy of student list
		List<Student> allStudents = studentDAO.getAllStudents();
		
		//Getting clone copy of student list
		List<Student> updatedStudentList = studentDAO.clone();
		Student student = new Student();
		student.setId(30);
		student.setName("SK");
		
		//Doing manipulation on cloned copy
		updatedStudentList.add(student);
		
		System.out.println("Original Student List::");
		allStudents.forEach(System.out::println);
		
		System.out.println("Updated Student List::");
		updatedStudentList.forEach(System.out::println);
	}
}
```
