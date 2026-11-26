# JDBC vs Spring JDBC vs JPA vs Hibernate

## 1. Introduction

When developing a Java Spring Boot application, we often need to store, retrieve, update, and delete data from a database.

For example, imagine we are developing a Bank Management System.

We need to:
- Create a bank account.
- Store customer details.
- Retrieve account information.
- Update account balance.
- Delete an account.

Java needs a way to communicate with the database. This is where JDBC, Spring JDBC, JPA, and Hibernate come into the picture.

Although they are related, they are not the same.

---

## 2. JDBC (Java Database Connectivity)

### What is JDBC?

JDBC stands for **Java Database Connectivity**.

It is a standard Java API that allows Java applications to communicate directly with databases.

Using JDBC, we can execute SQL queries from Java code.

### Simple example

Suppose we want to fetch a customer from the database.

Using JDBC, we need to:
1. Establish a database connection.
2. Create a SQL statement.
3. Execute the SQL query.
4. Read the result.
5. Close the resources.

### JDBC code example

```java
String sql = "SELECT * FROM users WHERE id = ?";

try (
    Connection con = DriverManager.getConnection(
        "jdbc:mysql://localhost:3306/bank",
        "root",
        "password"
    );

    PreparedStatement ps = con.prepareStatement(sql)
) {
    ps.setInt(1, 101);

    ResultSet rs = ps.executeQuery();

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }
}
```

### Important JDBC components

| Component | Purpose |
|---|---|
| DriverManager | Establishes database connections |
| Connection | Represents a database connection |
| Statement | Executes SQL queries |
| PreparedStatement | Executes parameterized SQL queries |
| ResultSet | Stores query results |
| SQLException | Handles database-related errors |

### Advantages of JDBC

- Direct control over SQL queries.
- Good performance for many database operations.
- Supports transactions.
- Works with different databases through JDBC drivers.

### Disadvantages of JDBC

- Requires writing repetitive code.
- We need to manually map database results to Java objects.
- Resource management requires attention.
- Large applications can become difficult to maintain.

**In simple words:** JDBC allows Java to communicate directly with the database, but we have to write most of the database-related code ourselves.

---

## 3. Spring JDBC

### What is Spring JDBC?

Spring JDBC is a module provided by the Spring Framework that simplifies traditional JDBC operations.

In normal JDBC, we need to manage connections, statements, result sets, and exceptions ourselves.

Spring JDBC handles much of this repetitive work for us.

Its most commonly used class is **JdbcTemplate**.

### What is JdbcTemplate?

`JdbcTemplate` is a Spring class that makes database operations easier.

It automatically handles:
- Opening and closing database resources.
- Creating and executing statements.
- Processing database exceptions.
- Managing JDBC resource cleanup.

We mainly focus on writing SQL queries and processing the results.

### JDBC vs Spring JDBC example

**Traditional JDBC:**

```java
Connection con = DriverManager.getConnection(url, user, password);

PreparedStatement ps = con.prepareStatement(
    "SELECT * FROM users WHERE id = ?"
);

ps.setInt(1, 101);

ResultSet rs = ps.executeQuery();
```

**Spring JDBC:**

```java
String sql = "SELECT * FROM users WHERE id = ?";

User user = jdbcTemplate.queryForObject(
    sql,
    (rs, rowNum) -> new User(
        rs.getInt("id"),
        rs.getString("name")
    ),
    101
);
```

Notice that Spring JDBC requires less boilerplate code.

### Common JdbcTemplate methods

| Method | Purpose |
|---|---|
| `query()` | Fetch multiple records |
| `queryForObject()` | Fetch a single result |
| `update()` | Insert, update, or delete records |
| `batchUpdate()` | Execute multiple SQL operations in a batch |
| `execute()` | Execute SQL statements |

### Example: Insert a user

```java
String sql = "INSERT INTO users(name, email) VALUES (?, ?)";

jdbcTemplate.update(
    sql,
    "Virendra",
    "virendra@gmail.com"
);
```

### Advantages of Spring JDBC

- Less boilerplate code than traditional JDBC.
- Automatic resource management.
- Simplified exception handling.
- Full control over SQL queries.
- Easy integration with Spring Boot.

### Disadvantages of Spring JDBC

- We still need to write SQL queries.
- We generally need to map query results to Java objects.
- It does not provide full ORM functionality.

**In simple words:** Spring JDBC is JDBC made easier by Spring. We still write SQL, but Spring handles much of the repetitive work.

---

## 4. JPA (Java Persistence API / Jakarta Persistence)

### What is JPA?

JPA stands for **Java Persistence API**. It is now known as Jakarta Persistence.

JPA is a specification that defines how Java objects should be stored, retrieved, updated, and deleted in a relational database.

**Important:** JPA is not a framework or a database. It is a set of rules and interfaces that ORM implementations follow.

### First understand ORM

ORM stands for **Object Relational Mapping**.

It is a technique that maps Java objects to database tables.

For example:

A Java class:

```java
public class User {

    private int id;
    private String name;
    private String email;

}
```

Can be mapped to a database table:

| id | name | email |
|---|---|---|
| 1 | Virendra | virendra@gmail.com |
| 2 | Rahul | rahul@gmail.com |

Here:
- Java class → Database table
- Java object → Database row
- Java field → Database column

Instead of manually writing SQL for every operation, we can work with Java objects through JPA.

### Important JPA annotations

| Annotation | Purpose |
|---|---|
| `@Entity` | Marks a class as a database entity |
| `@Table` | Specifies the database table name |
| `@Id` | Defines the primary key |
| `@GeneratedValue` | Configures primary key generation |
| `@Column` | Maps a field to a database column |
| `@OneToOne` | Defines a one-to-one relationship |
| `@OneToMany` | Defines a one-to-many relationship |
| `@ManyToOne` | Defines a many-to-one relationship |
| `@ManyToMany` | Defines a many-to-many relationship |

### Example: Entity class

```java
import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    // Getters and setters
}
```

Here:
- `@Entity` tells JPA that this class represents a database entity.
- `@Table` specifies the table name.
- `@Id` defines the primary key.
- `@GeneratedValue` tells JPA how the primary key should be generated.

### What is EntityManager?

`EntityManager` is an important JPA interface used to manage entity objects.

It provides methods such as:

| Method | Purpose |
|---|---|
| `persist()` | Saves a new entity |
| `find()` | Finds an entity by primary key |
| `merge()` | Merges changes into a managed entity |
| `remove()` | Deletes an entity |
| `createQuery()` | Creates a JPQL query |

**In simple words:** JPA defines the rules for mapping Java objects to database tables and provides standard APIs to work with those objects.

---

## 5. Hibernate

### What is Hibernate?

Hibernate is a popular Java ORM framework.

It is one of the most widely used implementations of the JPA specification.

Remember:

- JPA defines the rules.
- Hibernate implements those rules.

Hibernate takes care of converting Java object operations into SQL queries and communicating with the database.

### Example

Suppose we want to save a user.

With Hibernate through JPA, we can write:

```java
entityManager.persist(user);
```

Hibernate generates the required SQL INSERT statement and executes it against the database.

We do not need to manually write the INSERT query for this operation.

### Important Hibernate features

- ORM (Object Relational Mapping).
- Automatic SQL generation.
- First-level cache.
- Second-level cache (when configured).
- Lazy loading.
- Eager loading.
- Transaction management integration.
- HQL (Hibernate Query Language).
- Database portability.

### Important Hibernate concepts

**1. Session**

A Hibernate `Session` is used to interact with the database and manage entity objects.

**2. SessionFactory**

A `SessionFactory` creates Hibernate sessions. It is typically configured once and reused.

**3. HQL**

HQL stands for Hibernate Query Language.

It is an object-oriented query language that works with entity classes and their properties rather than directly with database table names.

**4. Caching**

Hibernate supports caching to reduce repeated database access.

**5. Lazy Loading**

Related data is loaded only when it is accessed.

**6. Eager Loading**

Related data is loaded immediately along with the main entity, depending on the query and mapping.

**In simple words:** Hibernate is a framework that implements JPA and makes it easier to work with databases using Java objects instead of writing SQL for every operation.

---

## 6. Relationship Between JDBC, Spring JDBC, JPA and Hibernate

This is the most important part.

Understand how they are connected:

```text
             Java Spring Boot Application
                        |
          --------------------------------
          |                              |
      Spring JDBC                    Spring Data JPA
          |                              |
      JdbcTemplate                  JPA Specification
          |                              |
         JDBC                        Hibernate
          |                              |
      JDBC Driver                   JDBC
          |                              |
          ----------- Database -----------
```

A few important points:

1. JDBC is the fundamental Java API for database communication.
2. Spring JDBC uses JDBC internally and simplifies its usage.
3. JPA defines a standard way to map Java objects to database tables.
4. Hibernate implements JPA and uses JDBC underneath to communicate with the database.
5. Spring Data JPA provides repository-based abstractions on top of JPA, reducing even more repetitive code.

**Remember:** Hibernate does not replace JDBC at the lowest level. It generally uses JDBC internally.

---

## 7. JDBC vs Spring JDBC vs JPA vs Hibernate

| Feature | JDBC | Spring JDBC | JPA | Hibernate |
|---|---|---|---|---|
| Type | Java API | Spring module | Specification | ORM framework |
| SQL writing | Manual | Manual | Often generated by implementation | Often generated automatically |
| ORM support | No | No | Yes, defines ORM standards | Yes |
| Boilerplate code | More | Less | Less | Less |
| Main API | Connection, Statement | JdbcTemplate | EntityManager | Session |
| Main purpose | Database connectivity | Simplify JDBC | Object persistence standard | Implement ORM |
| Uses JDBC internally | It is JDBC | Yes | Implementation uses it | Yes, generally |

### Easy way to remember

- JDBC = Write SQL and execute it.
- Spring JDBC = Write SQL with less boilerplate.
- JPA = Standard rules for ORM.
- Hibernate = Framework implementing JPA.

---

## 8. What is Spring Data JPA?

This is another very common interview question.

Spring Data JPA is a Spring project that makes working with JPA much easier.

It provides repository interfaces so that we do not need to write common database operations manually.

### Example

First, create an entity:

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

}
```

Now create a repository:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

That's it!

We automatically get methods such as:

```java
save(user);

findById(1L);

findAll();

deleteById(1L);
```

We do not need to manually implement these basic CRUD methods.

### How does it work internally?

```text
Controller
    |
Service
    |
UserRepository
    |
Spring Data JPA
    |
JPA
    |
Hibernate
    |
JDBC
    |
Database
```

Note: Hibernate is a common JPA provider, but it is not the only possible provider.

**In simple words:** Spring Data JPA reduces database-related code by providing ready-made repository methods.

---

## 9. When Should We Use Each One?

| Technology | When to use |
|---|---|
| JDBC | When you need direct SQL control and want to manage database operations yourself |
| Spring JDBC | When you want direct SQL control with less boilerplate |
| JPA | When you want a standard object-relational persistence approach |
| Hibernate | When you need ORM functionality through a JPA implementation |
| Spring Data JPA | When you want convenient repository-based database operations in Spring applications |

For most typical Spring Boot CRUD applications, Spring Data JPA with Hibernate is a common choice.

For complex SQL queries or performance-sensitive database operations, JDBC or Spring JDBC can also be useful.

---

## 10. Practical Spring Boot Example

Imagine we are building a Bank Management System.

We need to save customer information.

### Using Spring JDBC

```java
@Repository
public class UserDao {

    private final JdbcTemplate jdbcTemplate;

    public UserDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public int saveUser(String name, String email) {

        String sql = "INSERT INTO users(name, email) VALUES (?, ?)";

        return jdbcTemplate.update(sql, name, email);
    }
}
```

Here, we manually write the SQL query.

### Using Spring Data JPA

Entity:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    // Getters and setters
}
```

Repository:

```java
@Repository
public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

Service:

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User saveUser(User user) {
        return userRepository.save(user);
    }
}
```

Here, we do not write the INSERT SQL manually. Spring Data JPA calls the JPA implementation, commonly Hibernate, to perform the operation.

---

## 11. Important Interview Questions and Answers

### Q1. What is the difference between JDBC and JPA?

JDBC is a Java API used to communicate directly with databases using SQL queries.

JPA is a specification that defines how Java objects are mapped to database tables.

JDBC requires more manual coding, while JPA provides an object-oriented persistence approach through an implementation such as Hibernate.

### Q2. What is the difference between JPA and Hibernate?

JPA is a specification that defines standards for ORM.

Hibernate is an ORM framework that implements the JPA specification.

In simple words, JPA defines what should be done, and Hibernate provides how it is done.

### Q3. Is JPA a framework?

No. JPA is a specification, not a framework.

Hibernate and EclipseLink are examples of JPA implementations.

### Q4. Is Hibernate a replacement for JDBC?

No. Hibernate generally uses JDBC internally to communicate with databases.

It provides a higher-level ORM abstraction over JDBC.

### Q5. What is the difference between JDBC and Spring JDBC?

JDBC is the standard Java API for database connectivity.

Spring JDBC is a Spring module built on top of JDBC. It simplifies database operations using classes such as JdbcTemplate and handles resource cleanup and exception translation.

### Q6. What is JdbcTemplate?

JdbcTemplate is a class provided by Spring JDBC.

It simplifies database operations by handling repetitive tasks such as creating statements, processing results, and closing resources.

### Q7. What is ORM?

ORM stands for Object Relational Mapping.

It is a technique used to map Java objects to database tables and Java fields to database columns.

### Q8. What is EntityManager?

EntityManager is a JPA interface used to manage entity objects.

It provides operations such as persist, find, merge, and remove.

### Q9. What is Spring Data JPA?

Spring Data JPA is a Spring project that simplifies JPA operations using repository interfaces.

It provides ready-made CRUD methods through interfaces such as JpaRepository.

### Q10. Does Spring Data JPA use Hibernate?

By default, Spring Boot commonly uses Hibernate as the JPA implementation when the Spring Data JPA starter is included.

However, other JPA implementations can also be configured.

### Q11. What is the difference between Hibernate and Spring Data JPA?

Hibernate is an ORM framework and a JPA implementation.

Spring Data JPA provides repository abstractions that simplify working with JPA.

Spring Data JPA commonly uses Hibernate internally.

### Q12. Can we use JDBC and Hibernate in the same application?

Yes. We can use both in the same application.

For example, we can use Spring Data JPA for normal CRUD operations and JdbcTemplate for complex SQL queries or specific performance requirements.

### Q13. What is JPQL?

JPQL stands for Java Persistence Query Language.

It is an object-oriented query language defined by JPA. It works with entity names and Java entity properties rather than directly with database table and column names.

Example:

```java
SELECT u FROM User u WHERE u.name = :name
```

### Q14. What is the difference between SQL and JPQL?

SQL works directly with database tables and columns.

JPQL works with JPA entities and their properties.

### Q15. Which is better: JDBC or JPA?

It depends on the application.

JDBC provides more direct control over SQL queries.

JPA reduces manual database mapping and is useful for applications with many entity relationships and CRUD operations.

Spring Data JPA is a common choice for typical Spring Boot applications.

---

## 12. Quick Revision Notes

| Concept | Remember This |
|---|---|
| JDBC | Java API for database connectivity |
| Spring JDBC | Simplifies JDBC using JdbcTemplate |
| JPA | Specification for ORM |
| Hibernate | ORM framework and JPA implementation |
| ORM | Maps Java objects to database tables |
| Entity | Java class mapped to a database table |
| EntityManager | JPA interface to manage entities |
| JPQL | Query language for JPA entities |
| Spring Data JPA | Simplifies JPA using repositories |
| JpaRepository | Provides ready-made CRUD methods |

### Most Important Interview Lines

1. JDBC is used to connect Java applications with databases.
2. Spring JDBC simplifies traditional JDBC operations.
3. JPA is a specification, not an implementation.
4. Hibernate is a popular implementation of JPA.
5. Hibernate uses JDBC internally to communicate with the database.
6. Spring Data JPA simplifies database operations through repository interfaces.
7. JdbcTemplate belongs to Spring JDBC.
8. EntityManager belongs to JPA.
9. Session and SessionFactory are Hibernate concepts.
10. ORM maps Java objects to relational database tables.

---

## 13. Recommended Learning Sequence

Learn these topics in the following order:

1. JDBC fundamentals
2. Spring JDBC and JdbcTemplate
3. ORM concepts
4. JPA fundamentals
5. Hibernate architecture and features
6. Spring Data JPA
7. Entity relationships (One-to-One, One-to-Many, Many-to-One, Many-to-Many)
8. JPQL and native SQL queries
9. Transactions and `@Transactional`
10. Lazy loading vs eager loading

**Final takeaway:** JDBC is the foundation, Spring JDBC simplifies JDBC, JPA defines ORM standards, Hibernate implements JPA, and Spring Data JPA makes repository-based database operations easier.
