# 📘 Python Interview Questions

## 📌 Introduction

This guide covers the most commonly asked Python interview questions, organized by category with detailed answers.

---

## 🐍 Python Fundamentals

### Q1: What is Python? Why is it popular?

**Answer:**
Python is a high-level, interpreted, dynamically-typed programming language known for its readability and simplicity.

**Why it's popular:**
- Simple, readable syntax (looks like English)
- Extensive standard library
- Large ecosystem of third-party packages
- Cross-platform compatibility
- Strong community support
- Versatile (web, data science, AI, automation)

---

### Q2: What are Python's key features?

**Answer:**
1. **Easy to learn** — Clean, readable syntax
2. **Interpreted** — No compilation needed
3. **Dynamically typed** — No type declarations required
4. **Object-oriented** — Supports OOP concepts
5. **Extensive libraries** — NumPy, Pandas, Django, etc.
6. **Memory management** — Automatic garbage collection
7. **Portable** — Runs on multiple platforms

---

### Q3: What is PEP 8?

**Answer:**
PEP 8 is Python's official style guide that provides conventions for writing readable code.

**Key points:**
- Use 4 spaces for indentation (not tabs)
- Line length should be max 79 characters
- Use lowercase with underscores for variables (`my_variable`)
- Use CamelCase for classes (`MyClass`)
- Use UPPERCASE for constants (`MAX_VALUE`)
- Add two blank lines before classes/functions

---

### Q4: Explain Python's memory management.

**Answer:**
Python uses automatic memory management with:

1. **Reference counting** — Tracks how many references point to each object
2. **Garbage collection** — Removes objects with zero references
3. **Memory pools** — Optimizes allocation for small objects

```python
import sys
x = [1, 2, 3]
print(sys.getrefcount(x))  # Shows reference count
```

---

### Q5: What is the difference between `is` and `==`?

**Answer:**
- `==` compares **values** (equality)
- `is` compares **identity** (same object in memory)

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (same values)
print(a is b)  # False (different objects)
print(a is c)  # True (same object)
```

---

## 📦 Data Types & Structures

### Q6: What are mutable vs immutable types?

**Answer:**

| Mutable | Immutable |
|---------|-----------|
| List | Tuple |
| Dictionary | String |
| Set | Integer, Float |

**Mutable:** Can be changed after creation
**Immutable:** Cannot be changed; new object created

```python
# Mutable
lst = [1, 2, 3]
lst[0] = 10  # Works

# Immutable
s = "hello"
s[0] = "H"  # Error!
```

---

### Q7: What is the difference between List and Tuple?

**Answer:**

| List | Tuple |
|------|-------|
| Mutable | Immutable |
| `[]` syntax | `()` syntax |
| Slower | Faster |
| More memory | Less memory |
| Use for dynamic data | Use for fixed data |

```python
my_list = [1, 2, 3]    # Can modify
my_tuple = (1, 2, 3)   # Cannot modify
```

---

### Q8: Explain dictionary comprehension.

**Answer:**
Dictionary comprehension creates dictionaries in a single line.

```python
# Basic syntax
{key: value for item in iterable}

# Examples
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# With condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
# {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}
```

---

### Q9: How does a set work internally?

**Answer:**
Sets use a **hash table** internally:
- Elements are hashed to determine their position
- Provides O(1) average lookup, insert, delete
- Elements must be hashable (immutable)
- No duplicates (hash collision handling)

```python
my_set = {1, 2, 3}
my_set.add(2)  # No duplicate added
print(my_set)  # {1, 2, 3}
```

---

## 🔧 Functions

### Q10: Explain `*args` and `**kwargs`.

**Answer:**
- `*args` — Collects positional arguments as a **tuple**
- `**kwargs` — Collects keyword arguments as a **dictionary**

```python
def func(*args, **kwargs):
    print(args)    # (1, 2, 3)
    print(kwargs)  # {'a': 4, 'b': 5}

func(1, 2, 3, a=4, b=5)
```

---

### Q11: What is a lambda function?

**Answer:**
A lambda is an anonymous, single-expression function.

```python
# Regular function
def add(x, y):
    return x + y

# Lambda equivalent
add = lambda x, y: x + y

# Common uses
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))
```

---

### Q12: Explain decorators with example.

**Answer:**
Decorators modify function behavior without changing its code.

```python
def timer(func):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Time: {time.time() - start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(1)
    return "Done"

slow_function()  # Prints: Time: 1.00xxs
```

---

### Q13: What is a closure?

**Answer:**
A closure is a function that remembers the environment in which it was created.

```python
def outer(x):
    def inner(y):
        return x + y  # x is remembered
    return inner

add_5 = outer(5)
print(add_5(3))  # 8
```

---

## 🏗️ OOP Concepts

### Q14: Explain inheritance types in Python.

**Answer:**

1. **Single Inheritance** — One parent class
2. **Multiple Inheritance** — Multiple parent classes
3. **Multilevel Inheritance** — Chain of inheritance
4. **Hierarchical Inheritance** — Multiple children, one parent

```python
# Multiple inheritance
class A:
    pass

class B:
    pass

class C(A, B):  # Inherits from both A and B
    pass
```

---

### Q15: What is the MRO (Method Resolution Order)?

**Answer:**
MRO determines the order in which base classes are searched when looking for a method.

Python uses **C3 linearization** algorithm.

```python
class A:
    def method(self):
        print("A")

class B(A):
    def method(self):
        print("B")

class C(A):
    def method(self):
        print("C")

class D(B, C):
    pass

print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

D().method()  # Prints: B
```

---

### Q16: Explain the difference between `__str__` and `__repr__`.

**Answer:**
- `__str__` — User-friendly string (for end users)
- `__repr__` — Unambiguous string (for developers/debugging)

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Point at ({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Point({self.x}, {self.y})"

p = Point(3, 4)
print(str(p))   # Point at (3, 4)
print(repr(p))  # Point(3, 4)
```

---

### Q17: What are class methods and static methods?

**Answer:**

```python
class MyClass:
    class_var = 0
    
    def instance_method(self):
        # Has access to instance (self)
        pass
    
    @classmethod
    def class_method(cls):
        # Has access to class (cls), not instance
        cls.class_var += 1
    
    @staticmethod
    def static_method():
        # No access to class or instance
        # Just a regular function in class namespace
        pass
```

---

## ⚡ Advanced Topics

### Q18: Explain generators and their benefits.

**Answer:**
Generators produce values on-demand using `yield`, saving memory.

```python
# Generator function
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1

# Memory efficient - generates one value at a time
for num in count_up_to(1000000):
    if num > 5:
        break
    print(num)

# Generator expression
squares = (x**2 for x in range(1000000))
```

**Benefits:**
- Memory efficient
- Lazy evaluation
- Can represent infinite sequences

---

### Q19: What is the Global Interpreter Lock (GIL)?

**Answer:**
The GIL is a mutex that protects access to Python objects, preventing multiple threads from executing Python bytecode simultaneously.

**Impact:**
- CPU-bound tasks don't benefit from threading
- Use `multiprocessing` for CPU parallelism
- I/O-bound tasks still benefit from threading

```python
# For CPU-bound work, use multiprocessing
from multiprocessing import Pool

def cpu_task(x):
    return x ** 2

with Pool(4) as p:
    results = p.map(cpu_task, range(10))
```

---

### Q20: Explain context managers.

**Answer:**
Context managers handle setup and cleanup using `with` statement.

```python
# Using built-in
with open('file.txt') as f:
    data = f.read()
# File automatically closed

# Custom context manager (class)
class MyContext:
    def __enter__(self):
        print("Setup")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Cleanup")
        return False

# Custom context manager (decorator)
from contextlib import contextmanager

@contextmanager
def my_context():
    print("Setup")
    yield
    print("Cleanup")
```

---

### Q21: What is monkey patching?

**Answer:**
Monkey patching is modifying classes or modules at runtime.

```python
# Original class
class Calculator:
    def add(self, a, b):
        return a + b

# Monkey patch
def new_add(self, a, b):
    print(f"Adding {a} + {b}")
    return a + b

Calculator.add = new_add

calc = Calculator()
calc.add(2, 3)  # Prints: Adding 2 + 3
```

**Use cases:** Testing, fixing bugs in third-party libraries
**Warning:** Can make code harder to debug

---

## 🌐 Web & APIs

### Q22: What is WSGI?

**Answer:**
WSGI (Web Server Gateway Interface) is a specification for how web servers communicate with Python web applications.

```python
# Simple WSGI application
def application(environ, start_response):
    status = '200 OK'
    headers = [('Content-Type', 'text/plain')]
    start_response(status, headers)
    return [b'Hello, World!']
```

**Popular WSGI servers:** Gunicorn, uWSGI

---

### Q23: Explain REST API principles.

**Answer:**
REST (Representational State Transfer) is an architectural style:

1. **Stateless** — Each request contains all needed info
2. **Client-Server** — Separation of concerns
3. **Uniform Interface** — Consistent URLs and methods
4. **Resource-Based** — Everything is a resource

| Method | Purpose |
|--------|---------|
| GET | Read |
| POST | Create |
| PUT | Replace |
| PATCH | Update |
| DELETE | Remove |

---

## 💡 Problem-Solving

### Q24: How would you reverse a string?

**Answer:**
```python
# Method 1: Slicing (most Pythonic)
s = "hello"
reversed_s = s[::-1]

# Method 2: reversed() function
reversed_s = ''.join(reversed(s))

# Method 3: Loop
reversed_s = ''
for char in s:
    reversed_s = char + reversed_s
```

---

### Q25: How do you find duplicates in a list?

**Answer:**
```python
def find_duplicates(lst):
    seen = set()
    duplicates = set()
    for item in lst:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return list(duplicates)

# Or using Counter
from collections import Counter
def find_duplicates_v2(lst):
    return [item for item, count in Counter(lst).items() if count > 1]
```

---

## 🎯 Behavioral Questions

### Q26: Tell me about a challenging Python project.

**Framework for answering:**
1. **Situation** — Describe the project context
2. **Task** — Your specific responsibility
3. **Action** — What you did, technologies used
4. **Result** — Outcome, metrics, learnings

---

### Q27: How do you stay updated with Python?

**Good answers include:**
- Following Python Enhancement Proposals (PEPs)
- Reading official documentation
- Contributing to open source
- Attending Python conferences (PyCon)
- Following Python blogs and newsletters
- Practicing on LeetCode/HackerRank

---

## 📊 Quick Reference Table

| Topic | Key Points |
|-------|------------|
| Mutable vs Immutable | Lists are mutable, tuples are not |
| `is` vs `==` | Identity vs equality comparison |
| `*args`, `**kwargs` | Variable arguments |
| GIL | Limits threading for CPU tasks |
| Decorators | Functions that modify functions |
| Generators | Lazy evaluation with yield |
| Context Managers | Automatic setup/cleanup |

---

## ✅ Interview Tips

1. **Think aloud** — Explain your reasoning
2. **Ask clarifying questions** — Show analytical thinking
3. **Start with brute force** — Then optimize
4. **Write clean code** — Use meaningful names
5. **Test your solution** — Check edge cases
6. **Know complexity** — Time and space analysis

---

*Good luck with your interviews!* 🚀
