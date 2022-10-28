# Design Pattern
Proxy is a structural design pattern that provides an object that acts as a substitute for a real service object used by a client. A proxy receives client requests, does some work (access control, caching, etc.) and then passes the request to a service object.

```ruby
package proxy;
interface DatabaseExecuter {
  public void executeDatabase(String query) throws Exception;
}
```

```ruby
class DatabaseExecuterImpl implements DatabaseExecuter {

  @Override
  public void executeDatabase(String query) throws Exception {
    System.out.println("Going to execute Query: " + query);
  }
  
}
```

```ruby
class DatabaseExecuterProxy implements DatabaseExecuter {
  boolean ifAdmin;
  DatabaseExecuterImpl dbExecuter;
  
  public DatabaseExecuterProxy(String name, String passwd) {
    if(name == "Admin" && passwd == "Admin@123") {
      ifAdmin = true;
    }
    dbExecuter = new DatabaseExecuterImpl();
  }

  @Override
  public void executeDatabase(String query) throws Exception {
    if(ifAdmin) {
      dbExecuter.executeDatabase(query);
    } else {
      if(query.equals("DELETE")) {
        throw new Exception("DELETE not allowed for non-admin user");
      } else {
        dbExecuter.executeDatabase(query);
      }
    }
  }
}
```

```ruby
public class ProxyPatternExample {

  public static void main(String[] args) throws Exception {
  
    DatabaseExecuter nonAdminExecuter = new DatabaseExecuterProxy("NonAdmin", "Admin@123");
    nonAdminExecuter.executeDatabase("DELEE");
    
    DatabaseExecuter nonAdminExecuterDELETE = new DatabaseExecuterProxy("NonAdmin", "Admin@123");
    nonAdminExecuterDELETE.executeDatabase("DELETE");
    
    DatabaseExecuter adminExecuter = new DatabaseExecuterProxy("Admin", "Admin@123");
    adminExecuter.executeDatabase("DELETE");
  }
}
```
