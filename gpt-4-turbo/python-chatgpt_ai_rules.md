# python.mdc

# AI Assistant Rules for Python Projects

## Dependency Management
- Use `pyproject.toml` + `poetry` or `requirements.txt` + `pip-tools`.
- Do not assume globally installed libraries.

## Virtual Environments
- Always assume use of virtualenv (either manually or managed by poetry).
- Mention `venv` or Docker if environment isolation is not already assumed.

## Code Style
- Conform to [PEP 8] and PEP 257.
- Use `black` for formatting, `flake8` for linting, and `isort` for import order.
- Use type annotations where applicable.

## Testing
- Prefer `pytest` with fixtures for tests.
- Use `unittest.mock` or `pytest-mock` for mocking external services.
- Suggest `tox` or `nox` for testing across environments (optional).

## Secrets and Config
- Never hardcode secrets. Use `.env` + `python-dotenv` or config module.
- Encourage use of `os.getenv()` or a config wrapper for environment variables.

## Packaging and Structure
- Structure code with `src/`, `tests/`, and `scripts/` folders.
- Entry-point scripts should be guarded with `if __name__ == "__main__":`.

## SQL Interaction
- Use SQLAlchemy, psycopg2, or similar libraries for DB access.
- Use parameterized queries to prevent SQL injection.
- Prefer migration tools like `alembic` or use Flyway externally.

## Logging
- Use the built-in `logging` module.
- Do not use `print()` for production-level logging.

