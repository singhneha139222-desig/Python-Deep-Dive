# 📘 Iterators in Python

## 📌 Introduction

An **iterator** is an object that implements the iterator protocol: `__iter__()` and `__next__()` methods. It allows you to traverse through all elements of a collection, one at a time.

Think of an iterator like a **bookmark** in a book — it keeps track of where you are and knows how to get to the next page.

```
┌─────────────────────────────────────────────────────────────┐
│                    ITERATOR PROTOCOL                         │
│                                                              │
│   Iterable                        Iterator                   │
│   ┌─────────────┐                ┌─────────────┐            │
│   │ __iter__()  │  ──returns──▶  │ __iter__()  │            │
│   └─────────────┘                │ __next__()  │            │
│                                  └─────────────┘            │
│                                         │                    │
│   list, tuple, dict                     ↓                    │
│   str, range, file         ┌──── Returns next item          │
│                           │      or raises StopIteration    │
│                           ▼                                  │
│                      for item in iterable:                   │
│                          process(item)                       │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Iterable vs Iterator

| Concept | Definition | Example |
|---------|------------|---------|
| Iterable | Has `__iter__()`, can be looped | list, tuple, dict |
| Iterator | Has `__iter__()` AND `__next__()` | iter(list), generators |

### The for Loop Under the Hood

```python
# What you write
for item in [1, 2, 3]:
    print(item)

# What Python does
iterator = iter([1, 2, 3])  # Get iterator
while True:
    try:
        item = next(iterator)  # Get next item
        print(item)
    except StopIteration:
        break  # End of iteration
```

---

## 💻 Code Examples

### 🟢 Basic: Built-in Iteration

```python
# Lists are iterable
my_list = [1, 2, 3]
iterator = iter(my_list)  # Get iterator

print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3
# print(next(iterator))  # StopIteration!

# Strings are iterable
for char in "hello":
    print(char)

# Dictionaries are iterable (over keys)
my_dict = {"a": 1, "b": 2}
for key in my_dict:
    print(f"{key}: {my_dict[key]}")

# Files are iterable (over lines)
with open("file.txt") as f:
    for line in f:
        print(line.strip())
```

### 🟢 Basic: iter() and next()

```python
# iter() creates an iterator
numbers = [10, 20, 30]
it = iter(numbers)

print(type(it))  # <class 'list_iterator'>

# next() gets the next value
print(next(it))  # 10
print(next(it))  # 20
print(next(it))  # 30

# next() with default (no StopIteration)
print(next(it, "No more"))  # "No more"
print(next(it, 0))          # 0

# Check if object is iterable
from collections.abc import Iterable, Iterator

print(isinstance([1, 2, 3], Iterable))  # True
print(isinstance([1, 2, 3], Iterator))  # False
print(isinstance(iter([1, 2, 3]), Iterator))  # True
```

### 🟡 Intermediate: Custom Iterator Class

```python
class Counter:
    """Simple counter iterator"""
    
    def __init__(self, start, end):
        self.current = start
        self.end = end
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current >= self.end:
            raise StopIteration
        value = self.current
        self.current += 1
        return value

# Usage
counter = Counter(1, 5)
for num in counter:
    print(num)  # 1, 2, 3, 4

# Manual iteration
counter = Counter(1, 4)
print(next(counter))  # 1
print(next(counter))  # 2
print(next(counter))  # 3
# print(next(counter))  # StopIteration
```

### 🟡 Intermediate: Separate Iterable and Iterator

```python
class Range:
    """Iterable that can be reused"""
    
    def __init__(self, start, end):
        self.start = start
        self.end = end
    
    def __iter__(self):
        # Return new iterator each time
        return RangeIterator(self.start, self.end)

class RangeIterator:
    """Iterator for Range"""
    
    def __init__(self, start, end):
        self.current = start
        self.end = end
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current >= self.end:
            raise StopIteration
        value = self.current
        self.current += 1
        return value

# Can iterate multiple times!
my_range = Range(1, 4)

print("First iteration:")
for num in my_range:
    print(num)

print("Second iteration:")
for num in my_range:
    print(num)

# Both print 1, 2, 3
```

### 🟡 Intermediate: Practical Iterator Examples

```python
# Fibonacci Iterator
class Fibonacci:
    def __init__(self, max_count):
        self.max_count = max_count
        self.count = 0
        self.a, self.b = 0, 1
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.count >= self.max_count:
            raise StopIteration
        value = self.a
        self.a, self.b = self.b, self.a + self.b
        self.count += 1
        return value

print(list(Fibonacci(10)))  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# Reverse Iterator
class ReverseIterator:
    def __init__(self, data):
        self.data = data
        self.index = len(data)
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index -= 1
        return self.data[self.index]

print(list(ReverseIterator([1, 2, 3, 4, 5])))  # [5, 4, 3, 2, 1]
```

### 🔴 Advanced: Iterator with State

```python
class RunningAverage:
    """Iterator that calculates running average"""
    
    def __init__(self, data):
        self.data = iter(data)
        self.total = 0
        self.count = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        value = next(self.data)
        self.total += value
        self.count += 1
        return self.total / self.count

data = [10, 20, 30, 40, 50]
for avg in RunningAverage(data):
    print(f"{avg:.2f}")
# 10.00, 15.00, 20.00, 25.00, 30.00

# Batch Iterator
class BatchIterator:
    """Iterate in batches"""
    
    def __init__(self, data, batch_size):
        self.data = data
        self.batch_size = batch_size
        self.index = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration
        batch = self.data[self.index:self.index + self.batch_size]
        self.index += self.batch_size
        return batch

data = list(range(10))
for batch in BatchIterator(data, 3):
    print(batch)
# [0, 1, 2]
# [3, 4, 5]
# [6, 7, 8]
# [9]
```

### 🔴 Advanced: Chaining Iterators

```python
class Chain:
    """Chain multiple iterables"""
    
    def __init__(self, *iterables):
        self.iterables = iter(iterables)
        self.current = iter(next(self.iterables, []))
    
    def __iter__(self):
        return self
    
    def __next__(self):
        while True:
            try:
                return next(self.current)
            except StopIteration:
                self.current = iter(next(self.iterables))

print(list(Chain([1, 2], [3, 4], [5, 6])))  # [1, 2, 3, 4, 5, 6]

# Filter Iterator
class Filter:
    """Filter with predicate"""
    
    def __init__(self, predicate, iterable):
        self.predicate = predicate
        self.iterator = iter(iterable)
    
    def __iter__(self):
        return self
    
    def __next__(self):
        while True:
            value = next(self.iterator)
            if self.predicate(value):
                return value

evens = Filter(lambda x: x % 2 == 0, range(10))
print(list(evens))  # [0, 2, 4, 6, 8]
```

### 🔴 Advanced: Infinite Iterator with Control

```python
class InfiniteCounter:
    """Infinite counter with reset capability"""
    
    def __init__(self, start=0, step=1):
        self.start = start
        self.step = step
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        value = self.current
        self.current += self.step
        return value
    
    def reset(self):
        self.current = self.start

# Use with caution - infinite!
counter = InfiniteCounter(0, 2)
for i, num in enumerate(counter):
    if i >= 5:
        break
    print(num)  # 0, 2, 4, 6, 8

counter.reset()
print(next(counter))  # 0

# Take N Items Iterator
class Take:
    """Take first n items from iterator"""
    
    def __init__(self, n, iterable):
        self.n = n
        self.iterator = iter(iterable)
        self.count = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.count >= self.n:
            raise StopIteration
        self.count += 1
        return next(self.iterator)

infinite = InfiniteCounter()
print(list(Take(5, infinite)))  # [0, 1, 2, 3, 4]
```

---

## 📊 Iterator Protocol Summary

| Method | Purpose |
|--------|---------|
| `__iter__()` | Return iterator object |
| `__next__()` | Return next value or raise StopIteration |

### Built-in Iterator Functions

| Function | Description |
|----------|-------------|
| `iter(obj)` | Get iterator from iterable |
| `next(it)` | Get next item |
| `next(it, default)` | Get next or default |
| `enumerate(it)` | Add indices |
| `zip(it1, it2)` | Pair elements |
| `map(fn, it)` | Apply function |
| `filter(fn, it)` | Filter elements |
| `reversed(seq)` | Reverse sequence |
| `sorted(it)` | Sort elements |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Iterator Exhaustion

```python
# WRONG
it = iter([1, 2, 3])
print(list(it))  # [1, 2, 3]
print(list(it))  # [] - Exhausted!

# CORRECT - create new iterator
data = [1, 2, 3]
print(list(iter(data)))  # [1, 2, 3]
print(list(iter(data)))  # [1, 2, 3]
```

### ❌ Mistake 2: Not Returning self from __iter__

```python
# WRONG
class BadIterator:
    def __init__(self, data):
        self.data = data
    
    def __iter__(self):
        pass  # Returns None!

# CORRECT
class GoodIterator:
    def __iter__(self):
        return self
```

### ❌ Mistake 3: Forgetting StopIteration

```python
# WRONG - infinite loop
class BadIterator:
    def __init__(self, max_val):
        self.current = 0
        self.max = max_val
    
    def __iter__(self):
        return self
    
    def __next__(self):
        self.current += 1
        return self.current  # Never stops!

# CORRECT
class GoodIterator:
    def __next__(self):
        if self.current >= self.max:
            raise StopIteration
        self.current += 1
        return self.current
```

---

## 💡 Pro Tips

### 1. Use Generator for Simple Cases

```python
# Instead of iterator class
class Range:
    def __init__(self, n):
        self.n = n
        self.i = 0
    def __iter__(self): return self
    def __next__(self):
        if self.i >= self.n: raise StopIteration
        self.i += 1
        return self.i - 1

# Use generator
def my_range(n):
    for i in range(n):
        yield i
```

### 2. Separate Iterable from Iterator

```python
# Allow multiple iterations
class Data:
    def __iter__(self):
        return DataIterator(self)  # New iterator each time
```

### 3. Use itertools for Common Patterns

```python
from itertools import chain, islice, cycle
# Don't reinvent the wheel!
```

---

## 💼 Interview Insight

### Q1: What is the difference between iterable and iterator?

> - **Iterable**: Has `__iter__()` method that returns an iterator
> - **Iterator**: Has both `__iter__()` (returns self) and `__next__()` (returns next value)
> - All iterators are iterables, but not all iterables are iterators

### Q2: Why separate iterable from iterator?

> Separating them allows the iterable to be reused. Each call to `__iter__()` creates a fresh iterator, so the data can be iterated multiple times.

### Q3: How does a for loop work internally?

> 1. Calls `iter()` on the object to get an iterator
> 2. Repeatedly calls `next()` on the iterator
> 3. Catches `StopIteration` to end the loop

---

## 🧪 Practice Questions

### Easy:
1. Create a simple countdown iterator
2. Implement a skip iterator (skip every nth element)
3. Write an iterator that returns only even numbers

### Medium:
4. Implement a circular buffer iterator
5. Create a zip iterator without using built-in zip
6. Build a repeat iterator

### Hard:
7. Implement a tree traversal iterator
8. Create a merge iterator for sorted sequences
9. Build a lazy map/filter/reduce chain

---

## 📖 Summary

| Concept | Implementation |
|---------|----------------|
| Iterable | `__iter__()` returns iterator |
| Iterator | `__iter__()` + `__next__()` |
| StopIteration | Signal end of iteration |
| `iter(obj)` | Get iterator from iterable |
| `next(it)` | Get next value |
| Reusable | Separate iterable and iterator |

---

## ⏭️ Next Topic

**[Threading vs Multiprocessing](./04_Threading_vs_Multiprocessing.md)** — Concurrent and parallel programming!

---

*Iterators: One step at a time!* 🚶
