# IO

# Understanding Files and Directories
### Conceptualizing the File System
 A _file_ is record within a file system that stores user and system data. Files are organized using directories.
 
 A _directory_ is a record within a file system that contains files as well as other directories.

 A _path_ is a String representation of a file or directory within a file system. 

### Introducing the File Class
The **`File`** class is used to read information about existing files and directories, list the contents of a directory, and create/delete files and directories.

* One common mistake new Java developers make is forgetting that the `File` class can be used to represent directories as well as files

### Creating a File Object
The _absolute path_ of a file or directory is the full path from the root directory to the file or directory, including all subdirectories that contain the file or directory

The _relative path_ of a file or directory is the path from the current working directory to file or directory.
For example, the following is an absolute path to the `zoo.txt` file.

* Unix-based systems use the forward slash `/` for paths, whereas Windows-based systems use the backslash `\` character.
* a system property and a static variable defined in the File class.

```java
System.out.println(System.getProperty("file.separator")); 
System.out.println(java.io.File.separator)
```
### Working with a File Object

![Alt text](https://github.com/frhan/study/blob/master/images/Screen%20Shot%202019-02-19%20at%202.55.54%20PM.png)

# Introducing Streams

### Stream Fundamentals
The contents of a file may be accessed or written via a _stream_, which is a list of data elements presented sequentially.

### Stream Nomenclature
#### Byte Streams vs. Character Streams
The `java.io` API defines two sets of classes for reading and writing streams: those with Stream in their name and those with `Reader`/`Writer` in their name.

Differences between Streams and `Readers`/`Writers`:
1. The stream classes are used for inputting and outputting all types of binary or byte data.
2. The reader and writer classes are used for inputting and outputting only character and String data.

why use Character streams?
- There are advantages, though, to using the reader/writer classes, as they are specifically focused on managing character and string data
- the character stream classes are sometimes referred to as convenience classes for working with text data.

### Input and Output
- Most Input stream classes have a corresponding Output class and vice versa. For example, the `FileOutputStream` class writes data that can be read by a `FileInputStream`.

### Low-Level vs. High-Level Streams

- A low-level stream connects directly with the source of the data, such as a file, an array, or a String.
For example, a `FileInputStream` is a class that reads file data one byte at a time.
- a high-level stream is built on top of another stream using wrapping. Wrapping is the process by which an instance is passed to the constructor of another class and operations on the resulting instance are filtered and applied to the original instance.

```java
try (
BufferedReader bufferedReader = new BufferedReader(
new FileReader("zoo-data.txt"))) { 
    System.out.println(bufferedReader.readLine());
}
```
* High-level streams can take other high-level streams as input

```java
try ( ObjectInputStream objectStream = new ObjectInputStream( new BufferedInputStrem(new FileInputStream("zoo-data.txt")))) {
     System.out.println(objectStream.readObject());
}
```

#### Stream Base Classes
The `java.io` library defines four abstract classes that are the parents of all stream classes defined within the API: 
`InputStream`, `OutputStream`, `Reader`, and `Writer`.

```java
new BufferedInputStream(new FileReader("zoo-data.txt")); // DOES NOT COMPILE 
new BufferedWriter(new FileOutputStream("zoo-data.txt")); // DOES NOT COMPILE 
new ObjectInputStream(new FileOutputStream("zoo-data.txt")); // DOES NOT COMPILE 
new BufferedInputStream(new InputStream()); // DOES NOT COMPILE
```

#### Decoding Java I/O Class Names

Review of java.io Class Properties:
* A class with the word `InputStream` or `OutputStream` in its name is used for reading or writing binary data, respectively.

* A class with the word `Reader` or `Writer` in its name is used for reading or writing character or string data, respectively.

* Most, but not all, input classes have a corresponding output class.
* A low-level stream connects directly with the source of the data.
* A high-level stream is built on top of another stream using wrapping.
* A class with Buffered in its name reads or writes data in groups of bytes or characters and often improves performance in sequential file systems.

### Common Stream Operations
#### Closing the Stream
* Since streams are considered resources, it is imperative that they be closed after they
are used lest they lead to resource leaks.
* In a file system, failing to close a file properly could leave it locked by the operating system such that no other processes could read/write to it until the program is terminated

#### Flushing the Stream

* When data is written to an OutputStream, the underlying operating system does not necessarily guarantee that the data will make it to the file immediately. In many operating systems, the data may be cached in memory, with a write occurring only after a temporary cache is filled or after some amount of time has passed.

* Java provides a flush() method, which requests that all accumulated data be written immediately to disk.
The flush() method helps reduce the amount of data lost if the application terminates unexpectedly. It is not without cost, though.

#### Marking the Stream
* The `InputStream` and `Reader` classes include `mark(int)` and `reset()` methods to move the stream back to an earlier position
* call the `markSupported()` method, which returns true only if `mark()` is supported.
* trying to call `mark(int)` or `reset()` on a class that does not support these operations will throw an exception at runtime
*  If at any point you want to go back to the earlier position where you last called `mark()`, then you just call `reset()` and the stream will “revert” to an earlier state.
* you should not call the mark() operation with too large a value as this could take up a lot of memory.

```java
InputStream is = ... 
System.out.print ((char)is.read()); 
if(is.markSupported()) {
    is.mark(100); 
    System.out.print((char)is.read()); 
    System.out.print((char)is.read()); 
    is.reset();
} 

System.out.print((char)is.read()); 
System.out.print((char)is.read()); 
System.out.print((char)is.read());
```
* Finally, if you call `reset()` after you have passed your `mark()` read limit, an exception may be thrown at runtime since the marked position may become invalidated.
#### Skipping over Data
The `InputStream` and `Reader` classes also include a `skip(long)` method, which as you might expect skips over a certain number of bytes. It returns a `long` value, which indicates the number of `bytes` that were actually skipped.

# Working with **Streams**
### The FileInputStream and FileOutputStream Classes
 * `FileInputStream` and `FileOutputStream`-  They are used to read bytes from a file or write bytes to a file, respectively. These classes include constructors that take a `File` object or `String`, representing a path to the file.

* The data in a `FileInputStream` object is commonly accessed by successive calls to the `read()` method until a value of `-1` is returned, indicating that the end of the stream—in this case the end of the file—has been reached.

* A `FileOutputStream` object is accessed by writing successive bytes using the `write(int)` method.

```java
import java.io.*;
public class CopyFileSample {
public static void copy(File source, File destination) throws IOException {
    try (InputStream in = new FileInputStream(source); 
        OutputStream out = new FileOutputStream(destination)) { 
            int b;
            while((b = in.read()) != -1) {
                out.write(b); 
            }
    } 
}
public static void main(String[] args) throws IOException { 
    File source = new File("Zoo.class");
    File destination = new File("ZooCopy.class"); copy(source,destination);
} 
}
```
* If the destination file already exists, it will be overridden by this code.
### The _BufferedInputStream_ and _BufferedOutputStream_ Classes
* Instead of reading the data one byte at a time, we use the underlying `read(byte[])` method of `BufferedInputStream`, which returns the number of bytes read into the provided byte array.
* The data is written into the `BufferedOutputStream` using the `write(byte[],int,int)` method, which takes as input a byte array, an offset, and a length value, respectively. 
    
    - The offset value is the number of values to skip before writing characters, and it is often set to 0. The length value is the number of characters from the byte array to write.

```java
import java.io.*;

public class CopyBufferFileSample {
    public static void copy(File source, File destination) throws IOException {
        try (
        InputStream in = new BufferedInputStream(new FileInputStream(source)); 
        OutputStream out = new BufferedOutputStream(new FileOutputStream(destinatio)){
            byte[] buffer = new byte[1024];
            int lengthRead;
            while ((lengthRead = in.read(buffer)) > 0) {
                out.write(buffer,0,lengthRead);
                out.flush(); }
        } 
    }
}
```
#### why use the buffered Classes?
- The `Buffered` classes contain numerous performance enhancements for managing stream data in memory.

#### buffer size Tuning
It is also common to choose a power of 2 for the buffer size, since most underlying hardware is structured with file block and cache sizes that are a power of 2.

### The FileReader and FileWriter classes

- Like the `FileInputStream` and `FileOutputStream` classes, the FileReader and FileWriter classes contain read() and write() methods, respectively. 

These methods read/write char values instead of byte values; although similar to what you saw with streams, the API actually uses an int value to hold the data so that -1 can be returned if the end of the file is detected.
### The BufferedReader and BufferedWriter Classes
- Unlike `FileInputStream` and `FileReader`, where we used -1 to check for file termination of an int value, with `BufferedReader`, we stop reading the file when `readLine()` returns `null`.

### The ObjectInputStream and ObjectOutputStream Classes
- The process of converting an in-memory object to a stored data format is referred to as _serialization_, with the reciprocal process of converting stored data into an object, which is known as _deserialization_.

#### The Serializable Interface


### The PrintStream and PrintWriter Classes
### Review of Stream Classes 

Interacting with Users
---
### The Old Way
### The New Way
