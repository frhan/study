# Chapter 2 Thread Safety

- Writing thread-safe code is, at its core, about managing access to state, and in particular to shared, mutable state.

- If multiple threads access the same mutable state variable without appropriate synchronization, your program is broken. There are three ways to fix it:

  - Don’t share the state variable across threads;
  - Make the state variable immutable; or
  - Use synchronization whenever accessing the state variable.

- It is far easier to design a class to be thread-safe than to retrofit it for thread safety later.

- When designing thread-safe classes, good object-oriented techniques - encapsulation, immutability, and clear specification of invariants are your best friends.

- In any case, the concept of a thread-safe class makes sense only if the class encapsulates its own state

## What is thread safety?

- A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional syn- chronization or other coordination on the part of the calling code.

- Thread-safe classes encapsulate any needed synchronization so that clients need not provide their own.

## Atomicity

- The possibility of incorrect results in the presence of unlucky timing is so important in concurrent programming that it has a name: a race condition.

### Race conditions

- A race condition occurs when the correctness of a computation depends on the relative timing or interleaving of multiple threads by the runtime; in other words, when getting the right answer relies on lucky timing.

- check-then-act: you observe something to be true (file X doesn’t exist) and then take action based on that observation (create X); but in fact the observation could have become invalid between the time you observed it and the time you acted on it (someone else created X in the meantime), causing a problem (unexpected exception, overwritten data, file corruption).

### Compound actions

- Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or none of it has. An atomic operation is one that is atomic with respect to all operations, including itself, that operate on the same state.

- To ensure thread safety, check-then-act operations (like lazy initializa- tion) and read-modify-write operations (like increment) must always be atomic. We refer collectively to check-then-act and read-modify-write sequences as com- pound actions: sequences of operations that must be executed atomically in order to remain thread-safe.

- The `java.util.concurrent.atomic` package contains atomic variable classes for effecting atomic state transitions on numbers and object references.

- Where practical, use existing thread-safe objects, like AtomicLong, to manage your class’s state. It is simpler to reason about the possible states and state transitions for existing thread-safe objects than it is for arbitrary state variables, and this makes it easier to maintain and verify thread safety.

## Locking

To preserve state consistency, update related state variables in a single atomic operation.

### Intrinsic locks

- Java provides a built-in locking mechanism for enforcing atomicity: the synchro- nized block.

- A synchronized block has two parts: a reference to an object that will serve as the lock, and a block of code to be guarded by that lock.

- Static synchronized methods use the Class object for the lock

- Every Java object can implicitly act as a lock for purposes of synchronization; these built-in locks are called intrinsic locks or monitor locks

- Intrinsic locks in Java act as mutexes (or mutual exclusion locks), which means that at most one thread may own the lock. When thread A attempts to acquire a lock held by thread B, A must wait, or block, until B releases it. If B never releases the lock, A waits forever.

- In the context of concurrency, atomicity means the same thing as it does in transactional applications—that a group of statements appear to execute as a single, indivisible unit.

### Reentrancy

- Reentrancy facilitates encapsulation of locking behavior, and thus simplifies the development of object-oriented concurrent code

## Guarding state with locks

- if synchronization is used to coordinate access to a variable, it is needed everywhere that variable is accessed. Fur- ther, when using locks to coordinate access to a variable, the same lock must be used wherever that variable is accessed.

- For each mutable state variable that may be accessed by more than one thread, all accesses to that variable must be performed with the same lock held. In this case, we say that the variable is guarded by that lock.

- There is no inherent relationship between an object’s intrinsic lock and its state; an object’s fields need not be guarded by its intrinsic lock, though this is a perfectly valid locking convention that is used by many classes.

- The fact that every object has a built-in lock is just a convenience so that you needn’t explicitly create lock objects.

- Every shared, mutable variable should be guarded by exactly one lock. Make it clear to maintainers which lock that is.

- Not all data needs to be guarded by locks—only mutable data that will be accessed from multiple threads

- When a variable is guarded by a lock—meaning that every access to that vari- able is performed with that lock held—you’ve ensured that only one thread at a time can access that variable.

- For every invariant that involves more than one variable, all the variables involved in that invariant must be guarded by the same lock.

- At the same time, synchronizing every method can lead to liveness or performance problem

## Liveness and performance

- Acquiring and releasing a lock has some overhead, so it is undesirable to break down synchronized blocks too far

- Deciding how big or small to make synchronized blocks may require tradeoffs among competing design forces, including safety (which must not be compromised), simplicity, and performance.

- There is frequently a tension between simplicity and performance. When implementing a synchronization policy, resist the temptation to prematurely sacrifice simplicity (potentially compromising safety) for the sake of performance

- Avoid holding locks during lengthy computations or operations at risk of not completing quickly such as network or console I/O.
