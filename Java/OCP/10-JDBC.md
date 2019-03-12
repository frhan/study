
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
- a `DataSource` maintains a connection pool so that you can keep reusing the same connection rather than needing to get a new one each time.

- When `Class.forName()` is used, the error about an invalid class occurs on that line and throws a `ClassNotFoundException`:

```java
    Class.forName("not.a.driver");
```

# table 10.1 

## Obtaining a Statement

`Statement stmt = conn.createStatement();`

```java
Statement stmt = conn.createStatement(ResultSet.TYPE_FORWARD_ONLY,ResultSet.CONCUR_READ_ONLY);
```
### Choosing a `ResultSet` Type
- a `ResultSet` is in `TYPE_FORWARD_ONLY` mode
- Two other modes that you can request when creating a Statement are `TYPE_SCROLL_ INSENSITIVE` and `TYPE_SCROLL_SENSITIVE`.
- The difference between `TYPE_SCROLL_INSENSITIVE` and `TYPE_SCROLL_SENSITIVE` is what happens when data changes in the actual database while you are busy scrolling.
- With `TYPE_ SCROLL_INSENSITIVE`, you have a static view of what the `ResultSet` looked like when you did the query. If the data changed in the table, you will see it as it was when you did the query. 
- With `TYPE_SCROLL_SENSITIVE`, you would see the latest data when scrolling through the `ResultSet`.

### table 10.2
### Choosing a `ResultSet` Concurrency Mode
- By default, a `ResultSet` is in `CONCUR_READ_ONLY` mode.
- It means that you can’t update the result set  
- There is one other mode that you can request when creating a Statement.it lets you modify the database through the ResultSet. It is called `CONCUR_UPDATABLE`
## table 10.3

## Executing a Statement
- statement that change the data in a table - `executeUpdate()`
- `SQL UPDATE` statement is not the only statement that uses this method.

```java
Statement stmt = conn.createStatement(); 
int result = stmt.executeUpdate("insert into species values(10, 'Deer', 3)");
```

# table 10.4
#table 10.5

## Getting Data from a ResultSet

### Reading a ResultSet

```java
Map<Integer, String> idToNameMap = new HashMap<>();
ResultSet rs = stmt.executeQuery("select id, name from species"); while(rs.next()) {
int id = rs.getInt("id");
String name = rs.getString("name"); idToNameMap.put(id, name);
}
System.out.println(idToNameMap); // {1=African Elephant, 2=Zebra}
```
# figure 10.4
- Always use an `if` statement or `while` loop when calling `rs.next()`.
- Column indexes begin with 1.

### Getting Data for a Column
# table 10.6
### Scrolling ResultSet
# figure 10.5
## Closing Database Resources
- Closing a `Connection` also closes the `Statement` and `ResultSet`. 
- Closing a `Statement` also closes the `ResultSet`.
## Dealing with Exceptions

---

## `Class DriverManager`

`static Connection	getConnection(String url)`

`static Connection getConnection(String url, Properties info)`

## `Interface Connection`
- A connection (session) with a specific database. SQL statements are executed and results are returned within the context of a connection.
