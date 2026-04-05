# 📘 Dictionaries in Python

## 📌 Introduction

A **dictionary** is a collection of key-value pairs. Each key maps to a value, like a real dictionary maps words to definitions.

Think of it like a **phone book** — you look up a name (key) to find a phone number (value).

```
┌─────────────────────────────────────────────────────────────┐
│                      DICTIONARY                              │
│                                                              │
│   ┌─────────┬───────────┐                                   │
│   │   KEY   │   VALUE   │                                   │
│   ├─────────┼───────────┤                                   │
│   │ "name"  │  "Alice"  │                                   │
│   │ "age"   │    25     │                                   │
│   │ "city"  │ "NY"      │                                   │
│   └─────────┴───────────┘                                   │
│                                                              │
│   ✓ Ordered (Python 3.7+)                                   │
│   ✓ Mutable (can change)                                    │
│   ✗ Duplicate keys (values can duplicate)                   │
│   ✓ Fast lookup O(1)                                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Key Requirements

- Keys must be **immutable** (hashable): strings, numbers, tuples
- Keys must be **unique** (duplicates overwrite)
- Values can be **any type**

### Dict vs List Lookup

```python
# List - O(n) search
users_list = [("alice", 25), ("bob", 30), ("charlie", 35)]
# Must loop through to find alice's age

# Dict - O(1) lookup
users_dict = {"alice": 25, "bob": 30, "charlie": 35}
print(users_dict["alice"])  # Instant!
```

---

## 💻 Code Examples

### 🟢 Basic: Creating Dictionaries

```python
# Empty dictionary
empty = {}
empty = dict()

# Dictionary with values
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

# Mixed key types (but must be hashable)
mixed = {
    "name": "Alice",
    1: "one",
    (1, 2): "tuple key"
}

# From list of tuples
pairs = [("a", 1), ("b", 2), ("c", 3)]
d = dict(pairs)
print(d)  # {'a': 1, 'b': 2, 'c': 3}

# From keyword arguments
d = dict(name="Alice", age=25, city="NY")
print(d)  # {'name': 'Alice', 'age': 25, 'city': 'NY'}

# From zip
keys = ["a", "b", "c"]
values = [1, 2, 3]
d = dict(zip(keys, values))
print(d)  # {'a': 1, 'b': 2, 'c': 3}
```

### 🟢 Basic: Accessing Values

```python
person = {"name": "Alice", "age": 25, "city": "New York"}

# Direct access (raises KeyError if not found)
print(person["name"])  # Alice
# print(person["email"])  # KeyError!

# get() method (returns None or default if not found)
print(person.get("name"))        # Alice
print(person.get("email"))       # None
print(person.get("email", "N/A"))  # N/A (default)

# Check if key exists
if "name" in person:
    print("Name found!")

# Check if value exists
if "Alice" in person.values():
    print("Alice is a value!")
```

### 🟢 Basic: Modifying Dictionaries

```python
person = {"name": "Alice", "age": 25}

# Add new key-value pair
person["city"] = "New York"
print(person)  # {'name': 'Alice', 'age': 25, 'city': 'New York'}

# Update existing value
person["age"] = 26
print(person)  # {'name': 'Alice', 'age': 26, 'city': 'New York'}

# Remove by key
del person["city"]
print(person)  # {'name': 'Alice', 'age': 26}

# pop() - remove and return value
age = person.pop("age")
print(f"Removed age: {age}")  # 26
print(person)  # {'name': 'Alice'}

# pop() with default
email = person.pop("email", "not found")
print(email)  # not found (no error)

# popitem() - remove last inserted (Python 3.7+)
person = {"a": 1, "b": 2, "c": 3}
last = person.popitem()
print(f"Removed: {last}")  # ('c', 3)

# Clear all
person.clear()
print(person)  # {}
```

### 🟡 Intermediate: Dictionary Methods

```python
person = {"name": "Alice", "age": 25, "city": "NY"}

# keys(), values(), items()
print(list(person.keys()))    # ['name', 'age', 'city']
print(list(person.values()))  # ['Alice', 25, 'NY']
print(list(person.items()))   # [('name', 'Alice'), ('age', 25), ('city', 'NY')]

# update() - merge dictionaries
person.update({"age": 26, "email": "alice@email.com"})
print(person)  # {'name': 'Alice', 'age': 26, 'city': 'NY', 'email': 'alice@email.com'}

# update from iterable
person.update([("phone", "555-1234")])

# setdefault() - get or set if not exists
val = person.setdefault("country", "USA")
print(val)  # USA (set and returned)
print(person["country"])  # USA

val = person.setdefault("name", "Bob")
print(val)  # Alice (already existed)

# copy() - shallow copy
copy = person.copy()
```

### 🟡 Intermediate: Iterating Dictionaries

```python
person = {"name": "Alice", "age": 25, "city": "NY"}

# Iterate over keys (default)
for key in person:
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over items (key-value pairs)
for key, value in person.items():
    print(f"{key}: {value}")

# With enumerate
for i, (key, value) in enumerate(person.items()):
    print(f"{i}. {key}: {value}")
```

### 🟡 Intermediate: Dictionary Comprehension

```python
# Basic comprehension
squares = {x: x**2 for x in range(6)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With condition
evens = {x: x**2 for x in range(10) if x % 2 == 0}
print(evens)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# From lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
people = {name: age for name, age in zip(names, ages)}
print(people)  # {'Alice': 25, 'Bob': 30, 'Charlie': 35}

# Swap keys and values
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}
print(swapped)  # {1: 'a', 2: 'b', 3: 'c'}

# Filter dictionary
scores = {"Alice": 85, "Bob": 62, "Charlie": 90, "David": 55}
passed = {name: score for name, score in scores.items() if score >= 60}
print(passed)  # {'Alice': 85, 'Bob': 62, 'Charlie': 90}
```

### 🔴 Advanced: Nested Dictionaries

```python
# Nested dictionary
company = {
    "employees": {
        "alice": {
            "name": "Alice Smith",
            "department": "Engineering",
            "salary": 75000
        },
        "bob": {
            "name": "Bob Johnson",
            "department": "Marketing",
            "salary": 65000
        }
    },
    "departments": ["Engineering", "Marketing", "Sales"]
}

# Access nested values
print(company["employees"]["alice"]["salary"])  # 75000

# Modify nested value
company["employees"]["alice"]["salary"] = 80000

# Add new nested entry
company["employees"]["charlie"] = {
    "name": "Charlie Brown",
    "department": "Sales",
    "salary": 70000
}

# Safe nested access
def get_nested(d, *keys, default=None):
    for key in keys:
        if isinstance(d, dict):
            d = d.get(key, default)
        else:
            return default
    return d

print(get_nested(company, "employees", "alice", "salary"))  # 80000
print(get_nested(company, "employees", "xyz", "salary", default="N/A"))  # N/A
```

### 🔴 Advanced: DefaultDict

```python
from collections import defaultdict

# Regular dict - KeyError on missing key
d = {}
# d["count"] += 1  # KeyError!

# defaultdict - auto-creates missing keys
d = defaultdict(int)  # default value: 0
d["count"] += 1
print(d["count"])  # 1

# With list as default
groups = defaultdict(list)
students = [("A", "Alice"), ("B", "Bob"), ("A", "Amy")]
for grade, name in students:
    groups[grade].append(name)
print(dict(groups))  # {'A': ['Alice', 'Amy'], 'B': ['Bob']}

# With set as default
tags = defaultdict(set)
items = [("post1", "python"), ("post1", "web"), ("post2", "python")]
for post, tag in items:
    tags[post].add(tag)
print(dict(tags))  # {'post1': {'python', 'web'}, 'post2': {'python'}}
```

### 🔴 Advanced: Counter

```python
from collections import Counter

# Count elements
text = "mississippi"
count = Counter(text)
print(count)  # Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})

# Most common
print(count.most_common(2))  # [('i', 4), ('s', 4)]

# Count from list
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
word_count = Counter(words)
print(word_count)  # Counter({'apple': 3, 'banana': 2, 'cherry': 1})

# Arithmetic with counters
c1 = Counter({'a': 3, 'b': 2})
c2 = Counter({'a': 1, 'b': 3})
print(c1 + c2)  # Counter({'a': 4, 'b': 5})
print(c1 - c2)  # Counter({'a': 2}) - only positive
```

### 🔴 Advanced: OrderedDict

```python
from collections import OrderedDict

# Before Python 3.7, dict didn't preserve order
# OrderedDict still useful for:
# 1. Comparing order-sensitively
# 2. Move to end functionality

od = OrderedDict()
od["a"] = 1
od["b"] = 2
od["c"] = 3

# Move to end
od.move_to_end("a")
print(list(od.keys()))  # ['b', 'c', 'a']

# Move to beginning
od.move_to_end("c", last=False)
print(list(od.keys()))  # ['c', 'b', 'a']
```

---

## 📊 Dictionary Methods Reference

| Method | Description | Returns |
|--------|-------------|---------|
| `get(k, d)` | Get value or default | value/d |
| `keys()` | All keys | dict_keys |
| `values()` | All values | dict_values |
| `items()` | All key-value pairs | dict_items |
| `pop(k, d)` | Remove & return | value |
| `popitem()` | Remove & return last | tuple |
| `update(d)` | Merge dictionary | None |
| `setdefault(k, v)` | Get or set default | value |
| `clear()` | Remove all | None |
| `copy()` | Shallow copy | dict |
| `fromkeys(keys, val)` | Create from keys | dict |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: KeyError on Missing Key

```python
# WRONG
person = {"name": "Alice"}
age = person["age"]  # KeyError!

# CORRECT - use get()
age = person.get("age", 0)  # Returns 0

# Or check first
if "age" in person:
    age = person["age"]
```

### ❌ Mistake 2: Mutable Keys

```python
# WRONG - lists are not hashable
# d = {[1, 2]: "value"}  # TypeError!

# CORRECT - use tuple
d = {(1, 2): "value"}
```

### ❌ Mistake 3: Modifying While Iterating

```python
# WRONG
d = {"a": 1, "b": 2, "c": 3}
for key in d:
    if d[key] < 2:
        del d[key]  # RuntimeError!

# CORRECT - iterate over copy
for key in list(d.keys()):
    if d[key] < 2:
        del d[key]

# Or use comprehension
d = {k: v for k, v in d.items() if v >= 2}
```

---

## 💡 Pro Tips

### 1. Merge Dictionaries (Python 3.9+)

```python
d1 = {"a": 1, "b": 2}
d2 = {"c": 3, "d": 4}
merged = d1 | d2  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

### 2. Use dict.fromkeys() for Initialization

```python
keys = ["a", "b", "c"]
d = dict.fromkeys(keys, 0)  # {'a': 0, 'b': 0, 'c': 0}
```

### 3. Destructure Dictionary

```python
config = {"host": "localhost", "port": 8080, "debug": True}

# Get specific keys
host, port = config["host"], config["port"]

# Or with itemgetter
from operator import itemgetter
host, port = itemgetter("host", "port")(config)
```

### 4. Use Walrus Operator

```python
# Python 3.8+
if (value := my_dict.get("key")) is not None:
    print(f"Found: {value}")
```

---

## 💼 Interview Insight

### Q1: Time complexity of dict operations?

| Operation | Average | Worst |
|-----------|---------|-------|
| Get | O(1) | O(n) |
| Set | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Iterate | O(n) | O(n) |

### Q2: How does dict work internally?

> Dictionary uses a **hash table**. Keys are hashed to determine storage location. Collisions are handled with open addressing (probing).

### Q3: Difference between `dict` and `OrderedDict`?

> Since Python 3.7, regular `dict` maintains insertion order. `OrderedDict` still offers `move_to_end()` and order-sensitive equality comparison.

---

## 🔍 Real-world Use Cases

### 1. Configuration Settings

```python
config = {
    "database": {
        "host": "localhost",
        "port": 5432,
        "name": "myapp"
    },
    "debug": True,
    "log_level": "INFO"
}
```

### 2. Caching / Memoization

```python
cache = {}
def fibonacci(n):
    if n not in cache:
        if n <= 1:
            cache[n] = n
        else:
            cache[n] = fibonacci(n-1) + fibonacci(n-2)
    return cache[n]
```

### 3. Word Frequency

```python
from collections import Counter
text = "the quick brown fox jumps over the lazy dog"
freq = Counter(text.split())
print(freq.most_common(3))
```

### 4. JSON Data Handling

```python
import json
data = {"name": "Alice", "age": 25}
json_str = json.dumps(data)  # To JSON
parsed = json.loads(json_str)  # From JSON
```

---

## 🧪 Practice Questions

### Easy:
1. Create a dict mapping names to ages
2. Count character frequency in a string
3. Merge two dictionaries

### Medium:
4. Group words by their first letter
5. Find key with maximum value
6. Invert a dictionary (swap keys/values)

### Hard:
7. Implement a simple LRU cache
8. Flatten a nested dictionary
9. Find two numbers that sum to target using dict

---

## 🚀 Mini Tasks

### Task 1: Student Grades

```python
# Given a list of (name, score) tuples
# Create a dict of name: grade (A/B/C/D/F)
# Count students per grade
students = [("Alice", 92), ("Bob", 78), ("Charlie", 85), ("David", 65)]
```

### Task 2: Word Index

```python
# Create a word index showing which lines contain each word
text = """Python is great
Python is easy
Programming is fun"""
# Output: {'Python': [1, 2], 'is': [1, 2, 3], ...}
```

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Creation | `{}` or `dict()` |
| Access | `d[key]` or `d.get(key)` |
| Modify | `d[key] = value` |
| Delete | `del d[key]` or `d.pop(key)` |
| Iterate | `.keys()`, `.values()`, `.items()` |
| Comprehension | `{k: v for k, v in items}` |
| Special | `defaultdict`, `Counter`, `OrderedDict` |

---

## ⏭️ Next Topic

**[List Comprehension](./05_List_Comprehension.md)** — Pythonic collection creation!

---

*Dictionaries: The power of key-value mapping!* 🔑
