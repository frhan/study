

Using Try-With-Resources
------

```java
public void newApproach(Path path1, Path path2) throws IOException { 
    
    try ( BufferedReader in = Files.newBufferedReader(path1);
          BufferedWriter out = Files.newBufferedWriter(path2)) {
          }
}
```

* The new _try-with-resources_ statement automatically closes all resources opened in the try clause. This feature is also known as automatic resource management, because Java automatically takes care of the closing.

Try-With-Resources Basics
---