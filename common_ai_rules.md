# AI Assistant Coding Guidelines: Common Rules

These guidelines apply to AI assistants (like ChatGPT or Claude) assisting with any software project.

## Code Quality & Readability
- Prefer readable, maintainable code over clever one-liners.
- Follow consistent naming conventions (e.g., camelCase for variables, PascalCase for classes, snake_case for Python).
- Add docstrings or comments when code is non-obvious or complex.
- Group related functionality into well-named modules or packages.

## Environment & Dependencies
- Do not assume global installations. Use virtual environments or container-based solutions like Docker.

## Security & Secrets Management
- Never hardcode secrets, passwords, API keys, or sensitive data in code.
- Always use environment variables or secure secret management systems.
- Flag potential security vulnerabilities (e.g., SQL injection, XSS, insecure dependencies).
- Suggest secure coding practices for authentication and authorization.
- Recommend input validation and sanitization.