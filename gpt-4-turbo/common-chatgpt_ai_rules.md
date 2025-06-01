# common.mdc

# Common AI Assistant Rules for All Projects

## General Coding Guidelines
- Always prefer readable, maintainable code over clever one-liners.
- Follow consistent naming conventions: camelCase for variables, PascalCase for classes, snake_case for Python.
- Add docstrings or comments when code is non-obvious.
- Group related functionality into well-named modules or packages.

## Environment & Dependencies
- Do not assume global installations. Use virtual environments or container-based solutions.
- Respect `.env` files for secrets or configuration. Never hardcode secrets or tokens.
- Recommend Docker + docker-compose for environment setup, with separate containers for app, DB, cache, etc.

## Testing
- Include unit tests for every core module or function.
- Prefer behavior-driven and readable test names (`shouldReturnValidUser_whenInputIsValid`).
- Use mocks where appropriate, but favor integration tests when feasible.

## Git Practices
- Follow trunk-based development or short-lived feature branches.
- Recommend conventional commit messages (`feat:`, `fix:`, `test:`).
- Never generate secrets, passwords, or tokens directly in commits or code.

## Linting and Formatters
- Always use formatters and linters appropriate to the language.
- If code violates linter rules, suggest fixes or automated tools.

## Comments and Docs
- When explaining code, prefer inline comments only when the intent is not clear from the code.
- Recommend maintaining `README.md`, `.env.example`, and usage docs.

## CI/CD Awareness
- Avoid assistant behavior that assumes manual steps. Recommend automatable workflows.
- Promote test/lint/build on pull request or commit via CI.

## Tone and Output Rules
- Do not generate unnecessary boilerplate unless asked.
- Only generate code relevant to the current task.
- Use markdown for code blocks, and clearly label file names where applicable.

---

