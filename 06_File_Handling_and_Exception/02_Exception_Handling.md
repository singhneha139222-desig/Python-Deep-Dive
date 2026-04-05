# 📘 Exception Handling in Python

## 📌 Introduction

**Exception handling** is the process of responding to errors that occur during program execution. Instead of crashing, your program can gracefully handle problems and continue or exit cleanly.

Think of it like a **safety net** — when something goes wrong, instead of falling, you're caught and can recover.

```
┌─────────────────────────────────────────────────────────────┐
│                   EXCEPTION HANDLING                         │
│                                                              │
│   try:                                                       │
│       risky_operation()    ──→ May raise exception          │
│                                      ↓                       │
│   except ErrorType:        ←── Catches matching exception   │
│       handle_error()                                         │
│                                      ↓                       │
│   else:                    ──→ Runs if NO exception         │
│       success_code()                                         │
│                                      ↓                       │
│   finally:                 ──→ ALWAYS runs (cleanup)        │
│       cleanup()                                              │
│                                                              │
│   EAFP: "Easier to Ask Forgiveness than Permission"         │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Exception Hierarchy (Simplified)

```
BaseException
 └── Exception
      ├── ArithmeticError
      │    ├── ZeroDivisionError
      │    └── OverflowError
      ├── LookupError
      │    ├── IndexError
      │    └── KeyError
      ├── TypeError
      ├── ValueError
      ├── AttributeError
      ├── FileNotFoundError
      ├── PermissionError
      └── RuntimeError
```

### EAFP vs LBYL

| Style | Meaning | Python Way |
|-------|---------|------------|
| EAFP | Easier to Ask Forgiveness than Permission | ✅ Preferred |
| LBYL | Look Before You Leap | ❌ Less Pythonic |

```python
# LBYL (Less Pythonic)
if key in dictionary:
    value = dictionary[key]

# EAFP (More Pythonic)
try:
    value = dictionary[key]
except KeyError:
    value = default
```

---

## 💻 Code Examples

### 🟢 Basic: try/except

```python
# Basic try/except
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")

# With variable
try:
    number = int("not a number")
except ValueError as e:
    print(f"Error: {e}")
# Output: Error: invalid literal for int() with base 10: 'not a number'

# Multiple exceptions
try:
    value = my_list[10]
except IndexError:
    print("Index out of range")
except TypeError:
    print("Invalid index type")

# Multiple exceptions in one line
try:
    data = some_operation()
except (ValueError, TypeError, KeyError) as e:
    print(f"Error occurred: {e}")
```

### 🟢 Basic: Common Exceptions

```python
# ZeroDivisionError
try:
    result = 100 / 0
except ZeroDivisionError:
    print("Division by zero!")

# ValueError
try:
    number = int("hello")
except ValueError:
    print("Invalid integer!")

# TypeError
try:
    result = "2" + 2
except TypeError:
    print("Cannot add string and int!")

# KeyError
try:
    d = {"a": 1}
    value = d["b"]
except KeyError:
    print("Key not found!")

# IndexError
try:
    lst = [1, 2, 3]
    item = lst[10]
except IndexError:
    print("Index out of range!")

# FileNotFoundError
try:
    with open("nonexistent.txt") as f:
        content = f.read()
except FileNotFoundError:
    print("File not found!")

# AttributeError
try:
    x = None
    x.upper()
except AttributeError:
    print("Attribute doesn't exist!")
```

### 🟢 Basic: try/except/else/finally

```python
def divide(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        print("Cannot divide by zero!")
        return None
    except TypeError:
        print("Invalid types for division!")
        return None
    else:
        # Runs only if NO exception occurred
        print(f"Division successful!")
        return result
    finally:
        # ALWAYS runs, even if return happened
        print("Division attempt complete")

print(divide(10, 2))
# Output:
# Division successful!
# Division attempt complete
# 5.0

print(divide(10, 0))
# Output:
# Cannot divide by zero!
# Division attempt complete
# None
```

### 🟡 Intermediate: Raising Exceptions

```python
# Raise an exception
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    if age > 150:
        raise ValueError("Age cannot be greater than 150")
    return age

try:
    set_age(-5)
except ValueError as e:
    print(f"Invalid age: {e}")

# Re-raise exception
def process_data(data):
    try:
        return int(data)
    except ValueError:
        print("Logging error...")
        raise  # Re-raise the same exception

# Raise with cause
try:
    try:
        x = int("not a number")
    except ValueError as original:
        raise RuntimeError("Processing failed") from original
except RuntimeError as e:
    print(f"Error: {e}")
    print(f"Caused by: {e.__cause__}")
```

### 🟡 Intermediate: Exception Information

```python
import traceback
import sys

try:
    x = 1 / 0
except Exception as e:
    # Exception type
    print(f"Type: {type(e).__name__}")  # ZeroDivisionError
    
    # Exception message
    print(f"Message: {e}")  # division by zero
    
    # Arguments
    print(f"Args: {e.args}")  # ('division by zero',)
    
    # Traceback
    print("\nTraceback:")
    traceback.print_exc()
    
    # Get exception info
    exc_type, exc_value, exc_tb = sys.exc_info()
    print(f"\nException type: {exc_type}")
    print(f"Exception value: {exc_value}")
```

### 🟡 Intermediate: Catching All Exceptions

```python
# Catch all exceptions (use sparingly!)
try:
    risky_operation()
except Exception as e:
    print(f"An error occurred: {e}")

# Never catch BaseException (includes SystemExit, KeyboardInterrupt)
# WRONG:
# except BaseException:
#     pass  # Catches Ctrl+C!

# Better pattern: log and re-raise
import logging

try:
    risky_operation()
except Exception as e:
    logging.exception("Operation failed")
    raise  # Re-raise after logging
```

### 🔴 Advanced: Context-Specific Handling

```python
def read_config(filename):
    """Read config with detailed error handling"""
    try:
        with open(filename, 'r') as f:
            return f.read()
    except FileNotFoundError:
        raise FileNotFoundError(f"Config file '{filename}' not found. "
                               "Please create it or specify a valid path.")
    except PermissionError:
        raise PermissionError(f"No permission to read '{filename}'. "
                             "Check file permissions.")
    except IsADirectoryError:
        raise IsADirectoryError(f"'{filename}' is a directory, not a file.")
    except Exception as e:
        raise RuntimeError(f"Failed to read config: {e}") from e

# Usage
try:
    config = read_config("settings.ini")
except (FileNotFoundError, PermissionError) as e:
    print(f"Configuration error: {e}")
    config = use_default_config()
```

### 🔴 Advanced: Exception Chaining

```python
class DatabaseError(Exception):
    pass

class ConnectionError(DatabaseError):
    pass

def connect_to_database():
    try:
        # Simulate connection error
        raise OSError("Network unreachable")
    except OSError as e:
        # Chain the exceptions
        raise ConnectionError("Failed to connect to database") from e

try:
    connect_to_database()
except ConnectionError as e:
    print(f"Error: {e}")
    print(f"Original cause: {e.__cause__}")
    print(f"Context: {e.__context__}")

# Output:
# Error: Failed to connect to database
# Original cause: Network unreachable
```

### 🔴 Advanced: assert Statements

```python
# assert for debugging (disabled with -O flag)
def calculate_average(numbers):
    assert len(numbers) > 0, "List cannot be empty"
    assert all(isinstance(n, (int, float)) for n in numbers), "All elements must be numbers"
    return sum(numbers) / len(numbers)

# Assertions are for bugs, not user errors
# WRONG:
def get_user(user_id):
    assert user_id > 0  # Don't validate user input with assert!

# CORRECT:
def get_user(user_id):
    if user_id <= 0:
        raise ValueError("user_id must be positive")
```

### 🔴 Advanced: Exception Groups (Python 3.11+)

```python
# Python 3.11+ Exception Groups
def process_all(items):
    errors = []
    results = []
    
    for item in items:
        try:
            results.append(process(item))
        except Exception as e:
            errors.append(e)
    
    if errors:
        raise ExceptionGroup("Multiple errors occurred", errors)
    
    return results

# Handling exception groups
try:
    process_all(data)
except* ValueError as eg:
    print(f"Value errors: {eg.exceptions}")
except* TypeError as eg:
    print(f"Type errors: {eg.exceptions}")
```

---

## 📊 Exception Reference

### Built-in Exceptions

| Exception | When Raised |
|-----------|-------------|
| `ValueError` | Wrong value |
| `TypeError` | Wrong type |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |
| `AttributeError` | Attribute not found |
| `FileNotFoundError` | File doesn't exist |
| `PermissionError` | No access permission |
| `ZeroDivisionError` | Division by zero |
| `ImportError` | Import failed |
| `StopIteration` | Iterator exhausted |
| `RuntimeError` | Generic runtime error |
| `MemoryError` | Out of memory |
| `RecursionError` | Max recursion depth |

### Exception Block Syntax

```python
try:
    # Code that might raise exception
    pass
except SpecificError:
    # Handle specific error
    pass
except (Error1, Error2):
    # Handle multiple errors
    pass
except Exception as e:
    # Handle any exception
    pass
else:
    # Run if no exception
    pass
finally:
    # Always runs
    pass
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Bare except

```python
# WRONG - catches everything including KeyboardInterrupt!
try:
    risky_operation()
except:
    pass

# CORRECT - be specific
try:
    risky_operation()
except ValueError:
    handle_value_error()
except Exception as e:
    log_error(e)
    raise
```

### ❌ Mistake 2: Silencing Exceptions

```python
# WRONG - hiding errors
try:
    important_operation()
except Exception:
    pass  # Silent failure!

# CORRECT - at least log
try:
    important_operation()
except Exception as e:
    logging.error(f"Operation failed: {e}")
```

### ❌ Mistake 3: Too Broad Exception Handling

```python
# WRONG - catches too much
try:
    value = data['key']['nested']
except Exception:
    value = None

# CORRECT - catch specific errors
try:
    value = data['key']['nested']
except (KeyError, TypeError):
    value = None
```

---

## 💡 Pro Tips

### 1. Use Context Managers for Cleanup

```python
# Instead of try/finally
try:
    f = open('file.txt')
    # work with file
finally:
    f.close()

# Use with statement
with open('file.txt') as f:
    # work with file
# Automatically cleaned up
```

### 2. Create Helpful Error Messages

```python
def get_item(index):
    if index < 0:
        raise IndexError(
            f"Index must be non-negative, got {index}. "
            f"Use positive indices to access items."
        )
```

### 3. Use Logging Instead of Print

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    risky_operation()
except Exception as e:
    logging.exception("Operation failed")  # Includes traceback
```

---

## 💼 Interview Insight

### Q1: Difference between `except Exception` and bare `except`?

> - `except Exception`: Catches all exceptions that inherit from Exception
> - Bare `except:`: Catches everything including `SystemExit`, `KeyboardInterrupt`
> - Always use `except Exception` at minimum

### Q2: When to use `else` clause?

> Use `else` when you have code that should only run if no exception occurred, but that code itself shouldn't be protected by the try block.

### Q3: What is EAFP?

> "Easier to Ask Forgiveness than Permission" — Try the operation first, then handle exceptions, rather than checking conditions before every operation.

---

## 🧪 Practice Questions

### Easy:
1. Write a safe division function
2. Handle file not found error
3. Convert string to int with error handling

### Medium:
4. Implement retry logic with exponential backoff
5. Create a function that validates JSON data
6. Build a robust user input handler

### Hard:
7. Implement a transaction-like rollback system
8. Create a circuit breaker pattern
9. Build an error aggregator for batch processing

---

## 📖 Summary

| Concept | Usage |
|---------|-------|
| `try` | Code that might raise exception |
| `except` | Handle specific exception |
| `else` | Run if no exception |
| `finally` | Always runs (cleanup) |
| `raise` | Throw an exception |
| `from` | Chain exceptions |
| `as` | Capture exception object |

---

## ⏭️ Next Topic

**[Custom Exceptions](./03_Custom_Exceptions.md)** — Create your own exception types!

---

*Exceptions: Expect the unexpected!* ⚡
