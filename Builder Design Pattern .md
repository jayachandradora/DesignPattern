# Builder Design Pattern

The **Builder Design Pattern** is one of the **creational patterns** in object-oriented design, primarily used to construct complex objects step by step. It is particularly useful when the construction process involves multiple optional parts, and you want to isolate the construction logic from the final object representation.

### Real-Time Scenario: **Meal Builder**

Let's consider a scenario of an online food delivery service. When a customer orders a meal, they can customize it in several ways — choosing the base, toppings, sauces, sides, and drinks. The meal can have various combinations depending on the customer’s preferences. Using the **Builder Pattern**, we can model this process where the meal is built step by step, without exposing the construction logic to the client.

### Example: Building a Meal with Different Options

In this example, we’ll create a `Meal` object using the Builder pattern. A meal could have a `MainDish`, `SideDish`, `Drink`, and `Sauce`.

### Step 1: Define the `Meal` Class

```java
public class Meal {
    private String mainDish;
    private String sideDish;
    private String drink;
    private String sauce;

    // Constructor can be private to force the use of Builder
    private Meal(Builder builder) {
        this.mainDish = builder.mainDish;
        this.sideDish = builder.sideDish;
        this.drink = builder.drink;
        this.sauce = builder.sauce;
    }

    public String getMainDish() {
        return mainDish;
    }

    public String getSideDish() {
        return sideDish;
    }

    public String getDrink() {
        return drink;
    }

    public String getSauce() {
        return sauce;
    }

    @Override
    public String toString() {
        return "Meal [mainDish=" + mainDish + ", sideDish=" + sideDish +
               ", drink=" + drink + ", sauce=" + sauce + "]";
    }

    // Builder class
    public static class Builder {
        private String mainDish;
        private String sideDish;
        private String drink;
        private String sauce;

        // Optional step to set Main Dish
        public Builder setMainDish(String mainDish) {
            this.mainDish = mainDish;
            return this;
        }

        // Optional step to set Side Dish
        public Builder setSideDish(String sideDish) {
            this.sideDish = sideDish;
            return this;
        }

        // Optional step to set Drink
        public Builder setDrink(String drink) {
            this.drink = drink;
            return this;
        }

        // Optional step to set Sauce
        public Builder setSauce(String sauce) {
            this.sauce = sauce;
            return this;
        }

        // Build method to create Meal instance
        public Meal build() {
            return new Meal(this);
        }
    }
}
```

### Step 2: Use the `Meal` Builder

Now that we have the `Meal` class and its `Builder`, we can use it to create different kinds of meals with varying configurations.

```java
public class Main {
    public static void main(String[] args) {
        // Customer A chooses a basic meal
        Meal mealA = new Meal.Builder()
                            .setMainDish("Grilled Chicken")
                            .setSideDish("Fries")
                            .setDrink("Coke")
                            .build();

        // Customer B customizes their meal with sauce
        Meal mealB = new Meal.Builder()
                            .setMainDish("Veggie Burger")
                            .setSideDish("Salad")
                            .setDrink("Water")
                            .setSauce("Barbecue Sauce")
                            .build();

        // Print the meals
        System.out.println(mealA);
        System.out.println(mealB);
    }
}
```

### Explanation:

1. **Meal Class**: 
   - It represents the complex object that needs to be constructed. It has fields like `mainDish`, `sideDish`, `drink`, and `sauce`.
   - The `Meal` class has a private constructor to ensure that the object can only be constructed through the `Builder` class.
   
2. **Builder Class**:
   - The `Builder` class provides methods for setting each of the parts of the meal (`setMainDish()`, `setSideDish()`, `setDrink()`, `setSauce()`).
   - These setter methods return the `Builder` instance itself (`this`), allowing for method chaining.
   - The `build()` method constructs and returns the final `Meal` object.
   
3. **Flexibility**:
   - By using the Builder pattern, you can create meals with different combinations of properties without exposing the complexity of the construction process.
   - It allows optional parameters (e.g., sauces, sides) to be omitted when not needed.

### Advantages of Using the Builder Pattern:
1. **Readability and Maintainability**:
   - The code is more readable as you can clearly see what values are being set for each attribute.
   - It also allows for easy maintenance, especially when adding new properties to the `Meal` class.
   
2. **Fluent Interface**:
   - The builder provides a fluent interface, making it easy to chain method calls and create a meal object with only the properties needed.
   
3. **Immutability**:
   - The final `Meal` object is immutable after it is built, which reduces the chances of errors during its use.
   
4. **Separation of Concerns**:
   - The construction logic is separated from the rest of the program, making the code more modular and easier to test.
   
5. **Flexible Construction**:
   - You can create different versions of the meal by just changing which methods you call on the builder.

### Conclusion:

The Builder Design Pattern is extremely useful for creating objects with many optional parameters, ensuring that clients can construct complex objects in a clear, flexible, and maintainable way. In real-world scenarios like meal ordering, car manufacturing, or even constructing a document, the Builder pattern makes it easy to configure and create objects without having to deal with cumbersome constructors or setters.


## Separate Builder and Meal class implementation.

Here's how you can separate the `Builder` class and the `Meal` class into separate classes, without using the static inner class implementation:


### Step 1: **Builder Class (Separated)**

```java
public class MealBuilder {
    private String mainDish;
    private String sideDish;
    private String drink;
    private String sauce;

    // Method to set Main Dish
    public MealBuilder setMainDish(String mainDish) {
        this.mainDish = mainDish;
        return this;
    }

    // Method to set Side Dish
    public MealBuilder setSideDish(String sideDish) {
        this.sideDish = sideDish;
        return this;
    }

    // Method to set Drink
    public MealBuilder setDrink(String drink) {
        this.drink = drink;
        return this;
    }

    // Method to set Sauce
    public MealBuilder setSauce(String sauce) {
        this.sauce = sauce;
        return this;
    }

    // Final step: Build the Meal
    public Meal build() {
        return new Meal(mainDish, sideDish, drink, sauce);
    }
}
```

### Step 2: **Meal Class with Constructor**

Update the `Meal` class to have a public constructor, as we're not using the static inner builder class now. The builder will use this constructor to build the meal.

```java
public class Meal {
    private String mainDish;
    private String sideDish;
    private String drink;
    private String sauce;

    // Public constructor used by the Builder
    public Meal(String mainDish, String sideDish, String drink, String sauce) {
        this.mainDish = mainDish;
        this.sideDish = sideDish;
        this.drink = drink;
        this.sauce = sauce;
    }

    public String getMainDish() {
        return mainDish;
    }

    public String getSideDish() {
        return sideDish;
    }

    public String getDrink() {
        return drink;
    }

    public String getSauce() {
        return sauce;
    }

    @Override
    public String toString() {
        return "Meal [mainDish=" + mainDish + ", sideDish=" + sideDish +
               ", drink=" + drink + ", sauce=" + sauce + "]";
    }
}
```

### Step 3: **Main Class to Demonstrate Usage**

Now, in the `Main` class, we can use the `MealBuilder` class to construct `Meal` objects.

```java
public class Main {
    public static void main(String[] args) {
        // Using the MealBuilder to create a Meal
        MealBuilder builderA = new MealBuilder();
        Meal mealA = builderA.setMainDish("Grilled Chicken")
                             .setSideDish("Fries")
                             .setDrink("Coke")
                             .build();

        // Another Meal with a custom sauce
        MealBuilder builderB = new MealBuilder();
        Meal mealB = builderB.setMainDish("Veggie Burger")
                             .setSideDish("Salad")
                             .setDrink("Water")
                             .setSauce("Barbecue Sauce")
                             .build();

        // Print the meals
        System.out.println(mealA);
        System.out.println(mealB);
    }
}
```

### Explanation of Changes:

1. **Meal Class**:
   - This class no longer has the `Builder` class inside it. The `Meal` class now just has a constructor to accept all parameters (i.e., main dish, side dish, drink, and sauce).
   - The constructor is public so that the `MealBuilder` can access it directly.

2. **MealBuilder Class**:
   - This class is now completely separate from the `Meal` class.
   - It has the same functionality as the previous `Builder` but is now a standalone class.
   - You use the `MealBuilder` class to set optional fields and finally call the `build()` method to create a `Meal` object.

3. **Main Class**:
   - You create a `MealBuilder` instance, configure it using method chaining, and call `build()` to get the constructed `Meal` object.

### Benefits of this Approach:

1. **Separation of Concerns**: The `Meal` class and the `MealBuilder` class are clearly separated, making it easier to maintain and test.
2. **Fluent Interface**: The `MealBuilder` still supports a fluent interface, where you can chain method calls for setting different properties.
3. **More Flexibility**: Now you can choose to use the `MealBuilder` in isolation without directly modifying the `Meal` class. This is helpful when you need to extend functionality in the builder independently of the meal object.

This approach gives you a clean separation between the object you're building (`Meal`) and the builder itself (`MealBuilder`), which enhances the maintainability and scalability of the code.

