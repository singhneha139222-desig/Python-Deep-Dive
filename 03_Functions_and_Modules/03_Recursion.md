# 📘 Recursion in Python

## 📌 Introduction

**Recursion** is a programming technique where a function calls itself to solve a problem. It's like looking into two mirrors facing each other — the image repeats infinitely.

In programming, recursion breaks down complex problems into smaller, identical sub-problems until reaching a simple **base case** that can be solved directly.

```
┌─────────────────────────────────────────────────────────────┐
│                    RECURSION CONCEPT                         │
│                                                              │
│   Problem: factorial(5)                                      │
│                                                              │
│   factorial(5)                                               │
│       └── 5 × factorial(4)                                   │
│               └── 4 × factorial(3)                           │
│                       └── 3 × factorial(2)                   │
│                               └── 2 × factorial(1)           │
│                                       └── 1 (base case!)     │
│                                                              │
│   Result: 5 × 4 × 3 × 2 × 1 = 120                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. The Two Essential Parts

Every recursive function must have:

| Part | Description | Purpose |
|------|-------------|---------|
| **Base Case** | Condition where recursion stops | Prevents infinite recursion |
| **Recursive Case** | Function calls itself | Breaks problem into smaller parts |

```python
def recursive_function(problem):
    if base_condition:     # BASE CASE
        return simple_result
    else:                  # RECURSIVE CASE
        return recursive_function(smaller_problem)
```

### 2. How Recursion Works (Call Stack)

```
┌─────────────────────────────────────────────────────────────┐
│                      CALL STACK                              │
│                                                              │
│   factorial(3) calls factorial(2)                           │
│       ↓                                                      │
│   ┌─────────────────┐                                       │
│   │ factorial(3)    │ ← Waiting for factorial(2)            │
│   ├─────────────────┤                                       │
│   │ factorial(2)    │ ← Waiting for factorial(1)            │
│   ├─────────────────┤                                       │
│   │ factorial(1)    │ ← Returns 1 (base case)               │
│   └─────────────────┘                                       │
│                                                              │
│   Then unwinds:                                              │
│   factorial(1) returns 1                                     │
│   factorial(2) returns 2 × 1 = 2                            │
│   factorial(3) returns 3 × 2 = 6                            │
└─────────────────────────────────────────────────────────────┘
```

### 3. When to Use Recursion

✅ **Good for:**
- Tree/graph traversal
- Divide and conquer algorithms
- Problems with natural recursive structure
- Mathematical sequences (factorial, Fibonacci)

❌ **Avoid when:**
- Simple iteration works
- Deep recursion (Python has ~1000 limit)
- Performance is critical

---

## 💻 Code Examples

### 🟢 Basic: Factorial

```python
def factorial(n):
    """
    Calculate factorial of n.
    n! = n × (n-1) × (n-2) × ... × 1
    """
    # Base case
    if n <= 1:
        return 1
    
    # Recursive case
    return n * factorial(n - 1)

# Test
for i in range(6):
    print(f"{i}! = {factorial(i)}")
```

**Output:**
```
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
```

### 🟢 Basic: Countdown

```python
def countdown(n):
    """Count down from n to 1."""
    # Base case
    if n <= 0:
        print("Blast off!")
        return
    
    # Recursive case
    print(n)
    countdown(n - 1)

countdown(5)
```

**Output:**
```
5
4
3
2
1
Blast off!
```

### 🟢 Basic: Sum of List

```python
def sum_list(numbers):
    """Calculate sum of list recursively."""
    # Base case: empty list
    if len(numbers) == 0:
        return 0
    
    # Recursive case: first element + sum of rest
    return numbers[0] + sum_list(numbers[1:])

# Test
print(sum_list([1, 2, 3, 4, 5]))  # 15
print(sum_list([]))               # 0
print(sum_list([10]))             # 10
```

### 🟡 Intermediate: Fibonacci Sequence

```python
def fibonacci(n):
    """
    Return the nth Fibonacci number.
    F(0) = 0, F(1) = 1, F(n) = F(n-1) + F(n-2)
    """
    # Base cases
    if n == 0:
        return 0
    if n == 1:
        return 1
    
    # Recursive case
    return fibonacci(n - 1) + fibonacci(n - 2)

# Print first 10 Fibonacci numbers
print("Fibonacci sequence:")
for i in range(10):
    print(f"F({i}) = {fibonacci(i)}")
```

**Output:**
```
Fibonacci sequence:
F(0) = 0
F(1) = 1
F(2) = 1
F(3) = 2
F(4) = 3
F(5) = 5
F(6) = 8
F(7) = 13
F(8) = 21
F(9) = 34
```

### 🟡 Intermediate: Power Function

```python
def power(base, exp):
    """Calculate base raised to exp."""
    # Base case
    if exp == 0:
        return 1
    
    # Handle negative exponents
    if exp < 0:
        return 1 / power(base, -exp)
    
    # Recursive case
    return base * power(base, exp - 1)

# Test
print(f"2^5 = {power(2, 5)}")     # 32
print(f"3^4 = {power(3, 4)}")     # 81
print(f"2^-3 = {power(2, -3)}")   # 0.125
print(f"5^0 = {power(5, 0)}")     # 1
```

### 🟡 Intermediate: String Reversal

```python
def reverse_string(s):
    """Reverse a string recursively."""
    # Base case: empty or single character
    if len(s) <= 1:
        return s
    
    # Recursive case: last char + reverse of rest
    return s[-1] + reverse_string(s[:-1])

# Test
print(reverse_string("hello"))    # olleh
print(reverse_string("Python"))   # nohtyP
print(reverse_string("a"))        # a
print(reverse_string(""))         # (empty)
```

### 🟡 Intermediate: Palindrome Check

```python
def is_palindrome(s):
    """Check if string is a palindrome."""
    # Clean the string (lowercase, letters only)
    s = ''.join(c.lower() for c in s if c.isalnum())
    
    # Base case: 0 or 1 characters
    if len(s) <= 1:
        return True
    
    # Check first and last, then recurse on middle
    if s[0] != s[-1]:
        return False
    
    return is_palindrome(s[1:-1])

# Test
print(is_palindrome("radar"))          # True
print(is_palindrome("hello"))          # False
print(is_palindrome("A man a plan a canal Panama"))  # True
print(is_palindrome("Was it a car or a cat I saw?")) # True
```

### 🔴 Advanced: Binary Search

```python
def binary_search(arr, target, low=0, high=None):
    """
    Search for target in sorted array.
    Returns index or -1 if not found.
    """
    if high is None:
        high = len(arr) - 1
    
    # Base case: not found
    if low > high:
        return -1
    
    mid = (low + high) // 2
    
    # Base case: found
    if arr[mid] == target:
        return mid
    
    # Recursive cases
    if arr[mid] > target:
        return binary_search(arr, target, low, mid - 1)
    else:
        return binary_search(arr, target, mid + 1, high)

# Test
sorted_list = [1, 3, 5, 7, 9, 11, 13, 15]
print(f"Searching in: {sorted_list}")
print(f"Index of 7: {binary_search(sorted_list, 7)}")   # 3
print(f"Index of 1: {binary_search(sorted_list, 1)}")   # 0
print(f"Index of 15: {binary_search(sorted_list, 15)}") # 7
print(f"Index of 6: {binary_search(sorted_list, 6)}")   # -1
```

### 🔴 Advanced: Tower of Hanoi

```python
def hanoi(n, source, auxiliary, target):
    """
    Solve Tower of Hanoi puzzle.
    Move n disks from source to target using auxiliary.
    """
    if n == 1:
        print(f"Move disk 1 from {source} to {target}")
        return
    
    # Move n-1 disks from source to auxiliary
    hanoi(n - 1, source, target, auxiliary)
    
    # Move largest disk from source to target
    print(f"Move disk {n} from {source} to {target}")
    
    # Move n-1 disks from auxiliary to target
    hanoi(n - 1, auxiliary, source, target)

print("Tower of Hanoi with 3 disks:")
hanoi(3, 'A', 'B', 'C')
```

**Output:**
```
Tower of Hanoi with 3 disks:
Move disk 1 from A to C
Move disk 2 from A to B
Move disk 1 from C to B
Move disk 3 from A to C
Move disk 1 from B to A
Move disk 2 from B to C
Move disk 1 from A to C
```

### 🔴 Advanced: Directory Tree Traversal

```python
import os

def list_files(path, indent=0):
    """List all files in directory tree recursively."""
    prefix = "  " * indent
    
    try:
        items = os.listdir(path)
    except PermissionError:
        print(f"{prefix}[Permission Denied]")
        return
    
    for item in sorted(items):
        full_path = os.path.join(path, item)
        
        if os.path.isdir(full_path):
            print(f"{prefix}📁 {item}/")
            list_files(full_path, indent + 1)  # Recursive call
        else:
            print(f"{prefix}📄 {item}")

# Usage (be careful with large directories!)
# list_files(".")
```

### 🔴 Advanced: Memoization (Optimized Recursion)

```python
# Naive Fibonacci - very slow!
def fib_naive(n):
    if n <= 1:
        return n
    return fib_naive(n - 1) + fib_naive(n - 2)

# Memoized Fibonacci - fast!
def fib_memo(n, cache={}):
    if n in cache:
        return cache[n]
    if n <= 1:
        return n
    
    result = fib_memo(n - 1, cache) + fib_memo(n - 2, cache)
    cache[n] = result
    return result

# Using functools.lru_cache - even easier!
from functools import lru_cache

@lru_cache(maxsize=None)
def fib_cached(n):
    if n <= 1:
        return n
    return fib_cached(n - 1) + fib_cached(n - 2)

import time

# Compare performance
n = 35

start = time.time()
result = fib_naive(n)
print(f"Naive: F({n}) = {result}, time: {time.time() - start:.4f}s")

start = time.time()
result = fib_memo(n)
print(f"Memoized: F({n}) = {result}, time: {time.time() - start:.6f}s")

start = time.time()
result = fib_cached(n)
print(f"lru_cache: F({n}) = {result}, time: {time.time() - start:.6f}s")
```

**Output:**
```
Naive: F(35) = 9227465, time: 2.4531s
Memoized: F(35) = 9227465, time: 0.000023s
lru_cache: F(35) = 9227465, time: 0.000019s
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Missing Base Case

```python
# WRONG - infinite recursion!
def countdown_wrong(n):
    print(n)
    countdown_wrong(n - 1)  # Never stops!

# CORRECT
def countdown_right(n):
    if n <= 0:  # Base case
        return
    print(n)
    countdown_right(n - 1)
```

### ❌ Mistake 2: Base Case Never Reached

```python
# WRONG - base case never reached
def sum_positive(n):
    if n == 0:  # This might never be True!
        return 0
    return n + sum_positive(n - 2)  # Skips 0 if n is odd

# CORRECT
def sum_positive(n):
    if n <= 0:  # Use <= instead of ==
        return 0
    return n + sum_positive(n - 1)
```

### ❌ Mistake 3: Stack Overflow

```python
# WRONG - will hit recursion limit
def count_up(n):
    print(n)
    count_up(n + 1)  # Goes to infinity!

# Python's default recursion limit is ~1000
import sys
print(f"Recursion limit: {sys.getrecursionlimit()}")

# You can increase it (carefully!)
# sys.setrecursionlimit(2000)
```

### ❌ Mistake 4: Not Returning Recursive Result

```python
# WRONG - doesn't return!
def factorial_wrong(n):
    if n <= 1:
        return 1
    factorial_wrong(n - 1) * n  # Missing return!

# CORRECT
def factorial_right(n):
    if n <= 1:
        return 1
    return factorial_right(n - 1) * n
```

---

## 💡 Pro Tips

### 1. Use Memoization for Overlapping Subproblems
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def expensive_recursive_function(n):
    # Results are cached
    pass
```

### 2. Consider Iteration for Deep Recursion
```python
# Recursive (may hit stack limit)
def sum_recursive(n):
    if n <= 0:
        return 0
    return n + sum_recursive(n - 1)

# Iterative (no stack limit)
def sum_iterative(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total
```

### 3. Tail Recursion Optimization (Manual)
```python
# Not optimized (Python doesn't do TCO)
def factorial(n, accumulator=1):
    if n <= 1:
        return accumulator
    return factorial(n - 1, n * accumulator)
```

### 4. Visualize the Recursion Tree
```python
def fib(n, depth=0):
    print("  " * depth + f"fib({n})")
    if n <= 1:
        return n
    return fib(n - 1, depth + 1) + fib(n - 2, depth + 1)

fib(4)  # See the call tree
```

---

## 🔍 Real-world Use Cases

### 1. JSON/Nested Data Processing
```python
def flatten_dict(d, parent_key='', sep='_'):
    """Flatten a nested dictionary."""
    items = {}
    for key, value in d.items():
        new_key = f"{parent_key}{sep}{key}" if parent_key else key
        if isinstance(value, dict):
            items.update(flatten_dict(value, new_key, sep))
        else:
            items[new_key] = value
    return items

nested = {
    'a': 1,
    'b': {
        'c': 2,
        'd': {
            'e': 3
        }
    }
}
print(flatten_dict(nested))
# {'a': 1, 'b_c': 2, 'b_d_e': 3}
```

### 2. File System Operations
```python
def get_total_size(path):
    """Calculate total size of directory recursively."""
    import os
    total = 0
    
    if os.path.isfile(path):
        return os.path.getsize(path)
    
    for item in os.listdir(path):
        item_path = os.path.join(path, item)
        total += get_total_size(item_path)
    
    return total
```

### 3. HTML/XML Parsing
```python
def find_all_tags(element, tag_name, results=None):
    """Find all elements with given tag name."""
    if results is None:
        results = []
    
    if element.tag == tag_name:
        results.append(element)
    
    for child in element:
        find_all_tags(child, tag_name, results)
    
    return results
```

---

## 💼 Interview Insight

### Q1: What is recursion?
> A function that calls itself to solve smaller instances of the same problem.

### Q2: What are the two essential parts of recursion?
> **Base case** (stopping condition) and **recursive case** (self-call).

### Q3: What is the time complexity of naive Fibonacci?
> O(2^n) - exponential. With memoization, it becomes O(n).

### Q4: When should you avoid recursion?
> - When iteration is simpler
> - For very deep recursion (stack overflow risk)
> - When performance is critical and memoization isn't possible

### Q5: What is tail recursion?
> When the recursive call is the last operation. Some languages optimize this, but Python doesn't.

---

## 🔥 Most Asked Questions

**Q: What is the recursion limit in Python?**
> Default is ~1000. Check with `sys.getrecursionlimit()`.

**Q: How to avoid stack overflow?**
> - Use iteration instead
> - Increase recursion limit (carefully)
> - Use tail recursion pattern

**Q: What is memoization?**
> Caching results of expensive function calls to avoid redundant calculations.

---

## 🧪 Practice Questions

### Easy:
1. Write a recursive function to calculate the sum of digits
2. Write a recursive function to count elements in a list
3. Write a recursive function to print numbers 1 to n

### Medium:
4. Write a recursive function for binary search
5. Write a recursive function to flatten a nested list
6. Write a recursive function for merge sort

### Hard:
7. Solve the N-Queens problem recursively
8. Implement a recursive descent parser
9. Write a recursive function to generate all permutations

---

## 🚀 Mini Tasks

### Task 1: GCD (Greatest Common Divisor)
```python
# Implement using Euclidean algorithm:
# GCD(a, b) = GCD(b, a % b) if b != 0
# GCD(a, 0) = a
```

### Task 2: Print all subsets
```python
# Given [1, 2, 3], print:
# [], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]
```

### Task 3: Recursive tree structure
```python
# Print a family tree structure given nested dict
```

---

## 📖 Summary

| Concept | Description |
|---------|-------------|
| Recursion | Function calling itself |
| Base Case | Condition to stop recursion |
| Recursive Case | Self-call with smaller problem |
| Call Stack | Memory storing function calls |
| Memoization | Caching to optimize recursion |
| Stack Overflow | Too many recursive calls |

---

## ⏭️ Next Topic

**[Lambda Functions](./04_Lambda.md)** — Anonymous one-line functions!

---

*Recursion: To understand recursion, you must first understand recursion.* 🔄
