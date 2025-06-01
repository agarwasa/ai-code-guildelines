# AI Assistant Rules for Java/Spring Boot Projects

## Dependency Injection and Configuration
- Use constructor-based dependency injection over field injection for better testability
- Mark dependencies as `final` when using constructor injection
- Use `@Configuration` classes instead of XML configuration
- Annotate services, repositories, controllers with appropriate Spring stereotypes (`@Service`, `@Repository`, `@Controller`)
- Use `@ConditionalOn*` annotations for conditional bean creation
- Use `@Profile` annotations for environment-specific beans

## Configuration Management
- Externalize configuration to `application.yml` files with environment-specific profiles (`application-{profile}.yml`)
- Use `@ConfigurationProperties` for grouped/type-safe configuration binding
- Use `@Value` only for simple values; avoid magic strings and hardcoded values
- Validate configuration properties with JSR-303 annotations

## Testing Practices
- Use JUnit 5 for testing framework
- Use descriptive test method names that explain the scenario
- Follow the Given-When-Then pattern for test structure
- Use `@ParameterizedTest` for testing multiple scenarios
- For integration tests, use `@SpringBootTest` with TestContainers for database testing
- Use `@WebMvcTest` for testing only web layer
- Use `@DataJpaTest` for repository layer testing
- Avoid real service calls in tests — use mocks (Mockito) or WireMock as needed
- Use `@MockBean` to replace Spring beans with mocks in tests

## Java Language Best Practices
- Use `Optional` over null returns when methods might not return a value
- Use streams and lambdas where they improve clarity, not just for conciseness
- Follow consistent Java style guidelines (Google Java Style or team standards)
- Prefer immutable collections when data doesn't change

## Entity Design
- Implement `equals()` and `hashCode()` consistently for entities

## Code Quality and Performance
- Configure code quality tools (Checkstyle, PMD, SpotBugs, JaCoCo) in build process
- Use `@Cacheable` for expensive operations that can be cached

## Logging and Observability
- Use SLF4J for logging (`LoggerFactory.getLogger(...)` or `@Slf4j` annotation)
- Log exceptions with stack traces (`logger.error("Error occurred", ex)`)
- Configure structured logging for better searchability

## Security and Secrets
- Never commit passwords, tokens, secret keys, or sensitive data to version control
- Use environment variables or external secret management systems
- For development, reference `.env` files; for production, use Vault or cloud secret managers