# AI Assistant Rules for Python Projects

## Dependency Management
- Use `pyproject.toml` + `poetry` or `requirements.txt` + `pip-tools` for dependency management.
- Pin exact versions in production (requirements.lock).
- Separate development and production dependencies.
- Keep dependencies up to date but test thoroughly.
- Do not assume globally installed libraries.

## Virtual Environments
- Always assume use of virtualenv (either manually or managed by poetry).
- Use virtual environments for all projects.
- Mention `venv` or Docker if environment isolation is not already assumed.

## Code Style and Formatting
- Follow PEP 8 style guidelines strictly and PEP 257 for docstrings.
- Use `black` for automatic code formatting.
- Limit line length to 88 characters (Black default).
- Use descriptive variable names following snake_case convention.
- Organize imports: standard library, third-party, local imports.
- Use type annotations where applicable.

```python
# Standard library
import os
from typing import Optional, List

# Third-party
import requests
from sqlalchemy import Column, Integer, String

# Local imports
from .models import User
from .exceptions import UserNotFoundError
```

## Type Hints and Documentation
- Use type hints for all function parameters and return values.
- Use `Optional[Type]` for parameters that can be None.
- Leverage `typing` module for complex types (Union, Dict, List, etc.).
- Write comprehensive docstrings following Google or NumPy style.
- Use `mypy` for static type checking.

```python
from typing import Optional, List, Dict, Any

def get_user_by_email(email: str) -> Optional[User]:
    """
    Retrieve a user by their email address.
    
    Args:
        email: The email address to search for
        
    Returns:
        User object if found, None otherwise
        
    Raises:
        ValueError: If email format is invalid
    """
    if not email or "@" not in email:
        raise ValueError("Invalid email format")
    
    return User.query.filter_by(email=email).first()
```

## Testing Guidelines

### Test Structure and Organization
- Prefer `pytest` with fixtures for tests.
- Use `unittest.mock` or `pytest-mock` for mocking external services.
- Organize tests to mirror source code structure.
- Follow the AAA pattern (Arrange, Act, Assert).
- Use descriptive test function names.

```python
import pytest
from unittest.mock import Mock, patch

class TestUserService:
    
    def test_create_user_success(self, user_service, mock_db_session):
        # Arrange
        email = "test@example.com"
        name = "Test User"
        
        # Act
        result = user_service.create_user(email, name)
        
        # Assert
        assert result.email == email
        assert result.name == name
        mock_db_session.add.assert_called_once()
```

### Mocking and Fixtures
- Use pytest fixtures for test data and mock objects.
- Mock external dependencies and services.
- Use `pytest-mock` for easier mocking.
- Create factory functions for test data generation.
- Use parametrized tests for testing multiple scenarios.

```python
@pytest.fixture
def user_service(mock_db_session):
    return UserService(db_session=mock_db_session)

@pytest.fixture
def mock_db_session():
    return Mock()

@pytest.mark.parametrize(
    "email,expected_valid",
    [
        ("valid@example.com", True),
        ("invalid-email", False),
        ("", False),
    ]
)
def test_email_validation(email, expected_valid):
    result = validate_email(email)
    assert result == expected_valid
```

## Configuration and Secrets Management
- Never hardcode secrets. Use `.env` + `python-dotenv` or config module.
- Use environment variables for configuration.
- Encourage use of `os.getenv()` or a config wrapper for environment variables.
- Create configuration classes using Pydantic or dataclasses.
- Separate configuration for different environments.
- Validate configuration on application startup.
- Never commit secrets to version control.

```python
from pydantic import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    debug: bool = False
    log_level: str = "INFO"
    
    class Config:
        env_file = ".env"

settings = Settings()
```

## Packaging and Structure
- Structure code with `src/`, `tests/`, and `scripts/` folders.
- Entry-point scripts should be guarded with `if __name__ == "__main__":`.
- Organize code to mirror logical application structure.

## Database and SQL Interaction
- Use SQLAlchemy, psycopg2, or similar libraries for database access.
- Use parameterized queries to prevent SQL injection.
- Prefer migration tools like `alembic`.
- Use declarative base for model definition.
- Implement proper relationship mappings.
- Use context managers for database sessions.

```python
from sqlalchemy import Column, Integer, String, DateTime, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship
from datetime import datetime

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True)
    email = Column(String(255), unique=True, nullable=False, index=True)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    orders = relationship("Order", back_populates="user", lazy="select")
    
    def __repr__(self) -> str:
        return f"<User(email='{self.email}')>"
```

## Error Handling and Exceptions
- Create custom exception classes that inherit from appropriate base exceptions.
- Use specific exception types rather than generic `Exception`.
- Implement proper logging with context information.
- Use try-except blocks judiciously, don't catch everything.
- Follow the EAFP principle (Easier to Ask for Forgiveness than Permission).

```python
class UserServiceError(Exception):
    """Base exception for user service operations."""
    pass

class UserNotFoundError(UserServiceError):
    """Raised when a user cannot be found."""
    
    def __init__(self, user_id: int):
        super().__init__(f"User with ID {user_id} not found")
        self.user_id = user_id
```

## Logging
- Use the built-in `logging` module.
- Do not use `print()` for production-level logging.
- Use structured logging with JSON format for production.
- Configure appropriate log levels.
- Include correlation IDs for request tracing.

```python
import logging
import structlog

# Configure structured logging
structlog.configure(
    processors=[
        structlog.stdlib.filter_by_level,
        structlog.stdlib.add_logger_name,
        structlog.stdlib.add_log_level,
        structlog.stdlib.PositionalArgumentsFormatter(),
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.StackInfoRenderer(),
        structlog.processors.format_exc_info,
        structlog.processors.JSONRenderer()
    ],
    context_class=dict,
    logger_factory=structlog.stdlib.LoggerFactory(),
    cache_logger_on_first_use=True,
)

logger = structlog.get_logger()

def create_user(email: str, name: str) -> User:
    logger.info("Creating user", email=email, name=name)
    try:
        # Implementation
        logger.info("User created successfully", user_id=user.id)
        return user
    except Exception as e:
        logger.error("Failed to create user", error=str(e), email=email)
        raise
```

## Performance and Monitoring

### Asynchronous Programming
- Use `async`/`await` for I/O-bound operations.
- Implement proper connection pooling for async database operations.
- Use `asyncio.gather()` for concurrent operations.
- Handle exceptions properly in async functions.
- Use connection limits to prevent resource exhaustion.

```python
import asyncio
from typing import List

async def fetch_user_data(user_ids: List[int]) -> List[User]:
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_single_user(session, user_id) for user_id in user_ids]
        return await asyncio.gather(*tasks)
```

### Monitoring
- Use context managers for adding request context.
- Implement health check endpoints.
- Monitor application performance and resource usage.

## Security Best Practices

### Input Validation and Sanitization
- Use Pydantic for automatic validation.
- Sanitize all user inputs.
- Implement proper authentication and authorization.
- Use secure session management.
- Validate file uploads and limit file sizes.

```python
from pydantic import BaseModel, validator
import re

class CreateUserRequest(BaseModel):
    email: str
    name: str
    
    @validator('email')
    def validate_email(cls, v):
        if not re.match(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$', v):
            raise ValueError('Invalid email format')
        return v.lower()
    
    @validator('name')
    def validate_name(cls, v):
        if len(v.strip()) < 2:
            raise ValueError('Name must be at least 2 characters')
        return v.strip()
```

### General Security
- Never expose sensitive information in logs or error messages.
- Use HTTPS for all production deployments.
- Implement rate limiting for API endpoints.
- Keep dependencies updated and scan for vulnerabilities.
- Use secure defaults for all configuration options.
