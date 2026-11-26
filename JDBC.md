# JDBC (Java Database Connectivity) – Complete Notes

## 1. What is JDBC?

**JDBC (Java Database Connectivity)** is a Java API that allows Java applications to connect and interact with databases.

In simple words:
- JDBC connects Java applications with databases.
- It executes SQL queries.
- It retrieves data from databases.
- It performs INSERT, UPDATE, DELETE and SELECT operations.

**Example:** A Java application connecting to MySQL to store and retrieve user details.

## 2. Why do we use JDBC?

- To connect Java applications with databases.
- To execute SQL queries.
- To insert, update and delete records.
- To retrieve database records.
- To manage database transactions.

## 3. JDBC Architecture

Java Application → JDBC API → JDBC Driver → Database

**Explanation:**
1. Java Application sends a database request.
2. JDBC API provides classes and interfaces to communicate with the database.
3. JDBC Driver converts JDBC calls into database-specific communication.
4. Database executes the SQL query and returns the result.

## 4. Important JDBC Components

| Component | Purpose |
|---|---|
| DriverManager | Manages database drivers and establishes connections |
| Connection | Represents a connection with the database |
| Statement | Executes simple SQL queries |
| PreparedStatement | Executes parameterized SQL queries |
| CallableStatement | Executes stored procedures |
| ResultSet | Stores the result of SELECT queries |
| SQLException | Handles database-related errors |

## 5. Steps to Connect Java with Database

1. Import JDBC packages.
2. Load/register the JDBC driver (usually automatic with modern JDBC drivers).
3. Establish database connection.
4. Create Statement or PreparedStatement.
5. Execute SQL query.
6. Process the result.
7. Close database resources.

## 6. JDBC Connection Example

```java
import java.sql.*;

public class JDBCExample {

    public static void main(String[] args) {

        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";

        try {
            Connection con = DriverManager.getConnection(
                url, username, password
            );

            System.out.println("Database Connected Successfully");

            con.close();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## 7. Types of JDBC Statements

### 1. Statement
- Used to execute simple SQL queries.
- SQL query is passed directly.
- Not suitable for user-provided input because of SQL injection risks.

```java
Statement stmt = con.createStatement();

ResultSet rs = stmt.executeQuery(
    "SELECT * FROM users"
);
```

### 2. PreparedStatement
- Used to execute parameterized SQL queries.
- Supports placeholders (?) for values.
- Helps prevent SQL injection.
- Can be reused with different parameter values.

```java
PreparedStatement ps = con.prepareStatement(
    "SELECT * FROM users WHERE id = ?"
);

ps.setInt(1, 101);

ResultSet rs = ps.executeQuery();
```

### 3. CallableStatement
- Used to execute stored procedures.
- Supports IN, OUT and INOUT parameters.

```java
CallableStatement cs =
    con.prepareCall("{call getUser(?)}");

cs.setInt(1, 101);

ResultSet rs = cs.executeQuery();
```

## 8. Important JDBC Methods

| Method | Purpose |
|---|---|
| `getConnection()` | Establishes database connection |
| `createStatement()` | Creates Statement |
| `prepareStatement()` | Creates PreparedStatement |
| `prepareCall()` | Creates CallableStatement |
| `executeQuery()` | Executes SELECT queries |
| `executeUpdate()` | Executes INSERT, UPDATE and DELETE |
| `execute()` | Executes general SQL statements |
| `next()` | Moves to the next ResultSet row |
| `getString()` | Retrieves String value |
| `getInt()` | Retrieves Integer value |
| `close()` | Closes database resources |

## 9. Difference Between execute(), executeQuery() and executeUpdate()

| Method | Purpose | Return Type |
|---|---|---|
| `executeQuery()` | SELECT queries | ResultSet |
| `executeUpdate()` | INSERT, UPDATE, DELETE | int (affected rows) |
| `execute()` | General SQL execution | boolean |

## 10. What is ResultSet?

**ResultSet** is an object that stores the data returned by a SELECT query.

Example:

```java
ResultSet rs = stmt.executeQuery(
    "SELECT * FROM users"
);

while (rs.next()) {

    int id = rs.getInt("id");
    String name = rs.getString("name");

    System.out.println(id + " " + name);
}
```

**Important:** `rs.next()` moves the cursor to the next row.

## 11. What is JDBC Transaction Management?

A transaction is a group of database operations treated as a single unit.

Important methods:

| Method | Purpose |
|---|---|
| `setAutoCommit(false)` | Disables automatic commit |
| `commit()` | Saves transaction changes |
| `rollback()` | Reverts uncommitted changes |

Example:

```java
try {
    con.setAutoCommit(false);

    // Execute SQL operations

    con.commit();

} catch (SQLException e) {

    con.rollback();

} finally {

    con.setAutoCommit(true);
}
```

## 12. What is SQL Injection?

SQL Injection is a security vulnerability where an attacker manipulates SQL queries using malicious input.

**Prevention:** Use PreparedStatement instead of concatenating user input directly into SQL queries.

## 13. JDBC Best Practices

- Prefer PreparedStatement for parameterized queries.
- Always close database resources.
- Use try-with-resources.
- Handle SQLException properly.
- Never hardcode database credentials in production.
- Use transactions when multiple operations must succeed together.
- Validate user input.

## 14. Quick Revision

- JDBC = Java Database Connectivity.
- DriverManager = Establishes database connection.
- Connection = Database connection.
- Statement = Executes SQL.
- PreparedStatement = Executes parameterized SQL.
- CallableStatement = Executes stored procedures.
- ResultSet = Holds SELECT query results.
- executeQuery() = SELECT.
- executeUpdate() = INSERT, UPDATE, DELETE.
- commit() = Saves transaction.
- rollback() = Reverts uncommitted changes.

**Interview One-Liner:**

JDBC is a Java API that allows Java applications to connect with databases and perform CRUD operations using SQL queries.

# JDBC – Important Interview Questions & Answers

## Basic JDBC Questions

### Q1. What is JDBC?

JDBC stands for Java Database Connectivity. It is a Java API used to connect Java applications with databases and perform database operations like INSERT, UPDATE, DELETE and SELECT.

### Q2. Why do we use JDBC?

We use JDBC to establish database connections, execute SQL queries, retrieve records and manage database transactions from Java applications.

### Q3. What are the main components of JDBC?

The main components are:

- DriverManager
- Connection
- Statement
- PreparedStatement
- CallableStatement
- ResultSet

### Q4. What is JDBC Driver?

A JDBC Driver is software that enables Java applications to communicate with a particular database.

Example: MySQL JDBC Driver.

### Q5. What is DriverManager?

DriverManager is a JDBC class responsible for managing database drivers and establishing connections using `getConnection()`.

### Q6. What is Connection in JDBC?

Connection is an interface that represents an active connection between a Java application and a database.

### Q7. What is ResultSet?

ResultSet is an object that holds the data returned by a SELECT query. We use `next()` to move through the records.

## JDBC Statements Questions

### Q8. What are the types of Statements in JDBC?

There are three main types:

1. Statement – Executes simple SQL queries.
2. PreparedStatement – Executes parameterized SQL queries.
3. CallableStatement – Executes stored procedures.

### Q9. Difference between Statement and PreparedStatement?

| Statement | PreparedStatement |
|---|---|
| Executes SQL queries directly | Executes parameterized queries |
| Uses SQL string | Uses placeholders (?) |
| More vulnerable to SQL injection when concatenating input | Helps prevent SQL injection |
| Suitable for simple static queries | Suitable for parameterized queries |

### Q10. What is CallableStatement?

CallableStatement is used to execute stored procedures in a database. It supports IN, OUT and INOUT parameters.

### Q11. Difference between execute(), executeQuery() and executeUpdate()?

- `executeQuery()` – Used for SELECT queries.
- `executeUpdate()` – Used for INSERT, UPDATE and DELETE.
- `execute()` – Used for general SQL execution.

### Q12. What is the use of rs.next()?

`rs.next()` moves the ResultSet cursor to the next row and returns true if a row exists.

## Transaction Management Questions

### Q13. What is a Transaction in JDBC?

A transaction is a group of database operations executed as a single logical unit. Either all operations succeed or changes can be rolled back.

### Q14. What is commit()?

`commit()` permanently saves the changes made during the current transaction.

### Q15. What is rollback()?

`rollback()` reverses uncommitted changes made during the current transaction.

### Q16. What is AutoCommit?

AutoCommit is a connection setting that automatically commits each SQL statement after successful execution.

By default, AutoCommit is generally enabled.

### Q17. How do you manage transactions in JDBC?

We can manage transactions using:

- `setAutoCommit(false)`
- `commit()`
- `rollback()`

## Advanced JDBC Questions

### Q18. What is SQL Injection?

SQL Injection is a security attack where malicious SQL code is inserted through user input to manipulate database queries.

**Prevention:** Use PreparedStatement with parameterized queries.

### Q19. What is Batch Processing in JDBC?

Batch Processing allows us to execute multiple SQL statements together instead of executing them individually.

Important methods:
- `addBatch()`
- `executeBatch()`
- `clearBatch()`

### Q20. What is Connection Pooling?

Connection Pooling is a technique where database connections are created and reused instead of creating a new connection for every request.

**Benefits:**
- Improves performance.
- Reduces connection creation overhead.
- Efficiently manages database connections.

### Q21. What is the difference between JDBC and JPA?

| JDBC | JPA |
|---|---|
| Java Database Connectivity API | Java Persistence API specification |
| Uses SQL queries directly | Works with Java entities and ORM |
| More manual database handling | Simplifies database operations |
| Requires manual object mapping | Supports automatic object mapping through an implementation |

### Q22. What is try-with-resources in JDBC?

Try-with-resources automatically closes resources such as Connection, Statement and ResultSet when execution finishes.

Example:

```java
try (Connection con =
         DriverManager.getConnection(url, username, password);
     PreparedStatement ps =
         con.prepareStatement("SELECT * FROM users")) {

    ResultSet rs = ps.executeQuery();

} catch (SQLException e) {
    e.printStackTrace();
}
```

### Q23. What is the difference between JDBC and Spring JDBC?

- JDBC requires manual connection and resource management.
- Spring JDBC simplifies database operations using JdbcTemplate.
- Spring JDBC reduces boilerplate code.
- Spring JDBC handles resource cleanup and translates SQL exceptions into Spring's DataAccessException hierarchy.

### Q24. What is JdbcTemplate?

JdbcTemplate is a Spring Framework class that simplifies JDBC operations.

It handles:
- Opening and closing connections.
- Executing SQL queries.
- Managing statements and ResultSets.
- Exception translation.

Example:

```java
@Autowired
private JdbcTemplate jdbcTemplate;

public List<String> getUsers() {

    return jdbcTemplate.queryForList(
        "SELECT name FROM users",
        String.class
    );
}
```

### Q25. What are the advantages of JDBC?

- Platform-independent Java API.
- Supports multiple databases through drivers.
- Provides direct SQL control.
- Supports transaction management.
- Supports batch processing.

---

## Most Important Questions for Interviews

Focus especially on these 12 questions:

1. What is JDBC?
2. Explain JDBC architecture.
3. What are JDBC components?
4. Difference between Statement and PreparedStatement.
5. What is CallableStatement?
6. Difference between execute(), executeQuery() and executeUpdate().
7. What is ResultSet?
8. What is SQL Injection and how can we prevent it?
9. What is Transaction Management?
10. Difference between commit() and rollback().
11. What is Connection Pooling?
12. Difference between JDBC, Spring JDBC and JPA.

## Practical Interview Questions

1. How do you connect a Java application to MySQL?
2. How do you insert a record using PreparedStatement?
3. How do you retrieve multiple records using ResultSet?
4. How do you handle database exceptions?
5. How do you execute multiple queries in a single transaction?
6. How do you prevent SQL Injection?
7. How do you improve JDBC performance?

**Interview Tip:** Don't just memorize JDBC definitions. Practice connecting Java with MySQL and implementing CRUD operations using PreparedStatement and JdbcTemplate.
