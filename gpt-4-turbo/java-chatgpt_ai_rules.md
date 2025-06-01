# java.mdc

# AI Assistant Rules for Java / Spring Boot Projects

## Build Tools
- Prefer Gradle with Kotlin DSL (`build.gradle.kts`) over Groovy or Maven unless specified.
- Set up `application` plugin properly with `mainClass.set(...)`.

## Spring Boot Practices
- Use constructor-based dependency injection.
- Annotate services, repositories, controllers with appropriate stereotypes.
- Do not mix business logic inside controllers.

## Testing
- Use JUnit 5 with AssertJ or Hamcrest.
- For integration tests, prefer `@SpringBootTest` with Testcontainers for DBs.
- Avoid real service calls in tests — use mocks or WireMock as needed.

## Configuration
- Externalize config to `application.yml` files.
- Use `@ConfigurationProperties` for grouped configs.
- Use `@Value` only for simple values; avoid magic strings.

## Logging & Observability
- Use SLF4J for logging (`LoggerFactory.getLogger(...)`).
- Log exceptions with stack traces (`logger.error("Error occurred", ex)`).

## Security & Secrets
- Never commit passwords, tokens, or secret keys.
- For secrets in dev, reference environment variables or `.env` files.
- For real systems, recommend Vault or AWS Secrets Manager.

## Java Style
- Follow Google Java Style or consistent team rules.
- Recommend usage of `Optional` over `null` when possible.
- Use streams and lambdas where they improve clarity, not just conciseness.

---

