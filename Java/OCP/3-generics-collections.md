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

#### Unbounded Wildcards
An unbounded wildcard represents any data type. You use ? when you want to specify that any type is OK with you

```java
public static void printList(List<?> list) { 
    for (Object x: list) System.out.println(x);
}
public static void main(String[] args) {
    List<String> keywords = new ArrayList<>(); keywords.add("java");
    printList(keywords);
}
```
- `printList()` takes any type of list as a parameter. 

#### Upper-Bounded Wildcards

```java
ArrayList<Number> list = new ArrayList<Integer>(); // DOES NOT COMPILE
```
Instead, we need to use a wildcard:
```java
List<? extends Number> list = new ArrayList<Integer>();
```

```java
static class Sparrow extends Bird { } 
static class Bird { }

List<? extends Bird> birds = new ArrayList<Bird>(); 
birds.add(new Sparrow()); // DOES NOT COMPILE 
birds.add(new Bird()); // DOES NOT COMPILE
```

We have an interface and two classes that implement it:

```java
interface Flyer { void fly(); }
class HangGlider implements Flyer { 
    public void fly() {} 
} 
class Goose implements Flyer { 
    public void fly() {} 
}
```
- _Upper bounds_ are like anonymous classes in that they use extends regardless of whether we are working with a class or an interface.

#### Lower-Bounded Wildcards

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-07%20at%202.29.26%20PM.png)

### Putting It All Together

```java
class A {}
class B extends A { }
class C extends B { }
```

```java
List<?> list1 = new ArrayList<A>();
List<? extends A> list2 = new ArrayList<A>();
List<? super A> list3 = new ArrayList<A>();
List<? extends B> list4 = new ArrayList<A>(); // DOES NOT COMPILE
List<? super B> list5 = new ArrayList<A>();
List<?> list6 = new ArrayList<? extends A>(); // DOES NOT COMPILE
```
# Using Lists, Sets, Maps, and Queues
There are four main interfaces in the Java Collections Framework:
`List`: A _list_ is an ordered collection of elements that allows duplicate entries. 
Elements in a list can be accessed by an `int` index.
`Set`: A _set_ is a collection that does not allow duplicate entries.
`Queue`: A _queue_ is a collection that orders its elements in a specific order for processing.
`Map`: A _map_ is a collection that maps keys to values, with no duplicate keys allowed. The elements in a map are key/value pairs.

### Common Collections Methods

#### `add()`

```java
boolean add(E element)
```

```java
List<String> list = new ArrayList<>();
System.out.println(list.add("Sparrow")); // true
```
```java
Set<String> set = new HashSet<>();
System.out.println(set.add("Sparrow")); // true
System.out.println(set.add("Sparrow")); // false
```
- A `Set` does not allow duplicates

#### `remove()`
The `remove()` method removes a single matching value in the Collection and returns whether it was successful.

```java
boolean remove(Object object)
```
- Since calling remove() with an int uses the index, an index that doesn’t exist will throw an exception.  `birds.remove(100)`; throws an `IndexOutOfBoundsException`.

#### `isEmpty() and size()`

```java
boolean isEmpty() 
int size()
```

#### `clear()`
The clear() method provides an easy way to discard all elements of the Collection. The method signature is

```java
void clear()
```

#### `contains()`
The `contains()` method checks if a certain value is in the Collection. The method signature is

```java
boolean contains(Object object)
```
- This method calls `equals()` on each element of the `ArrayList` to see if there are any matches.

### Using the List Interface
The main thing that all `List` implementations have in common is that they are ordered and allow duplicates.

#### Comparing List Implementations

`ArrayList`

An `ArrayList` is like a resizable array. When elements are added, the `ArrayList` auto- matically grows. When you aren’t sure which collection to use, use an `ArrayList`

* The main benefit of an `ArrayList` is that you can look up any element in constant time.
* Adding or removing an element is slower than accessing an element

`LinkedList`

* A `LinkedList` is special because it implements both `List` and `Queue`. It has all of the methods of a `List`. It also has additional methods to facilitate adding or removing from the beginning and/or end of the list.

* The main benefits of a LinkedList are that you can access, add, and remove from the beginning and end of the list in constant time.

`Vector`

* `Vector` does the same thing as `ArrayList` except more slowly. The benefit to that decrease in speed is that it is thread-safe.

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-07%20at%205.03.32%20PM.png)

### Using the Set Interface
The main thing that all Set implementa- tions have in common is that they do not allow duplicates.

#### Comparing Set Implementations

`HashSet`

A `HashSet` stores its elements in a hash table. This means that it uses the `hashCode()` method of the objects to retrieve them more efficiently.

The main benefit is that adding elements and checking if an element is in the set both have constant time.

`TreeSet`

A `TreeSet` stores its elements in a sorted tree structure. 
The tradeoff is that adding and checking if an element is present are both O(log n)

#### Working with Set Methods

```java
Set<Integer> set = new HashSet<>();
boolean b1 = set.add(66);
```
The `hashCode()` method is used to know which bucket to look in so that Java doesn’t have to look through the whole set to find out if an `object` is there. The best case is that hash codes are unique, and Java has to call `equals()` on only one object. 

The `NavigableSet interface:

`TreeSet` implements the `NavigableSet` interface.

Just remember that lower and higher elements do not include the target element.

### Using the Queue Interface
we use a queue when elements are added and removed in a specific order. Queues are typically used for sorting elements prior to processing them.

#### Comparing Queue Implementations
A double-ended queue is different from a regular queue in that you can insert and remove elements from both the front and back of the queue.

The main benefit of a `LinkedList` is that it implements both the `List` and `Queue` interfaces.

An `ArrayDeque` is a “pure” double-ended queue.The main benefit of an `ArrayDeque` is that it is more efficient than a `LinkedList`.


#### Working with _`Queue`_ Methods

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%205.40.17%20PM.png)

When talking about LIFO (stack), people say `push/poll/peek`. When talking about FIFO (single-ended queue), people say `offer/poll/peek`.
```java
12: Queue<Integer> queue = new ArrayDeque<>();
13: System.out.println(queue.offer(10)); // true
14: System.out.println(queue.offer(4)); // true
15: System.out.println(queue.peek()); // 10
16: System.out.println(queue.poll()); // 10
17: System.out.println(queue.poll()); // 4
18: System.out.println(queue.peek()); // null
```
### Map

#### Comparing Map Implementations

A `HashMap` stores the keys in a hash table. This means that it uses the `hashCode()` method of the keys to retrieve their values more efficiently.

A `TreeMap` stores the keys in a sorted tree structure.

A `Hashtable` is like `Vector` in that it is really old and thread-safe and that you won’t be expected to use it.

#### Working with Map Methods

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%206.58.29%20PM.png)

### Comparing Collection Types
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%207.07.43%20PM.png)
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%207.07.49%20PM.png)

The data structures that involve sorting do not allow `nulls`.
This means that `TreeSet` cannot contain `null` elements. It also means that `TreeMap` cannot contain `null` keys.

`TreeMap` — no `null` keys
`Hashtable` — no `null` keys or values
`TreeSet` — no `null` elements
`ArrayDeque` — no `null` elements

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%207.13.24%20PM.png)

# `Comparator` vs. `Comparable`
**Remember that numbers sort before letters and uppercase letters sort before lowercase letters.**

### Comparable

```java
public interface Comparable<T> { 
    public int compareTo(T o);
}
```

There are three rules to know:
* The number zero is returned when the current object is equal to the argument to `compareTo()`.

* A number less than zero is returned when the current object is smaller than the argument to `compareTo()`.

* A number greater than zero is returned when the current object is larger than the argument to `compareTo()`.

**Remember that `id – a.id` sorts in ascending order and `a.id – id` sorts in descending order.**

### Comparator

```java
Comparator<Duck> byWeight = new Comparator<Duck>() {
    public int compare(Duck d1, Duck d2) {
        return d1.getWeight()—d2.getWeight();
    }
};
```

```java
Comparator<Duck> byWeight = (d1, d2) -> d1.getWeight()—d2.getWeight();
```
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-10%20at%208.01.37%20PM.png)

# Searching and Sorting

# Additions in Java 8
### Using Method References
### Removing Conditionally
### Updating All Elements
### Looping through a Collection
### Using New Java 8 Map APIs

