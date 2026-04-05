# 📘 Custom Exceptions in Python

## 📌 Introduction

**Custom exceptions** allow you to create domain-specific error types that make your code more readable and your error handling more precise. They communicate WHAT went wrong in a meaningful way.

Think of custom exceptions as **specialized alarm systems** — instead of a generic "error" alarm, you have specific alarms for different situations.

```
┌─────────────────────────────────────────────────────────────┐
│                   CUSTOM EXCEPTIONS                          │
│                                                              │
│   Exception (base)                                          │
│       │                                                      │
│       └── AppException (your base)                          │
│               │                                              │
│               ├── ValidationError                           │
│               │       ├── EmailValidationError              │
│               │       └── PasswordValidationError           │
│               │                                              │
│               ├── DatabaseError                             │
│               │       ├── ConnectionError                   │
│               │       └── QueryError                        │
│               │                                              │
│               └── BusinessLogicError                        │
│                       ├── InsufficientFundsError            │
│                       └── ItemOutOfStockError               │
│                                                              │
│   Your exceptions = Your domain language                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Why Custom Exceptions?

| Benefit | Description |
|---------|-------------|
| Clarity | Error names describe the problem |
| Specificity | Catch specific errors, not generic |
| Data | Include relevant error data |
| Hierarchy | Organize related errors |
| Documentation | Self-documenting error types |

### Exception Design Guidelines

1. **Inherit from Exception** (not BaseException)
2. **Use descriptive names** ending in "Error"
3. **Create a hierarchy** for related errors
4. **Include helpful data** in the exception
5. **Document** your exceptions

---

## 💻 Code Examples

### 🟢 Basic: Simple Custom Exception

```python
# Simplest custom exception
class MyAppError(Exception):
    pass

# Raise it
raise MyAppError("Something went wrong")

# Catch it
try:
    raise MyAppError("Error occurred")
except MyAppError as e:
    print(f"Caught: {e}")

# With default message
class ConfigError(Exception):
    def __init__(self, message="Configuration error occurred"):
        super().__init__(message)

raise ConfigError()  # Uses default message
raise ConfigError("Invalid config file")  # Custom message
```

### 🟢 Basic: Exception with Custom Data

```python
class ValidationError(Exception):
    """Raised when data validation fails"""
    
    def __init__(self, field, message, value=None):
        self.field = field
        self.value = value
        self.message = message
        super().__init__(f"{field}: {message}")

# Raise with data
def validate_email(email):
    if '@' not in email:
        raise ValidationError(
            field='email',
            message='Invalid email format',
            value=email
        )
    return email

# Catch and use the data
try:
    validate_email("invalid-email")
except ValidationError as e:
    print(f"Field: {e.field}")     # email
    print(f"Message: {e.message}") # Invalid email format
    print(f"Value: {e.value}")     # invalid-email
```

### 🟡 Intermediate: Exception Hierarchy

```python
# Base exception for your application
class AppException(Exception):
    """Base exception for the application"""
    pass

# Authentication errors
class AuthError(AppException):
    """Base for authentication errors"""
    pass

class InvalidCredentialsError(AuthError):
    """Wrong username or password"""
    pass

class TokenExpiredError(AuthError):
    """Authentication token has expired"""
    def __init__(self, token_id=None):
        self.token_id = token_id
        super().__init__("Authentication token has expired")

class PermissionDeniedError(AuthError):
    """User lacks required permissions"""
    def __init__(self, required_permission):
        self.required_permission = required_permission
        super().__init__(f"Permission denied: {required_permission} required")

# Usage
def authenticate(username, password):
    if not username or not password:
        raise InvalidCredentialsError("Username and password required")
    # ... authentication logic

def check_permission(user, permission):
    if permission not in user.permissions:
        raise PermissionDeniedError(permission)

# Catch specific or general
try:
    authenticate("", "password")
except InvalidCredentialsError as e:
    print("Bad credentials")
except AuthError as e:
    print("Authentication issue")
except AppException as e:
    print("Application error")
```

### 🟡 Intermediate: Rich Error Information

```python
from datetime import datetime
from dataclasses import dataclass
from typing import Any, Optional

@dataclass
class ErrorContext:
    """Additional context for errors"""
    timestamp: datetime
    user_id: Optional[str] = None
    request_id: Optional[str] = None
    additional_data: Optional[dict] = None

class DetailedError(Exception):
    """Exception with detailed context"""
    
    def __init__(self, message: str, code: str, context: Optional[ErrorContext] = None):
        self.message = message
        self.code = code
        self.context = context or ErrorContext(timestamp=datetime.now())
        super().__init__(message)
    
    def to_dict(self):
        """Convert to dictionary for logging/API response"""
        return {
            'error': self.code,
            'message': self.message,
            'timestamp': self.context.timestamp.isoformat(),
            'user_id': self.context.user_id,
            'request_id': self.context.request_id
        }

# Usage
context = ErrorContext(
    timestamp=datetime.now(),
    user_id='user123',
    request_id='req-456'
)

error = DetailedError(
    message="User not found",
    code="USER_NOT_FOUND",
    context=context
)

print(error.to_dict())
```

### 🟡 Intermediate: HTTP-Style Exceptions

```python
class HTTPError(Exception):
    """Base HTTP error"""
    status_code = 500
    default_message = "Internal Server Error"
    
    def __init__(self, message=None, details=None):
        self.message = message or self.default_message
        self.details = details
        super().__init__(self.message)
    
    def to_response(self):
        return {
            'status': self.status_code,
            'error': self.__class__.__name__,
            'message': self.message,
            'details': self.details
        }

class BadRequestError(HTTPError):
    status_code = 400
    default_message = "Bad Request"

class UnauthorizedError(HTTPError):
    status_code = 401
    default_message = "Unauthorized"

class ForbiddenError(HTTPError):
    status_code = 403
    default_message = "Forbidden"

class NotFoundError(HTTPError):
    status_code = 404
    default_message = "Resource Not Found"

class ConflictError(HTTPError):
    status_code = 409
    default_message = "Resource Conflict"

class ValidationError(BadRequestError):
    def __init__(self, errors: dict):
        self.errors = errors
        super().__init__(
            message="Validation failed",
            details=errors
        )

# Usage
def get_user(user_id):
    user = database.find(user_id)
    if not user:
        raise NotFoundError(f"User {user_id} not found")
    return user

def create_user(data):
    errors = validate(data)
    if errors:
        raise ValidationError(errors)
    # ... create user
```

### 🔴 Advanced: Exception with Logging

```python
import logging
import traceback
from datetime import datetime
from functools import wraps

class LoggedError(Exception):
    """Exception that automatically logs itself"""
    
    logger = logging.getLogger(__name__)
    
    def __init__(self, message, level=logging.ERROR, **kwargs):
        self.message = message
        self.timestamp = datetime.now()
        self.extra_data = kwargs
        
        # Log on creation
        self.logger.log(level, self._format_log_message())
        
        super().__init__(message)
    
    def _format_log_message(self):
        return (
            f"{self.__class__.__name__}: {self.message} | "
            f"Time: {self.timestamp} | "
            f"Data: {self.extra_data}"
        )

class CriticalError(LoggedError):
    """Critical error - logs at CRITICAL level"""
    
    def __init__(self, message, **kwargs):
        super().__init__(message, level=logging.CRITICAL, **kwargs)

# Usage
try:
    raise CriticalError(
        "Database connection lost",
        db_host="localhost",
        retry_count=3
    )
except CriticalError as e:
    print(f"Handle critical error: {e}")
```

### 🔴 Advanced: Retryable Exceptions

```python
import time
from functools import wraps

class RetryableError(Exception):
    """Exception that indicates operation can be retried"""
    default_retry_count = 3
    default_retry_delay = 1.0
    
    def __init__(self, message, retry_count=None, retry_delay=None):
        self.message = message
        self.retry_count = retry_count or self.default_retry_count
        self.retry_delay = retry_delay or self.default_retry_delay
        super().__init__(message)

class TemporaryNetworkError(RetryableError):
    """Network error that may resolve with retry"""
    default_retry_count = 5
    default_retry_delay = 2.0

class PermanentError(Exception):
    """Error that should not be retried"""
    pass

def retry_on_error(func):
    """Decorator that retries on RetryableError"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        last_error = None
        retry_count = 3
        retry_delay = 1.0
        
        for attempt in range(retry_count + 1):
            try:
                return func(*args, **kwargs)
            except RetryableError as e:
                last_error = e
                retry_count = e.retry_count
                retry_delay = e.retry_delay
                
                if attempt < retry_count:
                    print(f"Retry {attempt + 1}/{retry_count} after {retry_delay}s")
                    time.sleep(retry_delay)
            except PermanentError:
                raise
        
        raise last_error
    return wrapper

@retry_on_error
def fetch_data():
    # Simulated network call
    raise TemporaryNetworkError("Connection timeout")

# Usage
try:
    fetch_data()
except RetryableError as e:
    print(f"Failed after retries: {e}")
```

### 🔴 Advanced: Exception Context Manager

```python
from contextlib import contextmanager

class TransactionError(Exception):
    """Error during transaction"""
    pass

class Transaction:
    def __init__(self, name):
        self.name = name
        self.operations = []
    
    def add(self, operation):
        self.operations.append(operation)
    
    def rollback(self):
        print(f"Rolling back {len(self.operations)} operations")
        for op in reversed(self.operations):
            print(f"  Undoing: {op}")

@contextmanager
def transaction(name):
    """Context manager for transaction-like behavior"""
    t = Transaction(name)
    try:
        yield t
        print(f"Transaction '{name}' committed successfully")
    except Exception as e:
        t.rollback()
        raise TransactionError(f"Transaction '{name}' failed: {e}") from e

# Usage
try:
    with transaction("user_creation") as t:
        t.add("Create user record")
        t.add("Set up profile")
        t.add("Send welcome email")
        
        # Simulate error
        raise ValueError("Email service unavailable")
except TransactionError as e:
    print(f"Error: {e}")

# Output:
# Rolling back 3 operations
#   Undoing: Send welcome email
#   Undoing: Set up profile
#   Undoing: Create user record
# Error: Transaction 'user_creation' failed: Email service unavailable
```

---

## 📊 Exception Design Patterns

### 1. Error Code Pattern

```python
from enum import Enum

class ErrorCode(Enum):
    INVALID_INPUT = "E001"
    NOT_FOUND = "E002"
    UNAUTHORIZED = "E003"
    INTERNAL_ERROR = "E999"

class AppError(Exception):
    def __init__(self, code: ErrorCode, message: str):
        self.code = code
        self.message = message
        super().__init__(f"[{code.value}] {message}")
```

### 2. Factory Pattern

```python
class ErrorFactory:
    @staticmethod
    def validation_error(field, message):
        return ValidationError(field, message)
    
    @staticmethod
    def not_found(resource_type, resource_id):
        return NotFoundError(f"{resource_type} with id {resource_id} not found")
    
    @staticmethod
    def from_exception(e):
        """Convert standard exception to custom"""
        if isinstance(e, ValueError):
            return ValidationError("unknown", str(e))
        return AppError(str(e))
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Inheriting from BaseException

```python
# WRONG - catches KeyboardInterrupt!
class MyError(BaseException):
    pass

# CORRECT
class MyError(Exception):
    pass
```

### ❌ Mistake 2: Not Calling super().__init__

```python
# WRONG
class MyError(Exception):
    def __init__(self, message, code):
        self.message = message
        self.code = code
        # Forgot super().__init__!

# CORRECT
class MyError(Exception):
    def __init__(self, message, code):
        self.message = message
        self.code = code
        super().__init__(message)
```

### ❌ Mistake 3: Too Many Exception Classes

```python
# WRONG - one exception per error
class UserNotFoundError(Exception): pass
class ProductNotFoundError(Exception): pass
class OrderNotFoundError(Exception): pass

# CORRECT - parameterized exception
class NotFoundError(Exception):
    def __init__(self, resource_type, resource_id):
        self.resource_type = resource_type
        self.resource_id = resource_id
        super().__init__(f"{resource_type} {resource_id} not found")
```

---

## 💡 Pro Tips

### 1. Make Exceptions Serializable

```python
class SerializableError(Exception):
    def to_dict(self):
        return {
            'type': self.__class__.__name__,
            'message': str(self),
            'args': self.args
        }
```

### 2. Use `__str__` and `__repr__`

```python
class MyError(Exception):
    def __str__(self):
        return f"MyError: {self.message}"
    
    def __repr__(self):
        return f"MyError(message={self.message!r})"
```

### 3. Document Your Exceptions

```python
class InsufficientFundsError(Exception):
    """
    Raised when account has insufficient funds for a transaction.
    
    Attributes:
        account_id: The account that lacks funds
        required: Amount required for the transaction
        available: Amount currently available
    """
```

---

## 💼 Interview Insight

### Q1: When should you create custom exceptions?

> Create custom exceptions when:
> 1. Built-in exceptions don't describe the error well
> 2. You need to include additional error data
> 3. You want to catch specific application errors
> 4. You're building a library or API

### Q2: Should custom exceptions be specific or general?

> Balance is key:
> - Too general: Hard to handle specific cases
> - Too specific: Too many exception classes
> - Use hierarchy: General base, specific children

### Q3: How to make exceptions work with logging?

> Include `__str__` for message, add `to_dict()` for structured logging, or create self-logging exceptions.

---

## 🧪 Practice Questions

### Easy:
1. Create a `NegativeNumberError` exception
2. Create a `InvalidUsernameError` with username attribute
3. Build a simple exception hierarchy for a calculator

### Medium:
4. Create an `APIError` with status code and response body
5. Build a validation error that collects multiple field errors
6. Create a retryable vs non-retryable exception system

### Hard:
7. Design a complete exception hierarchy for an e-commerce system
8. Create exceptions with automatic logging and alerting
9. Build an exception system with error codes and i18n support

---

## 📖 Summary

| Concept | Implementation |
|---------|----------------|
| Basic | `class MyError(Exception): pass` |
| With data | Add `__init__` with attributes |
| Hierarchy | Inherit from base app exception |
| Serializable | Add `to_dict()` method |
| Self-logging | Log in `__init__` |
| Retryable | Add retry hints as attributes |

---

## ⏭️ Next Module

**[Module 7: Advanced Python](../07_Advanced_Python/README.md)** — Decorators, generators, and more!

---

*Custom Exceptions: Speak your domain's language!* 🗣️
