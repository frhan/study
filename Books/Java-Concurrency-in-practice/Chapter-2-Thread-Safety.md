# Chapter 2 Thread Safety

- Writing thread-safe code is, at its core, about managing access to state, and in particular to shared, mutable state.

If multiple threads access the same mutable state variable without appropriate synchronization, your program is broken. There are three ways to fix it:

- Don’t share the state variable across threads;
- Make the state variable immutable; or
- Use synchronization whenever accessing the state variable.

It is far easier to design a class to be thread-safe than to retrofit it for thread safety later.

When designing thread-safe classes, good object-oriented techniques - encapsulation, immutability, and clear specification of invariants are your best friends.

In any case, the concept of a thread-safe class makes sense only if the class encapsulates its own state

## What is thread safety?

A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional syn- chronization or other coordination on the part of the calling code.

Thread-safe classes encapsulate any needed synchronization so that clients need not provide their own.

## Atomicity

The possibility of incorrect results in the presence of unlucky timing is so important in concurrent programming that it has a name: a race condition.
