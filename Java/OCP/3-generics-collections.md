
Working with Generics
---

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

