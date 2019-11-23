# Advanced Class Design

## Reviewing OCA Concepts

### Using instanceof
### Understanding Virtual Method Invocation
### Annotating Overridden Methods


# Coding equals, hashCode, and toString

### `toString`

```java
System.out.println(new ArrayList()); // []
System.out.println(new String[0]); // [Ljava.lang.String;@65cc892e
```
`ArrayList` provided an implementation of `toString()` that listed the contents of the `ArrayList`.

### `equals`

Checking if two objects are equivalent uses the `equals()` method, or at least it does if the developer implementing the method overrides equals().

String does have an `equals()` method. It checks that the values are the same.

The Contract for `equals()` methods: 

1. It is _reflexive_: For any non‐null reference value x, `x.equals(x)` should return `true`.

2. It is _symmetric_: For any non‐null reference values x and y, `x.equals(y)` should return `true` if and only if `y.equals(x)` returns true.
3. It is _transitive_: For any non‐null reference values x, y, and z, if `x.equals(y` returns `true` and `y.equals(z)` returns true, then `x.equals(z)` should return `true`.

4. It is _consistent_: For any non‐null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.

5. For any non‐null reference value x, x.equals(null) should return false.

### `hashCode`
A _hash code_ is a number that puts instances of a class into a finite number of categories.

three points:

1. Within the same program, the result of `hashCode()` must not change. 
2. If `equals()` returns true when called with two objects, calling `hashCode()` on each of those objects must return the same result.This means hashCode() can use a subset of the variables that equals() uses.
3. If equals() returns false when called with two objects, calling hashCode() on each of those objects does not have to return a different result.

# Working with Enums
an _enum_ is a class that represents an enumeration. It is much better than a bunch of constants because it provides type‐safe checking. 

```java
public enum Season {
    WINTER, SPRING, SUMMER, FALL
}
```

```java
for(Season season: Season.values()) { 
    System.out.println(season.name() + " " + season.ordinal());
}
```
*  You can’t compare an `int` and `enum` value directly anyway. 

### Using Enums in Switch Statements

* you can’t extend an `enum`.

```java
public enum ExtendedSeason extends Season { } // DOES NOT COMPILE
```
* The following code does not compile:

```java
Season season = Season.FALL;
        switch (season){
            case Season.FALL:
}
```
### Adding Constructors, Fields, and Methods
```java
public enum Season {
2:  WINTER("Low"), SPRING("Medium"), SUMMER("High"), FALL("Medium");
3:  private String expectedVisitors;
4:  private Season(String expectedVisitors) {
5:      this.expectedVisitors = expectedVisitors;
6:  }
7:  public void printExpectedVisitors() {
8:      System.out.println(expectedVisitors);
9: } }

```
* The constructor is `private` because it can only be called from within the `enum`. The code will not compile with a `public` constructor.

* The first time that we ask for any of the enum values, Java constructs all of the enum values. After that, Java just returns the already‐constructed enum values.

```java
public enum OnlyOne { 
    ONCE(true);
    private OnlyOne(boolean b) { System.out.println("constructing");
}

public static void main(String[] args) {
    OnlyOne firstCall = OnlyOne.ONCE; // prints constructing
    OnlyOne secondCall = OnlyOne.ONCE; // doesn't print anything 
    
}
}
```


# Creating Nested Classes

A _nested class_ is a class that is defined within another class. 
A _nested class_ that is not static is called an _inner class_. 

There are four of types of nested classes:

* A member inner class is a class defined at the same level as instance variables. It is not static. Often, this is just referred to as an inner class without explicitly saying the type.
* A local inner class is defined within a method.
* An anonymous inner class is a special case of a local inner class that does not have a name.
* A static nested class is a `static` class that is defined at the same level as static variables.

### Member Inner Classes
A _member inner class_ is defined at the member level of a class (the same level as the methods, instance variables, and constructors).

* Can be declared public, private, or protected or use default access Can extend any class and implement interfaces
* Can be abstract or final
* Cannot declare static fields or methods
* Can access members of the outer class including private members

```java
public class Outer {
    private String greeting = "Hi";
    protected class Inner {
        public int repeat = 3;
        public void go() {
            for (int i = 0; i < repeat; i++)
            System.out.println(greeting); 
        }
    }
    
    public void callInner() { 
        Inner inner = new Inner(); 
        inner.go();
    }

    public static void main(String[] args) {
        Outer outer = new Outer();
        //outer.callInner(); 
        Inner inner = outer.new Inner(); // create the inner class
        nner.go();
    } 
}
```
_Inner classes_ can have the same variable names as outer classes.
```
System.out.println(x); 
System.out.println(this.x);
System.out.println(B.this.x); 
System.out.println(A.this.x);
```
#### Private interfaces

```java
public class CaseOfThePrivateInterface { 
    private interface Secret {
        public void shh(); 
    }
    class DontTell implements Secret { 
        public void shh() { }
    }
}
```
* The interface itself does not have to be public, though.
* Just like any inner class, an inner interface can be private

### Local Inner Classes
A local inner class is a nested class defined within a method. 

 Local inner classes have the following properties:
 
 * They do not have an access specifier.
 * They cannot be declared _static_ and cannot declare _static_ fields or methods.
 * They have access to all fields and methods of the enclosing class.
 * They do not have access to local variables of a method unless those variables are `final` or effectively `final`. 

 - If the code could still compile with the keyword final inserted before the local variable, the variable is effectively final.

```java
 public class Outer {
     private int length = 5; 
     public void calculate() {
         final int width = 20; 
         class Inner {
             public void multiply() { 
                 System.out.println(length * width);
            } 
         }
         Inner inner = new Inner();
         inner.multiply(); 
    }
    
    public static void main(String[] args) { 
        Outer outer = new Outer(); 
        outer.calculate();
    } 
}
```
### Anonymous Inner Classes
An _anonymous inner class_ is a local inner class that does not have a name.
It is declared and instantiated all in one statement using the `new` keyword.

The _anonymous inner class_ is the same whether you implement an `interface` or `extend` a class!

```java

public class AnonInner { 
    interface SaleTodayOnly {
        int dollarsOff(); 
    }
    
    public int admission(int basePrice) { 
        SaleTodayOnly sale = new SaleTodayOnly() {
            public int dollarsOff() { return 3; } 
        };
        return basePrice - sale.dollarsOff();
    }
}
```

### Static Nested Classes

A _static nested class_ is a static class defined at the member level.

It can be instantiated without an object of the enclosing class, so it can’t access the instance variables without an explicit object of the enclosing class.

In other words, it is like a regular class except for the following:
* The nesting creates a namespace because the enclosing class name must be used to refer to it.

* It can be made private or use one of the other access modifiers to encapsulate it. 

* The enclosing class can refer to the fields and methods of the static nested class.

```java
public class Enclosing { 
    static class Nested {
        private int price = 6; 
    }
    public static void main(String[] args) { 
    Nested nested = new Nested(); 
    System.out.println(nested.price);
    } 
}
```

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-07%20at%2011.22.16%20AM.png)