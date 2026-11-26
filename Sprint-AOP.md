# Spring AOP – Quick Revision Notes

## 1. What is AOP?

**AOP (Aspect-Oriented Programming)** is a programming approach used to separate common functionalities from the main business logic.

In simple words, AOP allows us to execute common code without writing it repeatedly in multiple methods.

Examples:
- Logging
- Security checks
- Transaction management
- Performance monitoring
- Exception tracking

## 2. Why do we use AOP?

Without AOP, we may need to write the same logging or security code in multiple methods.

With AOP, we write common logic once and apply it wherever required.

**Benefits:**
- Code reusability.
- Less code duplication.
- Separation of concerns.
- Cleaner business logic.
- Easier maintenance.

## 3. Important AOP Terminology

| Term | Meaning |
|---|---|
| Aspect | Class containing common functionality |
| Advice | Action executed at a particular point |
| Join Point | A point during program execution where advice can run |
| Pointcut | Expression that selects where advice should execute |
| Target Object | Object whose method is being intercepted |
| Weaving | Process of applying aspects to target objects |
| Proxy | Object that intercepts method calls and applies advice |

## 4. Types of Advice

| Advice | When does it execute? |
|---|---|
| `@Before` | Before method execution |
| `@After` | After method execution, whether successful or exceptional |
| `@AfterReturning` | After successful method execution |
| `@AfterThrowing` | When method throws an exception |
| `@Around` | Before and after method execution; can control execution |

**Remember:** `@Around` is the most flexible advice because it can control whether the target method executes.

## 5. How to Implement Spring AOP?

### Step 1: Add Dependency

For Maven:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### Step 2: Create an Aspect Class

```java
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore() {
        System.out.println("Method execution started");
    }

    @After("execution(* com.example.service.*.*(..))")
    public void logAfter() {
        System.out.println("Method execution completed");
    }
}
```

### Step 3: Create a Service Class

```java
import org.springframework.stereotype.Service;

@Service
public class UserService {

    public void getUser() {
        System.out.println("Fetching user details");
    }
}
```

Whenever `getUser()` is called through the Spring-managed proxy, the logging advice executes automatically.

## 6. Understanding Pointcut Expression

Example:

```java
execution(* com.example.service.*.*(..))
```

Meaning:

- `execution` → Selects method execution.
- First `*` → Any return type.
- `com.example.service` → Package name.
- Second `*` → Any class in that package.
- Third `*` → Any method.
- `(..)` → Any number and type of arguments.

## 7. AOP vs OOP

| AOP | OOP |
|---|---|
| Focuses on cross-cutting concerns | Focuses on objects |
| Separates common functionalities | Organizes code into classes and objects |
| Uses Aspects and Advice | Uses Classes and Objects |
| Helps reduce code duplication | Helps structure application logic |

## 8. Common Use Cases

- Logging method execution.
- Measuring API execution time.
- Transaction management.
- Security and authorization.
- Exception logging.
- Auditing user activities.

## 9. Quick Revision

Remember these important terms:

- AOP = Aspect-Oriented Programming.
- Aspect = Common functionality.
- Advice = Action to execute.
- Join Point = Method execution point.
- Pointcut = Selects execution points.
- Weaving = Applying aspects.
- Proxy = Intercepts method calls.

Remember five advice types:

`@Before` → Before execution.

`@After` → After execution.

`@AfterReturning` → After successful execution.

`@AfterThrowing` → When exception occurs.

`@Around` → Before and after execution.

**Interview One-Liner:**

Spring AOP is used to separate cross-cutting concerns such as logging, security, and transaction management from the main business logic.


# Spring AOP – Important Interview Questions

### Q1. What is AOP in Spring?

AOP stands for Aspect-Oriented Programming. It separates common functionalities like logging, security, and transaction management from the main business logic.

### Q2. What are Cross-Cutting Concerns?

Cross-cutting concerns are functionalities used across multiple parts of an application.

Examples:
- Logging
- Security
- Transaction management
- Auditing

### Q3. What is an Aspect?

An Aspect is a class that contains common functionality that needs to be applied to multiple methods or classes.

Example: LoggingAspect.

### Q4. What is Advice in Spring AOP?

Advice is the action performed by an Aspect at a particular point during method execution.

Types:
- Before
- After
- AfterReturning
- AfterThrowing
- Around

### Q5. What is a Join Point?

A Join Point is a point during program execution where advice can be applied.

In Spring AOP, a Join Point represents a method execution.

### Q6. What is a Pointcut?

A Pointcut is an expression used to select which methods should execute the defined advice.

Example:

```java
execution(* com.example.service.*.*(..))
```

### Q7. Difference between @Before and @After?

- `@Before` executes before the target method.
- `@After` executes after the target method, whether it completes successfully or throws an exception.

### Q8. Difference between @AfterReturning and @AfterThrowing?

- `@AfterReturning` executes when the method completes successfully.
- `@AfterThrowing` executes when the method throws an exception.

### Q9. What is @Around Advice?

`@Around` executes before and after the target method.

It can also control whether the target method executes using `ProceedingJoinPoint.proceed()`.

### Q10. What is a Proxy in Spring AOP?

A Proxy is an object that acts as an intermediary between the caller and the actual target object.

It intercepts method calls and applies the required advice.

### Q11. What is Weaving?

Weaving is the process of applying an Aspect to a target object.

Spring AOP generally performs this through runtime proxies.

### Q12. What is the difference between AOP and OOP?

- OOP focuses on objects and business functionality.
- AOP focuses on separating common functionalities across multiple classes.

### Q13. Does Spring AOP work on private methods?

No. Spring's proxy-based AOP does not intercept private method execution because private methods cannot be overridden by the proxy.

### Q14. What happens if one method calls another method within the same class?

This is called self-invocation.

Spring's proxy-based AOP generally does not intercept such internal calls because they bypass the proxy.

### Q15. What is the difference between JDK Dynamic Proxy and CGLIB Proxy?

- JDK Dynamic Proxy uses interfaces to create proxies.
- CGLIB creates proxies by extending classes.
- Spring can use either approach depending on the configuration and target class.

---

## Most Important Questions for Interview

Focus especially on these:

1. What is Spring AOP and why do we use it?
2. What are Cross-Cutting Concerns?
3. What is an Aspect?
4. What is Advice? Explain its five types.
5. Difference between Join Point and Pointcut.
6. What is a Proxy in Spring AOP?
7. What is Weaving?
8. Difference between `@Before` and `@After`.
9. What is `@Around` Advice?
10. What is Self-Invocation in Spring AOP?

**Interview Tip:** Be prepared to explain a practical logging example using `@Aspect`, `@Before`, and `@After`. Also understand why Spring AOP uses proxies.
