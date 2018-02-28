Lambda expressions
---

* lambda expressions, which let you represent a behavior or pass code in a concise way.

**Lambdas in a nutshell**

* Anonymous
* Function
* Passed around
* Concise

it originates from a system developed in academia called _lambda calculus_, which is used to describe computations.

Lambdas technically don’t let you do anything that you couldn’t do prior to Java 8.

**The lambda we just showed you has three parts**
* A list of parameters
* An arrow
* The body of the lambda

The basic syntax of a **lambda** is either
```
(parameters) -> expression
```
or (note the curly braces for statements)
```
(parameters) -> { statements; }
```

**Where and how to use lambdas**
* You can use a lambda expression in the con- text of a functional interface.

Functional interface
---
a _functional interface_ is an interface that specifies exactly one abstract method

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}

public interface Runnable{
    void run();
}
```

Function descriptor
---
* The signature of the abstract method of the functional interface essentially describes the signature of the lambda expression

* The notation ```() -> void``` represents a function with an empty list of parameters returning ```void```.

* ```(Apple, Apple) -> int``` denotes a function taking two Apples as parameters and returning an ```int```.

* a lambda expression can be assigned to a variable or passed to a method expecting a functional interface as argument, provided the lambda expression has the same signature as the abstract method of the functional interface.

* functional interfaces are annotated with ```@FunctionalInterface```


Four-step process to apply the execute around pattern
---
```java
public static String processFile() throws IOException { 
    try (BufferedReader br =
                    new BufferedReader(new FileReader("data.txt"))){
        return br.readLine();
    }
    

```
