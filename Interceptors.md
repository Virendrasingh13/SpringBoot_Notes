# Spring Boot Interceptors – Quick Revision Notes

## 1. What is an Interceptor?

**Interceptor** is a component of Spring MVC used to intercept HTTP requests before they reach the Controller and process them after Controller execution.

- Works after DispatcherServlet selects a handler.
- Can perform checks before Controller execution.
- Used for authentication checks, logging, request monitoring, etc.

## 2. Interceptor Flow

Client → DispatcherServlet → Interceptor → Controller → Response

## 3. Three Important Methods

Interceptors implement the `HandlerInterceptor` interface.

| Method | Purpose |
|---|---|
| `preHandle()` | Executes before Controller |
| `postHandle()` | Executes after Controller, before response rendering |
| `afterCompletion()` | Executes after request processing completes |

## 4. Important Return Value

- `return true` → Allows request to continue to Controller.
- `return false` → Stops normal request processing.

## 5. How to Create an Interceptor?

### Step 1: Implement HandlerInterceptor

```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

@Component
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(
            HttpServletRequest request,
            HttpServletResponse response,
            Object handler) {

        System.out.println("Before Controller Execution");
        return true;
    }

    @Override
    public void postHandle(
            HttpServletRequest request,
            HttpServletResponse response,
            Object handler,
            ModelAndView modelAndView) {

        System.out.println("After Controller Execution");
    }

    @Override
    public void afterCompletion(
            HttpServletRequest request,
            HttpServletResponse response,
            Object handler,
            Exception ex) {

        System.out.println("Request Processing Completed");
    }
}
```

### Step 2: Register Interceptor

Create `WebConfig.java`:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    private final MyInterceptor myInterceptor;

    public WebConfig(MyInterceptor myInterceptor) {
        this.myInterceptor = myInterceptor;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor);
    }
}
```

## 6. Common Use Cases

- Authentication and authorization checks.
- Request and response logging.
- API execution time tracking.
- Request monitoring.
- Checking user session.

## 7. Filter vs Interceptor

| Filter | Interceptor |
|---|---|
| Part of Servlet API | Part of Spring MVC |
| Executes before DispatcherServlet | Executes after handler selection |
| Uses `doFilter()` | Uses `preHandle()`, `postHandle()`, etc. |
| Works at Servlet level | Works at Controller level |

## 8. Interview One-Liner

**Interceptor:** A Spring MVC component used to intercept HTTP requests before and after Controller execution.

## 9. Quick Revision

Remember these three methods:

- `preHandle()` → Before Controller.
- `postHandle()` → After Controller, before response rendering.
- `afterCompletion()` → After request processing completes.

**Important:**
- `return true` → Continue request.
- `return false` → Stop normal request processing.

**Remember:** Filter works at Servlet level, whereas Interceptor works at Spring MVC level.
