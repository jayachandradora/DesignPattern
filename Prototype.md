# DesignPattern
`#Prototype comes under a creational design pattern that makes use of cloning objects if object creation complex and time-consuming.`

### Student.java
```
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
```
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
```
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
