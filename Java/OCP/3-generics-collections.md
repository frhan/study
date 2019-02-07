# Generics and Collections

# Working with Generics
_Generics_  allow us to write and use parameterized types.

```java
List<String> names = new ArrayList<String>();
names.add(new StringBuilder("Webby")); // DOES NOT COMPILE
```

### Generic Classes

The syntax for introducing a generic is to declare a _formal type parameter_ in angle brackets.

```java
public class Crate<T> { 
    private T contents; 
    public T emptyCrate() {
        return contents; 
        }
    public void packCrate(T contents) {
         this.contents = contents;
    } 
}
```

Generic classes aren’t limited to having a single type parameter. This class shows two generic parameters:

```java
public class SizeLimitedCrate<T, U> {
    private T contents;
    private U sizeLimit;

    public SizeLimitedCrate(T contents, U sizeLimit) {
        this.contents = contents;
        this.sizeLimit = sizeLimit; 
    }
}
```

### Generic Interfaces

```java
public interface Shippable<T> { 
    void ship(T t);
}
```
- Create a static variable as a generic type parameter. This is not allowed because the type is linked to the instance of the class.

### Generic Methods
the method uses a generic parameter:

```java
public static <T> Crate<T> ship(T t) { 
    System.out.println("Preparing " + t); 
    return new Crate<T>();
}
```

### Interacting with Legacy Code
* using generics gives us compile time safety

### Bounds

A _bounded parameter_ type is a generic type that specifies a bound for the generic.

A _wildcard generic_ type is an unknown generic type represented with a question mark (?).


### Putting It All Together

# Using Lists, Sets, Maps, and Queues
### Common Collections Methods
### Using the List Interface
### Using the Set Interface
### Using the Queue Interface
### Map
### Comparing Collection Types

# `Comparator` vs. `Comparable`
### Comparable
### Comparator

# Searching and Sorting

# Additions in Java 8
### Using Method References
### Removing Conditionally
### Updating All Elements
### Looping through a Collection
### Using New Java 8 Map APIs