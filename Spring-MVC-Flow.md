# Spring MVC Flow – Quick Revision Notes

## 1. What is Spring MVC?

**Spring MVC (Model-View-Controller)** is a framework used to develop web applications and REST APIs by separating application responsibilities into different components.

- Model → Handles application data.
- View → Displays data to the user.
- Controller → Handles incoming requests and coordinates processing.

## 2. What is DispatcherServlet?

**DispatcherServlet** is the Front Controller of Spring MVC.

- Receives incoming HTTP requests.
- Finds the appropriate Controller.
- Coordinates request processing.
- Handles the response returned by the Controller.

**Remember:** DispatcherServlet acts as the central entry point for Spring MVC requests.

## 3. Spring MVC Request Flow

Client → DispatcherServlet → HandlerMapping → Controller → Service → Repository → Database

Response Flow:

Database → Repository → Service → Controller → DispatcherServlet → Client

## 4. Important Components

| Component | Purpose |
|---|---|
| DispatcherServlet | Front Controller; receives requests |
| HandlerMapping | Finds the appropriate Controller handler |
| HandlerAdapter | Invokes the selected handler |
| Controller | Handles HTTP requests |
| Service | Contains business logic |
| Repository | Performs database operations |
| ViewResolver | Resolves logical view names to actual views |
| HttpMessageConverter | Converts request/response bodies, such as JSON |

## 5. Step-by-Step Spring MVC Flow

1. Client sends an HTTP request.
2. DispatcherServlet receives the request.
3. HandlerMapping identifies the appropriate Controller method.
4. HandlerAdapter invokes that method.
5. Controller calls the Service layer.
6. Service performs business logic and may call Repository.
7. Repository interacts with the database.
8. Controller returns the result.
9. DispatcherServlet processes the response.
10. Response is sent back to the client.

**Note:** In REST APIs, HttpMessageConverter converts Java objects into JSON and sends them to the client. In traditional MVC applications, ViewResolver resolves the view name.

## 6. Important Annotations

| Annotation | Purpose |
|---|---|
| `@Controller` | Defines an MVC Controller |
| `@RestController` | Controller that returns response data directly |
| `@RequestMapping` | Maps HTTP requests to handlers |
| `@GetMapping` | Handles GET requests |
| `@PostMapping` | Handles POST requests |
| `@PutMapping` | Handles PUT requests |
| `@DeleteMapping` | Handles DELETE requests |
| `@RequestBody` | Converts request body into a Java object |
| `@ResponseBody` | Writes return value directly to response body |

## 7. Spring MVC vs Spring Boot

| Spring MVC | Spring Boot |
|---|---|
| Web framework | Application development framework |
| Handles web request processing | Simplifies Spring application setup |
| Provides DispatcherServlet | Auto-configures Spring MVC when web starter is used |
| Focuses on MVC architecture | Provides auto-configuration and embedded servers |

## 8. Common Use Cases

- Developing REST APIs.
- Building web applications.
- Handling HTTP requests and responses.
- Connecting Controllers with Service and Repository layers.
- Returning JSON or HTML responses.

## 9. Interview One-Liners

**Spring MVC:** A web framework based on the Model-View-Controller design pattern used to develop web applications and REST APIs.

**DispatcherServlet:** The Front Controller of Spring MVC that receives and coordinates incoming HTTP requests.

**HandlerMapping:** Identifies which Controller handler should process a request.

**ViewResolver:** Resolves logical view names into actual views.

## 10. Quick Revision

Remember this flow:

Client → DispatcherServlet → HandlerMapping → HandlerAdapter → Controller → Service → Repository → Database

Important points:

- DispatcherServlet = Front Controller.
- HandlerMapping = Finds Controller.
- HandlerAdapter = Invokes Controller.
- Controller = Handles request.
- Service = Business logic.
- Repository = Database operations.
- ViewResolver = Resolves views.
- HttpMessageConverter = Converts Java objects and HTTP message bodies.

**Remember:** DispatcherServlet is the heart of Spring MVC request processing.
