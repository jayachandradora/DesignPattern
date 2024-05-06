# Factory Design Pattern Example in Java

The Factory Design Pattern is a creational design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. This pattern is useful when you want to decouple object creation from the client code.

## Implementation

```java
// Interface for the product
interface Vehicle {
    void manufacture();
}

// Concrete implementation of the product
class Car implements Vehicle {
    @Override
    public void manufacture() {
        System.out.println("Car is manufactured.");
    }
}

// Another concrete implementation of the product
class Bike implements Vehicle {
    @Override
    public void manufacture() {
        System.out.println("Bike is manufactured.");
    }
}

// Factory class to create objects based on input
class VehicleFactory {
    public Vehicle createVehicle(String type) {
        if (type.equalsIgnoreCase("Car")) {
            return new Car();
        } else if (type.equalsIgnoreCase("Bike")) {
            return new Bike();
        }
        return null;
    }
}

// Client code using the factory to create objects
public class Main {
    public static void main(String[] args) {
        VehicleFactory factory = new VehicleFactory();

        // Creating a car
        Vehicle car = factory.createVehicle("Car");
        if (car != null) {
            car.manufacture();
        }

        // Creating a bike
        Vehicle bike = factory.createVehicle("Bike");
        if (bike != null) {
            bike.manufacture();
        }
    }
}
```

## Explanation
*  Vehicle is an interface representing the product.
*  Car and Bike are concrete implementations of the Vehicle interface.
*  VehicleFactory is the factory class responsible for creating objects based on input.
*  The Main class demonstrates how to use the factory to create objects (Car and Bike) without worrying about the specific implementation details.
  
This pattern allows for easy addition of new types of vehicles in the future without modifying the client code.
