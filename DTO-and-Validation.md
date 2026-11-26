# Spring Boot DTO & Validation

## 1. What is DTO?

**DTO (Data Transfer Object)** is an object used to transfer data between different layers of an application.

- Controls what data we receive from the client.
- Controls what data we send back to the client.
- Prevents exposing unnecessary Entity fields.
- Separates API data from database structure.

**Example:**

User Entity:
- id
- name
- email
- password
- role
- createdAt

UserDTO:
- name
- email
- password

## 2. What is Validation?

**Validation** means checking whether incoming data is correct and follows predefined rules before processing it.

Examples:
- Name should not be empty.
- Email should be valid.
- Password should have at least 8 characters.
- Age should be greater than 18.

## 3. Common Validation Annotations

| Annotation | Purpose |
|---|---|
| `@NotNull` | Value should not be null |
| `@NotBlank` | Cannot be null, empty or whitespace |
| `@NotEmpty` | Cannot be null or empty |
| `@Email` | Checks email format |
| `@Size` | Defines minimum/maximum length |
| `@Min` | Defines minimum number |
| `@Max` | Defines maximum number |
| `@Pattern` | Checks regular expression |

## 4. How to Implement DTO & Validation?

### Step 1: Add Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

### Step 2: Create UserDTO

```java
public class UserDTO {

    @NotBlank(message = "Name is required")
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format")
    private String email;

    @NotBlank(message = "Password is required")
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;

    // Getters and Setters
}
```

### Step 3: Use @Valid in Controller

```java
@PostMapping("/register")
public String register(@Valid @RequestBody UserDTO userDTO) {

    return "User registered successfully";
}
```

**Important:** `@Valid` triggers validation. If validation fails, Spring MVC normally returns HTTP 400 Bad Request.

## 5. DTO vs Entity

| DTO | Entity |
|---|---|
| Transfers data | Represents database data |
| Used in API requests/responses | Used for database operations |
| Contains required API fields | Contains database-related fields |
| No database mapping required | Usually mapped using `@Entity` |

## 6. Common Use Cases

**DTO:**
- User registration
- Login requests
- API response objects
- Hiding sensitive fields

**Validation:**
- Checking email format
- Checking required fields
- Password length validation
- Checking numeric ranges

## 7. Interview One-Liners

**DTO:** A Data Transfer Object is used to transfer data between application layers without exposing the internal Entity structure.

**Validation:** Validation checks incoming data against predefined rules before processing or storing it.

**@Valid:** An annotation that triggers validation on an object based on its validation constraints.

## 8. Quick Revision

Remember this flow:

Client Request → DTO → @Valid → Controller → Service → Repository

- DTO = Data Transfer
- Entity = Database Representation
- @Valid = Triggers Validation
- @NotBlank = Cannot be empty or whitespace
- @Email = Validates email format
- @Size = Checks length
