

| Functional Interfaces  | # Parameters  |Return Type   |Single Abstract Method   |
|------------------------|---------------|--------------|-------------------------|
|  `Supplier<T>`         |0              |     T        |    `get`                |   
|  `Consumer<T>`         |               |              |                         |   
|  `BiConsumer<T, U>`    |               |              |                         |   


Implementing Supplier
---
A Supplier is used when you want to generate or supply values without taking any input.

```java
@FunctionalInterface 
public class Supplier<T> { public T get();
}
```

```java
Supplier<StringBuilder> s1 = StringBuilder::new;
Supplier<StringBuilder> s2 = () -> new StringBuilder();
System.out.println(s1.get());  
System.out.println(s2.get());
```

Implementing Consumer and BiConsumer
----
You use a Consumer when you want to do something with a parameter but not return anything.

```java
@FunctionalInterface 
public class Consumer<T> { void accept(T t);
}
```
```java
@FunctionalInterface 
public class BiConsumer<T, U> {
    void accept(T t, U u);
}
```
Implementing Predicate and BiPredicate
--------
```java
@FunctionalInterface
public class Predicate<T> { 
    boolean test(T t);
}

@FunctionalInterface 
public class BiPredicate<T, U> {
    boolean test(T t, U u); 
}
```

Implementing Function and BiFunction
------
A Function is responsible for turning one parameter into a value of a potentially different type and returning it. Similarly,a BiFunction is responsible for turning two parameters into a value and returning it
