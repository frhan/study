# NIO

## Introducing NIO.2
- the  `java.io` API uses byte streams to interact with file data
- The `NIO` API introduced the concepts of **buffers** and **channels** in place of `java.io` streams
- The basic idea is that you load the data from a file channel into a temporary buffer that, unlike byte streams, can be read forward and backward without blocking on the underlying resource.

### Introducing Path
- A `Path` object represents a hierarchical path on the storage system to a file or directory
- `Path` is a direct replacement for the legacy `java.io.File` class
- the `Path` interface contains support for symbolic links. A symbolic link is a special file within an operating system that serves as a reference or pointer to another file or directory.

### Creating Instances with Factory and Helper Classes
### **why Is Path an Interface?**
- The advantage of using the factory pattern here is that you can write the same code that will run on a variety of different platforms
#figure 9.1

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

#### **Path Object vs. actual file** 
- a Path object is not a file but a representation of a location within the file system. 
- retrieving the parent or root directory of a Path object does not require the file to exist, although the JVM may access the underlying file system to know how to process the path information.

### Providing Optional Arguments
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-07%20at%2012.30.07%20AM.png)

- An _atomic_ operation is any operation that is performed as a single indivisible unit of execution, which appears to the rest of the system as occurring instantaneously.
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

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-07%20at%2011.33.03%20PM.png)

### Checking File Visibility with `isHidden()`
-  In Linux- or Mac-based systems, this is often denoted by file or directory entries that begin with a period character.
- The `isHidden()` method throws the checked `IOException`, as there may be an I/O error reading the underlying file information.

### Testing File Accessibility with  `isReadable()` and `isExecutable()`
- This is important in file systems where the filename can be viewed within a directory, but the user may not have permission to read the contents of the file or execute it.

```java
System.out.println(Files.isReadable(Paths.get("/seal/baby.png")));
```
### Reading File Length with `size()`
- `Files.size(Path)` method is used to determine the size of the file in bytes.
- The `size()` method throws the checked `IOException` if the file does not exist or if the process is unable to read the file information.

### Managing File Modifications with `getLastModifiedTime()` and `setLastModifiedTime()`
- The Files class provides the method `Files.getLastModifiedTime(Path)`, which returns a `FileTime` object to accomplish this.
- The Files class also provides a mechanism for updating the last-modified date/time of a file using the `Files.setLastModifiedTime(Path,FileTime)` method
- Both of these methods have the ability to throw a checked `IOException` when the file is accessed or modified.

```java
try {
    final Path path = Paths.get("/rabbit/food.jpg"); 
    System.out.println(Files.getLastModifiedTime(path).toMillis());
    Files.setLastModifiedTime(path, FileTime.fromMillis(System.currentTimeMillis()));
    System.out.println(Files.getLastModifiedTime(path).toMillis());
} catch (IOException e) {
// Handle file I/O exception...
}
```

### Managing Ownership with _`getOwner()`_ and _`setOwner()`_
- the `Files.getOwner(Path)` method returns an instance of `UserPrincipal` that represents the owner of the file within the file system.

```java
UserPrincipal owner = FileSystems.getDefault()
                                .getUserPrincipalLookupService() 
                                .lookupPrincipalByName("jane");
```

### Improving Access with Views
-  A _view_ is a group of related attributes for a particular file system type. A file may support multiple views, allowing you to retrieve and update various sets of information about the file.
- more attributes are read than in a single method call, there are fewer roundtrips between Java and the operating system, whereas reading the same attributes with the previously described single method calls would require many such trips.

### Understanding Views
- Files.readAttributes(), returns a read-only view of the file attributes
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-07%20at%2011.49.21%20PM.png)
### Reading Attributes
### _BasicFileAttributes_
 - All attributes classes extend from `BasicFileAttributes`; therefore it contains attributes common to all supported file systems.

```java
BasicFileAttributes data = Files.readAttributes(path, BasicFileAttributes.class);
```
- The `isOther()` method is used to check for paths that are not files, directories, or symbolic links, such as paths that refer to resources or devices in some file systems. 
- The `lastAccessTime()` and `creationTime()` methods return other date/time information about the file.
- The `fileKey()` method returns a file system value that represents a unique identifier for the file within the file system or null if it is not supported by the file system.

### Modifying Attributes
- `Files.readAttributes()` method is useful for reading file data, it does not provide a direct mechanism for modifying file attributes. 

### _BasicFileAttributeView_
- cannot modify the other basic attributes directly, since this would change the property of the file system object
```java

BasicFileAttributeView view = Files.getFileAttributeView(path,BasicFileAttributeView.class);

BasicFileAttributes data = view.readAttributes();

FileTime lastModifiedTime = FileTime.fromMillis( data.lastModifiedTime().toMillis()+10_000);

view.setTimes(lastModifiedTime,null,null);
```

### Presenting the New Stream Methods
### Conceptualizing Directory Walking
- Every record in a file system has exactly one parent, with the exception of the root directory, which sits atop everything
- A common task in a file system is to iterate over the descendants of a particular file path, either recording information about them or, more commonly, filtering them for a specific set of files
- _Walking or traversing_ a directory is the process by which you start with a parent directory and iterate over all of its descendants until some condition is met or there are no more elements over which to iterate
- 
### Selecting a Search Strategy
- A _depth-first search_ traverses the structure from the root to an arbitrary leaf and then navigates back up toward the root, traversing fully down any paths it skipped along the way. The search depth is the distance from the root to current node.

- a _breadth-first search_ starts at the root and processes all elements of each particular depth, or distance from the root, before proceeding to the next depth level
### Walking a Directory
- The Files.walk(path) method returns a Stream<Path> object that traverses the directory in a depth-first, lazy manner.
- By lazy, we mean the set of elements is built and read while the directory is being traversed

```java
Path path = Paths.get("/bigcats");
try {
    Files.walk(path)
        .filter(p -> p.toString().endsWith(".java")) 
        .forEach(System.out::println);
} catch (IOException e) {
    // Handle file I/O exception...
}
```

### Avoiding Circular Paths
- A _cycle_ is an infinite circular depen- dency in which an entry in a directory is an ancestor of the directory. 

    For example, imagine that we had a directory `/birds/robin` that contains a symbolic link called `/birds/robin/allBirds` that pointed to `/birds`. Trying to traverse the `/birds/robin` directory would result in an infinite loop since each time the `allBirds` subdirectory was reached, we would go back to the parent path

### Searching a Directory
- `Files.find(Path,int,BiPredicate)` method behaves in a similar manner as the `Files.walk()` method, except that it requires the depth value to be explicitly set along with a `BiPredicate` to filter the data. 

```java
Path path = Paths.get("/bigcats"); long dateFilter = 1420070400000l;
try {
    Stream<Path> stream = Files.find(path, 10, 
                                    (p,a) -> p.toString().endsWit(".java") && a.lastModifiedTime().toMillis() > dateFilter);
                                    
    stream.forEach(System.out::println);
} catch (Exception e) {
// Handle file I/O exception...
}
```
### Listing Directory Contents
### Printing File Contents
- the NIO.2 API in Java 8 now includes a `Files.lines(Path)` method that returns a `Stream<String>` object and does not suffer from this same issue. The contents of the file are read and processed lazily, which means that only a small portion of the file is stored in memory at any given time.
```java
    Files.lines(path).forEach(System.out::println);
```


### Comparing Legacy File and NIO.2 Methods
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-07%20at%2011.31.54%20PM.png)
