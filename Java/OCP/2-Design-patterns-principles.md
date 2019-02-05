Design Patterns and Principles
---

Designing an Interface
---

Introducing Functional Programming
---

Implementing Polymorphism
---

Understanding Design Principles
---

Working with Design Patterns
---

### Applying the Singleton Pattern

The _singleton pattern_ is a creational pattern focused on creating only one instance of an object in memory within an application, sharable by all classes and threads within the application

If all of the constructors are declared private in the singleton class, then it is impossible to create a subclass with a valid constructor; therefore, the singleton class is effectively `final`.

 the static initialization block allows additional steps to be taken to set up the singleton after it has been created.

#### Applying Lazy Instantiation to Singletons
 ```java
 // Lazy instantiation
public class VisitorTicketTracker {
    private static VisitorTicketTracker instance;
    private VisitorTicketTracker() {

    }

    public static VisitorTicketTracker getInstance() {
        if(instance == null) {
            instance = new VisitorTicketTracker(); // NOT THREAD-SAFE!
        }
        return instance; 
    }// Data access methods ...
}
 ```
 * Creating a reusable object the first time it is requested is a software design pattern known as _lazy instantiation_
 * The downside of lazy instantiation is that users may see a noticeable delay the first time a particular type of resource is needed