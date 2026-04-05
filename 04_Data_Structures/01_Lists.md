# 📘 Lists in Python

## 📌 Introduction

A **list** is Python's most versatile data structure. It's an ordered, mutable (changeable) collection that can hold items of any type.

Think of a list like a **shopping list** — you can add items, remove items, change items, and the order matters.

```
┌─────────────────────────────────────────────────────────────┐
│                        LIST                                  │
│                                                              │
│   Index:    0        1        2        3        4           │
│           ┌────┐   ┌────┐   ┌────┐   ┌────┐   ┌────┐       │
│           │ 10 │   │ 20 │   │ 30 │   │ 40 │   │ 50 │       │
│           └────┘   └────┘   └────┘   └────┘   └────┘       │
│   Neg:     -5       -4       -3       -2       -1          │
│                                                              │
│   ✓ Ordered (index-based)                                   │
│   ✓ Mutable (can change)                                    │
│   ✓ Allows duplicates                                       │
│   ✓ Mixed types allowed                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Creating Lists

```python
# Empty list
empty = []
empty = list()

# List with values
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "cherry"]
mixed = [1, "hello", 3.14, True]  # Mixed types

# List from other iterables
from_string = list("hello")  # ['h', 'e', 'l', 'l', 'o']
from_range = list(range(5))  # [0, 1, 2, 3, 4]
```

### 2. List Indexing

| Index Type | Example | Result |
|------------|---------|--------|
| Positive | `list[0]` | First element |
| Positive | `list[2]` | Third element |
| Negative | `list[-1]` | Last element |
| Negative | `list[-2]` | Second to last |

### 3. List Slicing

```python
# Syntax: list[start:stop:step]
# start: inclusive, stop: exclusive
```

| Slice | Meaning | Example |
|-------|---------|---------|
| `[1:4]` | Index 1 to 3 | Elements 2nd-4th |
| `[:3]` | Start to index 2 | First 3 elements |
| `[2:]` | Index 2 to end | From 3rd element |
| `[::2]` | Every 2nd element | Skip one |
| `[::-1]` | Reverse | All reversed |

---

## 💻 Code Examples

### 🟢 Basic: Creating and Accessing

```python
# Create a list
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

# Access by index
print(f"First: {fruits[0]}")    # apple
print(f"Third: {fruits[2]}")    # cherry
print(f"Last: {fruits[-1]}")    # elderberry
print(f"Second last: {fruits[-2]}")  # date

# Length
print(f"Length: {len(fruits)}")  # 5

# Check membership
print(f"'banana' in list: {'banana' in fruits}")  # True
print(f"'mango' in list: {'mango' in fruits}")    # False
```

### 🟢 Basic: Slicing

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Basic slicing
print(numbers[2:5])    # [2, 3, 4]
print(numbers[:4])     # [0, 1, 2, 3]
print(numbers[6:])     # [6, 7, 8, 9]

# With step
print(numbers[::2])    # [0, 2, 4, 6, 8] - every 2nd
print(numbers[1::2])   # [1, 3, 5, 7, 9] - odd indices
print(numbers[::3])    # [0, 3, 6, 9] - every 3rd

# Negative slicing
print(numbers[-3:])    # [7, 8, 9] - last 3
print(numbers[:-2])    # [0, 1, 2, 3, 4, 5, 6, 7] - except last 2

# Reverse
print(numbers[::-1])   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

### 🟡 Intermediate: Modifying Lists

```python
# Modify by index
fruits = ["apple", "banana", "cherry"]
fruits[1] = "blueberry"
print(fruits)  # ['apple', 'blueberry', 'cherry']

# Modify by slice
numbers = [0, 1, 2, 3, 4, 5]
numbers[2:4] = [20, 30]
print(numbers)  # [0, 1, 20, 30, 4, 5]

# Insert via slice
numbers[2:2] = [100, 200]  # Insert at index 2
print(numbers)  # [0, 1, 100, 200, 20, 30, 4, 5]

# Delete via slice
numbers[2:4] = []
print(numbers)  # [0, 1, 20, 30, 4, 5]
```

### 🟡 Intermediate: List Methods

```python
fruits = ["apple", "banana"]

# Adding elements
fruits.append("cherry")        # Add to end
print(fruits)  # ['apple', 'banana', 'cherry']

fruits.insert(1, "avocado")    # Insert at index
print(fruits)  # ['apple', 'avocado', 'banana', 'cherry']

fruits.extend(["date", "elderberry"])  # Add multiple
print(fruits)  # ['apple', 'avocado', 'banana', 'cherry', 'date', 'elderberry']

# Removing elements
fruits.remove("avocado")       # Remove by value
print(fruits)  # ['apple', 'banana', 'cherry', 'date', 'elderberry']

popped = fruits.pop()          # Remove last, return it
print(f"Popped: {popped}, List: {fruits}")

popped = fruits.pop(1)         # Remove at index
print(f"Popped: {popped}, List: {fruits}")

del fruits[0]                  # Delete by index
print(fruits)  # ['cherry', 'date']

# Other methods
fruits = ["cherry", "apple", "banana", "apple"]
print(f"Count 'apple': {fruits.count('apple')}")  # 2
print(f"Index of 'banana': {fruits.index('banana')}")  # 2

fruits.sort()                  # Sort in place
print(f"Sorted: {fruits}")  # ['apple', 'apple', 'banana', 'cherry']

fruits.reverse()               # Reverse in place
print(f"Reversed: {fruits}")  # ['cherry', 'banana', 'apple', 'apple']

fruits.clear()                 # Remove all
print(f"Cleared: {fruits}")  # []
```

### 🟡 Intermediate: List Operations

```python
# Concatenation
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]

# Repetition
repeated = [0] * 5
print(repeated)  # [0, 0, 0, 0, 0]

pattern = [1, 2] * 3
print(pattern)  # [1, 2, 1, 2, 1, 2]

# Iteration
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(f"I like {fruit}")

# Iteration with index
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")

# List unpacking
first, *middle, last = [1, 2, 3, 4, 5]
print(f"First: {first}, Middle: {middle}, Last: {last}")
# First: 1, Middle: [2, 3, 4], Last: 5
```

### 🔴 Advanced: Nested Lists (2D Lists)

```python
# Create 2D list (matrix)
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Access elements
print(matrix[0])      # [1, 2, 3] - first row
print(matrix[0][1])   # 2 - row 0, column 1
print(matrix[2][2])   # 9 - row 2, column 2

# Iterate over 2D list
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()

# Flatten 2D list
flat = [num for row in matrix for num in row]
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create NxM matrix of zeros
n, m = 3, 4
zeros = [[0 for _ in range(m)] for _ in range(n)]
print(zeros)  # [[0,0,0,0], [0,0,0,0], [0,0,0,0]]
```

### 🔴 Advanced: Copy vs Reference

```python
# Shallow copy vs reference
original = [1, 2, [3, 4]]

# This is a REFERENCE (same object!)
reference = original
reference[0] = 100
print(original)  # [100, 2, [3, 4]] - Changed!

# Shallow copy methods
original = [1, 2, [3, 4]]
copy1 = original.copy()
copy2 = original[:]
copy3 = list(original)

copy1[0] = 100
print(original)  # [1, 2, [3, 4]] - Unchanged

# BUT nested objects are still shared!
copy1[2][0] = 999
print(original)  # [1, 2, [999, 4]] - Nested changed!

# Deep copy (for nested structures)
import copy
original = [1, 2, [3, 4]]
deep = copy.deepcopy(original)
deep[2][0] = 999
print(original)  # [1, 2, [3, 4]] - Unchanged!
```

### 🔴 Advanced: Sorting

```python
# Basic sort
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
numbers.sort()  # In-place
print(numbers)  # [1, 1, 2, 3, 4, 5, 6, 9]

# Sort descending
numbers.sort(reverse=True)
print(numbers)  # [9, 6, 5, 4, 3, 2, 1, 1]

# sorted() - returns new list
original = [3, 1, 4, 1, 5]
sorted_list = sorted(original)
print(original)     # [3, 1, 4, 1, 5] - Unchanged
print(sorted_list)  # [1, 1, 3, 4, 5]

# Custom sort key
words = ["apple", "pie", "banana", "a"]
words.sort(key=len)  # Sort by length
print(words)  # ['a', 'pie', 'apple', 'banana']

# Sort objects
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]
students.sort(key=lambda x: x["grade"], reverse=True)
print([s["name"] for s in students])  # ['Bob', 'Alice', 'Charlie']

# Multiple sort criteria
data = [("Alice", 25), ("Bob", 30), ("Alice", 20)]
data.sort(key=lambda x: (x[0], x[1]))  # By name, then age
print(data)  # [('Alice', 20), ('Alice', 25), ('Bob', 30)]
```

---

## 📊 List Methods Reference

| Method | Description | Returns |
|--------|-------------|---------|
| `append(x)` | Add x to end | None |
| `extend(iter)` | Add all items from iterable | None |
| `insert(i, x)` | Insert x at index i | None |
| `remove(x)` | Remove first x | None |
| `pop([i])` | Remove at index, return it | Element |
| `clear()` | Remove all items | None |
| `index(x)` | Index of first x | int |
| `count(x)` | Count occurrences of x | int |
| `sort()` | Sort in place | None |
| `reverse()` | Reverse in place | None |
| `copy()` | Shallow copy | list |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Modifying While Iterating

```python
# WRONG
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Dangerous!
print(numbers)  # [1, 3, 5] - may miss items!

# CORRECT - iterate over copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:  # Copy
    if num % 2 == 0:
        numbers.remove(num)

# BETTER - use list comprehension
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 != 0]
```

### ❌ Mistake 2: Using `=` Instead of `.copy()`

```python
# WRONG
a = [1, 2, 3]
b = a  # b is same object!
b.append(4)
print(a)  # [1, 2, 3, 4] - a also changed!

# CORRECT
a = [1, 2, 3]
b = a.copy()  # b is a new list
```

### ❌ Mistake 3: Index Out of Range

```python
# WRONG
fruits = ["apple", "banana"]
print(fruits[5])  # IndexError!

# CORRECT - check first
if len(fruits) > 5:
    print(fruits[5])
```

---

## 💡 Pro Tips

### 1. Use List Comprehension
```python
# Instead of loop
squares = [x**2 for x in range(10)]
```

### 2. Use `enumerate()` for Index
```python
for i, item in enumerate(items):
    print(f"{i}: {item}")
```

### 3. Use `zip()` for Parallel Iteration
```python
for name, age in zip(names, ages):
    print(f"{name} is {age}")
```

### 4. Unpack with `*`
```python
first, *rest = [1, 2, 3, 4, 5]
# first = 1, rest = [2, 3, 4, 5]
```

---

## 💼 Interview Insight

### Q1: What is the time complexity of list operations?
> - `append()`: O(1) amortized
> - `insert()`: O(n)
> - `pop()` from end: O(1)
> - `pop(i)`: O(n)
> - `x in list`: O(n)
> - `sort()`: O(n log n)

### Q2: Difference between `append()` and `extend()`?
> - `append(x)`: Adds x as a single element
> - `extend(iter)`: Adds all elements from iterable

### Q3: How to reverse a list?
```python
list.reverse()      # In-place
reversed(list)      # Iterator
list[::-1]          # New list
```

---

## 🧪 Practice Questions

### Easy:
1. Create a list of squares from 1 to 10
2. Find the sum and average of a list
3. Remove duplicates from a list

### Medium:
4. Rotate a list by k positions
5. Find the second largest element
6. Merge two sorted lists

### Hard:
7. Implement a stack using a list
8. Find all pairs that sum to a target
9. Flatten a deeply nested list

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Creation | `[]`, `list()`, comprehension |
| Indexing | Positive (0+) and negative (-1) |
| Slicing | `[start:stop:step]` |
| Mutable | Can add, remove, modify |
| Methods | append, extend, pop, sort, etc. |
| Copy | Use `.copy()` or `[:]` |

---

## ⏭️ Next Topic

**[Tuples](./02_Tuples.md)** — Immutable sequences!

---

*Lists: The Swiss Army knife of Python!* 🔧
