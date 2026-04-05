# 📘 Sets in Python

## 📌 Introduction

A **set** is an unordered collection of unique elements. No duplicates allowed!

Think of a set like a **bag of unique marbles** — you can add marbles, remove them, and check if a specific marble exists, but there's no order and no duplicates.

```
┌─────────────────────────────────────────────────────────────┐
│                        SET                                   │
│                                                              │
│         ┌───┐                                               │
│        ┌┤ 5 │                                               │
│       ┌┤└───┘  ┌───┐                                        │
│      ┌┤└──────→│ 2 │                                        │
│     ┌┤└────────└───┘   No duplicates!                       │
│     │└───┐  ┌───┐                                           │
│     │    └─→│ 8 │      No order!                            │
│     │       └───┘                                           │
│     │  ┌───┐                                                │
│     └─→│ 1 │                                                │
│        └───┘                                                │
│                                                              │
│   ✗ NOT Ordered (no index)                                  │
│   ✓ Mutable (can add/remove)                                │
│   ✗ NO Duplicates                                           │
│   ✓ Fast membership testing O(1)                            │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Set Properties

| Property | Description |
|----------|-------------|
| Unique | No duplicate elements |
| Unordered | No index access |
| Mutable | Can add/remove elements |
| Hashable elements | Only immutable items |
| Fast lookup | O(1) average |

### Set vs List for Membership

```python
# Set is MUCH faster for "in" checks
list_lookup = 1 in [1, 2, 3, ..., 1000000]  # O(n) - slow
set_lookup = 1 in {1, 2, 3, ..., 1000000}   # O(1) - fast!
```

---

## 💻 Code Examples

### 🟢 Basic: Creating Sets

```python
# Empty set - MUST use set(), not {}
empty = set()  # Correct
wrong = {}     # This is a dict!

# Set with values
numbers = {1, 2, 3, 4, 5}
fruits = {"apple", "banana", "cherry"}
mixed = {1, "hello", 3.14}  # Mixed types OK

# Duplicates are automatically removed
duplicates = {1, 2, 2, 3, 3, 3, 4}
print(duplicates)  # {1, 2, 3, 4}

# From list (removes duplicates)
my_list = [1, 2, 2, 3, 3, 3, 4]
unique = set(my_list)
print(unique)  # {1, 2, 3, 4}

# From string
letters = set("hello")
print(letters)  # {'h', 'e', 'l', 'o'}
```

### 🟢 Basic: Set Operations

```python
numbers = {1, 2, 3, 4, 5}

# Add element
numbers.add(6)
print(numbers)  # {1, 2, 3, 4, 5, 6}

# Add existing (no error, no duplicate)
numbers.add(3)
print(numbers)  # {1, 2, 3, 4, 5, 6}

# Remove element (error if not found)
numbers.remove(6)
print(numbers)  # {1, 2, 3, 4, 5}

# numbers.remove(100)  # KeyError!

# Discard element (no error if not found)
numbers.discard(100)  # No error
numbers.discard(5)
print(numbers)  # {1, 2, 3, 4}

# Pop (remove random element)
removed = numbers.pop()
print(f"Removed: {removed}")

# Clear all
numbers.clear()
print(numbers)  # set()
```

### 🟢 Basic: Membership and Length

```python
fruits = {"apple", "banana", "cherry"}

# Check membership (FAST - O(1))
print("apple" in fruits)   # True
print("mango" in fruits)   # False

# Length
print(len(fruits))  # 3

# Iterate (order not guaranteed)
for fruit in fruits:
    print(fruit)
```

### 🟡 Intermediate: Set Mathematics

```python
# Two sets
A = {1, 2, 3, 4, 5}
B = {4, 5, 6, 7, 8}

# UNION - all elements from both
# A ∪ B
union = A | B
union = A.union(B)
print(f"Union: {union}")  # {1, 2, 3, 4, 5, 6, 7, 8}

# INTERSECTION - common elements
# A ∩ B
intersection = A & B
intersection = A.intersection(B)
print(f"Intersection: {intersection}")  # {4, 5}

# DIFFERENCE - in A but not in B
# A - B
difference = A - B
difference = A.difference(B)
print(f"A - B: {difference}")  # {1, 2, 3}

# B - A
print(f"B - A: {B - A}")  # {6, 7, 8}

# SYMMETRIC DIFFERENCE - in either but not both
# A △ B
sym_diff = A ^ B
sym_diff = A.symmetric_difference(B)
print(f"Symmetric difference: {sym_diff}")  # {1, 2, 3, 6, 7, 8}
```

### 🟡 Intermediate: Set Relationships

```python
A = {1, 2, 3}
B = {1, 2, 3, 4, 5}
C = {1, 2, 3}
D = {6, 7, 8}

# Subset - is A contained in B?
print(A.issubset(B))     # True
print(A <= B)            # True (operator form)

# Proper subset (subset but not equal)
print(A < B)             # True
print(A < C)             # False (they're equal)

# Superset - does B contain A?
print(B.issuperset(A))   # True
print(B >= A)            # True

# Disjoint - no common elements?
print(A.isdisjoint(D))   # True
print(A.isdisjoint(B))   # False
```

### 🟡 Intermediate: Update Methods (In-place)

```python
A = {1, 2, 3}
B = {3, 4, 5}

# Update - add all elements from B (union in-place)
A.update(B)
print(A)  # {1, 2, 3, 4, 5}

# Intersection update - keep only common
A = {1, 2, 3, 4, 5}
A.intersection_update({2, 3, 4})
print(A)  # {2, 3, 4}

# Difference update - remove elements found in other
A = {1, 2, 3, 4, 5}
A.difference_update({3, 4, 5})
print(A)  # {1, 2}

# Symmetric difference update
A = {1, 2, 3}
A.symmetric_difference_update({2, 3, 4})
print(A)  # {1, 4}
```

### 🔴 Advanced: Frozenset (Immutable Set)

```python
# Frozenset - immutable version of set
frozen = frozenset([1, 2, 3, 4, 5])
print(frozen)  # frozenset({1, 2, 3, 4, 5})

# Cannot modify
# frozen.add(6)  # AttributeError!
# frozen.remove(1)  # AttributeError!

# All read operations work
print(3 in frozen)  # True
print(len(frozen))  # 5

# Can use as dictionary key!
cache = {}
key = frozenset([1, 2, 3])
cache[key] = "result"
print(cache[key])  # "result"

# Can be element of another set!
set_of_sets = {frozenset([1, 2]), frozenset([3, 4])}
print(set_of_sets)

# Set operations return frozenset
frozen1 = frozenset([1, 2, 3])
frozen2 = frozenset([3, 4, 5])
print(frozen1 | frozen2)  # frozenset({1, 2, 3, 4, 5})
```

### 🔴 Advanced: Set Comprehension

```python
# Basic set comprehension
squares = {x**2 for x in range(10)}
print(squares)  # {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}

# With condition
evens = {x for x in range(20) if x % 2 == 0}
print(evens)  # {0, 2, 4, 6, 8, 10, 12, 14, 16, 18}

# From string (unique characters)
text = "mississippi"
unique_chars = {char for char in text}
print(unique_chars)  # {'m', 'i', 's', 'p'}

# Nested comprehension
pairs = {(x, y) for x in range(3) for y in range(3) if x != y}
print(pairs)  # {(0, 1), (0, 2), (1, 0), (1, 2), (2, 0), (2, 1)}
```

### 🔴 Advanced: Practical Applications

```python
# Remove duplicates while preserving order (Python 3.7+)
def remove_duplicates_ordered(lst):
    return list(dict.fromkeys(lst))

data = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3]
print(remove_duplicates_ordered(data))  # [3, 1, 4, 5, 9, 2, 6]

# Find common elements in multiple lists
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
list3 = [3, 4, 5, 9, 10]

common = set(list1) & set(list2) & set(list3)
print(f"Common to all: {common}")  # {4, 5}

# Find unique elements across all
all_unique = set(list1) | set(list2) | set(list3)
print(f"All unique: {all_unique}")

# Efficient membership testing
allowed_users = {"alice", "bob", "charlie", "david"}
def is_allowed(username):
    return username.lower() in allowed_users

print(is_allowed("Alice"))  # True
print(is_allowed("eve"))    # False
```

---

## 📊 Set Methods Reference

| Method | Description | Returns |
|--------|-------------|---------|
| `add(x)` | Add element | None |
| `remove(x)` | Remove x (error if not found) | None |
| `discard(x)` | Remove x (no error if not found) | None |
| `pop()` | Remove and return arbitrary element | Element |
| `clear()` | Remove all elements | None |
| `copy()` | Return shallow copy | set |
| `union(s)` | All elements from both | set |
| `intersection(s)` | Common elements | set |
| `difference(s)` | In self but not s | set |
| `symmetric_difference(s)` | In either but not both | set |
| `issubset(s)` | Is self subset of s? | bool |
| `issuperset(s)` | Is self superset of s? | bool |
| `isdisjoint(s)` | No common elements? | bool |

---

## 📊 Set Operators

| Operator | Method | Operation |
|----------|--------|-----------|
| `|` | `union()` | Union |
| `&` | `intersection()` | Intersection |
| `-` | `difference()` | Difference |
| `^` | `symmetric_difference()` | XOR |
| `<=` | `issubset()` | Subset |
| `<` | - | Proper subset |
| `>=` | `issuperset()` | Superset |
| `>` | - | Proper superset |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Empty Set with {}

```python
# WRONG
empty = {}
print(type(empty))  # <class 'dict'> - Not a set!

# CORRECT
empty = set()
print(type(empty))  # <class 'set'>
```

### ❌ Mistake 2: Adding Mutable Elements

```python
# WRONG - lists are not hashable
my_set = {1, 2, 3}
# my_set.add([4, 5])  # TypeError!

# CORRECT - use tuple instead
my_set.add((4, 5))
print(my_set)  # {1, 2, 3, (4, 5)}
```

### ❌ Mistake 3: Indexing Sets

```python
# WRONG - sets have no order
my_set = {1, 2, 3}
# print(my_set[0])  # TypeError!

# CORRECT - convert to list if you need indexing
my_list = list(my_set)
print(my_list[0])  # 1 (but order may vary)
```

### ❌ Mistake 4: Using remove() on Missing Element

```python
# WRONG
my_set = {1, 2, 3}
# my_set.remove(99)  # KeyError!

# CORRECT - use discard()
my_set.discard(99)  # No error
```

---

## 💡 Pro Tips

### 1. Use Sets for Fast Lookup

```python
# Instead of checking list
if item in my_list:  # O(n)

# Use set
my_set = set(my_list)
if item in my_set:   # O(1)
```

### 2. Remove Duplicates from List

```python
unique = list(set(my_list))
```

### 3. Find Differences Between Lists

```python
new_items = set(list2) - set(list1)
```

### 4. Use Operators for Cleaner Code

```python
# Instead of
result = a.union(b).intersection(c)

# Use operators
result = (a | b) & c
```

---

## 💼 Interview Insight

### Q1: What is the time complexity of set operations?

| Operation | Average | Worst |
|-----------|---------|-------|
| `add(x)` | O(1) | O(n) |
| `remove(x)` | O(1) | O(n) |
| `x in s` | O(1) | O(n) |
| `union` | O(len(a)+len(b)) | - |
| `intersection` | O(min(len(a),len(b))) | - |

### Q2: How does a set work internally?

> Sets use a **hash table**. Each element's hash determines its bucket. This enables O(1) average lookup.

### Q3: What can be stored in a set?

> Only **hashable** (immutable) objects: numbers, strings, tuples (with hashable elements), frozensets. NOT: lists, dicts, sets.

---

## 🔍 Real-world Use Cases

### 1. Remove Duplicates

```python
emails = ["a@x.com", "b@x.com", "a@x.com", "c@x.com"]
unique_emails = list(set(emails))
```

### 2. Find Common Tags

```python
user1_tags = {"python", "javascript", "react"}
user2_tags = {"python", "django", "postgresql"}
common = user1_tags & user2_tags  # {"python"}
```

### 3. Permission System

```python
user_permissions = {"read", "write"}
required_permissions = {"read", "execute"}

if required_permissions.issubset(user_permissions):
    print("Access granted")
else:
    missing = required_permissions - user_permissions
    print(f"Missing permissions: {missing}")
```

### 4. Visited Tracker

```python
visited_urls = set()

def visit(url):
    if url in visited_urls:
        print("Already visited")
    else:
        visited_urls.add(url)
        print(f"Visiting {url}")
```

---

## 🧪 Practice Questions

### Easy:
1. Remove duplicates from a list using sets
2. Check if two lists have any common elements
3. Find all unique characters in a string

### Medium:
4. Find elements in list1 but not in list2
5. Check if one list is a subset of another
6. Find the symmetric difference of two lists

### Hard:
7. Find all unique pairs from a list that sum to target
8. Implement a function to check if two strings are anagrams
9. Find common elements in N lists

---

## 🚀 Mini Tasks

### Task 1: Tag Analyzer
```python
# Given multiple users with tags
# Find: common tags, unique tags per user, all tags
users = {
    "alice": {"python", "js", "react"},
    "bob": {"python", "java", "spring"},
    "charlie": {"python", "go", "docker"}
}
```

### Task 2: Duplicate Finder
```python
# Find all duplicate elements in a list
# Return them as a set
nums = [1, 2, 3, 2, 4, 3, 5, 1]
# Expected: {1, 2, 3}
```

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Creation | `set()` for empty, `{1,2,3}` with values |
| Unique | No duplicates allowed |
| Unordered | No index access |
| Operations | Union `|`, Intersection `&`, Difference `-` |
| Fast | O(1) membership testing |
| Frozenset | Immutable version |

---

## ⏭️ Next Topic

**[Dictionaries](./04_Dictionaries.md)** — Key-value pairs!

---

*Sets: When uniqueness matters!* 🎯
