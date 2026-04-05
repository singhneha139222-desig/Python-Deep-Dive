# 📘 Tuples in Python

## 📌 Introduction

A **tuple** is an ordered, immutable (unchangeable) collection in Python. Once created, you cannot modify its contents.

Think of a tuple like a **sealed envelope** — you can see what's inside, but you cannot change it.

```
┌─────────────────────────────────────────────────────────────┐
│                        TUPLE                                 │
│                                                              │
│   Index:    0        1        2        3        4           │
│           ┌────┐   ┌────┐   ┌────┐   ┌────┐   ┌────┐       │
│           │ 10 │   │ 20 │   │ 30 │   │ 40 │   │ 50 │ 🔒    │
│           └────┘   └────┘   └────┘   └────┘   └────┘       │
│                                                              │
│   ✓ Ordered (index-based)                                   │
│   ✗ IMMUTABLE (cannot change)                               │
│   ✓ Allows duplicates                                       │
│   ✓ Mixed types allowed                                     │
│   ✓ Hashable (can be dict key)                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Why Use Tuples?

1. **Data Integrity** — Cannot be accidentally modified
2. **Hashable** — Can be used as dictionary keys
3. **Performance** — Slightly faster than lists
4. **Memory** — Uses less memory than lists
5. **Semantic Meaning** — Signals "this should not change"

### Tuples vs Lists

| Feature | Tuple | List |
|---------|-------|------|
| Syntax | `(1, 2, 3)` | `[1, 2, 3]` |
| Mutable | ❌ No | ✅ Yes |
| Methods | Limited | Many |
| Use as dict key | ✅ Yes | ❌ No |
| Performance | Faster | Slower |
| Memory | Less | More |
| Use case | Fixed data | Dynamic data |

---

## 💻 Code Examples

### 🟢 Basic: Creating Tuples

```python
# Empty tuple
empty = ()
empty = tuple()

# Tuple with values
numbers = (1, 2, 3, 4, 5)
fruits = ("apple", "banana", "cherry")
mixed = (1, "hello", 3.14, True)

# Single element tuple - MUST have comma!
single = (42,)    # Correct - tuple
not_tuple = (42)  # Wrong - just integer 42

print(type(single))     # <class 'tuple'>
print(type(not_tuple))  # <class 'int'>

# Without parentheses (tuple packing)
coordinates = 10, 20, 30
print(type(coordinates))  # <class 'tuple'>

# From other iterables
from_list = tuple([1, 2, 3])
from_string = tuple("hello")  # ('h', 'e', 'l', 'l', 'o')
```

### 🟢 Basic: Accessing Elements

```python
fruits = ("apple", "banana", "cherry", "date", "elderberry")

# Indexing (same as lists)
print(fruits[0])    # apple
print(fruits[2])    # cherry
print(fruits[-1])   # elderberry

# Slicing (same as lists)
print(fruits[1:4])   # ('banana', 'cherry', 'date')
print(fruits[:3])    # ('apple', 'banana', 'cherry')
print(fruits[2:])    # ('cherry', 'date', 'elderberry')
print(fruits[::-1])  # Reversed tuple

# Length
print(len(fruits))  # 5

# Membership
print("banana" in fruits)  # True
print("mango" in fruits)   # False
```

### 🟢 Basic: Tuple Methods

```python
# Tuples have only 2 methods!

numbers = (1, 2, 3, 2, 4, 2, 5)

# count() - count occurrences
print(numbers.count(2))  # 3

# index() - find position
print(numbers.index(4))  # 4 (first occurrence)
```

### 🟡 Intermediate: Tuple Unpacking

```python
# Basic unpacking
coordinates = (10, 20, 30)
x, y, z = coordinates
print(f"x={x}, y={y}, z={z}")  # x=10, y=20, z=30

# Swap values (Pythonic way)
a, b = 5, 10
a, b = b, a
print(f"a={a}, b={b}")  # a=10, b=5

# Unpacking with *
numbers = (1, 2, 3, 4, 5)
first, *middle, last = numbers
print(f"First: {first}")   # 1
print(f"Middle: {middle}") # [2, 3, 4] - becomes list!
print(f"Last: {last}")     # 5

# Ignore values with _
data = ("Alice", 25, "Developer", "New York")
name, _, job, _ = data  # Ignore age and city
print(f"{name} is a {job}")

# Nested unpacking
point = ((10, 20), (30, 40))
(x1, y1), (x2, y2) = point
print(f"Points: ({x1},{y1}) to ({x2},{y2})")
```

### 🟡 Intermediate: Function Returns

```python
# Functions often return tuples
def get_min_max(numbers):
    return min(numbers), max(numbers)  # Returns tuple

data = [5, 2, 8, 1, 9]
result = get_min_max(data)
print(result)  # (1, 9)

# Unpack directly
minimum, maximum = get_min_max(data)
print(f"Min: {minimum}, Max: {maximum}")

# divmod() returns tuple
quotient, remainder = divmod(17, 5)
print(f"17 ÷ 5 = {quotient} remainder {remainder}")

# enumerate() returns tuples
for index, value in enumerate(["a", "b", "c"]):
    print(f"{index}: {value}")
```

### 🟡 Intermediate: Tuple as Dictionary Key

```python
# Tuples can be dictionary keys (lists cannot!)

# Location-based data
locations = {
    (40.7128, -74.0060): "New York",
    (51.5074, -0.1278): "London",
    (35.6762, 139.6503): "Tokyo"
}

coords = (40.7128, -74.0060)
print(locations[coords])  # New York

# Grid-based game
game_board = {}
game_board[(0, 0)] = "Player"
game_board[(1, 2)] = "Enemy"
game_board[(3, 3)] = "Treasure"

# Memoization with tuple keys
cache = {}
def fibonacci(n):
    if n in cache:
        return cache[n]
    if n <= 1:
        return n
    result = fibonacci(n-1) + fibonacci(n-2)
    cache[n] = result
    return result
```

### 🔴 Advanced: Named Tuples

```python
from collections import namedtuple

# Create a named tuple class
Point = namedtuple('Point', ['x', 'y'])
Person = namedtuple('Person', 'name age city')  # Space-separated also works

# Create instances
p1 = Point(10, 20)
p2 = Point(x=30, y=40)

print(p1.x, p1.y)  # 10 20
print(p2)          # Point(x=30, y=40)

# Still supports indexing
print(p1[0])  # 10

# Convert to dict
print(p1._asdict())  # {'x': 10, 'y': 20}

# Create from iterable
data = [50, 60]
p3 = Point._make(data)
print(p3)  # Point(x=50, y=60)

# Replace values (creates new tuple)
p4 = p1._replace(x=100)
print(p4)  # Point(x=100, y=20)

# Person example
alice = Person("Alice", 25, "New York")
print(f"{alice.name} is {alice.age} from {alice.city}")
```

### 🔴 Advanced: Tuple Internals

```python
# Memory comparison
import sys

my_list = [1, 2, 3, 4, 5]
my_tuple = (1, 2, 3, 4, 5)

print(f"List size: {sys.getsizeof(my_list)} bytes")   # ~96 bytes
print(f"Tuple size: {sys.getsizeof(my_tuple)} bytes") # ~80 bytes

# Performance comparison
import timeit

# Creation
list_time = timeit.timeit('[1, 2, 3, 4, 5]', number=1000000)
tuple_time = timeit.timeit('(1, 2, 3, 4, 5)', number=1000000)
print(f"List creation: {list_time:.4f}s")
print(f"Tuple creation: {tuple_time:.4f}s")

# Tuple constant folding (compile-time optimization)
# Tuples with constants are stored once
def example():
    return (1, 2, 3)  # Same tuple object each call
```

### 🔴 Advanced: Tuple with Mutable Elements

```python
# Tuples can contain mutable objects!
# The reference is immutable, not the object itself

t = ([1, 2], [3, 4])
print(t)  # ([1, 2], [3, 4])

# Cannot reassign
# t[0] = [5, 6]  # TypeError!

# BUT can modify the list inside
t[0].append(999)
print(t)  # ([1, 2, 999], [3, 4])

# This is a common gotcha!
# Be careful with mutable elements in tuples
```

---

## 📊 Tuple Operations Reference

| Operation | Example | Description |
|-----------|---------|-------------|
| Create | `(1, 2, 3)` | Parentheses or just commas |
| Single | `(42,)` | Must have trailing comma |
| Index | `t[0]` | Access by position |
| Slice | `t[1:3]` | Get subset |
| Concat | `t1 + t2` | Combine tuples |
| Repeat | `t * 3` | Repeat tuple |
| Length | `len(t)` | Number of elements |
| Count | `t.count(x)` | Occurrences of x |
| Index | `t.index(x)` | Position of x |
| Unpack | `a, b = t` | Assign to variables |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Single Element Without Comma

```python
# WRONG
single = (42)  # This is just the integer 42!
print(type(single))  # <class 'int'>

# CORRECT
single = (42,)  # Note the comma
print(type(single))  # <class 'tuple'>

# Also correct
single = 42,
```

### ❌ Mistake 2: Trying to Modify Tuple

```python
# WRONG
t = (1, 2, 3)
t[0] = 10  # TypeError: 'tuple' object does not support item assignment

# WORKAROUND: Convert to list, modify, convert back
t = (1, 2, 3)
temp = list(t)
temp[0] = 10
t = tuple(temp)
print(t)  # (10, 2, 3)
```

### ❌ Mistake 3: Unpack Mismatch

```python
# WRONG
data = (1, 2, 3)
a, b = data  # ValueError: too many values to unpack

# CORRECT
a, b, c = data

# Or use * to catch extra
a, *rest = data
print(a, rest)  # 1 [2, 3]
```

---

## 💡 Pro Tips

### 1. Use Tuples for Heterogeneous Data

```python
# Good: Different types of related data
person = ("Alice", 25, "Engineer")

# Better: Use named tuple for clarity
from collections import namedtuple
Person = namedtuple('Person', 'name age job')
person = Person("Alice", 25, "Engineer")
```

### 2. Return Multiple Values from Functions

```python
def analyze(numbers):
    return min(numbers), max(numbers), sum(numbers)/len(numbers)

low, high, avg = analyze([1, 2, 3, 4, 5])
```

### 3. Use Tuple as Dictionary Key

```python
# Store data by coordinates
grid = {(x, y): value for x in range(3) for y in range(3) for value in [0]}
```

### 4. Implicit Tuple Packing

```python
# No parentheses needed
a = 1, 2, 3  # Same as (1, 2, 3)
```

---

## 💼 Interview Insight

### Q1: Why use tuple instead of list?

> 1. **Immutability** — Data won't accidentally change
> 2. **Hashable** — Can use as dict key or set element
> 3. **Performance** — Faster iteration and smaller memory
> 4. **Semantics** — Indicates "this is fixed data"

### Q2: Can tuple contain mutable objects?

> Yes! The tuple itself is immutable (can't add/remove/reassign elements), but if it contains a mutable object like a list, that list can be modified.

### Q3: How to convert tuple to list and vice versa?

```python
t = (1, 2, 3)
l = list(t)      # [1, 2, 3]
t2 = tuple(l)    # (1, 2, 3)
```

---

## 🔍 Real-world Use Cases

### 1. Database Records

```python
# Each row is a tuple
employees = [
    (1, "Alice", "Engineering", 75000),
    (2, "Bob", "Marketing", 65000),
    (3, "Charlie", "Sales", 70000)
]
```

### 2. Geographic Coordinates

```python
locations = {
    "office": (40.7128, -74.0060),
    "warehouse": (40.7580, -73.9855)
}
```

### 3. Configuration Constants

```python
WINDOW_SIZE = (800, 600)
RGB_WHITE = (255, 255, 255)
API_LIMITS = (100, 1000, 10000)  # Free, Basic, Pro
```

---

## 🧪 Practice Questions

### Easy:
1. Create a tuple with 5 different data types
2. Swap two variables using tuple unpacking
3. Find the largest element in a tuple

### Medium:
4. Count occurrences of each element in a tuple
5. Merge two tuples and sort the result
6. Create a function that returns first and last element

### Hard:
7. Implement a simple point class using named tuple
8. Find all tuples in a list where sum equals target
9. Create an immutable record system using named tuples

---

## 🚀 Mini Tasks

### Task 1: Student Records
```python
# Create tuples for 3 students: (name, age, grade)
# Find the student with highest grade
# Print all students who passed (grade >= 60)
```

### Task 2: Coordinate Operations
```python
# Create two points as tuples
# Calculate distance between them
# Find the midpoint
```

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Creation | `()` or `tuple()`, single needs comma |
| Immutable | Cannot change after creation |
| Unpacking | `a, b, c = tuple` |
| Use case | Fixed data, dict keys, function returns |
| Named tuple | `namedtuple` for readable tuples |
| Methods | Only `count()` and `index()` |

---

## ⏭️ Next Topic

**[Sets](./03_Sets.md)** — Unique elements and set operations!

---

*Tuples: When your data should stay put!* 🔒
