# 📘 Context Managers in Python

## 📌 Introduction

A **context manager** is a Python object that defines the setup and teardown actions for a block of code. It's used with the `with` statement to ensure proper resource management.

Think of it like a **hotel check-in/check-out system** — the `with` statement handles check-in (setup), you use the room (resource), and it automatically handles check-out (cleanup).

```
┌─────────────────────────────────────────────────────────────┐
│                   CONTEXT MANAGER                            │
│                                                              │
│   with open('file.txt') as f:                               │
│       data = f.read()                                        │
│                                                              │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    │
│   │ __enter__() │ →  │ Use resource│ →  │ __exit__()  │    │
│   │   Setup     │    │   in block  │    │   Cleanup   │    │
│   │ return file │    │   f.read()  │    │  f.close()  │    │
│   └─────────────┘    └─────────────┘    └─────────────┘    │
│                                                              │
│   __exit__ called even if exception occurs!                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### The Context Manager Protocol

| Method | When Called | Purpose |
|--------|-------------|---------|
| `__enter__()` | Start of `with` block | Setup, return resource |
| `__exit__()` | End of `with` block | Cleanup, handle exceptions |

### Benefits

1. **Automatic cleanup** — Resources always released
2. **Exception safety** — Cleanup happens even on errors
3. **Cleaner code** — No try/finally boilerplate
4. **Explicit scope** — Clear resource lifetime

---

## 💻 Code Examples

### 🟢 Basic: Using Built-in Context Managers

```python
# File handling (most common)
with open('file.txt', 'r') as f:
    content = f.read()
# File automatically closed here

# Multiple files
with open('input.txt') as src, open('output.txt', 'w') as dst:
    dst.write(src.read())

# Threading locks
import threading
lock = threading.Lock()
with lock:
    # Critical section
    shared_data += 1
# Lock released

# Database connections
import sqlite3
with sqlite3.connect('database.db') as conn:
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users")
# Connection closed, changes committed
```

### 🟢 Basic: Without vs With Context Manager

```python
# WITHOUT context manager (manual cleanup)
f = None
try:
    f = open('file.txt')
    data = f.read()
finally:
    if f:
        f.close()

# WITH context manager (automatic cleanup)
with open('file.txt') as f:
    data = f.read()
# Much cleaner!

# Exception handling comparison
# Without
try:
    f = open('file.txt')
    try:
        data = f.read()
        process(data)
    finally:
        f.close()
except FileNotFoundError:
    print("File not found")

# With
try:
    with open('file.txt') as f:
        data = f.read()
        process(data)
except FileNotFoundError:
    print("File not found")
```

### 🟡 Intermediate: Class-Based Context Manager

```python
class FileManager:
    """Custom context manager for files"""
    
    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        print(f"Opening {self.filename}")
        self.file = open(self.filename, self.mode)
        return self.file  # This is assigned to 'as' variable
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Closing {self.filename}")
        if self.file:
            self.file.close()
        
        # Exception handling
        if exc_type is not None:
            print(f"Exception occurred: {exc_type.__name__}: {exc_val}")
        
        return False  # Don't suppress exceptions

# Usage
with FileManager('test.txt', 'w') as f:
    f.write('Hello, World!')
# Output:
# Opening test.txt
# Closing test.txt
```

### 🟡 Intermediate: Using contextlib

```python
from contextlib import contextmanager

@contextmanager
def file_manager(filename, mode='r'):
    """Generator-based context manager"""
    print(f"Opening {filename}")
    f = open(filename, mode)
    try:
        yield f  # This is the 'as' variable
    finally:
        print(f"Closing {filename}")
        f.close()

with file_manager('test.txt', 'w') as f:
    f.write('Hello!')

# Timer context manager
import time

@contextmanager
def timer(label):
    start = time.time()
    try:
        yield
    finally:
        elapsed = time.time() - start
        print(f"{label}: {elapsed:.4f}s")

with timer("Processing"):
    time.sleep(1)
# Output: Processing: 1.0012s
```

### 🟡 Intermediate: Exception Handling in Context Managers

```python
class ManagedResource:
    def __enter__(self):
        print("Acquiring resource")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Releasing resource")
        
        if exc_type is None:
            print("No exception")
            return False
        
        # Handle specific exceptions
        if exc_type is ValueError:
            print(f"Handling ValueError: {exc_val}")
            return True  # Suppress the exception
        
        if exc_type is TypeError:
            print(f"Logging TypeError: {exc_val}")
            return False  # Let it propagate
        
        return False  # Default: don't suppress

# Exception suppressed
with ManagedResource():
    raise ValueError("This will be caught")
print("Continues normally")

# Exception propagates
try:
    with ManagedResource():
        raise TypeError("This will propagate")
except TypeError as e:
    print(f"Caught outside: {e}")
```

### 🔴 Advanced: Nested Context Managers

```python
from contextlib import ExitStack

# Multiple resources
def process_files(filenames):
    with ExitStack() as stack:
        files = [stack.enter_context(open(fn)) for fn in filenames]
        # All files will be closed when exiting
        for f in files:
            print(f.read())

# Dynamic resource management
@contextmanager
def managed_connection(host):
    print(f"Connecting to {host}")
    yield f"connection_{host}"
    print(f"Disconnecting from {host}")

def process_hosts(hosts):
    with ExitStack() as stack:
        connections = []
        for host in hosts:
            conn = stack.enter_context(managed_connection(host))
            connections.append(conn)
        
        # Use all connections
        for conn in connections:
            print(f"Using {conn}")

process_hosts(['server1', 'server2', 'server3'])
```

### 🔴 Advanced: Context Managers for State Management

```python
from contextlib import contextmanager

# Temporary directory change
import os

@contextmanager
def change_directory(path):
    old_dir = os.getcwd()
    os.chdir(path)
    try:
        yield
    finally:
        os.chdir(old_dir)

with change_directory('/tmp'):
    print(os.getcwd())  # /tmp
print(os.getcwd())  # Back to original

# Temporary environment variable
@contextmanager
def set_env(key, value):
    old_value = os.environ.get(key)
    os.environ[key] = value
    try:
        yield
    finally:
        if old_value is None:
            del os.environ[key]
        else:
            os.environ[key] = old_value

with set_env('DEBUG', 'true'):
    print(os.environ['DEBUG'])  # true
# DEBUG is restored/removed

# Redirect stdout
import sys
from io import StringIO

@contextmanager
def capture_output():
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    try:
        yield sys.stdout
    finally:
        sys.stdout = old_stdout

with capture_output() as output:
    print("This is captured")
print(f"Captured: {output.getvalue()}")
```

### 🔴 Advanced: Reentrant Context Managers

```python
from contextlib import contextmanager

class Counter:
    """Reentrant counter that tracks nesting"""
    
    def __init__(self):
        self.count = 0
    
    def __enter__(self):
        self.count += 1
        print(f"Enter (depth: {self.count})")
        return self
    
    def __exit__(self, *args):
        print(f"Exit (depth: {self.count})")
        self.count -= 1
        return False

counter = Counter()

with counter:
    print(f"Level 1: {counter.count}")
    with counter:
        print(f"Level 2: {counter.count}")
        with counter:
            print(f"Level 3: {counter.count}")
# Output:
# Enter (depth: 1)
# Level 1: 1
# Enter (depth: 2)
# Level 2: 2
# Enter (depth: 3)
# Level 3: 3
# Exit (depth: 3)
# Exit (depth: 2)
# Exit (depth: 1)
```

### 🔴 Advanced: Database Transaction Manager

```python
from contextlib import contextmanager

class Database:
    def __init__(self):
        self.in_transaction = False
        self.data = {}
    
    def execute(self, query):
        print(f"Executing: {query}")
    
    def begin(self):
        self.in_transaction = True
        self._backup = self.data.copy()
    
    def commit(self):
        self.in_transaction = False
        self._backup = None
    
    def rollback(self):
        self.data = self._backup
        self.in_transaction = False

@contextmanager
def transaction(db):
    """Context manager for database transactions"""
    db.begin()
    try:
        yield db
        db.commit()
        print("Transaction committed")
    except Exception as e:
        db.rollback()
        print(f"Transaction rolled back: {e}")
        raise

db = Database()

# Successful transaction
with transaction(db):
    db.execute("INSERT INTO users VALUES (1, 'Alice')")
    db.execute("INSERT INTO orders VALUES (1, 100)")

# Failed transaction
try:
    with transaction(db):
        db.execute("INSERT INTO users VALUES (2, 'Bob')")
        raise ValueError("Something went wrong")
except ValueError:
    pass  # Transaction was rolled back
```

---

## 📊 contextlib Utilities

| Function | Purpose |
|----------|---------|
| `@contextmanager` | Create from generator |
| `closing()` | Auto-close objects |
| `suppress()` | Suppress exceptions |
| `redirect_stdout()` | Redirect output |
| `ExitStack` | Dynamic management |
| `nullcontext()` | No-op context |

```python
from contextlib import closing, suppress, redirect_stdout, nullcontext

# closing - for objects with close()
from urllib.request import urlopen
with closing(urlopen('http://example.com')) as page:
    content = page.read()

# suppress - ignore specific exceptions
with suppress(FileNotFoundError):
    os.remove('nonexistent.txt')

# redirect_stdout
with redirect_stdout(open('log.txt', 'w')):
    print("This goes to file")

# nullcontext - conditional context
debug_mode = False
cm = open('debug.log', 'w') if debug_mode else nullcontext()
with cm:
    print("Maybe logging")
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting to Return in __enter__

```python
# WRONG
class BadManager:
    def __enter__(self):
        self.resource = create_resource()
        # Forgot to return!
    
    def __exit__(self, *args):
        self.resource.close()

with BadManager() as r:
    print(r)  # None!

# CORRECT
class GoodManager:
    def __enter__(self):
        self.resource = create_resource()
        return self.resource  # Return the resource
```

### ❌ Mistake 2: Not Handling Cleanup on Exception

```python
# WRONG
@contextmanager
def bad_manager():
    resource = acquire()
    yield resource
    release(resource)  # Won't run if exception!

# CORRECT
@contextmanager
def good_manager():
    resource = acquire()
    try:
        yield resource
    finally:
        release(resource)  # Always runs
```

### ❌ Mistake 3: Suppressing All Exceptions

```python
# WRONG - hides bugs
class BadManager:
    def __exit__(self, exc_type, exc_val, exc_tb):
        return True  # Suppresses ALL exceptions!

# CORRECT - only suppress expected ones
class GoodManager:
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is ExpectedException:
            return True
        return False
```

---

## 💡 Pro Tips

### 1. Use @contextmanager for Simple Cases

```python
from contextlib import contextmanager

@contextmanager
def simple_resource():
    setup()
    try:
        yield resource
    finally:
        cleanup()
```

### 2. Use ExitStack for Dynamic Resources

```python
from contextlib import ExitStack

with ExitStack() as stack:
    files = [stack.enter_context(open(f)) for f in filenames]
```

### 3. Context Managers for Testing

```python
@contextmanager
def mock_time(frozen_time):
    original = time.time
    time.time = lambda: frozen_time
    try:
        yield
    finally:
        time.time = original
```

---

## 💼 Interview Insight

### Q1: What is a context manager?

> A context manager is an object that implements `__enter__` and `__exit__` methods, allowing it to be used with the `with` statement for automatic resource management and cleanup.

### Q2: When is `__exit__` called?

> `__exit__` is always called when exiting the `with` block, whether normally or due to an exception. Its parameters contain exception info if an exception occurred.

### Q3: How to suppress exceptions in a context manager?

> Return `True` from `__exit__` to suppress the exception. Return `False` (or nothing) to let it propagate.

---

## 🧪 Practice Questions

### Easy:
1. Create a timer context manager
2. Write a context manager that prints enter/exit messages
3. Use contextlib to create a simple resource manager

### Medium:
4. Create a context manager for database transactions
5. Implement a context manager that captures stdout
6. Build a context manager for temporary files

### Hard:
7. Implement a nested transaction manager
8. Create a context manager for distributed locks
9. Build a context manager pool for connection reuse

---

## 📖 Summary

| Pattern | Implementation |
|---------|----------------|
| Class-based | `__enter__`, `__exit__` |
| Generator-based | `@contextmanager` |
| Exception handling | Check `exc_type` in `__exit__` |
| Multiple resources | `ExitStack` |
| Suppress exceptions | Return `True` from `__exit__` |

---

## ⏭️ Next Module

**[Module 8: Libraries and Frameworks](../08_Libraries_and_Frameworks/README.md)** — NumPy, Pandas, Flask, and more!

---

*Context Managers: Clean up after yourself!* 🧹
