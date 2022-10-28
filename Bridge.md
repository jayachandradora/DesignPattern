# Bridge Design Pattern

## Definition

Bridge Design Pattern is a part of a structural design pattern, as the name suggests it act as an intermediary between two components. the Bridge Design Pattern decouples an abstraction from its implementation 
so that the two can vary independently which means, this pattern is used to separate abstraction from its implementation so that can be modified independently.
 
More specifically this pattern involves an interface that acts as a bridge between the abstraction classes and implementer classes. With the bridge pattern, 
both types of classes can be modified without affecting each other.

### Usecase Bridge Design Pattern
- We need to avoid a permanent binding between an abstraction and its implementation i.e to avoid tight coupling between interface/abstract class and other classes.
- Both abstraction and their implementation should be extensible by subclassing.
- When changing in their implementation should not have an impact on the customer/client i.e their code should not have to be recompiled.
- When we need to share an implementation among multiple objects, and
- When we need to hide the implementation of an abstraction completely from clients.

### Implementation
Let’s assume that our requirement is to process the payment of the customer who had either opted for an option of Credit/Debit card or NetBanking or 
Pay on the Delivery during its purchase process.
 
Typically, we are provided with NetBanking, Credit/Debit card, and Pay on the Delivery process as payment choices to do the payments.

### Example

#### PaymentSystem.java
Construct an implementer i.e “paymentSystem” which has a method to process the payments:

```ruby
package BridgeDesign;  
public interface paymentSystem {  
    void ProcessPayment();  
    void ProcessPayment(String string);  
}  
```
#### SBIpaymentSystem.java
```ruby
package BridgeDesign;  
public class SBIpaymentSystem implements paymentSystem {  
    @Override  
    public void ProcessPayment(String string) {  
        System.out.println("Using SBI payment gateway for " + string);  
    }  
    @Override  
    public void ProcessPayment() {  
        System.out.println("User will Pay on Delivery");  
    }  
}   
```
### PNBpaymentSystem.java
```ruby
package BridgeDesign;  
public class PNBpaymentSystem implements paymentSystem {  
    @Override  
    public void ProcessPayment(String string) {  
        System.out.println("Using PNB payment gateway for " + string);  
    }  
    @Override  
    public void ProcessPayment() {  
        System.out.println("User will Pay on Delivery");  
    }  
}   
```
#### abstract.java
```ruby
package BridgeDesign;  
public abstract class Payment {  
    paymentSystem payment; //instance  
    public abstract void makePayment(); //method responsible to makePayment  
}  
```

#### CreditCardPayment.java
```ruby
package BridgeDesign;  
public class CreditCardPayment extends Payment {  
    @Override  
    public void makePayment() {  
        payment.ProcessPayment("Credit Card Payment");  
    }  
}  
```

#### DebitCardPayment.java
```ruby
package BridgeDesign;  
public class DebitCardPayment extends Payment {  
    @Override  
    public void makePayment() {  
        payment.ProcessPayment("Debit Card Payment");  
    }  
}   
```

#### NetBanking.java
```ruby
package BridgeDesign;  
public class NetBanking extends Payment {  
    @Override  
    public void makePayment() {  
        payment.ProcessPayment("Net Banking");  
    }  
}   
```

#### PaymentProcessor.java
```ruby
package BridgeDesign;  
public class PaymentProcessor {  
    public static void main(String[] args) {  
        //Using PNB payment gateway for Credit Card Payment  
        Payment order1 = new CreditCardPayment();  
        order1.payment = new PNBpaymentSystem();  
        order1.makePayment();  
        //Using PNB payment gateway for Debit Card Payment  
        Payment order12 = new DebitCardPayment();  
        order12.payment = new PNBpaymentSystem();  
        order12.makePayment();  
        //Using PNB payment gateway for Net Banking  
        Payment order11 = new NetBanking();  
        order11.payment = new PNBpaymentSystem();  
        order11.makePayment();  
        //User will Pay on Delivery  
        Payment order111 = new PayOnDelivery();  
        order111.payment = new PNBpaymentSystem(); //Pay on Delivery is irrespective of banks  
        order111.makePayment();  
        //Using same payment gateway with a different bank  
        //Using SBI payment gateway for Credit Card Payment  
        Payment order2 = new CreditCardPayment();  
        order2.payment = new SBIpaymentSystem();  
        order2.makePayment();  
        //Using SBI payment gateway for Debit Card Payment  
        Payment order21 = new DebitCardPayment();  
        order21.payment = new SBIpaymentSystem();  
        order21.makePayment();  
        //Using SBI payment gateway for Net Banking  
        Payment order22 = new NetBanking();  
        order22.payment = new SBIpaymentSystem();  
        order22.makePayment();  
        //User will Pay on Delivery  
        Payment order222 = new PayOnDelivery();  
        order222.payment = new SBIpaymentSystem(); //Pay on Delivery is irrespective of banks  
        order222.makePayment();  
    }  
}    
```
