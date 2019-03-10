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
![Alt test](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-10%20at%201.09.32%20AM.png)
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

### Checking Path Type with `isAbsolute()` and `toAbsolutePath()`
- `isAbsolute()`, returns `true` if the path the object references is absolute and `false` if the path the object references is relative.
- `toAbsolutePath()`, converts a relative Path object to an absolute Path object by joining it to the current working directory.

```java
Path path1 = Paths.get("C:\\birds\\egret.txt"); 
System.out.println("Path1 is Absolute? "+path1.isAbsolute());
System.out.println("Absolute Path1: "+path1.toAbsolutePath());
```
- if the Path object already represents an absolute path, then the output is a new Path object with the same value.

### Creating a New Path with `subpath()`
- `subpath(int,int)` returns a relative subpath of the `Path` object, referenced by an inclusive start index and an exclusive end index.

```java
Path path = Paths.get("/mammal/carnivore/raccoon.image"); 
System.out.println("Path is: "+path);
System.out.println("Subpath from 0 to 3 is: "+path.subpath(0,3)); 
System.out.println("Subpath from 1 to 3 is: "+path.subpath(1,3)); 
System.out.println("Subpath from 1 to 2 is: "+path.subpath(1,2));
```
```
Path is: /mammal/carnivore/raccoon.image
Subpath from 0 to 3 is: mammal/carnivore/raccoon.image
Subpath from 1 to 3 is: carnivore/raccoon.image
Subpath from 1 to 2 is: carnivore
```
- 0-indexed element is mammal in this example and not the root directory; therefore, the maximum index that can be used is 3
```java
System.out.println("Subpath from 0 to 4 is: "+path.subpath(0,4)); // THROWS //EXCEPTION AT RUNTIME
System.out.println("Subpath from 1 to 1 is: "+path.subpath(1,1)); // THROWS //EXCEPTION AT RUNTIME
```
### Using Path Symbols
![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-03-09%20at%2011.01.08%20PM.png)
- the path value `../bear.txt` refers to a file named `bear.txt` in the parent of the current directory
- the path value `./penguin.txt` refers to a file named `penguin.txt` in the current directory
- `../../lion.data` refers to a file `lion.data` that is two directories up from the current working directory.

### Deriving a Path with `relativize()`
- `relativize(Path)` for constructing the relative path from one Path object to another

```java
Path path1 = Paths.get("fish.txt");
Path path2 = Paths.get("birds.txt"); 
System.out.println(path1.relativize(path2)); 
System.out.println(path2.relativize(path1));
```
- The `relativize()` method requires that both paths be absolute or both relative, and it will throw an `IllegalArgumentException` if a relative path value is mixed with an absolute path value.

```java
Path path1 = Paths.get("/primate/chimpanzee");
Path path2 = Paths.get("bananas.txt"); 
Path1.relativize(path3); // THROWS EXCEPTION AT RUNTIME
```
### Joining Path Objects with `resolve()`
- a `resolve(Path)` method for creating a new `Path` by joining an existing path to the current path.

```java
final Path path1 = Paths.get("/cats/../panther"); 
final Path path2 = Paths.get("food"); 
System.out.println(path1.resolve(path2));
```
### Cleaning Up a Path with `normalize()`
- The `normalize()` method of the Path interface can normalize a path. 
- *Normalizing means* that it removes all the `.` and `..` codes in the middle of the path string, and resolves what path the path string refers to
### Checking for File Existence with `toRealPath()`
- The `toRealPath(Path)` method takes a `Path` object that may or may not point to an existing file within the file system, and it returns a reference to a real path within the file system.
- it also verifies that the file referenced by the path actually exists, and thus it throws a checked `IOException` at runtime if the file cannot be located. 
- The toRealPath() method performs additional steps, such as removing redundant path elements

## Interacting with Files

### Testing a Path with `exists()`
- The `Files.exists(Path)` method takes a `Path` object and returns `true` if, and only if, it references a file that exists in the file system.

### Testing Uniqueness with `isSameFile()`
- The `Files.isSameFile(Path,Path)` method is useful for determining if two Path objects relate to the same file within the file system. 
- It takes two Path objects as input and follows symbolic links.
- This `isSameFile()` method does not compare the contents of the file

### Making Directories with `createDirectory()` and `createDirectories()`
- `Files.createDirectory(Path)` method to create a directory. 
- `createDirectories()`, which like `mkdirs()` creates the target directory along with any nonexistent parent directories leading up to the target directory in the path.
- The directory creation methods can throw the checked `IOException`, such as when the directory cannot be created or already exists.

```java
try {
    Files.createDirectory(Paths.get("/bison/field"));
    Files.createDirectories(Paths.get("/bison/field/pasture/green"));
} catch (IOException e) {
// Handle file I/O exception...
}
```

### Duplicating File Contents with `copy()`
- `Files.copy(Path,Path)`, which copies a file or directory from one location to another. The `copy()` method throws the checked `IOException`, such as when the file or directory does not exist or cannot be read.
- Directory copies are shallow rather than deep, meaning that files and subdirectories within the directory are not copied.
- copying files and directories will traverse symbolic links, although it
will not overwrite a file or directory if it already exists, nor will it copy file attributes.

```java
try {
    Files.copy(Paths.get("/panda"), Paths.get("/panda-save"));
    Files.copy(Paths.get("/panda/bamboo.txt"), Paths.get("/panda-save/bamboo.txt"));
} catch (IOException e) {
// Handle file I/O exception...
}
```

### Changing a File Location with `move()`
The `Files.move(Path,Path)` method moves or renames a file or directory within the file system

```java
try {
    Files.move(Paths.get("c:\\zoo"), Paths.get("c:\\zoo-new"));
    Files.move(Paths.get("c:\\user\\addresses.txt"),
                 Paths.get("c:\\zoo-new\\addresses.txt"));
} catch (IOException e) {
// Handle file I/O exception...
}
```
- the `move()` method will follow links, throw an exception if the file already exists, and not perform an atomic move.

### Removing a File with `delete()` and `deleteIfExists()`
- The `Files.delete(Path)` method deletes a file or empty directory within the file system.
- The `delete()` method throws the checked `IOException` under a variety of circumstances. 
    - if the path represents a non-empty directory, the operation will throw the runtime `DirectoryNotEmptyException`.
    - If the target of the path is a symbol link, then the symbolic link will be deleted, not the target of the link.

- The `deleteIfExists(Path)` method is identical to the `delete(Path)` method, except that it will not throw an exception if the file or directory does not exist, but instead it will return a `boolean` value of `false`.

### Reading and Writing File Data with `newBufferedReader()` and `newBufferedWriter()`
- `Files.newBufferedReader(Path,Charset)`, reads the file specified at the `Path` location using a `java.io.BufferedReader` object

```java
try (BufferedReader reader = Files.newBufferedReader(path, Charset.forName("US-ASCII"))){

}
```

```java
try (BufferedWriter writer = Files.newBufferedWriter(path, Charset.forName("UTF-16")) ){
}
```
### Reading Files with readAllLines()
- The `Files.readAllLines()` method reads all of the lines of a text file and returns the results as an ordered `List` of `String` values. 
```java
Path path = Paths.get("/fish/sharks.log");

try {
    final List<String> lines = Files.readAllLines(path);
    for(String line: lines) { 
        System.out.println(line);
    }
} catch (IOException e) {
// Handle file I/O exception... }
```

### Understanding File Attributes
- the Files class also provides numerous methods accessing file and directory metadata, referred to as file _attributes_. Put simply, metadata is data that describes other data. In this context, file metadata is data about the file or directory record within the file system and not the contents of the file.

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
