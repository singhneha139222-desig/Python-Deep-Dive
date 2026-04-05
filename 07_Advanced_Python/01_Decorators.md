# 📘 Decorators in Python

## 📌 Introduction

A **decorator** is a function that modifies the behavior of another function without changing its source code. It's one of Python's most powerful features for clean, reusable code.

Think of decorators like **gift wrapping** — you take a function, wrap it with extra functionality, and return the wrapped version.

```
┌─────────────────────────────────────────────────────────────┐
│                      DECORATORS                              │
│                                                              │
│   @decorator                                                 │
│   def function():      →    function = decorator(function)  │
│       pass                                                   │
│                                                              │
│   ┌─────────────────────────────────────────┐               │
│   │           decorator(func)                │               │
│   │  ┌─────────────────────────────────┐    │               │
│   │  │    wrapper(*args, **kwargs)     │    │               │
│   │  │  • Before logic (setup)         │    │               │
│   │  │  • result = func(*args, **kwargs)│   │               │
│   │  │  • After logic (cleanup)        │    │               │
│   │  │  • return result                │    │               │
│   │  └─────────────────────────────────┘    │               │
│   │           return wrapper                 │               │
│   └─────────────────────────────────────────┘               │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### How Decorators Work

```python
# The @ syntax is syntactic sugar
@decorator
def function():
    pass

# Is equivalent to:
def function():
    pass
function = decorator(function)
```

### Decorator Types

| Type | Use Case |
|------|----------|
| Function decorator | Modify function behavior |
| Class decorator | Modify class behavior |
| Method decorator | Modify method behavior |
| Decorator with args | Configurable decorators |

---

## 💻 Code Examples

### 🟢 Basic: First Decorator

```python
def simple_decorator(func):
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@simple_decorator
def greet():
    print("Hello!")

greet()
# Output:
# Before function call
# Hello!
# After function call

# Without @ syntax (equivalent)
def greet_plain():
    print("Hello!")

greet_decorated = simple_decorator(greet_plain)
greet_decorated()
```

### 🟢 Basic: Preserving Function Arguments

```python
def decorator(func):
    def wrapper(*args, **kwargs):  # Accept any arguments
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)  # Pass them through
        print(f"Finished {func.__name__}")
        return result  # Return the result
    return wrapper

@decorator
def add(a, b):
    return a + b

@decorator
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(add(3, 5))        # 8
print(greet("Alice"))   # Hello, Alice!
print(greet("Bob", greeting="Hi"))  # Hi, Bob!
```

### 🟢 Basic: Common Use Cases

```python
import time

# 1. Timer decorator
def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done"

slow_function()  # slow_function took 1.0012 seconds

# 2. Debug decorator
def debug(func):
    def wrapper(*args, **kwargs):
        args_str = ', '.join(repr(a) for a in args)
        kwargs_str = ', '.join(f"{k}={v!r}" for k, v in kwargs.items())
        all_args = ', '.join(filter(None, [args_str, kwargs_str]))
        print(f"Calling {func.__name__}({all_args})")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result!r}")
        return result
    return wrapper

@debug
def multiply(a, b):
    return a * b

multiply(3, 4)
# Calling multiply(3, 4)
# multiply returned 12
```

### 🟡 Intermediate: functools.wraps

```python
from functools import wraps

# Without @wraps - metadata is lost
def bad_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@bad_decorator
def my_func():
    """My function docstring"""
    pass

print(my_func.__name__)  # 'wrapper' - Wrong!
print(my_func.__doc__)   # None - Lost!

# With @wraps - metadata is preserved
def good_decorator(func):
    @wraps(func)  # Preserves metadata
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@good_decorator
def my_func():
    """My function docstring"""
    pass

print(my_func.__name__)  # 'my_func' - Correct!
print(my_func.__doc__)   # 'My function docstring' - Preserved!
```

### 🟡 Intermediate: Decorators with Arguments

```python
from functools import wraps

# Decorator factory - returns a decorator
def repeat(times):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(times=3)
def greet(name):
    print(f"Hello, {name}!")
    return f"Greeted {name}"

results = greet("Alice")
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
print(results)  # ['Greeted Alice', 'Greeted Alice', 'Greeted Alice']

# Retry decorator with arguments
def retry(max_attempts=3, delay=1):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            attempts = 0
            while attempts < max_attempts:
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    attempts += 1
                    if attempts == max_attempts:
                        raise
                    print(f"Attempt {attempts} failed: {e}")
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def unstable_function():
    import random
    if random.random() < 0.7:
        raise ValueError("Random failure!")
    return "Success!"
```

### 🟡 Intermediate: Multiple Decorators

```python
from functools import wraps

def bold(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return f"<b>{func(*args, **kwargs)}</b>"
    return wrapper

def italic(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return f"<i>{func(*args, **kwargs)}</i>"
    return wrapper

@bold
@italic
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))  # <b><i>Hello, Alice!</i></b>

# Order matters! Decorators apply bottom-to-top
# This is equivalent to: bold(italic(greet))
```

### 🔴 Advanced: Class-Based Decorators

```python
from functools import update_wrapper

class CountCalls:
    """Decorator that counts function calls"""
    
    def __init__(self, func):
        update_wrapper(self, func)
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} of {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def say_hello():
    print("Hello!")

say_hello()  # Call 1 of say_hello \n Hello!
say_hello()  # Call 2 of say_hello \n Hello!
say_hello()  # Call 3 of say_hello \n Hello!
print(say_hello.count)  # 3

# Class decorator with arguments
class Retry:
    def __init__(self, max_attempts=3):
        self.max_attempts = max_attempts
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(self.max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == self.max_attempts - 1:
                        raise
                    print(f"Attempt {attempt + 1} failed")
        return wrapper

@Retry(max_attempts=5)
def risky_operation():
    pass
```

### 🔴 Advanced: Decorator for Methods

```python
from functools import wraps

def validate_positive(func):
    """Decorator for methods that validates positive numbers"""
    @wraps(func)
    def wrapper(self, value, *args, **kwargs):
        if value < 0:
            raise ValueError(f"{func.__name__} requires positive value")
        return func(self, value, *args, **kwargs)
    return wrapper

class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance
    
    @validate_positive
    def deposit(self, amount):
        self.balance += amount
        return self.balance
    
    @validate_positive
    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount
        return self.balance

account = BankAccount(100)
account.deposit(50)   # Works
# account.deposit(-10)  # ValueError
```

### 🔴 Advanced: Decorator with Optional Arguments

```python
from functools import wraps

def log(func=None, *, level='INFO', include_result=True):
    """Decorator that works with or without arguments"""
    
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print(f"[{level}] Calling {func.__name__}")
            result = func(*args, **kwargs)
            if include_result:
                print(f"[{level}] {func.__name__} returned {result}")
            return result
        return wrapper
    
    if func is not None:
        # Called without arguments: @log
        return decorator(func)
    # Called with arguments: @log(level='DEBUG')
    return decorator

# Both work:
@log
def add(a, b):
    return a + b

@log(level='DEBUG', include_result=False)
def multiply(a, b):
    return a * b

add(2, 3)
# [INFO] Calling add
# [INFO] add returned 5

multiply(2, 3)
# [DEBUG] Calling multiply
```

### 🔴 Advanced: Memoization Decorator

```python
from functools import wraps, lru_cache

# Custom memoization
def memoize(func):
    cache = {}
    
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    
    wrapper.cache = cache  # Expose cache for inspection
    wrapper.clear_cache = lambda: cache.clear()
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(100))  # Instant!

# Built-in lru_cache (preferred)
@lru_cache(maxsize=128)
def expensive_computation(n):
    return sum(i ** 2 for i in range(n))

expensive_computation(1000)
expensive_computation(1000)  # Cached!
print(expensive_computation.cache_info())
```

---

## 📊 Common Decorators

| Decorator | Purpose |
|-----------|---------|
| `@property` | Create getter/setter |
| `@staticmethod` | No self parameter |
| `@classmethod` | Class as first parameter |
| `@functools.wraps` | Preserve function metadata |
| `@functools.lru_cache` | Memoization |
| `@dataclass` | Auto-generate class methods |
| `@abstractmethod` | Abstract method marker |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting to Return Result

```python
# WRONG
def decorator(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)  # Result lost!
    return wrapper

# CORRECT
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)  # Return result!
    return wrapper
```

### ❌ Mistake 2: Forgetting @wraps

```python
# WRONG - metadata lost
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

# CORRECT
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

### ❌ Mistake 3: Wrong Parentheses with Arguments

```python
# WRONG - returns decorator, doesn't apply it
@my_decorator  # Missing parentheses when decorator takes args!
def func():
    pass

# CORRECT
@my_decorator()  # Call the decorator factory
def func():
    pass
```

---

## 💡 Pro Tips

### 1. Use functools.wraps Always

```python
from functools import wraps
# Always preserves __name__, __doc__, etc.
```

### 2. Use lru_cache for Memoization

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def expensive(n):
    return n ** n
```

### 3. Stack Decorators Thoughtfully

```python
# Remember: bottom-up application
@timer      # Applied last
@cache      # Applied second  
@validate   # Applied first
def func():
    pass
```

---

## 💼 Interview Insight

### Q1: What is a decorator?

> A decorator is a function that takes another function as input, extends or modifies its behavior, and returns a new function—without modifying the original function's code.

### Q2: How do decorators with arguments work?

> They use a "decorator factory" pattern—a function that returns a decorator. The outer function takes the arguments, and the inner function is the actual decorator.

### Q3: What does @functools.wraps do?

> It preserves the original function's metadata (__name__, __doc__, __module__, etc.) when wrapping it, making debugging and introspection work correctly.

---

## 🧪 Practice Questions

### Easy:
1. Write a decorator that prints "Function starting" before execution
2. Create a timer decorator
3. Write a decorator that converts return value to uppercase

### Medium:
4. Create a retry decorator with configurable attempts
5. Build a cache decorator with expiration
6. Write a rate-limiting decorator

### Hard:
7. Implement a type-checking decorator using annotations
8. Create a decorator that logs to file with rotation
9. Build a circuit breaker decorator

---

## 📖 Summary

| Pattern | When to Use |
|---------|-------------|
| Basic decorator | Simple before/after logic |
| With arguments | Configurable behavior |
| Class-based | Need state between calls |
| @wraps | Always use for metadata |
| @lru_cache | Memoization |
| Stacked | Multiple modifications |

---

## ⏭️ Next Topic

**[Generators](./02_Generators.md)** — Memory-efficient iteration!

---

*Decorators: Enhance without modifying!* 🎁
