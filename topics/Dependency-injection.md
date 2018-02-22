Dependency injection
=====================

What it is?
---------
 is a technique whereby one object supplies the dependencies of another object(Service).
 An injection is the passing of a dependency to a dependent object(Client) that would use it.
Why?
The intent behind dependency injection is to decouple objects to the extent that no client code has to be changed simply because an object it depends on needs to be changed to a different one.

Dependency injection is one form of the broader technique of inversion of control
rather than low level code calling up to high level code, high level code can receive lower level code that it can call down to.

Advantages?
Dependency injection separates the creation of a client's dependencies from the client's behavior, which allows program designs to be loosely coupled
Dependency injection involves four roles:
the service object(s) to be used
the client object that is depending on the services it uses
the interfaces that define how the client may use the services
the injector, which is responsible for constructing the services and injecting them into the client

## IoC means letting other code call you rather than insisting on doing the calling.
Three types of dependency injection
constructor injection: the dependencies are provided through a class constructor.
setter injection: the client exposes a setter method that the injector uses to inject the dependency.
nterface injection: the dependency provides an injector method that will inject the dependency into any client passed to it. Clients must implement an interface that exposes a setter method that accepts the dependency.


REf:
* https://stormpath.com/blog/spring-boot-dependency-injection
