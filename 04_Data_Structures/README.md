# 📘 Module 4: Data Structures

Welcome to **Module 4**! Data structures are the foundation of efficient programming. They determine how you store, organize, and access data in your programs.

---

## 📌 Overview of This Module

Python provides several built-in data structures that make it easy to work with collections of data. Understanding when and how to use each one is crucial for writing efficient code.

```
┌─────────────────────────────────────────────────────────────┐
│              PYTHON DATA STRUCTURES                          │
│                                                              │
│   Lists        Tuples       Sets         Dictionaries       │
│   [1,2,3]      (1,2,3)      {1,2,3}      {"a":1}           │
│                                                              │
│   ✓ Ordered    ✓ Ordered    ✗ Ordered    ✓ Ordered*        │
│   ✓ Mutable    ✗ Immutable  ✓ Mutable    ✓ Mutable         │
│   ✓ Duplicates ✓ Duplicates ✗ Duplicates ✗ Dup Keys        │
│   ✓ Indexed    ✓ Indexed    ✗ Indexed    ✓ Key-based       │
│                                                              │
│   * Ordered since Python 3.7                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

| Skill | Description |
|-------|-------------|
| ✅ Lists | Create, modify, and manipulate ordered collections |
| ✅ Tuples | Work with immutable sequences |
| ✅ Sets | Handle unique elements and set operations |
| ✅ Dictionaries | Use key-value pairs effectively |
| ✅ Comprehensions | Write concise, Pythonic code |
| ✅ Choose wisely | Select the right structure for each task |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_Lists.md` | **Lists** | Creating, indexing, slicing, methods, nested lists |
| `02_Tuples.md` | **Tuples** | Immutable sequences, packing/unpacking, named tuples |
| `03_Sets.md` | **Sets** | Unique elements, set operations, frozensets |
| `04_Dictionaries.md` | **Dictionaries** | Key-value pairs, methods, nested dicts |
| `05_List_Comprehension.md` | **Comprehensions** | List, dict, set, generator comprehensions |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_Lists.md
        ↓
        Most commonly used, foundation for others
        
Step 2: 02_Tuples.md
        ↓
        Like lists but immutable
        
Step 3: 03_Sets.md
        ↓
        Unique elements, set math
        
Step 4: 04_Dictionaries.md
        ↓
        Key-value storage
        
Step 5: 05_List_Comprehension.md
        ↓
        Pythonic way to create collections
```

**Estimated Time:** 6-8 hours (with practice)

---

## 📊 Quick Comparison Table

| Feature | List | Tuple | Set | Dict |
|---------|------|-------|-----|------|
| Syntax | `[1,2,3]` | `(1,2,3)` | `{1,2,3}` | `{"a":1}` |
| Ordered | ✅ | ✅ | ❌ | ✅* |
| Mutable | ✅ | ❌ | ✅ | ✅ |
| Duplicates | ✅ | ✅ | ❌ | Keys: ❌ |
| Indexed | ✅ | ✅ | ❌ | By Key |
| Hashable | ❌ | ✅ | ❌ | Keys: ✅ |
| Use Case | General | Fixed data | Unique items | Key-value |

*Dict is ordered since Python 3.7

---

## 💡 Tips Before Starting

### When to Use Each:

- **List** — When you need an ordered, changeable collection
- **Tuple** — When data shouldn't change (coordinates, config)
- **Set** — When you need unique items or set operations
- **Dictionary** — When you need to look up values by key

### Performance Considerations:

```python
# Lookup time complexity:
list:  O(n)  # Must search through elements
tuple: O(n)  # Same as list
set:   O(1)  # Hash-based lookup
dict:  O(1)  # Hash-based key lookup

# Use set/dict for frequent lookups!
```

---

## 🎯 Module Objectives

By the end of this module, you should be able to:

- [ ] Create and manipulate lists using various methods
- [ ] Understand tuple immutability and use cases
- [ ] Perform set operations (union, intersection, difference)
- [ ] Use dictionaries for efficient data lookup
- [ ] Write list, dict, and set comprehensions
- [ ] Choose the appropriate data structure for any task

---

## 🔗 Quick Links

1. [Lists](./01_Lists.md)
2. [Tuples](./02_Tuples.md)
3. [Sets](./03_Sets.md)
4. [Dictionaries](./04_Dictionaries.md)
5. [Comprehensions](./05_List_Comprehension.md)

---

## 📝 Quick Reference

```python
# LIST - Ordered, mutable, allows duplicates
fruits = ["apple", "banana", "cherry"]
fruits.append("date")
fruits[0]  # "apple"

# TUPLE - Ordered, immutable
coordinates = (10, 20)
x, y = coordinates  # Unpacking

# SET - Unordered, unique elements
unique = {1, 2, 3, 3, 3}  # {1, 2, 3}
a | b  # Union
a & b  # Intersection

# DICTIONARY - Key-value pairs
person = {"name": "Alice", "age": 25}
person["name"]  # "Alice"
person.get("email", "N/A")  # "N/A"

# COMPREHENSIONS
squares = [x**2 for x in range(5)]
evens = {x for x in range(10) if x % 2 == 0}
cubes = {x: x**3 for x in range(5)}
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 5: Object-Oriented Programming](../05_Object_Oriented_Programming/README.md)**
- Classes and Objects
- Inheritance
- Polymorphism
- Encapsulation

---

> 💪 **Remember:** The right data structure can make your code 10x faster and easier to understand!

---

*Happy Learning! 🚀*
