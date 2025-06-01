# AI Code Assistant Rules - Java/Spring Boot

## Spring Boot Specific Guidelines

### Dependency Injection and Configuration
- Use constructor injection over field injection for better testability
- Mark dependencies as `final` when using constructor injection
- Use `@Configuration` classes instead of XML configuration
- Leverage Spring Boot's auto-configuration, but understand what it's doing
- Use `@ConditionalOn*` annotations for conditional bean creation
```java
// Preferred
@Service
public class UserService {
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### Spring Profiles and Configuration
- Use `@Profile` annotations for environment-specific beans
- Leverage `application-{profile}.yml` for environment-specific properties
- Use `@ConfigurationProperties` for type-safe configuration binding
- Validate configuration properties with JSR-303 annotations
- Never hardcode values that might change between environments

### REST API Development
- Use appropriate HTTP status codes and methods
- Implement proper exception handling with `@ControllerAdvice`
- Use DTOs for API requests/responses, not entity objects directly
- Implement request validation with Bean Validation annotations
- Return consistent error response formats
```java
@RestController
@RequestMapping("/api/users")
@Validated
public class UserController {
    
    @PostMapping
    public ResponseEntity<UserDto> createUser(@Valid @RequestBody CreateUserRequest request) {
        // Implementation
    }
}
```

## Java Language Best Practices

### Modern Java Features
- Use record classes for simple data carriers (Java 14+)
- Leverage pattern matching and switch expressions where appropriate
- Use `var` for local variables when type is obvious
- Prefer `Optional` over null returns for methods that might not return a value
- Use streams for collection processing, but don't over-complicate simple loops

### Exception Handling
- Create custom exception classes that extend appropriate base exceptions
- Use checked exceptions for recoverable conditions, unchecked for programming errors
- Never catch and ignore exceptions without good reason
- Log exceptions with stack traces at appropriate levels
- Use try-with-resources for resource management

### Collections and Generics
- Use appropriate collection types (List, Set, Map) based on usage patterns
- Prefer immutable collections when data doesn't change
- Use generic types properly to avoid raw type warnings
- Consider using `EnumSet` and `EnumMap` for enum-based collections
- Use parallel streams judiciously, only when beneficial

## Testing Guidelines

### Unit Testing with JUnit 5
- Use descriptive test method names that explain the scenario
- Follow the Given-When-Then pattern for test structure
- Use `@ParameterizedTest` for testing multiple scenarios
- Mock external dependencies with Mockito
- Use `@TestInstance(Lifecycle.PER_CLASS)` when appropriate
```java
@DisplayName("User Service Tests")
class UserServiceTest {
    
    @Test
    @DisplayName("Should create user when valid data provided")
    void shouldCreateUserWhenValidDataProvided() {
        // Given
        CreateUserRequest request = new CreateUserRequest("john@example.com");
        
        // When
        UserDto result = userService.createUser(request);
        
        // Then
        assertThat(result.email()).isEqualTo("john@example.com");
    }
}
```

### Integration Testing
- Use `@SpringBootTest` for full application context tests
- Use `@WebMvcTest` for testing only web layer
- Use `@DataJpaTest` for repository layer testing
- Leverage TestContainers for database integration tests
- Use `@MockBean` to replace Spring beans with mocks in tests

## Data Access and JPA

### Repository Patterns
- Extend appropriate Spring Data repository interfaces
- Use custom query methods with `@Query` annotation when needed
- Implement custom repository methods for complex queries
- Use `@Transactional` appropriately on service methods, not repositories
- Consider using specifications for dynamic queries

### Entity Design
- Use `@Entity` and JPA annotations properly
- Implement `equals()` and `hashCode()` consistently for entities
- Use appropriate fetch strategies (LAZY vs EAGER)
- Map relationships correctly with proper cascade settings
- Use `@Version` for optimistic locking when needed
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();
}
```

## Build and Dependency Management

### Maven/Gradle Configuration
- Use Spring Boot's dependency management BOM
- Pin dependency versions in production builds
- Use appropriate scopes for dependencies (compile, test, provided)
- Exclude transitive dependencies that conflict
- Use properties for version management

### Code Quality Tools
- Configure Checkstyle, PMD, and SpotBugs in build
- Use JaCoCo for code coverage reporting
- Set up SonarQube integration for continuous quality monitoring
- Configure build to fail on quality gate violations
- Use Maven Enforcer or Gradle equivalents for build constraints

## Performance and Monitoring

### Application Performance
- Use `@Cacheable` for expensive operations that can be cached
- Configure connection pooling appropriately (HikariCP settings)
- Use `@Async` for non-blocking operations where appropriate
- Monitor JVM metrics and garbage collection
- Use profiling tools to identify performance bottlenecks

### Logging and Observability
- Use SLF4J with Logback for logging
- Configure structured logging for better searchability
- Use MDC (Mapped Diagnostic Context) for correlation IDs
- Implement health checks with Spring Boot Actuator
- Add custom metrics for business-specific monitoring
```java
@Service
@Slf4j
public class UserService {
    
    public UserDto createUser(CreateUserRequest request) {
        MDC.put("operation", "createUser");
        try {
            log.info("Creating user with email: {}", request.email());
            // Implementation
        } finally {
            MDC.clear();
        }
    }
}
```