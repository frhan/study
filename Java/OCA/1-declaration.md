
Declaration and Access Control
-------------------------------

Java Refresher
--------------
* class
* object
* State(instance variable)
* Behavior(Methods)

Inheritance
---
* allows code defined in one class to be reused in other classes.
* The superclass knows nothing of the classes that inherit from it

Interface
---------
* 100-percent abstract superclass that defines the methods a subclass must support, but not how they must be supported

Identifiers and Keywords
------------------------
* must start with a letter,a currency character($), or connecting character _
* can't start with digit
* after first character,identifier can contain any combination of letters,currency character,connecting character or numbers

**illegal**
```java
  int e#;
  int .f;
  int 7g;
```
Define Classes
------------
**Source File Declaration Rules**

* There can be only one public class per source code file.
* If there is a public class in a file, the name of the file must match the name of the public class.
* import and package statements apply to all classes within a source code file.
* A file can have more than one nonpublic class.

**Compiling with javac**
```java
javac [options] [source files]
```
**Launching Applications with java**

```java
java [options] class [args]
java -version MyClass x 1
```

**Using _public static void main(String[ ] args)_**

_main()_ is the method that the JVM uses to start execution of a Java program.

__public static void main(String[] args)__

Other versions of _main()_ with other signatures are perfectly legal, but they're treated as normal methods.

**The following are all legal declarations for the "special" main() :**

```java
static public void main(String[] args)
public static void main(String... x)
static public void main(String bang_a_gong[])
main() can be overloaded
```

**Import Statements and the Java API**

If you see a reference to a class you're not sure of, you can look through the entire package for that class

**Static Import Statements**

Static imports can be used when you want to "save typing" while using a class's static members.

After static imports:

```java
import static java.lang.System.out;
import static java.lang.Integer.*;
public class TestStaticImport {
 	public static void main(String[] args)
 		out.println(MAX_VALUE);
 		out.println(toHexString(42));
     }
}
```

**Here are a couple of rules for using static imports:**

* You must say import static ; you can't say static import .
* Watch out for ambiguously named static members.Integer class and the Long class, referring to MAX_VALUE will cause a compiler error
* You can do a static import on static object references, constants (remember they're static and final ), and static methods.

sometimes you can use the wildcard character * to do some simple searching for you. (You can search within a package or within a class.)

``` java
import java.*; // Legal, but this WILL NOT search across packages.
```

**Class Declarations and Modifiers**
* every class, method, and instance variable you declare has an access control, whether you explicitly type one or not

* a class can be declared with only public or default access; the other two access control levels don't make sense for a class
