# 📘 Loops in Python

## 📌 Introduction

**Loops** allow you to execute a block of code multiple times. Instead of writing the same code repeatedly, you use a loop to automate repetition.

Think of it like:
- **For loop:** "For each student in the class, take attendance"
- **While loop:** "While there are dishes to wash, keep washing"

```
┌─────────────────────────────────────────────────────────────┐
│                        LOOP CONCEPT                          │
│                                                              │
│   Without Loop:              With Loop:                      │
│   print("Hello")             for i in range(5):             │
│   print("Hello")                 print("Hello")              │
│   print("Hello")                                             │
│   print("Hello")             # Prints "Hello" 5 times!       │
│   print("Hello")                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Two Types of Loops in Python

| Loop Type | When to Use |
|-----------|-------------|
| `for` loop | When you know HOW MANY times to repeat |
| `while` loop | When you know WHEN to stop (condition-based) |

### The `for` Loop

```python
for item in sequence:
    # Code to execute for each item
```

### The `while` Loop

```python
while condition:
    # Code to execute while condition is True
```

### The `range()` Function

Creates a sequence of numbers:

| Syntax | Result | Description |
|--------|--------|-------------|
| `range(5)` | 0, 1, 2, 3, 4 | Start=0, End=5 (exclusive) |
| `range(2, 5)` | 2, 3, 4 | Start=2, End=5 (exclusive) |
| `range(0, 10, 2)` | 0, 2, 4, 6, 8 | Start=0, End=10, Step=2 |
| `range(5, 0, -1)` | 5, 4, 3, 2, 1 | Countdown |

---

## 💻 Code Examples

---

## 1️⃣ For Loops

### 🟢 Basic: Iterating Over a List

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(f"I like {fruit}")
```

**Output:**
```
I like apple
I like banana
I like cherry
```

### 🟢 Basic: Using range()

```python
# Print numbers 0 to 4
print("Numbers 0-4:")
for i in range(5):
    print(i)

# Print numbers 1 to 5
print("\nNumbers 1-5:")
for i in range(1, 6):
    print(i)

# Print even numbers 0 to 10
print("\nEven numbers 0-10:")
for i in range(0, 11, 2):
    print(i)

# Countdown
print("\nCountdown:")
for i in range(5, 0, -1):
    print(i)
print("Blast off!")
```

**Output:**
```
Numbers 0-4:
0
1
2
3
4

Numbers 1-5:
1
2
3
4
5

Even numbers 0-10:
0
2
4
6
8
10

Countdown:
5
4
3
2
1
Blast off!
```

### 🟢 Basic: Iterating Over Strings

```python
word = "Python"

# Each character
for char in word:
    print(char, end=" ")
print()  # New line

# With index
for i, char in enumerate(word):
    print(f"Index {i}: {char}")
```

**Output:**
```
P y t h o n 

Index 0: P
Index 1: y
Index 2: t
Index 3: h
Index 4: o
Index 5: n
```

### 🟡 Intermediate: Enumerate for Index + Value

```python
fruits = ["apple", "banana", "cherry", "date"]

# Without enumerate (works, but clunky)
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")

print()

# With enumerate (Pythonic!)
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

print()

# Start enumerate from 1
for num, fruit in enumerate(fruits, start=1):
    print(f"{num}. {fruit}")
```

**Output:**
```
0: apple
1: banana
2: cherry
3: date

0: apple
1: banana
2: cherry
3: date

1. apple
2. banana
3. cherry
4. date
```

### 🟡 Intermediate: Iterating Over Dictionaries

```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

# Keys (default)
print("Keys:")
for key in person:
    print(key)

# Values
print("\nValues:")
for value in person.values():
    print(value)

# Key-Value pairs
print("\nKey-Value pairs:")
for key, value in person.items():
    print(f"{key}: {value}")
```

**Output:**
```
Keys:
name
age
city

Values:
Alice
25
New York

Key-Value pairs:
name: Alice
age: 25
city: New York
```

### 🟡 Intermediate: zip() for Parallel Iteration

```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
cities = ["NYC", "LA", "Chicago"]

# Iterate over multiple lists together
for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old and lives in {city}")

# Create dict from two lists
name_age = dict(zip(names, ages))
print(f"\nDictionary: {name_age}")
```

**Output:**
```
Alice is 25 years old and lives in NYC
Bob is 30 years old and lives in LA
Charlie is 35 years old and lives in Chicago

Dictionary: {'Alice': 25, 'Bob': 30, 'Charlie': 35}
```

### 🟡 Intermediate: Nested For Loops

```python
# Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} x {j} = {i*j}")
    print("---")

# Matrix traversal
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

print("Matrix:")
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()  # New line after each row
```

**Output:**
```
1 x 1 = 1
1 x 2 = 2
1 x 3 = 3
---
2 x 1 = 2
2 x 2 = 4
2 x 3 = 6
---
3 x 1 = 3
3 x 2 = 6
3 x 3 = 9
---
Matrix:
1 2 3 
4 5 6 
7 8 9 
```

### 🔴 Advanced: List Comprehension

```python
# Traditional loop
squares = []
for x in range(5):
    squares.append(x ** 2)
print(f"Traditional: {squares}")

# List comprehension (one line!)
squares = [x ** 2 for x in range(5)]
print(f"Comprehension: {squares}")

# With condition
evens = [x for x in range(10) if x % 2 == 0]
print(f"Even numbers: {evens}")

# With transformation and condition
words = ["Hello", "WORLD", "Python"]
lower_long = [w.lower() for w in words if len(w) > 4]
print(f"Processed: {lower_long}")

# Nested list comprehension
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
print(f"Matrix: {matrix}")
```

**Output:**
```
Traditional: [0, 1, 4, 9, 16]
Comprehension: [0, 1, 4, 9, 16]
Even numbers: [0, 2, 4, 6, 8]
Processed: ['hello', 'world', 'python']
Matrix: [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

### 🔴 Advanced: Dictionary and Set Comprehension

```python
# Dictionary comprehension
squares_dict = {x: x**2 for x in range(5)}
print(f"Squares dict: {squares_dict}")

# From two lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
name_age = {name: age for name, age in zip(names, ages)}
print(f"Name-Age: {name_age}")

# Set comprehension (no duplicates)
numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
unique_squares = {x**2 for x in numbers}
print(f"Unique squares: {unique_squares}")
```

**Output:**
```
Squares dict: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
Name-Age: {'Alice': 25, 'Bob': 30, 'Charlie': 35}
Unique squares: {16, 1, 4, 9}
```

---

## 2️⃣ While Loops

### 🟢 Basic: Simple While Loop

```python
count = 0

while count < 5:
    print(f"Count: {count}")
    count += 1  # Don't forget to update!

print("Done!")
```

**Output:**
```
Count: 0
Count: 1
Count: 2
Count: 3
Count: 4
Done!
```

### 🟢 Basic: User Input Loop

```python
# Keep asking until valid input
while True:
    user_input = input("Enter a positive number: ")
    
    if user_input.isdigit() and int(user_input) > 0:
        print(f"You entered: {user_input}")
        break
    else:
        print("Invalid input. Try again.")
```

**Output:**
```
Enter a positive number: -5
Invalid input. Try again.
Enter a positive number: abc
Invalid input. Try again.
Enter a positive number: 42
You entered: 42
```

### 🟡 Intermediate: Sum Until Sentinel Value

```python
# Sum numbers until user enters 0
total = 0
print("Enter numbers to sum (0 to stop):")

while True:
    num = int(input("Number: "))
    
    if num == 0:
        break
    
    total += num
    print(f"Running total: {total}")

print(f"Final sum: {total}")
```

### 🟡 Intermediate: While with Multiple Conditions

```python
attempts = 0
max_attempts = 3
password = "secret"
logged_in = False

while attempts < max_attempts and not logged_in:
    user_input = input("Enter password: ")
    attempts += 1
    
    if user_input == password:
        logged_in = True
        print("Login successful!")
    else:
        remaining = max_attempts - attempts
        if remaining > 0:
            print(f"Wrong password. {remaining} attempts remaining.")
        else:
            print("Account locked!")
```

### 🔴 Advanced: While-Else Clause

```python
# The else block runs if loop completes WITHOUT break
numbers = [1, 3, 5, 7, 9]
target = 4

index = 0
while index < len(numbers):
    if numbers[index] == target:
        print(f"Found {target} at index {index}")
        break
    index += 1
else:
    print(f"{target} not found in the list")

# Same with for loop
for i, num in enumerate(numbers):
    if num == target:
        print(f"Found {target} at index {i}")
        break
else:
    print(f"{target} not found in the list")
```

**Output:**
```
4 not found in the list
4 not found in the list
```

---

## 3️⃣ For vs While: When to Use Which?

| Use `for` when: | Use `while` when: |
|-----------------|-------------------|
| Iterating over a sequence | You don't know iterations in advance |
| You know the number of iterations | Loop based on a condition |
| Working with collections | User input validation |
| Counting/incrementing | Game loops |

### Example: Same Task, Different Approaches

```python
# Task: Print numbers 1 to 5

# FOR loop (preferred for known iterations)
print("Using for:")
for i in range(1, 6):
    print(i, end=" ")
print()

# WHILE loop (works, but more verbose)
print("Using while:")
i = 1
while i <= 5:
    print(i, end=" ")
    i += 1
print()
```

---

## 4️⃣ Common Loop Patterns

### Pattern 1: Accumulator Pattern
```python
# Sum all numbers
numbers = [1, 2, 3, 4, 5]
total = 0  # Accumulator

for num in numbers:
    total += num

print(f"Sum: {total}")  # 15
```

### Pattern 2: Counter Pattern
```python
# Count occurrences
text = "hello world"
count = 0

for char in text:
    if char == 'l':
        count += 1

print(f"Letter 'l' appears {count} times")  # 3
```

### Pattern 3: Search Pattern
```python
# Find first occurrence
numbers = [1, 3, 5, 7, 9]
target = 5
found_index = -1

for i, num in enumerate(numbers):
    if num == target:
        found_index = i
        break

print(f"Found at index: {found_index}")  # 2
```

### Pattern 4: Filter Pattern
```python
# Keep only matching elements
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = []

for num in numbers:
    if num % 2 == 0:
        evens.append(num)

print(f"Even numbers: {evens}")  # [2, 4, 6, 8, 10]

# Same with list comprehension
evens = [num for num in numbers if num % 2 == 0]
```

### Pattern 5: Transform Pattern
```python
# Transform all elements
words = ["hello", "world", "python"]
upper_words = []

for word in words:
    upper_words.append(word.upper())

print(f"Uppercase: {upper_words}")  # ['HELLO', 'WORLD', 'PYTHON']

# Same with list comprehension
upper_words = [word.upper() for word in words]
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Infinite Loop

```python
# Wrong - infinite loop!
x = 0
while x < 5:
    print(x)
    # Forgot to increment x!

# Correct
x = 0
while x < 5:
    print(x)
    x += 1  # Don't forget!
```

### ❌ Mistake 2: Off-by-One Error

```python
# Prints 0-4, not 1-5!
for i in range(5):
    print(i)

# For 1-5:
for i in range(1, 6):  # End is exclusive!
    print(i)
```

### ❌ Mistake 3: Modifying List While Iterating

```python
# Wrong - unexpected behavior!
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Don't do this!
print(numbers)  # [1, 3, 5] - might miss some!

# Correct - create new list
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 != 0]
print(numbers)  # [1, 3, 5]

# Or iterate over a copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:  # Slice creates a copy
    if num % 2 == 0:
        numbers.remove(num)
```

### ❌ Mistake 4: Unnecessary range(len())

```python
# Works, but not Pythonic
fruits = ["apple", "banana", "cherry"]
for i in range(len(fruits)):
    print(fruits[i])

# Better
for fruit in fruits:
    print(fruit)

# If you need index too
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

### ❌ Mistake 5: Wrong Loop Type

```python
# Using while when for is better
fruits = ["apple", "banana", "cherry"]
i = 0
while i < len(fruits):  # Overly complex!
    print(fruits[i])
    i += 1

# Just use for
for fruit in fruits:
    print(fruit)
```

---

## 💡 Pro Tips

### 1. Use enumerate() Instead of range(len())
```python
for i, item in enumerate(items):
    print(f"{i}: {item}")
```

### 2. Use zip() for Parallel Iteration
```python
for name, age in zip(names, ages):
    print(f"{name} is {age}")
```

### 3. Use Comprehensions When Possible
```python
# More readable and faster
squares = [x**2 for x in range(10)]
```

### 4. Use _ for Unused Variables
```python
# When you don't need the variable
for _ in range(5):
    print("Hello")
```

### 5. Use reversed() for Reverse Iteration
```python
for item in reversed(items):
    print(item)
```

---

## 🔍 Real-world Use Cases

### 1. Reading File Line by Line
```python
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())
```

### 2. Processing API Responses
```python
users = [
    {"name": "Alice", "active": True},
    {"name": "Bob", "active": False},
    {"name": "Charlie", "active": True}
]

active_users = [u["name"] for u in users if u["active"]]
print(f"Active users: {active_users}")
```

### 3. Retry Logic
```python
import time

max_retries = 3
for attempt in range(1, max_retries + 1):
    print(f"Attempt {attempt}...")
    success = False  # Simulated result
    
    if success:
        print("Success!")
        break
    
    if attempt < max_retries:
        print("Failed. Retrying...")
        time.sleep(1)
else:
    print("All attempts failed!")
```

---

## 💼 Interview Insight

### Q1: What's the difference between for and while loops?
> - `for`: Iterates over a sequence, known iterations
> - `while`: Runs until condition is False, unknown iterations

### Q2: How do you iterate over a dictionary?
```python
for key in dict:              # Keys
for value in dict.values():   # Values
for key, value in dict.items():  # Both
```

### Q3: What is list comprehension?
> A concise way to create lists: `[expression for item in iterable if condition]`

### Q4: How do you avoid modifying a list while iterating?
> - Create a new list with comprehension
> - Iterate over a copy: `for item in list[:]:`
> - Use filter() function

### Q5: What does the `else` clause do in loops?
> Executes if the loop completes without `break`.

---

## 🔥 Most Asked Questions

**Q: Can you have a for loop inside a while loop?**
> Yes! You can nest any loop inside any other loop.

**Q: What's the maximum number of loop iterations?**
> Unlimited, but be careful of infinite loops!

**Q: How do you loop backwards?**
```python
for i in range(10, 0, -1):  # 10 to 1
for item in reversed(list):  # Reverse iteration
```

**Q: How do you skip every other element?**
```python
for i in range(0, 10, 2):  # Step of 2
for item in list[::2]:     # Slice with step
```

---

## ⚡ Shortcut Tricks

```python
# Quick sum
total = sum(numbers)

# Quick min/max
minimum = min(numbers)
maximum = max(numbers)

# Loop with index
for i, item in enumerate(items):

# Parallel loops
for a, b in zip(list1, list2):

# Flatten nested list
flat = [x for sublist in nested for x in sublist]

# Create dict from loops
d = {k: v for k, v in zip(keys, values)}
```

---

## 🧪 Practice Questions

### Easy:
1. Print numbers 1 to 10
2. Calculate factorial of a number
3. Print all items in a list

### Medium:
4. Find the largest number in a list
5. Count vowels in a string
6. Print the Fibonacci sequence

### Hard:
7. Find prime numbers in a range
8. Implement binary search
9. Generate Pascal's triangle

---

## 🚀 Mini Tasks / Assignments

### Task 1: Multiplication Table
```python
# Print multiplication table for a given number
# Format:
# 5 x 1 = 5
# 5 x 2 = 10
# ...
```

### Task 2: Pattern Printing
```python
# Print this pattern:
# *
# **
# ***
# ****
# *****
```

### Task 3: Number Guessing Game
```python
# Generate random number 1-100
# User guesses, give hints (too high/low)
# Count attempts
```

---

## 📖 Summary

| Concept | Syntax | Use Case |
|---------|--------|----------|
| `for` | `for item in sequence:` | Known iterations |
| `while` | `while condition:` | Condition-based |
| `range()` | `range(start, stop, step)` | Number sequences |
| `enumerate()` | `for i, item in enumerate(seq):` | Index + value |
| `zip()` | `for a, b in zip(l1, l2):` | Parallel iteration |
| Comprehension | `[x for x in seq]` | Create lists |
| Loop else | `for/while ... else:` | No break executed |

---

## ⏭️ Next Topic

Now you know how to repeat code! Let's learn to control loop behavior:

**[Break, Continue, Pass](./03_Break_Continue_Pass.md)** — How to control loop execution.

---

*Loops are the engine of automation. Master them!* 🔄
