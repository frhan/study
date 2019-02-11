Functional Programming
---

# Built-In Functional Interfaces

| Functional Interfaces  | # Parameters    | Return Type    |Single Abstract Method   |
|------------------------|-----------------|----------------|-------------------------|
|  `Supplier<T>`         | `0`             |     `T`        |    `get`                |   
|  `Consumer<T>`         | `1(T)`          |   `void`       |    `accept`             |   
|  `BiConsumer<T, U>`    | `2(T,U)`        |   `void`       |    `accept`             |   
|  `Predicate<T>`        | `1(T)`          |  `boolean`     |    `test`               |   
|  `BiPredicate<T, U>`   | `2(T,U)`        |  `boolean`     |    `test`               |   
|  `Function<T, R>`      | `1(T)`          |    `R`         |    `apply`              |   
|  `BiFunction<T, U, R>` | `2(T,U)`        |    `R`         |    `apply`              |   
|  `UnaryOperator<T>`    | `1(T)`          |    `T`         |    `apply`              |   
|  `BinaryOperator<T>`   | `2(T,T)`        |    `T`         |    `apply`              |   

### Implementing Supplier

A Supplier is used when you want to generate or supply values without taking any input.

```java
@FunctionalInterface 
public class Supplier<T> { 
    public T get();
}
```

```java
Supplier<StringBuilder> s1 = StringBuilder::new;
Supplier<StringBuilder> s2 = () -> new StringBuilder();
System.out.println(s1.get());  
System.out.println(s2.get());
```

### Implementing Consumer and BiConsumer

You use a Consumer when you want to do something with a parameter but not return anything.

```java
@FunctionalInterface 
public class Consumer<T> { 
    void accept(T t);
}
```
```java
@FunctionalInterface 
public class BiConsumer<T, U> {
    void accept(T t, U u);
}
```

```java 
Consumer<String> c1 = System.out::println; 
Consumer<String> c2 = x -> System.out.println(x);
```

### Implementing Predicate and BiPredicate

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

#### Example:

```java
Predicate<String> p1 = String::isEmpty;
Predicate<String> p2 = x -> x.isEmpty();
```
#### BiPredicate:

```java
BiPredicate<String, String> b2 = (string, prefix) -> string.startsWith(prefix);
```

- the first parameter in the lambda is used as the instance on which to call the method

### Implementing Function and BiFunction
A `Function` is responsible for turning one parameter into a value of a potentially different type and returning it. 

Similarly,a `BiFunction` is responsible for turning two parameters into a value and returning it.

```java
@FunctionalInterface 
public class Function<T, R> { 
    R apply(T t);
}
```

```java
@FunctionalInterface 
public class BiFunction<T, U, R> { 
    R apply(T t, U u);
}
```

For example, this function converts a String to the length of the String:

```java
Function<String, Integer> f1 = String::length; 
Function<String, Integer> f2 = x -> x.length();
```

### Implementing UnaryOperator and BinaryOperator
`UnaryOperator` and `BinaryOperator` are a special case of a function. They require all type parameters to be the same type.

```java
@FunctionalInterface 
public class UnaryOperator<T> extends Function<T, T> { }

@FunctionalInterface 
public class BinaryOperator<T> extends BiFunction<T, T, T> { }
```
This means that method signatures look like this:

```java
T apply(T t);
T apply(T t1, T t2);
```

```java
UnaryOperator<String> u1 = String::toUpperCase; 
UnaryOperator<String> u2 = x -> x.toUpperCase();
System.out.println(u1.apply("chirp"));
System.out.println(u2.apply("chirp"));
```

### Checking Functional Interfaces
- a Predicate returns a `boolean` primitive and not a `Boolean` object

# Returning an Optional

```java
Optional o = Optional.ofNullable(value);
```
- returning an Optional is a clear statement in the API that there might not be a value in there

# Using Streams

- A _stream_ pipeline is the operations that run on a stream to produce a result. 

- With _streams_, the data isn’t generated up front—it is created when needed.

There are three parts to a stream pipeline: 
 - _Source_: Where the stream comes from.
 - _Intermediate operations_: Transforms the stream into another one.Since streams use lazy evaluation, the intermediate operations do not run until the terminal operation runs. 
 - _Terminal operation_: Actually produces a result.Since streams can be used only once, the stream is no longer valid after a terminal operation completes.

### Creating Stream Sources

the _Stream_ interface is in the `java.util.stream` package.

```java
Stream<String> empty = Stream.empty(); // count = 0
Stream<Integer> singleElement = Stream.of(1); // count = 1 
Stream<Integer> fromArray = Stream.of(1, 2, 3); // count = 2
```

### Using Common Terminal Operations

| Method | What Happens for Infinite Streams | Return Value | Reduction|
|--------|-----------------------------------|---------------|---------|
| `allMatch() /anyMatch() /noneMatch()` | Sometimes terminates | `boolean` | No|
|`collect()`| Does not terminate | `Varies`| No|
| `count()`|Does not terminate| `long` | Does not terminate|
|`findAny() /findFirst()`| Terminates | `Optional<T>`| No|
|`forEach()` | Does not terminate| `void`|No|
|`min()/max()`|Does not terminate|`Optional<T>`|Yes|
|`reduce()`| Does not terminate | `Varies`| Yes|


_`count()`_
---
```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo"); 
System.out.println(s.count()); // 3
```

_`min() and max()`_
---

```java
Optional<T> min(<? super T> comparator) 
Optional<T> max(<? super T> comparator)
```

```java
Stream<String> s = Stream.of("monkey", "ape", "bonobo");
Optional<String> min = s.min((s1, s2) -> s1.length()—s2.length()); 
min.ifPresent(System.out::println); // ape
```

_`findAny() and findFirst()`_
---
The `findAny()` and `findFirst()` methods return an element of the stream unless the stream is empty. If the stream is empty, they return an empty `Optional`.

```java
Optional<T> findAny()
Optional<T> findFirst()
```

```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo"); 
Stream<String> infinite = Stream.generate(() -> "chimp"); 
s.findAny().ifPresent(System.out::println); // monkey 
infinite.findAny().ifPresent(System.out::println); // chim
```

_`allMatch(), anyMatch() and noneMatch()`_
---
```java
boolean anyMatch(Predicate <? super T> predicate) 
boolean allMatch(Predicate <? super T> predicate) 
boolean noneMatch(Predicate <? super T> predicate)
```
```java 
List<String> list = Arrays.asList("monkey", "2", "chimp"); 
Stream<String> infinite = Stream.generate(() -> "chimp"); 
Predicate<String> pred = x -> Character.isLetter(x.charAt(0)); 

System.out.println(list.stream().anyMatch(pred)); // true 
System.out.println(list.stream().allMatch(pred)); // false 
System.out.println(list.stream().noneMatch(pred)); // false 
System.out.println(infinite.anyMatch(pred)); // true
```
- we can reuse the same predicate, but we need a different stream each time
 
 _`forEach()`_
 ---

 ```java
 void forEach(Consumer<? super T> action)
 ```
 _`reduce()`_
 ---
The `reduce()` method combines a stream into a single object.

```java
T reduce(T identity, BinaryOperator<T> accumulator) Optional<T> 
reduce(BinaryOperator<T> accumulator)
<U> U reduce(U identity, BiFunction<U,? super T,U> accumulator, BinaryOperator<U> combiner)
```
- _identity_: The `identity` element is both the initial value of the reduction and the default result if there are no elements in the stream.
- _accumulator_: The `accumulator` function takes two parameters: a partial result of the reduction and the next element of the stream. It returns a new partial result. 
- _combiner_: The `combiner` function takes two result containers and merges their contents.

```java
Stream<String> stream = Stream.of("w", "o", "l", "f"); 
String word = stream.reduce("", (s, c) -> s + c); 
System.out.println(word); // wolf
```
When you don’t specify an identity, an Optional is returned because there might not be any data

_`collect()`_
---

The `collect()` method is a special type of reduction called a mutable reduction. It is more efficient than a regular reduction because we use the same mutable object while accumulating.

```java
<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner)
<R,A> R collect(Collector<? super T, A,R> collector)
```
- The first parameter is a `Supplier` that creates the object that will store the results as we collect data
- The second parameter is a `BiConsumer`, which takes two parameters and doesn’t return anything. It is responsible for adding one more element to the data collection.
- The final parameter is another `BiConsumer`. It is responsible for taking two data collections and merging them. This is useful when we are processing in parallel. 

```java
Stream<String> stream = Stream.of("w", "o", "l", "f"); 
StringBuilder word = stream.collect(StringBuilder::new, 
StringBuilder::append, StringBuilder:append)
```

# Using Common Intermediate Operations

### _`filter()`_

#### In mathematical logic, a predicate is commonly understood to be a Boolean-valued function P: X→ {true, false}, called the predicate on X.
```java
Stream<T> filter(Predicate<? super T> predicate)
```

```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.filter(x -> x.startsWith("m")).forEach(System.out::print); // monkey
```

### _`distinct()`_

The `distinct()` method returns a stream with duplicate values removed. 

```java
Stream<String> s = Stream.of("duck", "duck", "duck", "goose"); 
s.distinct().forEach(System.out::print); // duckgoose
```
- Java calls `equals()` to determine whether the objects are the same.

### _`limit() and skip()`_
The `limit()` and `skip()` methods make a Stream smaller. They could make a finite stream smaller, or they could make a finite stream out of an infinite stream. 

```java
Stream<T> limit(int maxSize) 
Stream<T> skip(int n)
```

```java
Stream<Integer> s = Stream.iterate(1, n -> n + 1); 
s.skip(5).limit(2).forEach(System.out::print); // 67
```
### _`map()`_

The `map()` method creates a one-to-one mapping from the elements in the stream to the elements of the next step in the stream.

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo"); 
s.map(String::length).forEach(System.out::print); // 676
```

### _`flatMap()`_

- The `flatMap()` method takes each element in the stream and makes any elements it contains toplevel elements in a single stream.
- This is helpful when you want to remove empty elements from a stream or you want to combine a stream of lists

```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)
```
```java
List<String> zero = Arrays.asList();
List<String> one = Arrays.asList("Bonobo");
List<String> two = Arrays.asList("Mama Gorilla", "Baby Gorilla");

Stream<List<String>> animals = Stream.of(zero, one, two);
animals.flatMap(l -> l.stream()).forEach(System.out::println);
```

```
Bonobo
Mama Gorilla Baby Gorilla
```
#### _`sorted()`_

- Java uses natural ordering unless we specify a comparator.

```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```

```java
Stream<String> s = Stream.of("brown bear-", "grizzly-");
 s.sorted(Comparator.reverseOrder())
  .forEach(System.out::print); // grizzly-brown bear-
```

```java
s.sorted(Comparator::reverseOrder); // DOES NOT COMPILE
```
- `Comparator` is a _functional interface_. This means that we can use method references or lambdas to implement it. 
- The `Comparator` interface implements one method that takes two String parameters and returns an int. 

- `Comparator::reverseOrder` doesn’t do that. It is a reference to a function that takes zero parameters and returns a `Comparator`.

### _`peek()`_

```java
Stream<T> peek(Consumer<? super T> action)
```
- The most common use for `peek()` is to output the contents of the stream as it goes by.

```java
Stream<String> stream = Stream.of("black bear", "brown bear", "grizzly");
long count = stream.filter(s -> s.startsWith("g"))
                    .peek(System.out::println).count(); // grizzly 
System.out.println(count); // 1
```

### Putting Together the Pipeline

```java
List<String> list = Arrays.asList("Toby", "Anna", "Leroy", "Alex");
stream.filter(n -> n.length() == 4)
      .sorted()
      .limit(2)
      .forEach(System.out::println);
```

What do you think the following does?

```java
Stream.generate(() -> "Elsa")
      .filter(n -> n.length() == 4) 
      .sorted()
      .limit(2) 
      .forEach(System.out::println);
```
#### It actually hangs until you kill the program or it throws an exception after running out of memory. The foreman has instructed `sorted()` to wait until everything to sort is present.

```java
Stream.generate(() -> "Elsa") 
      .filter(n -> n.length() == 4) 
      .limit(2)
      .sorted() 
      .forEach(System.out::println);
```
- This one prints `Elsa` twice.

```java
Stream.generate(() -> "Olaf Lazisson") 
      .filter(n -> n.length() == 4) .limit(2)
      .sorted() 
      .forEach(System.out::println);
```
- This one hangs as well until we kill the program. The `filter` doesn’t allow anything through, so `limit()` never sees two elements.

Peeking behind the scenes
----

```java
Stream<Integer> infinite = Stream.iterate(1, x -> x + 1); 
infinite.limit(5)
        .filter(x -> x % 2 == 1) 
        .forEach(System.out::print); // 135
```

```java
Stream<Integer> infinite = Stream.iterate(1, x -> x + 1); 
infinite.limit(5)
        .peek(System.out::print) 
        .filter(x -> x % 2 == 1) 
        .forEach(System.out::print) //11233455
```

```java
Stream<Integer> infinite = Stream.iterate(1, x -> x + 1); 
infinite.filter(x -> x % 2 == 1)
        .limit(5) 
        .forEach(System.out::print); // 13579
```

```java
Stream<Integer> infinite = Stream.iterate(1, x -> x + 1); 
infinite.filter(x -> x % 2 == 1)
        .peek(System.out::print) 
        .limit(5) 
        .forEach(System.out::print);
```

# Working with Primitives

`IntStream`: Used for the primitive types `int`, `short`, `byte`, and `char`

`LongStream`: Used for the primitive type `long`

`DoubleStream`: Used for the primitive types `double` and `float`

```java
DoubleStream empty = DoubleStream.empty();
DoubleStream oneValue = DoubleStream.of(3.14);
DoubleStream varargs = DoubleStream.of(1.0, 1.1, 1.2);
DoubleStream random = DoubleStream.generate(Math::random); 
DoubleStream fractions = DoubleStream.iterate(.5, d -> d / 2);
```

```java
IntStream range = IntStream.range(1, 6);
IntStream rangeClosed = IntStream.rangeClosed(1, 5);
```
-  The `range()` method indicates that we want the numbers 1–6, not including the number 6
- `rangeClosed()` an inclusive range


![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.08.21%20PM.png )

```java
Stream<String> objStream = Stream.of("penguin", "fish"); 
IntStream intStream = objStream.mapToInt(s -> s.length())
```
![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.08.33%20PM.png)

### Using Optional with Primitive Streams

```java
IntStream stream = IntStream.rangeClosed(1,10); 
OptionalDouble optional = stream.average();
```
- The difference is that `OptionalDouble` is for a primitive and `Optional<Double>` is for the `Double` wrapper class.
- The `sum()` method does not return an optional. If you try to add up an empty stream, you simply get zero. 

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.18.48%20PM.png)

### Summarizing Statistics

`Statistic` is just a big word for a number that was calculated from data.

```java
private static int range(IntStream ints) { 
    IntSummaryStatistics stats = ints.summaryStatistics(); 
    if (stats.getCount() == 0) throw new RuntimeException(); 
    return stats.getMax()—stats.getMin();
}
```

### Learning the Functional Interfaces for Primitives

#### Functional Interfaces for boolean

```java
BooleanSupplier b1 = () -> true;
System.out.println(b1.getAsBoolean());
```

# Functional Interfaces for double, int, and long

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.54.51%20PM.png)

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.54.59%20PM.png)

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.55.06%20PM.png)

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2010.55.13%20PM.png)

# Working with Advanced Stream Pipeline Concepts

### Linking Streams to the Underlying Data

```java
25: List<String> cats = new ArrayList<>();
26: cats.add("Annie");
27: cats.add("Ripley");
28: Stream<String> stream = cats.stream();
29: cats.add("KC");
30: System.out.println(stream.count());
```
- streams are lazily evaluated. This means that the stream isn’t actually created on line 28.
-  On line 29, the List gets a new element. On line 30, the stream pipeline actually runs.

### Chaining Optionals

```java
private static void threeDigit(Optional<Integer> optional) {
    optional.map(n -> "" + n)
            .filter(s -> s.length() == 3) 
            .ifPresent(System.out::println);
}
```

the signature `static Optional<Integer> calculator(String s)`? Using `map` doesn’t work:

```java
Optional<Integer> result = optional.map(ChainingOptionals::calculator); // DOES NOT COMPILE
```

- The problem is that `calculator` returns `Optional<Integer>`. The `map()` method adds another  `Optional`, giving us `Optional<Optional<Integer>>`

The solution is to call `flatMap()` instead:

```java
Optional<Integer> result = optional.flatMap(ChainingOptionals::calculator);
```

#### Checked exceptions and Functional interfaces


Collecting Results
-------------

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2011.27.29%20PM.png)

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-01-31%20at%2011.27.36%20PM.png)

Collecting Using Basic Collectors
----------

```java
Stream<String> ohMy = Stream.of("lions", "tigers", "bears");
String result = ohMy.collect(Collectors.joining(", ")); 
System.out.println(result); // lions, tigers, bears
```

```java
Stream<String> ohMy = Stream.of("lions", "tigers", "bears");
Double result = ohMy.collect(Collectors.averagingInt(String::length));
System.out.println(result); // 5.333333333333333
```

To Remember
-----------

* Streams can be used only once, this throws an exception that the _“stream has already been operated upon or closed.”_

```java
Stream<String> stream = Stream.iterate("-", (s) -> s + s); 
boolean b1 = stream.noneMatch(predicate);
boolean b2 = stream.anyMatch(predicate);
```
* The `partitioningBy()` operation always returns a map with two Boolean keys, even if there are no corresponding values. By contrast, `groupingBy()` returns only keys that are actually needed.

```java
 Stream<String> s = Stream.empty();
 Stream<String> s2 = Stream.empty();
 Map<Boolean, List<String>> c = s.collect(Collectors.partitioningBy(b -> b.startsWith("c")));
 Map<Boolean, List<String>> c2 = s2.collect(Collectors.partitioningBy(b -> b.startsWith("c")));
 System.out.println(c+" "+c2);
```

