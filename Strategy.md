# DesignPattern
Strategy Design Pattern

## 1. PaymentMethod.java
```ruby
package payment;
/**
 * Contract to implelemt amont to be paid 
 */
public interface PaymentMethod {
 
	public abstract void pay(int amount);
}
```
## 2. CreditcardPaymentMethod.java
```ruby
package payment;
 
/**
 *This class supports Creditcard Payment Method
 */
public class CreditcardPaymentMethod implements PaymentMethod {
 
	private String cardHolderName;
	private String card;
	private String cvv;
	private String dateOfExpiry;
	
	public CreditcardPaymentMethod(String cardHolderName, String card, String cvv, String dateOfExpiry) {
		super();
		this.cardHolderName = cardHolderName;
		this.card = card;
		this.cvv = cvv;
		this.dateOfExpiry = dateOfExpiry;
	}
 
	public String getCardHolderName() {
		return cardHolderName;
	}
 
	public String getCard() {
		return card;
	}
 
	public String getCvv() {
		return cvv;
	}
 
	public String getDateOfExpiry() {
		return dateOfExpiry;
	}
 
 
	@Override
	public void pay(int amount) {
		System.out.println(amount +" is paid using debit card :"+card);
	}
}
```
## 3. PaypalPaymentMethod.java
```ruby
package payment;
 
/**
 * This class supports Paypal Payment Method
 */
public class PaypalPaymentMethod implements PaymentMethod {
 
	private String email;
	private String password;
	
	public PaypalPaymentMethod(String email, String password) {
		super();
		this.email = email;
		this.password = password;
	}
 
	public String getEmail() {
		return email;
	}
 
	public String getPassword() {
		return password;
	}
 
	@Override
	public void pay(int amount) {
		System.out.println(amount+" is paid using Paypal");
	}
}
```
## 4. Product.java
```ruby
package payment;
/**
 *This Model class represent product information
 */
public class Product {
 
	private String name;
	private String upcCode;
	private int price;
	
	public Product(String name, String upcCode, int price) {
		super();
		this.name = name;
		this.upcCode = upcCode;
		this.price = price;
	}
	public String getName() {
		return name;
	}
	public String getUpcCode() {
		return upcCode;
	}
	public int getPrice() {
		return price;
	}
}
```
## 5. Shoppingcart.java
```ruby
package payment;
import java.util.ArrayList;
import java.util.List;

/**
 * This class provides methods to:
 *  addProduct -- > add product in shopping cart
 *  removeProduct -- > remove product from shopping cart
 *  calculateTotalPrice --> calculate total price of the products added in cart
 *  payment -- > proceed for payment(This is where Strategy design pattern
 *  is getting used,this method accept various algorithms for payment like
 *  PaypalPayment & CreditcardPayment etc..)
 */
public class Shoppingcart {

	private List<Product> productList;
	
	public Shoppingcart() {
		productList = new ArrayList<Product>();
	}
	
	public void addProduct(Product product) {
		productList.add(product);
	}
	
	public void removeProduct(Product product) {
		productList.remove(product);
	}
	
	private int calculateTotalPrice() {
		return productList.stream().map(p->p.getPrice()).reduce(0, Integer::sum);
	}
	
	public void payment(PaymentMethod paymentMethod) {
		paymentMethod.pay(calculateTotalPrice());
	}
}
```
## 6. ClientTest.java
```ruby
package payment;
 

/**
 * Client program is able to proceed with payment
 * Strategies like CreditcardPayment or PaypalPayment etc..
 */
public class ClientTest {
 
	public static void main(String[] args) {
 
		//Creating first Instance of Shoppingcart
		Shoppingcart shoppingcart = new Shoppingcart();
		
		
		Product product1 = new Product("Soap", "88889895", 100);
		Product product2 = new Product("Shampoo", "89989895", 120);
		Product product3 = new Product("Cookies", "84449895", 100);
		
		//adding three product in shopping cart
		shoppingcart.addProduct(product1);
		shoppingcart.addProduct(product2);
		shoppingcart.addProduct(product3);
		
		//Proceed to payment Strategy as CreditcardPayment
		shoppingcart.payment(new CreditcardPaymentMethod("KK", "987654326372626", "898", "11/23"));
		
		System.out.println("--------------------------------------------------------");
		//Creating Second Instance of Shoppingcart
		shoppingcart = new Shoppingcart();
		
		
		product1 = new Product("Milk", "888009895", 200);
		product2 = new Product("Beer", "89909995", 320);
		
		//adding two products in shopping cart
		shoppingcart.addProduct(product1);
		shoppingcart.addProduct(product2);
		
		//Proceed to payment Strategy as PaypalPayment
		shoppingcart.payment(new PaypalPaymentMethod("kk@gmail.com", "pass"));
	}
}
```
