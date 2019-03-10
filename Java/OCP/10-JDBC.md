
# JDBC

## Introducing the Interfaces of JDBC
-  figure 10.2
`Driver`: Knows how to get a connection to the database

`Connection`: Knows how to communicate with the database

`Statement`: Knows how to run the SQL

`ResultSet`: Knows what was returned by a `SELECT` query

## Connecting to a Database

### Building a JDBC URL
# figure 10.3
### Getting a Database Connection
- There are two main ways to get a `Connection`: 
`DriverManager` or `DataSource`. 

    - `DriverManager` is the one covered on the exam. Do not use a `DriverManager` in code someone is paying you to write. 

    - A `DataSource` is a factory, and it has more features than `DriverManager`
    
```java
import java.sql.*;
public class TestConnect {
public static void main(String[] args) throws SQLException { 
    Connection conn = DriverManager.getConnection("jdbc:derby:zoo"); System.out.println(conn);
} 
}

```
- a `DataSource` maintains a connection pool so that you can keep reusing the same connection rather than needing to get a new one each time,
# table 10.1 

## Obtaining a Statement

## Executing a Statement

## Getting Data from a ResultSet

## Closing Database Resources

## Dealing with Exceptions
