# Spring Boot Filters – Quick Revision Notes

## 1. What is a Filter?

**Filter** is a component of the Java Servlet API used to intercept and process HTTP requests and responses before they reach the Servlet or after they leave it.

- Works before the DispatcherServlet.
- Can modify requests and responses.
- Used for authentication, logging, CORS, etc.

## 2. Filter Flow

Client → Filter → DispatcherServlet → Controller → Response

## 3. Important Methods

Filters implement the `Filter` interface.

| Method | Purpose |
|---|---|
| `init()` | Initializes the Filter |
| `doFilter()` | Processes incoming requests and outgoing responses |
| `destroy()` | Cleans up resources when Filter is removed |

## 4. What is FilterChain?

- `FilterChain` allows a request to move to the next Filter or Servlet.
- `chain.doFilter(request, response)` passes control forward.
- If `chain.doFilter()` is not called, the request does not continue through the normal chain.

## 5. How to Create a Filter?

### Step 1: Implement Filter Interface

```java
import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter(urlPatterns = "/*")
public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) {
        System.out.println("Filter Initialized");
    }

    @Override
    public void doFilter(ServletRequest request,
                         ServletResponse response,
                         FilterChain chain)
            throws IOException, ServletException {

        System.out.println("Before Request Processing");

        chain.doFilter(request, response);

        System.out.println("After Request Processing");
    }

    @Override
    public void destroy() {
        System.out.println("Filter Destroyed");
    }
}
```

**Note:** For `@WebFilter` to work in Spring Boot, enable servlet component scanning using `@ServletComponentScan`. Alternatively, register the Filter using `@Component` or `FilterRegistrationBean`.

## 6. Common Use Cases

- Authentication and security checks.
- Request and response logging.
- CORS handling.
- Request/response modification.
- Tracking request processing time.

## 7. Filter vs Interceptor

| Filter | Interceptor |
|---|---|
| Part of Servlet API | Part of Spring MVC |
| Executes before DispatcherServlet | Executes after handler selection |
| Uses `doFilter()` | Uses `preHandle()`, `postHandle()`, etc. |
| Works at Servlet level | Works at Controller level |

## 8. Interview One-Liner

**Filter:** A Java Servlet API component used to intercept and process HTTP requests and responses before they reach the Servlet or after they leave it.

## 9. Quick Revision

Remember these three methods:

- `init()` → Initializes Filter.
- `doFilter()` → Processes request and response.
- `destroy()` → Cleans up Filter resources.

**Important:** `chain.doFilter()` passes the request to the next Filter or Servlet.

**Remember:** Filter works at the Servlet level, not specifically at the Controller level.
