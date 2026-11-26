# Spring Boot Bean Lifecycle – Quick Revision Notes

## 1. What is a Bean?

A **Bean** is an object that is created, managed, and maintained by the Spring IoC Container.

- Spring creates Bean objects.
- Spring manages their dependencies.
- Spring controls their lifecycle.
- Beans are commonly created using `@Component`, `@Service`, `@Repository`, `@Controller`, or `@Bean`.

## 2. What is Bean Lifecycle?

**Bean Lifecycle** refers to the complete process of creating, initializing, using, and destroying a Bean managed by the Spring Container.

In simple words:

Spring creates an object → Initializes it → Uses it → Destroys it.

## 3. Bean Lifecycle Flow

1. Bean Instantiation – Spring creates the object.
2. Dependency Injection – Spring injects required dependencies.
3. Aware Interfaces – Spring provides container-related information if implemented.
4. BeanPostProcessor (Before Initialization) – Executes before initialization callbacks.
5. Initialization – Executes initialization methods.
6. BeanPostProcessor (After Initialization) – Executes after initialization callbacks.
7. Bean Ready – Bean is ready to use.
8. Destruction – Spring executes destruction callbacks when the Bean is destroyed.

**Flow:**

Instantiation → Dependency Injection → Aware Interfaces → Before Initialization → Initialization → After Initialization → Ready to Use → Destruction

## 4. Important Lifecycle Annotations

| Annotation | Purpose |
|---|---|
| `@PostConstruct` | Executes after dependency injection |
| `@PreDestroy` | Executes before Bean destruction |
| `@Component` | Marks a class as a Spring Bean |
| `@Bean` | Declares a Bean through a configuration method |

## 5. How to Implement Bean Lifecycle?

Create a class `MyBean.java`:

```java
import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;
import org.springframework.stereotype.Component;

@Component
public class MyBean {

    public MyBean() {
        System.out.println("1. Bean Created");
    }

    @PostConstruct
    public void init() {
        System.out.println("2. Bean Initialized");
    }

    public void performTask() {
        System.out.println("3. Bean is Ready to Use");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("4. Bean Destroyed");
    }
}
```

**Expected output (simplified):**

```text
1. Bean Created
2. Bean Initialized
3. Bean is Ready to Use
4. Bean Destroyed
```

Note: Actual output depends on when the Bean is instantiated and when the application context is closed.

## 6. Bean Scopes

Bean Scope defines how many instances of a Bean Spring creates and how long they are managed.

| Scope | Purpose |
|---|---|
| `singleton` | One instance per Spring IoC Container (default) |
| `prototype` | New instance each time requested from the container |
| `request` | One instance per HTTP request |
| `session` | One instance per HTTP session |
| `application` | One instance per ServletContext |
| `websocket` | One instance per WebSocket session |

**Important:** `@PreDestroy` is automatically called for Spring-managed singleton Beans when the container shuts down. Spring does not manage the destruction lifecycle of prototype Beans after handing them to the client.

## 7. Common Use Cases

- Initializing database connections or resources.
- Loading configuration data.
- Initializing cache.
- Starting background resources.
- Closing connections and releasing resources during shutdown.

## 8. Interview One-Liners

**Bean:** An object managed by the Spring IoC Container.

**Bean Lifecycle:** The complete process of creating, initializing, using, and destroying a Spring Bean.

**@PostConstruct:** Executes after dependency injection and before the Bean is ready for use.

**@PreDestroy:** Executes before a managed Bean is destroyed.

**Bean Scope:** Defines the lifecycle and number of instances of a Spring Bean.

## 9. Quick Revision

Remember this flow:

Instantiation → Dependency Injection → Initialization → Ready to Use → Destruction

Important annotations:

- `@PostConstruct` → Initialization.
- `@PreDestroy` → Destruction.
- `@Component` → Creates a Spring-managed Bean.
- `@Bean` → Defines a Bean using a configuration method.

**Remember:** Spring IoC Container is responsible for managing the Bean Lifecycle.
