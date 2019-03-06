# NIO

# Introducing NIO.2
- the  `java.io` API uses byte streams to interact with file data
- The `NIO` API introduced the concepts of **buffers** and **channels** in place of `java.io` streams

### Introducing Path
- A `Path` object represents a hierarchical path on the storage system to a file or directory
- `Path` is a direct replacement for the legacy `java.io.File` class
- the `Path` interface contains support for symbolic links. A symbolic link is a special file within an operating system that serves as a reference or pointer to another file or directory.

### Creating Instances with Factory and Helper Classes
### Creating Paths

```java
Path path1 = Paths.get("pandas/cuddly.png");
Path path3 = Paths.get("/home/zoodirector");
```
- a `Path` using the `Paths` class using a `vararg` of type `String`, such as `Paths.get(String,String...)`.
-  a `Path` using the `Paths` class is with a `URI` value.
- `new URI(String)` does throw a checked `URISyntaxException`, which would have to be caught in any application

```java
Path path1 = Paths.get(new URI("file://pandas/cuddly.png")); // THROWS EXCEPTION                                                                   //AT RUNTIME
Path path2 = Paths.get(new URI("file:///c:/zoo-info/November/employees.txt")); 
Path path3 = Paths.get(new URI("file:///home/zoodirectory"));
```

### absolute vs. relative Is file system dependent
- If a path starts with a forward slash, it is an absolute path, such as `/bird/parrot`. 
- If a path starts with a drive letter, it is an absolute path, such as `C:\bird\emu`. 
- Otherwise, it is a relative path, such as `..\eagle`


### Accessing the Underlying _`FileSystem`_ Object
- The `FileSystem` class has a protected constructor, so we use the plural `FileSystems` factory class to obtain an instance of `FileSystem`
 ```java
 Path path2 = FileSystems.getDefault().getPath("c:","zooinfo","November", "employees.txt");
 ```

### Using the Paths Class
### Interacting with Paths and Files
### Providing Optional Arguments
- table 9.1

### Using Path Objects
- Many of the methods in the Path interface transform the path value in some way and return a `new Path` object, allowing the methods to be chained.

```java
Paths.get("/zoo/../home").getParent().normalize().toAbsolutePath();
```
### Viewing the Path with `toString()`, `getNameCount()`, and `getName()`
- `getNameCount()` and `getName(int)`, are often used in conjunction to retrieve the number of elements in the path and a reference to each element, respectively.
- If the Path object represents the root element itself, then the number of names in the Path object returned by `getNameCount()` will be 0.

### Accessing Path Components with `getFileName()`, `getParent()`, and `getRoot()`
- `getFileName()`, returns a `Path` instance representing the filename, which is the farthest element from the root.
- `getParent()`, returns a `Path` instance representing the parent path or `null` if there is no such parent.

### Interacting with Files

### Understanding File Attributes
- the Files class also provides numerous methods accessing file and directory metadata, referred to as file attributes. Put simply, metadata is data that describes other data. In this context, file metadata is data about the file or directory record within the file system and not the contents of the file.

#### Discovering Basic File Attributes
#### Reading Common Attributes with isDirectory(), isRegularFile(),and isSymbolicLink()

### Discovering Basic File Attributes
### Improving Access with Views

### Presenting the New Stream Methods
### Conceptualizing Directory Walking
### Selecting a Search Strategy
### Walking a Directory
### Avoiding Circular Paths
### Listing Directory Contents
### Printing File Contents

Comparing Legacy File and NIO.2 Methods
---