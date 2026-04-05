# 📘 Lambda Functions

## 📌 Introduction

A **lambda function** is a small, anonymous (unnamed) function defined in a single line. It's perfect for simple operations where defining a full function would be overkill.

Think of lambda as a **shortcut** for simple functions:

```python
# Regular function
def add(x, y):
    return x + y

# Lambda equivalent
add = lambda x, y: x + y
```

```
┌─────────────────────────────────────────────────────────────┐
│                    LAMBDA SYNTAX                             │
│                                                              │
│   lambda arguments: expression                               │
│          ↓              ↓                                    │
│      input(s)      what to return                           │
│                                                              │
│   Examples:                                                  │
│   lambda x: x * 2           # Double a number               │
│   lambda x, y: x + y        # Add two numbers               │
│   lambda: "Hello"           # No arguments                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Lambda vs Regular Functions

| Feature | Lambda | Regular Function |
|---------|--------|------------------|
| Name | Anonymous (unless assigned) | Has a name |
| Size | Single expression | Multiple statements |
| Return | Implicit (expression result) | Explicit `return` |
| Use case | Simple operations | Complex logic |
| Readability | Good for short operations | Better for complex operations |

### 2. When to Use Lambda

✅ **Good for:**
- Simple transformations
- Short callbacks
- With map(), filter(), sorted()
- Quick one-time operations

❌ **Avoid when:**
- Logic is complex
- Multiple statements needed
- Documentation required
- Reused multiple times

---

## 💻 Code Examples

### 🟢 Basic: Simple Lambda Functions

```python
# Lambda with one argument
double = lambda x: x * 2
print(f"Double of 5: {double(5)}")  # 10

# Lambda with two arguments
add = lambda a, b: a + b
print(f"5 + 3 = {add(5, 3)}")  # 8

# Lambda with no arguments
greet = lambda: "Hello, World!"
print(greet())  # Hello, World!

# Lambda with default argument
greet_person = lambda name="Guest": f"Hello, {name}!"
print(greet_person())           # Hello, Guest!
print(greet_person("Alice"))    # Hello, Alice!
```

**Output:**
```
Double of 5: 10
5 + 3 = 8
Hello, World!
Hello, Guest!
Hello, Alice!
```

### 🟢 Basic: Lambda as Inline Function

```python
# Instead of assigning, use directly
print((lambda x: x ** 2)(5))  # 25

# In a list
operations = [
    lambda x: x + 1,
    lambda x: x * 2,
    lambda x: x ** 2
]

num = 5
for op in operations:
    print(f"Result: {op(num)}")
```

**Output:**
```
25
Result: 6
Result: 10
Result: 25
```

---

## 🔄 Lambda with Built-in Functions

### 🟡 Intermediate: map() Function

`map()` applies a function to every item in an iterable.

```python
# Syntax: map(function, iterable)

numbers = [1, 2, 3, 4, 5]

# Double each number
doubled = list(map(lambda x: x * 2, numbers))
print(f"Doubled: {doubled}")  # [2, 4, 6, 8, 10]

# Square each number
squared = list(map(lambda x: x ** 2, numbers))
print(f"Squared: {squared}")  # [1, 4, 9, 16, 25]

# Convert to strings
strings = list(map(lambda x: str(x), numbers))
print(f"Strings: {strings}")  # ['1', '2', '3', '4', '5']

# With multiple iterables
a = [1, 2, 3]
b = [4, 5, 6]
sums = list(map(lambda x, y: x + y, a, b))
print(f"Sums: {sums}")  # [5, 7, 9]
```

### 🟡 Intermediate: filter() Function

`filter()` keeps items where the function returns True.

```python
# Syntax: filter(function, iterable)

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Keep only even numbers
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(f"Evens: {evens}")  # [2, 4, 6, 8, 10]

# Keep numbers greater than 5
greater = list(filter(lambda x: x > 5, numbers))
print(f"Greater than 5: {greater}")  # [6, 7, 8, 9, 10]

# Filter non-empty strings
words = ["hello", "", "world", "", "python"]
non_empty = list(filter(lambda x: x, words))
print(f"Non-empty: {non_empty}")  # ['hello', 'world', 'python']

# Filter by length
long_words = list(filter(lambda x: len(x) > 4, words))
print(f"Long words: {long_words}")  # ['hello', 'world', 'python']
```

### 🟡 Intermediate: sorted() with Lambda

`sorted()` uses key function to determine sort order.

```python
# Sort by custom criteria

# Sort numbers by absolute value
numbers = [-5, 2, -8, 1, 7, -3]
sorted_abs = sorted(numbers, key=lambda x: abs(x))
print(f"Sorted by absolute: {sorted_abs}")  # [1, 2, -3, -5, 7, -8]

# Sort strings by length
words = ["apple", "pie", "banana", "a", "cherry"]
sorted_len = sorted(words, key=lambda x: len(x))
print(f"Sorted by length: {sorted_len}")  # ['a', 'pie', 'apple', 'banana', 'cherry']

# Sort dictionaries by value
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]
sorted_grade = sorted(students, key=lambda x: x["grade"], reverse=True)
print("Sorted by grade:")
for s in sorted_grade:
    print(f"  {s['name']}: {s['grade']}")
```

**Output:**
```
Sorted by absolute: [1, 2, -3, -5, 7, -8]
Sorted by length: ['a', 'pie', 'apple', 'banana', 'cherry']
Sorted by grade:
  Bob: 92
  Alice: 85
  Charlie: 78
```

### 🟡 Intermediate: reduce() Function

`reduce()` applies a function cumulatively to reduce to single value.

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# Sum all numbers
total = reduce(lambda a, b: a + b, numbers)
print(f"Sum: {total}")  # 15

# Multiply all numbers
product = reduce(lambda a, b: a * b, numbers)
print(f"Product: {product}")  # 120

# Find maximum
maximum = reduce(lambda a, b: a if a > b else b, numbers)
print(f"Max: {maximum}")  # 5

# Concatenate strings
words = ["Hello", " ", "World", "!"]
sentence = reduce(lambda a, b: a + b, words)
print(f"Sentence: {sentence}")  # Hello World!

# With initial value
total_with_initial = reduce(lambda a, b: a + b, numbers, 100)
print(f"Sum with initial 100: {total_with_initial}")  # 115
```

---

## 🔴 Advanced Examples

### 🔴 Advanced: Complex Sorting

```python
# Sort by multiple criteria

data = [
    ("Alice", 25, 85000),
    ("Bob", 30, 75000),
    ("Charlie", 25, 90000),
    ("Diana", 30, 85000)
]

# Sort by age, then by salary (descending)
sorted_data = sorted(data, key=lambda x: (x[1], -x[2]))
print("Sorted by age, then salary (desc):")
for person in sorted_data:
    print(f"  {person}")
```

**Output:**
```
Sorted by age, then salary (desc):
  ('Charlie', 25, 90000)
  ('Alice', 25, 85000)
  ('Diana', 30, 85000)
  ('Bob', 30, 75000)
```

### 🔴 Advanced: Lambda with Conditional (Ternary)

```python
# Lambda can include ternary operator

# Classify as 'even' or 'odd'
classify = lambda x: "even" if x % 2 == 0 else "odd"
print([classify(i) for i in range(5)])  # ['even', 'odd', 'even', 'odd', 'even']

# Return max of two
max_val = lambda a, b: a if a > b else b
print(max_val(10, 20))  # 20

# Clamp value to range
clamp = lambda x, lo, hi: lo if x < lo else (hi if x > hi else x)
print(clamp(5, 0, 10))   # 5
print(clamp(-5, 0, 10))  # 0
print(clamp(15, 0, 10))  # 10
```

### 🔴 Advanced: Lambda in Data Processing

```python
# Process list of dictionaries
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 30, "active": False},
    {"name": "Charlie", "age": 35, "active": True},
    {"name": "Diana", "age": 28, "active": True}
]

# Get active users
active = list(filter(lambda u: u["active"], users))
print(f"Active users: {[u['name'] for u in active]}")

# Get names of users over 27
over_27_names = list(map(
    lambda u: u["name"],
    filter(lambda u: u["age"] > 27, users)
))
print(f"Over 27: {over_27_names}")

# Sort by age
by_age = sorted(users, key=lambda u: u["age"])
print("By age:", [f"{u['name']}({u['age']})" for u in by_age])
```

**Output:**
```
Active users: ['Alice', 'Charlie', 'Diana']
Over 27: ['Bob', 'Charlie', 'Diana']
By age: ['Alice(25)', 'Diana(28)', 'Bob(30)', 'Charlie(35)']
```

### 🔴 Advanced: Lambda Factory (Closures)

```python
# Create functions dynamically

def multiplier(n):
    """Return a lambda that multiplies by n."""
    return lambda x: x * n

double = multiplier(2)
triple = multiplier(3)
times_10 = multiplier(10)

print(f"double(5) = {double(5)}")     # 10
print(f"triple(5) = {triple(5)}")     # 15
print(f"times_10(5) = {times_10(5)}") # 50

# Power functions
def power_func(exp):
    return lambda x: x ** exp

square = power_func(2)
cube = power_func(3)

print(f"square(4) = {square(4)}")  # 16
print(f"cube(4) = {cube(4)}")      # 64
```

### 🔴 Advanced: Immediately Invoked Lambda

```python
# Call lambda immediately

result = (lambda x, y: x + y)(5, 3)
print(f"Result: {result}")  # 8

# Use in list comprehension
data = [(lambda x: x**2)(i) for i in range(5)]
print(f"Squares: {data}")  # [0, 1, 4, 9, 16]

# Initialize with computed value
config = {
    "max_items": (lambda: 100)(),
    "timeout": (lambda: 30 * 60)(),  # 30 minutes in seconds
}
print(config)  # {'max_items': 100, 'timeout': 1800}
```

---

## 📊 Comparison: Lambda vs List Comprehension

```python
numbers = [1, 2, 3, 4, 5]

# map + lambda
doubled_map = list(map(lambda x: x * 2, numbers))

# List comprehension (often preferred)
doubled_comp = [x * 2 for x in numbers]

print(f"map + lambda: {doubled_map}")     # [2, 4, 6, 8, 10]
print(f"comprehension: {doubled_comp}")    # [2, 4, 6, 8, 10]

# filter + lambda
evens_filter = list(filter(lambda x: x % 2 == 0, numbers))

# List comprehension
evens_comp = [x for x in numbers if x % 2 == 0]

print(f"filter + lambda: {evens_filter}")  # [2, 4]
print(f"comprehension: {evens_comp}")      # [2, 4]
```

**Which to use?**
- List comprehension is usually more readable for simple cases
- Lambda is better for passing to functions like `sorted(key=...)`
- Lambda is needed for `reduce()`

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Lambda with Statements

```python
# WRONG - lambda can't have statements
# double = lambda x: x *= 2  # SyntaxError!
# greet = lambda: print("Hi"); return "Hello"  # SyntaxError!

# CORRECT - single expression only
double = lambda x: x * 2
```

### ❌ Mistake 2: Overusing Lambda

```python
# BAD - hard to read
result = list(map(lambda x: x.strip().lower().replace(" ", "_"), names))

# BETTER - use regular function
def normalize(name):
    return name.strip().lower().replace(" ", "_")

result = list(map(normalize, names))

# Or list comprehension
result = [name.strip().lower().replace(" ", "_") for name in names]
```

### ❌ Mistake 3: Late Binding in Loops

```python
# WRONG - all lambdas capture final value of i
funcs = []
for i in range(5):
    funcs.append(lambda: i)

print([f() for f in funcs])  # [4, 4, 4, 4, 4] - Not expected!

# CORRECT - capture value with default argument
funcs = []
for i in range(5):
    funcs.append(lambda x=i: x)

print([f() for f in funcs])  # [0, 1, 2, 3, 4] - Correct!
```

### ❌ Mistake 4: Forgetting filter() Keeps Truthy

```python
# filter keeps items where function returns True

numbers = [0, 1, 2, 3, 0, 4, 5, 0]

# This keeps non-zero (truthy) values
result = list(filter(lambda x: x, numbers))
print(result)  # [1, 2, 3, 4, 5]

# To keep zeros explicitly
result = list(filter(lambda x: x == 0, numbers))
print(result)  # [0, 0, 0]
```

---

## 💡 Pro Tips

### 1. Use operator Module Instead of Lambda

```python
from operator import add, mul, itemgetter, attrgetter

numbers = [1, 2, 3, 4, 5]

# Instead of lambda a, b: a + b
from functools import reduce
total = reduce(add, numbers)  # Cleaner!

# Instead of lambda x: x['name']
students = [{"name": "Alice"}, {"name": "Bob"}]
names = list(map(itemgetter("name"), students))
print(names)  # ['Alice', 'Bob']

# Sort by attribute
from operator import attrgetter
# sorted(objects, key=attrgetter('name'))
```

### 2. Use Lambda for Default Dict

```python
from collections import defaultdict

# Count occurrences
counter = defaultdict(lambda: 0)
for char in "hello":
    counter[char] += 1
print(dict(counter))  # {'h': 1, 'e': 1, 'l': 2, 'o': 1}

# List of lists
groups = defaultdict(lambda: [])
```

### 3. Chain Operations

```python
from functools import reduce

# Process pipeline
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

result = reduce(
    lambda x, y: x + y,
    map(lambda x: x ** 2,
        filter(lambda x: x % 2 == 0, data))
)
print(f"Sum of squares of evens: {result}")  # 220

# Equivalent (more readable)
evens = [x for x in data if x % 2 == 0]
squares = [x ** 2 for x in evens]
result = sum(squares)
```

---

## 🔍 Real-world Use Cases

### 1. Sorting Complex Data

```python
# Sort files by extension, then by name
files = ["report.pdf", "image.png", "data.csv", "notes.txt", "chart.png"]
sorted_files = sorted(files, key=lambda f: (f.split('.')[-1], f))
print(sorted_files)
```

### 2. Event Handlers (GUI/Web)

```python
# Button click handlers
buttons = {}
for action in ["save", "load", "quit"]:
    buttons[action] = lambda a=action: print(f"Button {a} clicked!")

buttons["save"]()  # Button save clicked!
```

### 3. Data Transformation

```python
# Clean and transform data
raw_data = ["  Alice  ", "BOB", " charlie "]
clean_data = list(map(lambda x: x.strip().title(), raw_data))
print(clean_data)  # ['Alice', 'Bob', 'Charlie']
```

---

## 💼 Interview Insight

### Q1: What is a lambda function?
> A small anonymous function defined with `lambda` keyword, can have any number of arguments but only one expression.

### Q2: When should you use lambda vs regular function?
> Lambda for simple, one-line operations. Regular function for complex logic, documentation, or reuse.

### Q3: What is the difference between map() and filter()?
> `map()` transforms each element. `filter()` selects elements based on condition.

### Q4: Can lambda have multiple expressions?
> No, lambda is limited to a single expression.

### Q5: What is reduce() used for?
> Applies a function cumulatively to reduce a sequence to a single value.

---

## 🧪 Practice Questions

### Easy:
1. Create a lambda to check if a number is positive
2. Use map() to convert a list of integers to strings
3. Use filter() to get words starting with 'a'

### Medium:
4. Sort a list of tuples by second element
5. Use reduce() to find the longest string in a list
6. Combine filter() and map() to process data

### Hard:
7. Create a function factory that returns lambda functions
8. Implement a custom reduce with lambda
9. Sort dictionaries by nested values

---

## 📖 Summary

| Concept | Syntax | Use Case |
|---------|--------|----------|
| Lambda | `lambda x: x * 2` | Anonymous function |
| map() | `map(func, iterable)` | Transform each item |
| filter() | `filter(func, iterable)` | Select items |
| sorted() | `sorted(data, key=func)` | Custom sorting |
| reduce() | `reduce(func, iterable)` | Aggregate to single value |

---

## ⏭️ Next Topic

**[Modules and Packages](./05_Modules_Packages.md)** — Organize and reuse code!

---

*Lambda: Small function, big power!* ⚡
