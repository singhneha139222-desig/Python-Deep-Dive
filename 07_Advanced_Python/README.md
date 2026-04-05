# 📘 Module 7: Advanced Python

Welcome to **Module 7**! This module covers advanced Python features that separate beginners from intermediate developers. Master these concepts to write more elegant, efficient, and Pythonic code.

---

## 📌 Overview of This Module

Advanced Python features enable you to write cleaner, more efficient, and more maintainable code. These are the tools that experienced Python developers use daily.

```
┌─────────────────────────────────────────────────────────────┐
│                    ADVANCED PYTHON                           │
│                                                              │
│   DECORATORS        Modify function behavior                 │
│   @decorator        without changing code                    │
│                                                              │
│   GENERATORS        Memory-efficient                         │
│   yield             lazy evaluation                          │
│                                                              │
│   ITERATORS         Custom iteration                         │
│   __iter__          patterns                                 │
│                                                              │
│   THREADING         Concurrent I/O                           │
│   MULTIPROCESSING   Parallel CPU work                        │
│                                                              │
│   CONTEXT MANAGERS  Resource management                      │
│   with statement    (auto cleanup)                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

| Skill | Description |
|-------|-------------|
| ✅ Decorators | Modify functions without changing their code |
| ✅ Generators | Create memory-efficient iterables |
| ✅ Iterators | Build custom iteration patterns |
| ✅ Concurrency | Threading and multiprocessing |
| ✅ Context Managers | Automatic resource management |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_Decorators.md` | **Decorators** | Function decorators, class decorators, functools |
| `02_Generators.md` | **Generators** | yield, generator expressions, lazy evaluation |
| `03_Iterators.md` | **Iterators** | Iterator protocol, custom iterators |
| `04_Threading_vs_Multiprocessing.md` | **Concurrency** | Threading, multiprocessing, asyncio |
| `05_Context_Managers.md` | **Context Managers** | with statement, custom context managers |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_Decorators.md
        ↓
        Fundamental metaprogramming tool
        
Step 2: 02_Generators.md
        ↓
        Memory-efficient data processing
        
Step 3: 03_Iterators.md
        ↓
        Custom iteration patterns
        
Step 4: 04_Threading_vs_Multiprocessing.md
        ↓
        Concurrent and parallel programming
        
Step 5: 05_Context_Managers.md
        ↓
        Clean resource management
```

**Estimated Time:** 8-10 hours (with practice)

---

## 📊 Quick Comparison

### Concurrency Models

| Model | Best For | GIL Impact | Example |
|-------|----------|------------|---------|
| Threading | I/O-bound | Blocked | HTTP requests |
| Multiprocessing | CPU-bound | Bypassed | Data processing |
| Asyncio | Many I/O | Cooperative | Web servers |

### Generator vs List

| Feature | Generator | List |
|---------|-----------|------|
| Memory | O(1) | O(n) |
| Reusable | No | Yes |
| Indexable | No | Yes |
| Length | Unknown | Known |
| Speed | Lazy | Eager |

---

## 💡 Tips Before Starting

### Key Mindset Shifts:

1. **Lazy is Good** — Don't compute until needed
2. **Functions are Objects** — Pass them, return them
3. **Resources Need Management** — Always clean up
4. **Concurrency ≠ Parallelism** — Know the difference

### When to Use What:

```python
# DECORATORS - when you want to:
# - Add logging, timing, caching
# - Modify behavior without changing code
# - Apply same logic to multiple functions

# GENERATORS - when you want to:
# - Process large data without loading all
# - Create pipelines
# - Handle infinite sequences

# CONTEXT MANAGERS - when you want to:
# - Ensure cleanup (files, connections)
# - Temporary state changes
# - Transaction-like behavior
```

---

## 🎯 Module Objectives

By the end of this module, you should be able to:

- [ ] Write function and class decorators
- [ ] Create memory-efficient generators
- [ ] Implement custom iterator protocols
- [ ] Choose between threading and multiprocessing
- [ ] Build custom context managers
- [ ] Apply these patterns in real projects

---

## 🔗 Quick Links

1. [Decorators](./01_Decorators.md)
2. [Generators](./02_Generators.md)
3. [Iterators](./03_Iterators.md)
4. [Threading vs Multiprocessing](./04_Threading_vs_Multiprocessing.md)
5. [Context Managers](./05_Context_Managers.md)

---

## 📝 Quick Reference

```python
# DECORATOR
def timer(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Took {time.time() - start:.2f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

# GENERATOR
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(5):
    print(num)

# ITERATOR
class Counter:
    def __init__(self, max):
        self.max = max
        self.n = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.n >= self.max:
            raise StopIteration
        self.n += 1
        return self.n

# CONTEXT MANAGER
from contextlib import contextmanager

@contextmanager
def managed_resource():
    print("Setup")
    yield "resource"
    print("Cleanup")

with managed_resource() as r:
    print(f"Using {r}")
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 8: Libraries and Frameworks](../08_Libraries_and_Frameworks/README.md)**
- NumPy for numerical computing
- Pandas for data analysis
- Flask/FastAPI for web development

---

> 💪 **Remember:** These advanced features are what make Python powerful. Take time to understand them deeply!

---

*Happy Learning! 🚀*
