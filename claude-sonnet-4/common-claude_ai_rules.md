# AI Code Assistant Rules - Common

## Code Quality Standards

### Security First
- Never hardcode secrets, passwords, API keys, or sensitive data in code
- Always use environment variables or secure secret management systems
- Flag potential security vulnerabilities (SQL injection, XSS, insecure dependencies)
- Suggest secure coding practices for authentication and authorization
- Recommend input validation and sanitization

### Documentation and Comments
- Generate meaningful comments for complex business logic
- Create comprehensive docstrings/javadocs for public methods and classes
- Suggest descriptive variable and function names over clever abbreviations
- Include usage examples in documentation when helpful
- Avoid obvious comments that simply restate what the code does

### Error Handling
- Always implement proper error handling with specific exception types
- Include meaningful error messages that help with debugging
- Log errors with appropriate severity levels
- Suggest graceful degradation patterns where applicable
- Never suppress exceptions without proper justification

## Development Practices

### Testing
- Suggest unit tests for new functions and methods
- Recommend integration tests for database interactions and external APIs
- Generate test data that covers edge cases and boundary conditions
- Follow the test pyramid principle (unit > integration > e2e)
- Use descriptive test names that explain the scenario being tested

### Code Structure
- Follow SOLID principles and recommend refactoring when violations are detected
- Suggest breaking down large functions into smaller, focused methods
- Recommend appropriate design patterns when they solve actual problems
- Avoid over-engineering simple solutions
- Prioritize readability and maintainability over cleverness

### Database Interactions
- Always use parameterized queries to prevent SQL injection
- Suggest appropriate indexing strategies for query performance
- Recommend connection pooling and proper resource management
- Use database transactions appropriately for data consistency
- Validate data before database operations

## Environment and Configuration

### Environment Management
- Separate configuration from code using environment variables
- Support multiple environments (dev, test, staging, prod)
- Never commit environment-specific configuration to version control
- Use configuration validation to fail fast on startup
- Document all required environment variables

### Dependencies
- Keep dependencies up to date but avoid unnecessary upgrades
- Suggest removing unused dependencies
- Use specific version pinning for production deployments
- Document reasons for dependency choices
- Prefer well-maintained, popular libraries over custom implementations

## Code Review and Collaboration

### Version Control
- Write clear, descriptive commit messages
- Suggest appropriate branch naming conventions
- Recommend atomic commits that represent single logical changes
- Include relevant issue/ticket numbers in commit messages
- Avoid committing debugging code, commented-out code, or temporary files

### Performance Considerations
- Identify potential performance bottlenecks
- Suggest efficient algorithms and data structures
- Recommend caching strategies where appropriate
- Flag N+1 query problems in database interactions
- Consider memory usage and garbage collection impact

## Compliance and Standards

### Code Formatting
- Follow established team formatting standards consistently
- Use automated formatting tools (configured in pre-commit hooks)
- Maintain consistent indentation and spacing
- Organize imports and remove unused imports
- Follow language-specific naming conventions

### Logging and Monitoring
- Use structured logging with appropriate log levels
- Include correlation IDs for tracing across services
- Log important business events and errors
- Avoid logging sensitive information
- Use consistent log message formats across the application