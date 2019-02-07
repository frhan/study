Concurrency
----

# Introducing Threads

_shared environment_, we mean that the threads in the same process share the same memory space and can communicate directly with one another.

A _task_ is a single unit of work performed by a thread.

### Distinguishing Thread Types

A _system_ thread is created by the JVM and runs in the background of the application

a _user-defined_ thread is one created by the application developer to accomplish a specific task. 


a _daemon_ thread is one that will not prevent the JVM from exiting when the program finishes.

### Understanding Thread Concurrency

The property of executing multiple threads and processes at the same time is referred to as concurrency.

Operating systems use a _thread scheduler_ to determine which threads should be cur- rently executing.

A _context switch_ is the process of storing a thread’s current state and later restoring the state of the thread to continue execution.

A _thread priority_ is a numeric value associated with a thread that is taken into consideration by the thread scheduler when determining which threads should currently be executing.

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-04%20at%208.42.20%20PM.png)

### Introducing Runnable

_Runnable_ for short, is a functional interface that takes no arguments and returns no data.

```java
@FunctionalInterface 
public interface Runnable { 
    void run();
}
```
lambda expressions each rely on the Runnable interface:

```java
() -> System.out.println("Hello World")
```

### Creating a Thread

a Thread instance will execute can be done two ways in Java:
- Provide a Runnable object or lambda expression to the Thread constructor.
- Create a class that extends Thread and overrides the run() method.

- be careful about cases where a Thread or Runnable is created but no `start()` method is called.

### Polling with Sleep

`Thread.sleep()` method to implement polling. The `Thread.sleep()` method requests the current thread of execution rest for a specified number of milliseconds.

# Creating Threads with the _ExecutorService_

### Introducing the Single-Thread Executor

 ```java
 ExecutorService service = null; 
 try {
     service = Executors.newSingleThreadExecutor();
     System.out.println("begin");
     service.execute(() -> System.out.println("Printing zoo inventory")); 
     service.execute(() -> {for(int i=0; i<3; i++)
     System.out.println("Printing record: "+i);} );
     service.execute(() -> System.out.println("Printing zoo inventory"));
     System.out.println("end"); 
    } finally {
         if(service != null) service.shutdown();
    }         
    } 
}
```
 
 - With a _single-thread executor_, results are guaranteed to be executed in the order in which they are added to the executor service.
 
### Shutting Down a Thread Executor

![alt text ](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-04%20at%208.58.27%20PM.png)

- you should be aware that shutdown() does not actually stop any tasks that have already been submitted to the thread executor

want to cancel all running and upcoming tasks?
- The `ExecutorService` provides a method called `shutdownNow()`, which attempts to stop all running tasks and discards any that have not been started yet. Note that `shutdownNow()` attempts to stop all running tasks.
- `shutdownNow()` returns a `List<Runnable>` of tasks that were submitted to the thread executor but that were never started.

### Submitting Tasks

submit tasks to an `ExecutorService` instance multiple ways:
 - `execute()`, is inherited from the `Executor` interface, which the `ExecutorService` interface extends.
 - Java added `submit()` methods to the `ExecutorService` interface, which, like `execute()`, can be used to complete tasks asynchronously.
- Unlike `execute()`, though, `submit()` returns a `Future` object that can be used to determine if the task is complete. 

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-04%20at%209.16.52%20PM.png)

![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-04%20at%209.16.57%20PM.png)

### submitting Tasks: execute() vs submit()
`execute()` does not support `Callable` expressions, we tend to prefer `submit()` over `execute()`, even if you don't store the `Future` reference.

### Submitting Task Collections

* The `invokeAll()` method executes all tasks in a provided collection and returns a `List` of ordered `Future` objects, with one `Future` object corresponding to each submitted task, in the order they were in the original collection.

* The `invokeAny()` method executes a collection of tasks and returns the result of one of the tasks that successfully completes execution, cancelling all unfinished tasks.

The `ExecutorService` interface also includes overloaded versions of `invokeAll()` and `invokeAny()` that take a `timeout` value and `TimeUnit` parameter.

### Waiting for Results

the `submit()` method returns a `java.util.concurrent.Future<V> object`, or `Future<V>` for short.

```java
Future<?> future = service.submit(() -> System.out.println("Hello Zoo"));
```
![alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-04%20at%209.26.21%20PM.png)

Introducing `Callable`
---

`Callable` for short, which is similar to `Runnable` except that its `call()` method returns a value and can throw a checked exception.
 
### Ambiguous lambda expressions: Callable vs. Supplier

`Supplier` functional interface, in that they both take no arguments and return a generic type. One difference is that the method in `Callable` can `throw` a checked Exception.
- the `get()` methods on a Future object return the matching generic type or null

```java
public static void use(Callable<Integer> expression) {}
use(() -> {throw new IOException();}); // DOES NOT COMPILE
use((Callable<Integer>)() -> {throw new IOException("");}); // COMPILES
```

- Since `Callable` supports a return type when used with `ExecutorService`, it is often preferred over `Runnable` when using the Concurrency API

### Waiting for All Tasks to Finish
*  we use the `awaitTermination(long timeout, TimeUnit unit)` method available for all thread.
* The method waits the specified time to complete all tasks, returning sooner if all tasks finish or an `InterruptedException` is detected

```java
service.awaitTermination(1, TimeUnit.MINUTES);
```

### Scheduling Tasks

* The `ScheduledExecutorService`, which is a subinterface of `ExecutorService`, can be used for just such a task.

```java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
```

### Increasing Concurrency with Pools



# Synchronizing Data Access

### Protecting Data with Atomic Classes

_Atomic_ is the property of an operation to be carried out as a single unit of execution without any interference by another thread. 

Any thread trying to access the sheepCount variable while an atomic operation is in process will have to wait until the atomic operation on the variable is complete

Concurrency API includes numerous useful classes that are conceptually the same as our primitive classes but that support atomic operations.

### Improving Access with Synchronized Blocks


Using Concurrent Collections
---

### Introducing Concurrent Collections
### Understanding Memory Consistency Errors
### Working with Concurrent Classes
### Obtaining Synchronized Collections

# Working with Parallel Streams

A _parallel stream_ is a stream that is capable of processing results concurrently, using multiple threads.

### Creating Parallel Streams

#### `parallel()`

```java
Stream<Integer> stream = Arrays.asList(1,2,3,4,5,6).stream(); 
Stream<Integer> parallelStream = stream.parallel();
```

#### `parallelStream()`

```java
Stream<Integer> parallelStream2 = Arrays.asList(1,2,3,4,5,6).parallelStream();
```

### Processing Tasks in Parallel

```java
Arrays.asList(1,2,3,4,5,6)
      .parallelStream()
      .forEach(s -> System.out.print(s+" "));
```
-  the results are no longer ordered or predictable.
- `forEachOrdered()`, which forces a parallel stream to process the results in order at the cost of performance
 
### Understanding Performance Improvements
- a _parallel stream_ can have a four-fold improvement in the results. Even better, the results scale with the number of processors. _Scaling_ is the property that, as we add more resources such as CPUs, the results gradually improve.

-  _Parallel streams_ tend to achieve the most improvement when the number of elements in the stream is significantly large

### Understanding Independent Operations
_Independent operations_, we mean that the results of an operation on one element of a stream do not require or impact the results of another element of the stream.

```java
Arrays.asList("jackal","kangaroo","lemur") 
      .parallelStream()
      .map(s -> s.toUpperCase()) 
      .forEach(System.out::println);
```

### Avoiding Stateful Operations
* _stateful lambda expression_ is one whose result depends on any state that might change during the execution of a pipeline.
* It strongly recommended that you avoid stateful operations when using parallel streams, so as to remove any potential data side effect

### Processing Parallel Reductions

#### Performing Order-Based Tasks

```java
System.out.print(Arrays.asList(1,2,3,4,5,6).parallelStream().findAny().get());
```
The result is that the output could be 4, 1, or really any value in the stream.

- Any stream operation that is based on order, including `findFirst()`, `limit()`, or `skip()`, may actually perform more slowly in a parallel environment.

- using an unordered version has no effect, but on parallel streams, the results can greatly improve performance:

```java
Arrays.asList(1,2,3,4,5,6).stream().unordered().parallel();
```
#### Combining Results with `reduce()`

Requirements for `reduce()` Arguments:
* The _identity_ must be defined such that for all elements in the `stream u, combiner.apply(identity, u)` is equal to u.
* The _accumulator_ operator op must be associative and stateless such that `(a op b) op c is equal to a op (b op c)`.
* The _combiner_ operator must also be associative and stateless and compatible with the identity, such that for all u and t `combiner.apply(u,accumulator.apply(identity,t))` is equal to `accumulator.apply(u,t)`.

### Combing Results with `collect()`

```java
Stream<String> stream = Stream.of("w", "o", "l", "f").parallel();
SortedSet<String> set = stream.collect(ConcurrentSkipListSet::new, Set::add, Set::addAll);
System.out.println(set); // [f, l, o, w]
```
 -  `ConcurrentSkipListSet` are sorted according to their natural ordering.

#### Using the One-Argument collect() Method

```java
Stream<String> stream = Stream.of("w", "o", "l", "f").parallel(); 
Set<String> set = stream.collect(Collectors.toSet());
System.out.println(set); // [f, w, l, o]
```
#### Requirements for Parallel Reduction with `collect()`
* The stream is parallel.
* The parameter of the collect operation has the `Collector.Characteristics.CONCURRENT characteristic`.
* Either the stream is unordered, or the collector has the characteristic `Collector.Characteristics.UNORDERED`.

The `Collectors` class includes two sets of methods for retrieving collectors
that are both `UNORDERED` and `CONCURRENT`, `Collectors.toConcurrentMap()` and `Collectors.groupingByConcurrent()`, and therefore it is capable of performing parallel reductions efficiently. 

```java
Stream<String> ohMy = Stream.of("lions", "tigers", "bears").parallel(); ConcurrentMap<Integer, String> map = ohMy.collect(Collectors.toConcurrentMap(String::length, k -> k, (s1, s2) -> s1 + "," + s2));
System.out.println(map); // {5=lions,bears, 6=tigers}
System.out.println(map.getClass()); // java.util.concurrent.ConcurrentHashMap
```

`groupingBy()` example to use a parallel stream and parallel reduction:

```java
Stream<String> ohMy = Stream.of("lions", "tigers", "bears").parallel(); ConcurrentMap<Integer, List<String>> map = ohMy.collect(
Collectors.groupingByConcurrent(String::length)); System.out.println(map); // {5=[lions, bears], 6=[tigers]}
```

# Managing Concurrent Processes

### Creating a `CyclicBarrier`

A synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point. `CyclicBarriers` are useful in programs involving a fixed sized party of threads that must occasionally wait for each other. The barrier is called _cyclic_ because it can be re-used after the waiting threads are released.

The `CyclicBarrier` takes in its constructors a limit value, indicating the number of threads to wait for. As each thread finishes, it calls the `await()` method on the cyclic barrier. Once the specified number of threads have each called `await()`, the barrier is released and all threads can continue.

-  The `CyclicBarrier` class allows us to perform complex, multi-threaded tasks, while all threads stop and wait at logical barriers.

### Applying the Fork/Join Framework


Identifying Threading Problems
---

### Understanding Liveness
### Managing Race Conditions
