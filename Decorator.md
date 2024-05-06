# Decorator Design Pattern

## Definition

The decorator pattern is a structural design pattern that allows behaviour and functionality to be dynamically added to an object without affecting 
the behaviour of other objects in the same class. Because it offers a wrapper to an existing class, the decorator design pattern is also known as Wrapper. 
To implement the wrapper, this approach uses abstract classes or interfaces with composition. 
Decorator classes are used to extend the functionality of the base class.

## Use Case 

- When one has an object that requires functionality extension. For extending functionality, decorators are a versatile alternative to subclassing.
- When one has to recursively rewrap or change the functionalities of an object according to the requirements, dynamically without affecting other objects of the class.

A real-life example of a decorator design pattern would be a pizza, pizza base here would be the original class, and the variety of different toppings would act as 
the added functionalities. The customer can add toppings (functionalities) as per their choice and the pizza base (original class) will remain intact.

![image](https://user-images.githubusercontent.com/115500959/198528895-99b5cb3f-f96f-43b4-a083-9a699be5713b.png)

## Implementation
1. Create an interface (or an abstract class), Color, and define a method fill() to be used by all the concrete and decorator classes.
2. Create a concrete class like the class 'Black' used in the above diagram. This class implements the component class and overrides the fill() method.
3. Now, create an abstract decorator class, ColorDecorator which implements the Color interface.
4. Define the object, colored of the base interface, Color. This is the object which requires an extension and will be wrapped by the decorator class.
5. Initialise the constructor and pass the object, colored, as a parameter in the constructor. This object is inherited from the base class.
6. Create a concrete decorator class, PatternDecorator which inherits the base decorator class, ColorDecorator. Define a new method addPattern(), 
to add functionalities to the wrapped object.

## Example 1

```java
// Component interface
interface Pizza {
    String getDescription();
    double getCost();
}

// Concrete component
class PlainPizza implements Pizza {
    @Override
    public String getDescription() {
        return "Plain Pizza";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}

// Decorator abstract class
abstract class ToppingDecorator implements Pizza {
    protected Pizza pizza;

    public ToppingDecorator(Pizza pizza) {
        this.pizza = pizza;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription();
    }

    @Override
    public double getCost() {
        return pizza.getCost();
    }
}

// Concrete decorator
class Cheese extends ToppingDecorator {
    public Cheese(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", Cheese";
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 1.0;
    }
}

// Concrete decorator
class Pepperoni extends ToppingDecorator {
    public Pepperoni(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", Pepperoni";
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 1.5;
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        // Create a plain pizza
        Pizza pizza = new PlainPizza();

        // Add toppings dynamically
        pizza = new Cheese(pizza);
        pizza = new Pepperoni(pizza);

        // Output description and cost
        System.out.println("Description: " + pizza.getDescription());
        System.out.println("Cost: $" + pizza.getCost());
    }
}

```

## Example 2

```ruby
interface Color{
    void fill();
}

//concrete component of the base interface
class Black implements Color{
    @Override
    public void fill(){
        System.out.println("Black color");
    }
}

//abstract decorator class
abstract class ColorDecorator implements Color{

    //base class object
    protected Color colored;
    
    //constructor with base class object as the parameter
    public ColorDecorator(Color colored){
        this.colored = colored;
    }
    
    public void fill(){
        colored.fill();
    }
}

//concrete decorator class extending base decorator class
class PatternDecorator extends ColorDecorator{
    public PatternDecorator(Color colored){
        super(colored);
    }
    
    public void fill(){
        colored.fill();
        addPattern(colored);
    }
    
    private void addPattern(Color colored){
        System.out.println("Pattern");
    }
}

class Demo{
    public static void main(String[] args){
    
    Color black = new Black();
    Color pattern = new PatternDecorator(new Black());
     
    System.out.println("\nStyle: Solid");
    black.fill();
    
    System.out.println("\nStyle: Pattern");
    pattern.fill();
    }
}
```

## Pros:
- A decorator design pattern provides a high degree of flexibility as an alternative to subclassing for functionality extension.
- Instead of editing the existing code, decorators allow behaviour modification at runtime.
- Problems with permutation are resolved. Wrapping an object in numerous decorators allows you to mix multiple behaviours.
- The decorator design pattern adheres to the concept that classes should be extensible but not modifiable.
- Decorator pattern follows the Single responsibility principle which states that a monolithic class with multiple tasks can be broken down into various classes, each with a particular responsibility or task.

## Comparison with Other Similar Design Patterns
- The purpose of a decorator pattern is to enhance and decorate a particular object, whereas the purpose of a composite pattern is to combine together components as a whole.
- Decorator design pattern allows you to add functionalities to an object without subclassing, the emphasis in Composite design pattern is on representation rather than decoration.
- The decorator pattern enables you to modify or extend existing functionality at runtime. The strategy pattern enables you to alter the implementation of anything that is being used in runtime.
- In contrast to the Decorator pattern, which alters and decorate the functionality of pre-existing instances, the Proxy pattern offers the same interface as the object it holds the pointer to and does not affect the data in any way.
- Even though the Proxy and Decorator patterns offer similar structures, their goals are different. While the primary goal of a Proxy is to provide easy usage or manage access, a Decorator adds in extra responsibilities.
- Though the structure of the Adapter and Decorator patterns are similar, the aim of each pattern differs. The adapter pattern is used to connect two interfaces, whereas the Decorator pattern is used to introduce additional functionality without changing the current code.
