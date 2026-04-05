# 📘 Module 3: Functions and Modules

Welcome to **Module 3**! This module covers one of the most important concepts in programming — **functions**. Functions let you organize code into reusable blocks, making your programs cleaner, more efficient, and easier to maintain.

---

## 📌 Overview of This Module

**Functions** are like recipes — you define them once and use them whenever needed. Instead of writing the same code multiple times, you put it in a function and call it.

**Modules** are like toolboxes — collections of related functions and code that you can import and use in your programs.

---

## 📚 What You Will Learn

| Skill | Description |
|-------|-------------|
| ✅ Define functions | Create reusable code blocks |
| ✅ Use parameters | Pass data into functions |
| ✅ Return values | Get results from functions |
| ✅ Use *args, **kwargs | Handle flexible arguments |
| ✅ Write recursive functions | Functions that call themselves |
| ✅ Create lambda functions | Anonymous one-line functions |
| ✅ Import modules | Use pre-built code libraries |
| ✅ Create packages | Organize your own modules |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_Functions.md` | **Functions Basics** | Defining, calling, return values, docstrings |
| `02_Arguments_kwargs.md` | **Arguments & *args/**kwargs** | Parameters, default values, *args, **kwargs |
| `03_Recursion.md` | **Recursion** | Functions calling themselves, base cases |
| `04_Lambda.md` | **Lambda Functions** | Anonymous functions, map, filter, reduce |
| `05_Modules_Packages.md` | **Modules & Packages** | Importing, creating modules, pip |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_Functions.md
        ↓
        Learn to create and use basic functions
        
Step 2: 02_Arguments_kwargs.md
        ↓
        Master function parameters and flexibility
        
Step 3: 03_Recursion.md
        ↓
        Understand functions calling themselves
        
Step 4: 04_Lambda.md
        ↓
        Write concise anonymous functions
        
Step 5: 05_Modules_Packages.md
        ↓
        Organize and reuse code across files
```

**Estimated Time:** 5-7 hours (with practice)

---

## 💡 Tips Before Starting

### Why Functions Matter:

1. **DRY Principle** — Don't Repeat Yourself
   - Write once, use many times
   
2. **Abstraction** — Hide complexity
   - Use `calculate_tax()` without knowing how
   
3. **Testing** — Easier to test small pieces
   - Test each function independently
   
4. **Collaboration** — Divide work
   - Team members work on different functions

### Visual: Function Concept

```
┌─────────────────────────────────────────────────────────────┐
│                      FUNCTION                                │
│                                                              │
│   ┌──────────┐     ┌────────────────┐     ┌──────────┐     │
│   │  INPUT   │────►│   FUNCTION     │────►│  OUTPUT  │     │
│   │(arguments)│     │  (processing)  │     │ (return) │     │
│   └──────────┘     └────────────────┘     └──────────┘     │
│                                                              │
│   Example:                                                   │
│   ┌──────────┐     ┌────────────────┐     ┌──────────┐     │
│   │  5, 3    │────►│    add(a, b)   │────►│    8     │     │
│   └──────────┘     │  return a + b  │     └──────────┘     │
│                    └────────────────┘                       │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Module Progress Tracker

```
[ ] 01_Functions.md
    [ ] Basic function syntax
    [ ] Return values
    [ ] Docstrings
    [ ] Practice questions
    
[ ] 02_Arguments_kwargs.md
    [ ] Positional arguments
    [ ] Keyword arguments
    [ ] Default values
    [ ] *args and **kwargs
    [ ] Practice questions
    
[ ] 03_Recursion.md
    [ ] Recursive thinking
    [ ] Base cases
    [ ] Common examples
    [ ] Practice questions
    
[ ] 04_Lambda.md
    [ ] Lambda syntax
    [ ] map, filter, reduce
    [ ] Practice questions
    
[ ] 05_Modules_Packages.md
    [ ] Importing modules
    [ ] Creating modules
    [ ] Packages
    [ ] Practice questions
```

---

## 🎯 Module Objectives Checklist

By the end of this module, you should be able to answer:

- [ ] How do you define a function in Python?
- [ ] What is the difference between parameters and arguments?
- [ ] What does `return` do?
- [ ] What is a default parameter?
- [ ] What is the difference between *args and **kwargs?
- [ ] What is recursion and when should you use it?
- [ ] How do you write a lambda function?
- [ ] How do you import a module?
- [ ] What is the difference between a module and a package?

---

## 🔗 Quick Links to Subtopics

1. [Functions Basics](./01_Functions.md)
2. [Arguments and *args/**kwargs](./02_Arguments_kwargs.md)
3. [Recursion](./03_Recursion.md)
4. [Lambda Functions](./04_Lambda.md)
5. [Modules and Packages](./05_Modules_Packages.md)

---

## 📝 Quick Reference Card

```python
# BASIC FUNCTION
def greet(name):
    """Say hello to someone."""
    return f"Hello, {name}!"

result = greet("Alice")  # "Hello, Alice!"

# DEFAULT PARAMETERS
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# *ARGS (variable positional arguments)
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4)  # 10

# **KWARGS (variable keyword arguments)
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

# LAMBDA FUNCTION
square = lambda x: x ** 2
square(5)  # 25

# MAP, FILTER, REDUCE
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x%2==0, numbers))

# IMPORTING MODULES
import math
from math import sqrt, pi
from math import sqrt as square_root

# RECURSION
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 4: Data Structures](../04_Data_Structures/README.md)**
- Lists
- Tuples
- Sets
- Dictionaries
- List Comprehensions

---

> 💪 **Remember:** Functions are the building blocks of good software. Master them, and you can build anything!

---

*Happy Learning! 🚀*
