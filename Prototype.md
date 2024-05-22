# Prototype Design Pattern

The Prototype Design Pattern is a creational design pattern that focuses on creating objects based on a template or prototype instance. Instead of creating objects from scratch, the pattern allows cloning of existing objects, which serves as a prototype. This approach is particularly useful when the construction of a new instance is more expensive than copying an existing one.

In the Prototype Design Pattern, new objects are created by copying or cloning existing objects, known as prototypes. This process allows the creation of new instances with the same state as the prototype, effectively serving as a blueprint for object creation.

### How it Works:

**Prototype Interface/Abstract Class:** Define an interface or abstract class that declares a method for cloning itself. This interface or abstract class serves as the common base for all concrete prototypes.

**Concrete Prototypes:** Implement the Prototype interface/abstract class to create concrete prototypes. These prototypes should provide a method to clone themselves. The cloning operation should create a new instance of the concrete prototype and copy the state from the existing instance to the new one.

**Client Code:** In the client code, create instances of concrete prototypes and use them to create new objects by cloning. The client should request a clone of the prototype whenever it needs a new object, instead of creating the object directly.

### Example 1:

```java

// Shape interface serving as the prototype
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
        return new Circle(type); // Clone by creating a new instance
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
        return new Rectangle(type); // Clone by creating a new instance
    }
}

// Client code
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

### Example 2:

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

inn this example, the Circle class implements the Cloneable interface and overrides the clone() method to provide a shallow copy. We then demonstrate the shallow copy process by creating a clone of the Circle prototype.

For deep copy, we simulate the process by creating a new Circle instance and manually copying the state (in this case, the type). In a real-world scenario, you might need to implement a deep copy method that recursively clones any referenced objects.


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

### Shallow Copy Example:

In this example, we'll create a Person class representing a person with a name and a List of hobbies. We'll implement shallow copy for the Person class.

```java

import java.util.ArrayList;
import java.util.List;

// Person class representing a person with name and hobbies
class Person implements Cloneable {
    private String name;
    private List<String> hobbies;

    public Person(String name, List<String> hobbies) {
        this.name = name;
        this.hobbies = hobbies;
    }

    public String getName() {
        return name;
    }

    public List<String> getHobbies() {
        return hobbies;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class ShallowCopyExample {
    public static void main(String[] args) {
        // Original person
        List<String> hobbies = new ArrayList<>();
        hobbies.add("Reading");
        hobbies.add("Gardening");

        Person originalPerson = new Person("Alice", hobbies);

        // Shallow copy
        Person shallowCopyPerson = null;
        try {
            shallowCopyPerson = (Person) originalPerson.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }

        // Modify hobbies of the shallow copy
        shallowCopyPerson.getHobbies().add("Painting");

        // Original person's hobbies are also affected
        System.out.println("Original Person Hobbies: " + originalPerson.getHobbies());
        System.out.println("Shallow Copy Person Hobbies: " + shallowCopyPerson.getHobbies());
    }
}

```

In this example, when we create a shallow copy of the Person object and modify the hobbies list of the shallow copy, the original person's hobbies list is also affected. This is because the shallow copy only copies references to the original objects, not the objects themselves.

### Deep Copy Example:

In this example, we'll create a Person class similar to the previous example but implement a deep copy for the Person class.

```java

import java.util.ArrayList;
import java.util.List;

// Person class representing a person with name and hobbies
class Person implements Cloneable {
    private String name;
    private List<String> hobbies;

    public Person(String name, List<String> hobbies) {
        this.name = name;
        this.hobbies = hobbies;
    }

    public String getName() {
        return name;
    }

    public List<String> getHobbies() {
        return hobbies;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        // Deep copy hobbies list
        List<String> clonedHobbies = new ArrayList<>(this.hobbies);
        // Create and return a new Person object with cloned hobbies
        return new Person(this.name, clonedHobbies);
    }
}

public class DeepCopyExample {
    public static void main(String[] args) {
        // Original person
        List<String> hobbies = new ArrayList<>();
        hobbies.add("Reading");
        hobbies.add("Gardening");

        Person originalPerson = new Person("Alice", hobbies);

        // Deep copy
        Person deepCopyPerson = null;
        try {
            deepCopyPerson = (Person) originalPerson.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }

        // Modify hobbies of the deep copy
        deepCopyPerson.getHobbies().add("Painting");

        // Original person's hobbies remain unaffected
        System.out.println("Original Person Hobbies: " + originalPerson.getHobbies());
        System.out.println("Deep Copy Person Hobbies: " + deepCopyPerson.getHobbies());
    }
}
```

In this example, when we create a deep copy of the Person object and modify the hobbies list of the deep copy, the original person's hobbies list remains unaffected. This is because the deep copy creates a new list with the same elements as the original list, effectively creating a new independent copy.
