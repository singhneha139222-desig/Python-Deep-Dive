# 📘 Comprehensions in Python

## 📌 Introduction

**Comprehensions** are concise, Pythonic ways to create collections (lists, sets, dicts, generators) from iterables. They combine loop and filter logic into a single readable line.

Think of comprehensions as **one-liner factory machines** — you feed in raw materials (iterables), apply transformations, and get a new collection.

```
┌─────────────────────────────────────────────────────────────┐
│                    COMPREHENSION TYPES                       │
│                                                              │
│   LIST         [ expression for item in iterable ]          │
│   [1,4,9]                                                    │
│                                                              │
│   SET          { expression for item in iterable }          │
│   {1,4,9}                                                    │
│                                                              │
│   DICT         { key: value for item in iterable }          │
│   {1:1, 2:4}                                                 │
│                                                              │
│   GENERATOR    ( expression for item in iterable )          │
│   <generator>                                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Why Comprehensions?

| Benefit | Description |
|---------|-------------|
| Concise | Replace 3-5 lines with 1 |
| Readable | Clear intent at a glance |
| Pythonic | Idiomatic Python style |
| Fast | Optimized internally |
| Functional | No side effects |

### Basic Syntax

```python
# List comprehension
[expression for item in iterable]

# With condition (filter)
[expression for item in iterable if condition]

# With transformation and filter
[transform(item) for item in iterable if condition]
```

---

## 💻 Code Examples

### 🟢 Basic: List Comprehension

```python
# Traditional loop
squares = []
for x in range(5):
    squares.append(x**2)
print(squares)  # [0, 1, 4, 9, 16]

# List comprehension (same result)
squares = [x**2 for x in range(5)]
print(squares)  # [0, 1, 4, 9, 16]

# More examples
numbers = [1, 2, 3, 4, 5]

# Double each number
doubled = [n * 2 for n in numbers]
print(doubled)  # [2, 4, 6, 8, 10]

# Convert to strings
strings = [str(n) for n in numbers]
print(strings)  # ['1', '2', '3', '4', '5']

# Uppercase words
words = ["hello", "world", "python"]
upper = [w.upper() for w in words]
print(upper)  # ['HELLO', 'WORLD', 'PYTHON']
```

### 🟢 Basic: With Condition (Filter)

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter: only even numbers
evens = [n for n in numbers if n % 2 == 0]
print(evens)  # [2, 4, 6, 8, 10]

# Filter: only positive
values = [-2, -1, 0, 1, 2, 3]
positives = [v for v in values if v > 0]
print(positives)  # [1, 2, 3]

# Filter: words with more than 4 letters
words = ["apple", "pie", "banana", "cat", "dog"]
long_words = [w for w in words if len(w) > 4]
print(long_words)  # ['apple', 'banana']

# Combine transform and filter
# Square of even numbers
even_squares = [n**2 for n in numbers if n % 2 == 0]
print(even_squares)  # [4, 16, 36, 64, 100]
```

### 🟢 Basic: If-Else in Comprehension

```python
numbers = [1, 2, 3, 4, 5]

# If-else (transformation) - different from filter!
# Syntax: [true_val if condition else false_val for item in iterable]
labels = ["even" if n % 2 == 0 else "odd" for n in numbers]
print(labels)  # ['odd', 'even', 'odd', 'even', 'odd']

# Absolute values
values = [-2, -1, 0, 1, 2]
absolute = [v if v >= 0 else -v for v in values]
print(absolute)  # [2, 1, 0, 1, 2]

# Replace negatives with 0
clipped = [v if v >= 0 else 0 for v in values]
print(clipped)  # [0, 0, 0, 1, 2]

# Combining if-else with filter
# Uppercase if starts with 'a', else original, but only long words
words = ["apple", "ant", "banana", "avocado", "pie"]
result = [w.upper() if w.startswith('a') else w for w in words if len(w) > 3]
print(result)  # ['APPLE', 'banana', 'AVOCADO']
```

### 🟡 Intermediate: Set Comprehension

```python
# Set comprehension - unique results
numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]

# Unique squares
squares = {n**2 for n in numbers}
print(squares)  # {1, 4, 9, 16}

# Unique characters in string
text = "mississippi"
unique_chars = {char for char in text}
print(unique_chars)  # {'m', 'i', 's', 'p'}

# Unique word lengths
words = ["apple", "banana", "pie", "cat", "avocado"]
lengths = {len(w) for w in words}
print(lengths)  # {3, 5, 6, 7}
```

### 🟡 Intermediate: Dictionary Comprehension

```python
# Basic dict comprehension
squares = {x: x**2 for x in range(6)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# From two lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
people = {name: age for name, age in zip(names, ages)}
print(people)  # {'Alice': 25, 'Bob': 30, 'Charlie': 35}

# Swap keys and values
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}
print(swapped)  # {1: 'a', 2: 'b', 3: 'c'}

# Filter dict
scores = {"Alice": 85, "Bob": 62, "Charlie": 90, "David": 55}
passed = {name: score for name, score in scores.items() if score >= 60}
print(passed)  # {'Alice': 85, 'Bob': 62, 'Charlie': 90}

# Transform values
doubled_scores = {name: score * 2 for name, score in scores.items()}
print(doubled_scores)

# Uppercase keys
upper_keys = {k.upper(): v for k, v in scores.items()}
print(upper_keys)  # {'ALICE': 85, 'BOB': 62, ...}
```

### 🟡 Intermediate: Generator Expression

```python
# Generator expression - lazy evaluation
# Uses () instead of []

# Memory efficient - doesn't create list
squares_gen = (x**2 for x in range(1000000))
print(type(squares_gen))  # <class 'generator'>

# Only computes as needed
print(next(squares_gen))  # 0
print(next(squares_gen))  # 1
print(next(squares_gen))  # 4

# Can iterate once
for i, sq in enumerate(squares_gen):
    if i > 5:
        break
    print(sq)

# Common use: sum, min, max
total = sum(x**2 for x in range(100))
print(total)  # 328350

# No extra memory for intermediate list!
# Compare:
# sum([x**2 for x in range(100)])  # Creates list first
# sum(x**2 for x in range(100))    # No list created

# Generator with condition
evens = (x for x in range(100) if x % 2 == 0)
```

### 🔴 Advanced: Nested Comprehensions

```python
# Nested list comprehension
# Equivalent to nested loops

# Flatten 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Traditional way
flat = []
for row in matrix:
    for num in row:
        flat.append(num)

# Comprehension way
flat = [num for row in matrix for num in row]
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Read order: outer loop first, then inner loop
# [num for row in matrix for num in row]
#      ^^^^^^^^^^^^^^^^^  ^^^^^^^^^^^
#         outer loop       inner loop

# Create 2D list
rows, cols = 3, 4
matrix = [[0 for _ in range(cols)] for _ in range(rows)]
print(matrix)  # [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

# Identity matrix
n = 3
identity = [[1 if i == j else 0 for j in range(n)] for i in range(n)]
print(identity)  # [[1, 0, 0], [0, 1, 0], [0, 0, 1]]

# Cartesian product
colors = ["red", "green"]
sizes = ["S", "M", "L"]
products = [(color, size) for color in colors for size in sizes]
print(products)
# [('red', 'S'), ('red', 'M'), ('red', 'L'),
#  ('green', 'S'), ('green', 'M'), ('green', 'L')]

# Filter in nested
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
evens = [n for row in matrix for n in row if n % 2 == 0]
print(evens)  # [2, 4, 6, 8]
```

### 🔴 Advanced: Complex Transformations

```python
# Multiple conditions
numbers = range(1, 51)

# Divisible by both 3 and 5
div_3_and_5 = [n for n in numbers if n % 3 == 0 if n % 5 == 0]
print(div_3_and_5)  # [15, 30, 45]

# Same as:
div_3_and_5 = [n for n in numbers if n % 3 == 0 and n % 5 == 0]

# Chained transformations
words = ["  Hello  ", "  World  ", "  Python  "]
cleaned = [word.strip().lower() for word in words]
print(cleaned)  # ['hello', 'world', 'python']

# With function calls
def process(n):
    return n ** 2 + 2 * n + 1

processed = [process(n) for n in range(5)]
print(processed)  # [1, 4, 9, 16, 25]

# Conditional function application
def safe_divide(a, b):
    return a / b if b != 0 else 0

pairs = [(10, 2), (8, 4), (6, 0), (4, 2)]
results = [safe_divide(a, b) for a, b in pairs]
print(results)  # [5.0, 2.0, 0, 2.0]
```

### 🔴 Advanced: Walrus Operator in Comprehension

```python
# Python 3.8+ walrus operator (:=)
# Assign and use in same expression

# Filter and transform expensive computation
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Without walrus - computes twice
# result = [expensive(x) for x in data if expensive(x) > 10]

# With walrus - compute once
import math
result = [y for x in data if (y := x ** 2 + math.sin(x)) > 10]
print(result)  # [9.14..., 15.24..., 24.65..., 35.99..., ...]

# Another example
text = "hello world this is a test"
long_words = [word for word in text.split() if (n := len(word)) > 3]
print(long_words)  # ['hello', 'world', 'this', 'test']
```

---

## 📊 Comprehension Patterns

| Pattern | Syntax | Example |
|---------|--------|---------|
| Basic | `[expr for x in iter]` | `[x*2 for x in nums]` |
| Filter | `[expr for x in iter if cond]` | `[x for x in nums if x>0]` |
| Transform | `[true if cond else false for x in iter]` | `["yes" if x else "no" for x in bools]` |
| Nested | `[expr for x in iter1 for y in iter2]` | `[(x,y) for x in a for y in b]` |
| Dict | `{k: v for x in iter}` | `{x: x**2 for x in range(5)}` |
| Set | `{expr for x in iter}` | `{x%10 for x in nums}` |
| Generator | `(expr for x in iter)` | `(x**2 for x in range(1000))` |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Wrong Order for If-Else vs Filter

```python
numbers = [1, 2, 3, 4, 5]

# FILTER (if at end) - excludes items
evens = [n for n in numbers if n % 2 == 0]
# Result: [2, 4]

# TRANSFORM (if-else before for) - transforms all items
labels = ["even" if n % 2 == 0 else "odd" for n in numbers]
# Result: ['odd', 'even', 'odd', 'even', 'odd']

# WRONG - syntax error
# wrong = [n if n % 2 == 0 for n in numbers]  # Missing else!
```

### ❌ Mistake 2: Too Complex Comprehensions

```python
# WRONG - too complex, hard to read
result = [transform(x) for x in data if condition1(x) and condition2(x) for y in process(x) if y > 0]

# CORRECT - use regular loop for complex logic
result = []
for x in data:
    if condition1(x) and condition2(x):
        for y in process(x):
            if y > 0:
                result.append(transform(x))
```

### ❌ Mistake 3: Side Effects in Comprehension

```python
# WRONG - don't use comprehension for side effects
[print(x) for x in range(5)]  # Works but bad practice

# CORRECT - use regular loop
for x in range(5):
    print(x)
```

### ❌ Mistake 4: Mutable Default in Matrix

```python
# WRONG - all rows share same list!
n = 3
matrix = [[0] * n] * n
matrix[0][0] = 1
print(matrix)  # [[1, 0, 0], [1, 0, 0], [1, 0, 0]] - All changed!

# CORRECT - use comprehension
matrix = [[0 for _ in range(n)] for _ in range(n)]
matrix[0][0] = 1
print(matrix)  # [[1, 0, 0], [0, 0, 0], [0, 0, 0]] - Only first row changed
```

---

## 💡 Pro Tips

### 1. Use `_` for Unused Variables

```python
# When you don't need the variable
zeros = [0 for _ in range(10)]
```

### 2. Generator for Large Data

```python
# Don't create huge lists
# BAD: sum([x**2 for x in range(10000000)])
# GOOD: 
sum(x**2 for x in range(10000000))
```

### 3. Use enumerate() and zip()

```python
# With enumerate
indexed = [(i, x**2) for i, x in enumerate(range(5))]

# With zip
pairs = [(a, b) for a, b in zip(list1, list2)]
```

### 4. Chain Methods

```python
# Clean and transform in one go
words = ["  Hello  ", "WORLD", "  PythON  "]
clean = [w.strip().lower().capitalize() for w in words]
# ['Hello', 'World', 'Python']
```

---

## 💼 Interview Insight

### Q1: When to use comprehension vs loop?

> **Use comprehension when:**
> - Creating a new collection from existing one
> - Logic is simple (1-2 conditions)
> - No complex side effects
>
> **Use loop when:**
> - Complex logic with multiple steps
> - Need to modify existing data
> - Comprehension becomes unreadable

### Q2: List comprehension vs map/filter?

```python
# Both work, comprehension more Pythonic
# List comp
squares = [x**2 for x in range(10)]
evens = [x for x in range(10) if x % 2 == 0]

# map/filter
squares = list(map(lambda x: x**2, range(10)))
evens = list(filter(lambda x: x % 2 == 0, range(10)))
```

### Q3: Performance of comprehensions?

> Comprehensions are generally **faster** than equivalent loops because:
> - Optimized at bytecode level
> - No repeated method lookups
> - Typically ~10-30% faster

---

## 🔍 Real-world Use Cases

### 1. Data Cleaning

```python
raw_data = ["  Alice  ", "", "Bob", "  ", "Charlie"]
cleaned = [name.strip() for name in raw_data if name.strip()]
# ['Alice', 'Bob', 'Charlie']
```

### 2. File Processing

```python
# Read and filter lines
with open("data.txt") as f:
    non_empty = [line.strip() for line in f if line.strip()]
```

### 3. JSON Transformation

```python
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 30, "active": False},
    {"name": "Charlie", "age": 35, "active": True}
]

# Get active user names
active_names = [u["name"] for u in users if u["active"]]
# ['Alice', 'Charlie']

# Create lookup dict
name_to_age = {u["name"]: u["age"] for u in users}
# {'Alice': 25, 'Bob': 30, 'Charlie': 35}
```

### 4. Matrix Operations

```python
# Transpose matrix
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
# [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

---

## 🧪 Practice Questions

### Easy:
1. Create a list of squares from 1 to 20
2. Filter words that start with 'a' from a list
3. Create a dict mapping numbers to their cubes

### Medium:
4. Flatten a 2D list using comprehension
5. Find all palindromes in a list of words
6. Create a multiplication table using nested comprehension

### Hard:
7. Prime number sieve using comprehension
8. Group anagrams from a list of words
9. Matrix multiplication using comprehensions

---

## 🚀 Mini Tasks

### Task 1: Word Analysis

```python
text = "The quick brown fox jumps over the lazy dog"
# Create:
# 1. List of word lengths
# 2. Set of unique first letters
# 3. Dict mapping word to length (only for words > 3 chars)
```

### Task 2: Number Grid

```python
# Create a 5x5 grid where each cell contains:
# - "*" if on the diagonal
# - row number otherwise
# Expected:
# [['*', 1, 1, 1, 1],
#  [2, '*', 2, 2, 2],
#  [3, 3, '*', 3, 3],
#  [4, 4, 4, '*', 4],
#  [5, 5, 5, 5, '*']]
```

---

## 📖 Summary

| Type | Syntax | Returns |
|------|--------|---------|
| List | `[expr for x in iter]` | list |
| Set | `{expr for x in iter}` | set |
| Dict | `{k: v for x in iter}` | dict |
| Generator | `(expr for x in iter)` | generator |
| Filter | `[expr for x in iter if cond]` | filtered |
| Transform | `[a if cond else b for x in iter]` | transformed |

---

## ⏭️ Next Module

**[Module 5: Object-Oriented Programming](../05_Object_Oriented_Programming/README.md)** — Classes, objects, and OOP principles!

---

*Comprehensions: Write less, do more!* ✨
