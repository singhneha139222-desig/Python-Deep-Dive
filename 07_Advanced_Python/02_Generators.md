# 📘 Generators in Python

## 📌 Introduction

A **generator** is a special type of function that returns an iterator, yielding values one at a time instead of returning all at once. This makes them incredibly memory-efficient.

Think of generators like a **vending machine** — it doesn't store all drinks in your hands; it gives you one at a time when you ask.

```
┌─────────────────────────────────────────────────────────────┐
│                      GENERATORS                              │
│                                                              │
│   Regular Function              Generator Function           │
│   ┌─────────────────┐          ┌─────────────────┐         │
│   │ def func():     │          │ def gen():      │         │
│   │     return [    │          │     yield 1     │         │
│   │       1, 2, 3   │          │     yield 2     │         │
│   │     ]           │          │     yield 3     │         │
│   └─────────────────┘          └─────────────────┘         │
│                                                              │
│   Memory: O(n)                 Memory: O(1)                 │
│   All at once                  One at a time                │
│                                                              │
│   [1, 2, 3, ..., 1000000]     ← vs →    yield 1, 2, 3...   │
│   Huge memory!                          Tiny memory!        │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### yield vs return

| Aspect | `return` | `yield` |
|--------|----------|---------|
| Execution | Ends function | Pauses function |
| Value | Returns once | Returns many times |
| State | Lost | Preserved |
| Memory | All at once | One at a time |
| Type | Regular value | Generator object |

### Generator Types

1. **Generator Function** — uses `yield`
2. **Generator Expression** — one-liner `(x for x in range(10))`

---

## 💻 Code Examples

### 🟢 Basic: First Generator

```python
# Regular function - returns list
def get_numbers_list(n):
    numbers = []
    for i in range(n):
        numbers.append(i)
    return numbers

# Generator function - yields values
def get_numbers_gen(n):
    for i in range(n):
        yield i

# Compare memory usage
list_nums = get_numbers_list(1000000)  # ~8MB in memory
gen_nums = get_numbers_gen(1000000)    # ~100 bytes!

# Using generator
gen = get_numbers_gen(5)
print(next(gen))  # 0
print(next(gen))  # 1
print(next(gen))  # 2

# Or iterate
for num in get_numbers_gen(5):
    print(num)
# 0, 1, 2, 3, 4
```

### 🟢 Basic: Generator Expression

```python
# List comprehension (creates list)
squares_list = [x**2 for x in range(10)]
print(type(squares_list))  # <class 'list'>

# Generator expression (creates generator)
squares_gen = (x**2 for x in range(10))
print(type(squares_gen))  # <class 'generator'>

# Memory comparison
import sys
list_mem = sys.getsizeof([x**2 for x in range(10000)])
gen_mem = sys.getsizeof(x**2 for x in range(10000))
print(f"List: {list_mem} bytes")  # ~85,000 bytes
print(f"Gen: {gen_mem} bytes")    # ~112 bytes

# Use generator with functions
total = sum(x**2 for x in range(1000))  # No [] needed!
maximum = max(x**2 for x in range(1000))
```

### 🟢 Basic: Understanding yield

```python
def countdown(n):
    print("Starting countdown")
    while n > 0:
        print(f"About to yield {n}")
        yield n
        print(f"Resumed after yielding {n}")
        n -= 1
    print("Countdown complete")

gen = countdown(3)

print("Generator created, nothing executed yet")

print(next(gen))
# Starting countdown
# About to yield 3
# 3

print("Doing something else...")

print(next(gen))
# Resumed after yielding 3
# About to yield 2
# 2

# Continue with loop
for num in gen:
    print(f"Got: {num}")
# Resumed after yielding 2
# About to yield 1
# Got: 1
# Resumed after yielding 1
# Countdown complete
```

### 🟡 Intermediate: Infinite Generators

```python
def infinite_counter(start=0):
    """Generate infinite sequence"""
    n = start
    while True:
        yield n
        n += 1

# Must use with break or islice!
counter = infinite_counter()
for i, num in enumerate(counter):
    if i >= 5:
        break
    print(num)  # 0, 1, 2, 3, 4

# Using itertools.islice
from itertools import islice

counter = infinite_counter(10)
first_5 = list(islice(counter, 5))
print(first_5)  # [10, 11, 12, 13, 14]

# Fibonacci generator
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
print(list(islice(fib, 10)))  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### 🟡 Intermediate: Generator Pipeline

```python
def read_lines(filename):
    """Generator: Read file line by line"""
    with open(filename) as f:
        for line in f:
            yield line.strip()

def filter_empty(lines):
    """Generator: Skip empty lines"""
    for line in lines:
        if line:
            yield line

def parse_numbers(lines):
    """Generator: Convert to numbers"""
    for line in lines:
        try:
            yield int(line)
        except ValueError:
            pass

def square_numbers(numbers):
    """Generator: Square each number"""
    for num in numbers:
        yield num ** 2

# Pipeline - memory efficient!
pipeline = square_numbers(
    parse_numbers(
        filter_empty(
            read_lines('numbers.txt')
        )
    )
)

# Only one line in memory at a time!
for squared in pipeline:
    print(squared)

# Alternative: using generator expressions
lines = (line.strip() for line in open('numbers.txt'))
non_empty = (line for line in lines if line)
numbers = (int(line) for line in non_empty if line.isdigit())
squares = (n ** 2 for n in numbers)
```

### 🟡 Intermediate: yield from

```python
# Without yield from
def chain_old(*iterables):
    for it in iterables:
        for item in it:
            yield item

# With yield from (cleaner)
def chain_new(*iterables):
    for it in iterables:
        yield from it

# Usage
result = list(chain_new([1, 2], [3, 4], [5, 6]))
print(result)  # [1, 2, 3, 4, 5, 6]

# Nested generator delegation
def flatten(nested):
    """Flatten deeply nested lists"""
    for item in nested:
        if isinstance(item, list):
            yield from flatten(item)  # Recursive delegation
        else:
            yield item

nested = [1, [2, [3, 4]], [5, 6], 7]
print(list(flatten(nested)))  # [1, 2, 3, 4, 5, 6, 7]
```

### 🔴 Advanced: send() and throw()

```python
def accumulator():
    """Generator that receives values"""
    total = 0
    while True:
        value = yield total
        if value is not None:
            total += value

acc = accumulator()
print(next(acc))      # 0 (initialize)
print(acc.send(10))   # 10
print(acc.send(20))   # 30
print(acc.send(5))    # 35

# Coroutine pattern
def averager():
    """Running average calculator"""
    count = 0
    total = 0
    average = None
    while True:
        value = yield average
        if value is not None:
            total += value
            count += 1
            average = total / count

avg = averager()
next(avg)  # Prime the generator
print(avg.send(10))  # 10.0
print(avg.send(20))  # 15.0
print(avg.send(30))  # 20.0
print(avg.send(40))  # 25.0
```

### 🔴 Advanced: Generator as Context Manager

```python
from contextlib import contextmanager

@contextmanager
def managed_file(filename, mode='r'):
    """Generator-based context manager"""
    f = open(filename, mode)
    try:
        yield f
    finally:
        f.close()

with managed_file('test.txt', 'w') as f:
    f.write('Hello!')

# Timer context manager
import time

@contextmanager
def timer(name):
    start = time.time()
    try:
        yield
    finally:
        elapsed = time.time() - start
        print(f"{name} took {elapsed:.2f}s")

with timer("Heavy computation"):
    sum(i**2 for i in range(1000000))
```

### 🔴 Advanced: Generator-Based State Machine

```python
def traffic_light():
    """State machine using generator"""
    while True:
        yield 'RED'
        yield 'YELLOW'
        yield 'GREEN'
        yield 'YELLOW'

light = traffic_light()
for _ in range(8):
    print(next(light))
# RED, YELLOW, GREEN, YELLOW, RED, YELLOW, GREEN, YELLOW

# More complex state machine
def vending_machine():
    """Vending machine state machine"""
    while True:
        coin = yield "Insert coin (send amount)"
        if coin >= 100:
            product = yield "Select product (send name)"
            change = coin - 100
            yield f"Dispensing {product}. Change: {change}"
        else:
            yield f"Need {100 - coin} more"

vm = vending_machine()
print(next(vm))           # Insert coin
print(vm.send(150))       # Select product
print(vm.send("Soda"))    # Dispensing Soda. Change: 50
```

---

## 📊 Generator Methods

| Method | Description |
|--------|-------------|
| `next(gen)` | Get next value |
| `gen.send(value)` | Send value into generator |
| `gen.throw(exc)` | Throw exception into generator |
| `gen.close()` | Close generator |

---

## 📊 itertools Module

```python
from itertools import (
    count,      # Infinite counter
    cycle,      # Cycle through iterable
    repeat,     # Repeat value
    islice,     # Slice iterator
    chain,      # Chain iterables
    zip_longest,# Zip with fill
    groupby,    # Group consecutive elements
    takewhile,  # Take while condition
    dropwhile,  # Drop while condition
    filterfalse # Opposite of filter
)

# count: infinite counter
from itertools import count, islice
print(list(islice(count(10, 2), 5)))  # [10, 12, 14, 16, 18]

# cycle: repeat forever
from itertools import cycle
colors = cycle(['red', 'green', 'blue'])
for _, color in zip(range(5), colors):
    print(color)  # red, green, blue, red, green

# chain: combine iterables
from itertools import chain
combined = chain([1, 2], [3, 4], [5, 6])
print(list(combined))  # [1, 2, 3, 4, 5, 6]
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Generator Exhaustion

```python
# WRONG - generator exhausted after first use
gen = (x**2 for x in range(5))
print(list(gen))  # [0, 1, 4, 9, 16]
print(list(gen))  # [] - Empty!

# CORRECT - create new generator
def get_squares():
    return (x**2 for x in range(5))

print(list(get_squares()))  # [0, 1, 4, 9, 16]
print(list(get_squares()))  # [0, 1, 4, 9, 16]
```

### ❌ Mistake 2: Trying to Index Generator

```python
# WRONG
gen = (x for x in range(10))
# gen[0]  # TypeError: generator object is not subscriptable

# CORRECT - convert to list or use islice
gen = (x for x in range(10))
print(list(gen)[0])  # 0

# Or use islice for first n
from itertools import islice
gen = (x for x in range(10))
print(list(islice(gen, 1))[0])  # 0
```

### ❌ Mistake 3: Not Handling StopIteration

```python
# WRONG - StopIteration not handled
gen = (x for x in range(3))
print(next(gen))  # 0
print(next(gen))  # 1
print(next(gen))  # 2
# print(next(gen))  # StopIteration!

# CORRECT - use default
gen = (x for x in range(3))
for _ in range(5):
    print(next(gen, "Done"))
# 0, 1, 2, Done, Done
```

---

## 💡 Pro Tips

### 1. Use Generator Expressions in Function Calls

```python
# Don't create intermediate list
total = sum(x**2 for x in range(1000))  # Good
total = sum([x**2 for x in range(1000)])  # Creates list
```

### 2. Chain Generators for Pipelines

```python
# Each step is a generator
raw = read_file()
cleaned = clean_data(raw)
parsed = parse_data(cleaned)
result = process_data(parsed)
```

### 3. Use itertools for Common Patterns

```python
from itertools import islice, chain, groupby
# Don't reinvent the wheel!
```

---

## 💼 Interview Insight

### Q1: Difference between generator and iterator?

> - **Iterator**: Object with `__iter__` and `__next__` methods
> - **Generator**: Special iterator created with `yield` or generator expression
> - All generators are iterators, but not all iterators are generators

### Q2: When to use generators?

> - Large datasets that don't fit in memory
> - Data pipelines/streaming
> - Infinite sequences
> - Lazy evaluation

### Q3: What is `yield from`?

> `yield from` delegates to another generator or iterable, making it easier to create nested generators and avoiding explicit loops.

---

## 🧪 Practice Questions

### Easy:
1. Create a generator that yields even numbers
2. Write a generator expression for squares
3. Implement a countdown generator

### Medium:
4. Create a file reader generator
5. Implement a running average generator
6. Build a paginated data generator

### Hard:
7. Implement a generator-based pipeline
8. Create a coroutine for data processing
9. Build a tree traversal generator

---

## 📖 Summary

| Concept | Usage |
|---------|-------|
| `yield` | Pause and return value |
| `next()` | Get next value |
| `send()` | Send value into generator |
| `yield from` | Delegate to sub-generator |
| Generator expr | `(x for x in iterable)` |
| Memory | O(1) regardless of size |

---

## ⏭️ Next Topic

**[Iterators](./03_Iterators.md)** — Build your own iteration!

---

*Generators: Efficient iteration, one at a time!* ⚡
