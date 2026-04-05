# 📘 Break, Continue, and Pass

## 📌 Introduction

Sometimes you need more control over your loops. Python provides three special statements:

- **`break`** — Exit the loop immediately
- **`continue`** — Skip the rest of this iteration, go to next
- **`pass`** — Do nothing (placeholder)

Think of driving:
- **break** — Stop the car and get out
- **continue** — Skip this song, play next
- **pass** — Keep driving (do nothing special)

```
┌─────────────────────────────────────────────────────────────┐
│                    LOOP CONTROL FLOW                         │
│                                                              │
│   for/while:                                                 │
│       ├── Statement 1                                        │
│       ├── if condition:                                      │
│       │       break      → EXIT LOOP COMPLETELY             │
│       │       continue   → SKIP TO NEXT ITERATION           │
│       │       pass       → DO NOTHING, CONTINUE NORMALLY    │
│       ├── Statement 2                                        │
│       └── ...                                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Comparison Table

| Statement | What it Does | Use Case |
|-----------|--------------|----------|
| `break` | Exits the entire loop | Stop when target found |
| `continue` | Skips to next iteration | Skip unwanted items |
| `pass` | Does nothing | Placeholder for future code |

### Visual Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│   Loop Start                                                 │
│       │                                                      │
│       ▼                                                      │
│   ┌─────────────┐     No     ┌─────────────┐               │
│   │  More items? │───────────►│  Loop End   │               │
│   └──────┬──────┘            └─────────────┘               │
│          │ Yes                     ▲                        │
│          ▼                         │                        │
│   ┌─────────────┐                  │                        │
│   │ Execute code │                 │                        │
│   └──────┬──────┘                  │                        │
│          │                         │                        │
│          ▼                         │                        │
│   ┌─────────────┐   break          │                        │
│   │  Control?   │──────────────────┘                        │
│   └──────┬──────┘                                           │
│          │ continue                                         │
│          └────────────────┐                                 │
│                           │                                 │
│          ┌────────────────┘                                 │
│          ▼                                                  │
│   ┌─────────────┐                                          │
│   │ Next item   │                                          │
│   └─────────────┘                                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 💻 Code Examples

---

## 1️⃣ The `break` Statement

### 🟢 Basic: Exit Loop When Target Found

```python
# Find first even number
numbers = [1, 3, 5, 8, 9, 10]

for num in numbers:
    print(f"Checking {num}...")
    if num % 2 == 0:
        print(f"Found even number: {num}")
        break  # Exit immediately

print("Loop ended")
```

**Output:**
```
Checking 1...
Checking 3...
Checking 5...
Checking 8...
Found even number: 8
Loop ended
```

### 🟢 Basic: Break in While Loop

```python
# Simple game - keep guessing until correct
secret = 7
attempts = 0

while True:  # Infinite loop
    guess = int(input("Guess (1-10): "))
    attempts += 1
    
    if guess == secret:
        print(f"Correct! Took {attempts} attempts.")
        break  # Exit the infinite loop
    
    print("Wrong, try again!")
```

### 🟡 Intermediate: Break with Else Clause

```python
# The else runs ONLY if loop completes WITHOUT break

# Case 1: Break is triggered
print("Searching for 5:")
for num in [1, 2, 3, 5, 7, 9]:
    if num == 5:
        print(f"Found 5!")
        break
else:
    print("5 not found")  # Won't run (break was hit)

print()

# Case 2: Break is NOT triggered
print("Searching for 50:")
for num in [1, 2, 3, 5, 7, 9]:
    if num == 50:
        print(f"Found 50!")
        break
else:
    print("50 not found")  # Will run (no break)
```

**Output:**
```
Searching for 5:
Found 5!

Searching for 50:
50 not found
```

### 🟡 Intermediate: Break in Nested Loops

```python
# Break only exits the INNERMOST loop!
for i in range(3):
    print(f"Outer loop i={i}")
    
    for j in range(3):
        print(f"  Inner loop j={j}")
        if j == 1:
            print("  Breaking inner loop!")
            break  # Only breaks inner loop
    
    print(f"  Inner loop ended, back to outer")

print("Both loops ended")
```

**Output:**
```
Outer loop i=0
  Inner loop j=0
  Inner loop j=1
  Breaking inner loop!
  Inner loop ended, back to outer
Outer loop i=1
  Inner loop j=0
  Inner loop j=1
  Breaking inner loop!
  Inner loop ended, back to outer
Outer loop i=2
  Inner loop j=0
  Inner loop j=1
  Breaking inner loop!
  Inner loop ended, back to outer
Both loops ended
```

### 🔴 Advanced: Breaking Out of Nested Loops

```python
# Method 1: Use a flag
found = False
for i in range(5):
    for j in range(5):
        if i * j == 12:
            print(f"Found: i={i}, j={j}")
            found = True
            break
    if found:
        break

# Method 2: Use a function (cleaner)
def find_product(target):
    for i in range(5):
        for j in range(5):
            if i * j == target:
                return i, j
    return None

result = find_product(12)
if result:
    print(f"Found: i={result[0]}, j={result[1]}")

# Method 3: Use exception (less common)
class Found(Exception): pass

try:
    for i in range(5):
        for j in range(5):
            if i * j == 12:
                raise Found(f"i={i}, j={j}")
except Found as e:
    print(f"Found: {e}")
```

---

## 2️⃣ The `continue` Statement

### 🟢 Basic: Skip Specific Items

```python
# Print only odd numbers
for num in range(10):
    if num % 2 == 0:
        continue  # Skip even numbers
    print(num)
```

**Output:**
```
1
3
5
7
9
```

### 🟢 Basic: Skip Invalid Data

```python
data = ["apple", "", "banana", None, "cherry", ""]

print("Processing data:")
for item in data:
    if not item:  # Skip empty strings and None
        print("  Skipping invalid item...")
        continue
    
    print(f"  Processing: {item}")
```

**Output:**
```
Processing data:
  Processing: apple
  Skipping invalid item...
  Processing: banana
  Skipping invalid item...
  Processing: cherry
  Skipping invalid item...
```

### 🟡 Intermediate: Continue in While Loop

```python
# Sum only positive numbers
numbers = [1, -2, 3, -4, 5, -6, 7]
total = 0
index = 0

while index < len(numbers):
    num = numbers[index]
    index += 1  # IMPORTANT: increment before continue!
    
    if num < 0:
        print(f"Skipping negative: {num}")
        continue
    
    total += num
    print(f"Added {num}, total = {total}")

print(f"Final sum: {total}")
```

**Output:**
```
Added 1, total = 1
Skipping negative: -2
Added 3, total = 4
Skipping negative: -4
Added 5, total = 9
Skipping negative: -6
Added 7, total = 16
Final sum: 16
```

### 🟡 Intermediate: Continue with Multiple Conditions

```python
# Process only valid entries
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 17, "active": True},
    {"name": "Charlie", "age": 30, "active": False},
    {"name": "Diana", "age": 22, "active": True},
]

print("Active adult users:")
for user in users:
    # Skip inactive users
    if not user["active"]:
        continue
    
    # Skip minors
    if user["age"] < 18:
        continue
    
    print(f"  {user['name']} (age {user['age']})")
```

**Output:**
```
Active adult users:
  Alice (age 25)
  Diana (age 22)
```

### 🔴 Advanced: Continue vs If-Else (Code Style)

```python
# Using nested if-else (harder to read)
for num in range(10):
    if num % 2 == 0:
        if num % 3 == 0:
            print(f"{num}: Divisible by 2 and 3")
        else:
            print(f"{num}: Only divisible by 2")
    else:
        if num % 3 == 0:
            print(f"{num}: Only divisible by 3")
        else:
            print(f"{num}: Not divisible by 2 or 3")

print("\n--- Using continue (guard clauses) ---\n")

# Using continue (cleaner, flatter)
for num in range(10):
    if num % 2 != 0 and num % 3 != 0:
        print(f"{num}: Not divisible by 2 or 3")
        continue
    
    if num % 2 == 0 and num % 3 == 0:
        print(f"{num}: Divisible by 2 and 3")
        continue
    
    if num % 2 == 0:
        print(f"{num}: Only divisible by 2")
        continue
    
    print(f"{num}: Only divisible by 3")
```

---

## 3️⃣ The `pass` Statement

### 🟢 Basic: Placeholder for Future Code

```python
# pass is a placeholder - does nothing

# Placeholder class
class MyClass:
    pass  # Will implement later

# Placeholder function
def my_function():
    pass  # Will implement later

# Placeholder in if statement
x = 10
if x > 5:
    pass  # TODO: handle this case
else:
    print("x is not greater than 5")

# The code runs without error
print("Program continues normally")
```

**Output:**
```
Program continues normally
```

### 🟡 Intermediate: pass vs continue

```python
# They are NOT the same!

print("Using pass:")
for i in range(5):
    if i == 2:
        pass  # Does nothing, continues to next line
    print(f"  i = {i}")

print("\nUsing continue:")
for i in range(5):
    if i == 2:
        continue  # Skips rest of this iteration
    print(f"  i = {i}")
```

**Output:**
```
Using pass:
  i = 0
  i = 1
  i = 2
  i = 3
  i = 4

Using continue:
  i = 0
  i = 1
  i = 3
  i = 4
```

### 🟡 Intermediate: pass in Exception Handling

```python
# Silently ignore an exception
try:
    result = 10 / 0
except ZeroDivisionError:
    pass  # Intentionally ignore this error

print("Program continues...")

# Better practice: at least log it
import logging

try:
    result = 10 / 0
except ZeroDivisionError:
    pass  # Or: logging.warning("Division by zero occurred")
```

### 🔴 Advanced: pass vs Ellipsis (...)

```python
# Both can be used as placeholders

# pass - traditional placeholder
def func1():
    pass

# Ellipsis (...) - modern alternative, often used in type hints
def func2():
    ...

# Ellipsis in type hints (shows "not implemented")
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...  # Method signature only

# Both work the same for placeholders
class MyClass1:
    pass

class MyClass2:
    ...
```

---

## 4️⃣ Combining break, continue, and pass

### Practical Example: Data Processing Pipeline

```python
def process_items(items):
    """Process items with various validations."""
    results = []
    
    for item in items:
        # Skip None values
        if item is None:
            print(f"Skipping None")
            continue
        
        # Skip empty strings
        if isinstance(item, str) and not item.strip():
            print(f"Skipping empty string")
            continue
        
        # Stop if we encounter "STOP"
        if item == "STOP":
            print(f"Encountered STOP signal, ending processing")
            break
        
        # Placeholder for complex validation
        if isinstance(item, dict):
            pass  # TODO: implement dict validation
        
        # Process valid item
        print(f"Processing: {item}")
        results.append(item)
    
    return results

# Test
data = ["apple", "", None, "banana", "STOP", "cherry", "date"]
processed = process_items(data)
print(f"\nProcessed items: {processed}")
```

**Output:**
```
Processing: apple
Skipping empty string
Skipping None
Processing: banana
Encountered STOP signal, ending processing

Processed items: ['apple', 'banana']
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting to Update Counter Before Continue

```python
# Wrong - infinite loop!
i = 0
while i < 5:
    if i == 2:
        continue  # i never increments!
    print(i)
    i += 1

# Correct
i = 0
while i < 5:
    if i == 2:
        i += 1  # Increment BEFORE continue
        continue
    print(i)
    i += 1
```

### ❌ Mistake 2: Breaking Wrong Loop

```python
# Break only exits innermost loop
for i in range(3):
    for j in range(3):
        if j == 1:
            break  # Only breaks inner loop!
        print(f"i={i}, j={j}")
```

### ❌ Mistake 3: Using pass When You Mean continue

```python
# Wrong - pass doesn't skip
for i in range(5):
    if i == 2:
        pass  # Still prints 2!
    print(i)

# Correct - use continue to skip
for i in range(5):
    if i == 2:
        continue
    print(i)
```

### ❌ Mistake 4: Unreachable Code After Break

```python
# Code after break never runs
for i in range(5):
    if i == 2:
        break
        print("This never prints!")  # Unreachable!
```

### ❌ Mistake 5: Misunderstanding Loop Else

```python
# else runs when NO break occurs
for i in range(5):
    print(i)
else:
    print("Loop completed")  # Always runs if no break

# With break
for i in range(5):
    if i == 2:
        break
else:
    print("Loop completed")  # Never runs!
```

---

## 💡 Pro Tips

### 1. Use break for Early Exit
```python
# Find first match efficiently
def find_first(items, target):
    for item in items:
        if item == target:
            return item
    return None  # Not found
```

### 2. Use continue for Clean Code
```python
# Instead of deep nesting
for item in items:
    if not is_valid(item):
        continue
    if not is_active(item):
        continue
    process(item)  # Clean, flat code
```

### 3. Avoid Empty except with pass
```python
# Bad - silences all errors
try:
    risky_operation()
except:
    pass

# Better - be specific
try:
    risky_operation()
except SpecificError:
    pass  # Intentionally ignoring this specific error
```

### 4. Use ... for Type Stubs
```python
from typing import Protocol

class Serializable(Protocol):
    def serialize(self) -> bytes: ...
    def deserialize(self, data: bytes) -> None: ...
```

### 5. Function Return Instead of Flag + Break
```python
# Instead of flag
found = False
for item in items:
    if match(item):
        found = True
        break
if found:
    process(item)

# Use function
def find_match(items):
    for item in items:
        if match(item):
            return item
    return None

result = find_match(items)
if result:
    process(result)
```

---

## 🔍 Real-world Use Cases

### 1. Input Validation Loop
```python
while True:
    email = input("Enter email: ")
    
    if not email:
        print("Email cannot be empty")
        continue
    
    if "@" not in email:
        print("Invalid email format")
        continue
    
    print(f"Email accepted: {email}")
    break
```

### 2. File Processing with Skip
```python
def process_csv_lines(filename):
    with open(filename, 'r') as file:
        for line_num, line in enumerate(file, 1):
            # Skip empty lines
            if not line.strip():
                continue
            
            # Skip comments
            if line.startswith('#'):
                continue
            
            # Skip header
            if line_num == 1:
                continue
            
            yield line.strip()
```

### 3. Search with Early Exit
```python
def find_user(users, user_id):
    for user in users:
        if user['id'] == user_id:
            return user  # Found, exit early
    return None  # Not found

# Usage
user = find_user(all_users, target_id)
if user:
    print(f"Found: {user['name']}")
else:
    print("User not found")
```

### 4. Retry Logic
```python
import time

max_retries = 3
for attempt in range(1, max_retries + 1):
    try:
        result = risky_operation()
        break  # Success, exit loop
    except TemporaryError:
        if attempt == max_retries:
            raise  # Final attempt failed
        print(f"Attempt {attempt} failed, retrying...")
        time.sleep(1)
        continue
else:
    print("All attempts completed")
```

---

## 💼 Interview Insight

### Q1: What's the difference between break and continue?
> - `break`: Exits the loop completely
> - `continue`: Skips rest of current iteration, goes to next

### Q2: What does the else clause do in a loop?
> It executes only if the loop completes normally (without `break`).

### Q3: Can break exit multiple nested loops?
> No, `break` only exits the innermost loop. Use functions or flags for multiple levels.

### Q4: What is pass used for?
> It's a placeholder that does nothing. Used when syntax requires a statement but no action is needed.

### Q5: Is pass the same as continue?
> No! `pass` does nothing and continues to next line. `continue` skips to next iteration.

---

## 🔥 Most Asked Questions

**Q: When should I use break vs return?**
> Use `return` in functions (cleaner). Use `break` in main code or when you need to do something after the loop.

**Q: Can I use break/continue outside loops?**
> No, you'll get `SyntaxError: 'break' outside loop`.

**Q: Does pass affect performance?**
> Negligible. It compiles to a single bytecode instruction.

**Q: Can I have multiple breaks in one loop?**
> Yes, you can have multiple `break` statements in different conditions.

---

## ⚡ Shortcut Tricks

```python
# Quick search with any()
found = any(x > 10 for x in numbers)

# Quick filter without continue
valid = [x for x in items if is_valid(x)]

# Quick first match
first = next((x for x in items if match(x)), None)

# Use walrus operator with break
while (line := input()) != 'quit':
    print(line)
```

---

## 🧪 Practice Questions

### Easy:
1. Print numbers 1-10, but skip 5
2. Find the first number divisible by 7 in range 1-100
3. Create a password validation loop

### Medium:
4. Find prime numbers in range, stop at first prime > 50
5. Process list of numbers, skip negatives, stop at zero
6. Implement a simple menu system with break

### Hard:
7. Break out of nested loops to find a specific pattern
8. Implement retry logic with exponential backoff
9. Parse a file, skip invalid lines, stop at end marker

---

## 🚀 Mini Tasks / Assignments

### Task 1: Number Finder
```python
# Find first number divisible by both 3 and 7
# in range 1-100
# Use break when found
```

### Task 2: Data Cleaner
```python
# Process list: ["hello", "", "world", None, "STOP", "extra"]
# Skip empty and None
# Stop at "STOP"
# Return cleaned list
```

### Task 3: Login System
```python
# Create login with:
# - Max 3 attempts
# - Skip empty passwords (continue)
# - Exit on correct password (break)
# - Handle lockout
```

---

## 📖 Summary

| Statement | Purpose | Effect |
|-----------|---------|--------|
| `break` | Exit loop | Stops loop, runs code after loop |
| `continue` | Skip iteration | Goes to next iteration |
| `pass` | Placeholder | Does nothing, continues normally |
| `else` (loop) | No break clause | Runs if loop completes without break |

### Flow Comparison

```
Normal:    Statement → Statement → Statement → End
Continue:  Statement → Statement → [SKIP] ──→ Next iteration
Break:     Statement → Statement → [EXIT] ──→ After loop
Pass:      Statement → Statement → [Nothing] → Statement → End
```

---

## ⏭️ Next Module

Congratulations! You've completed **Module 2: Control Flow**! 🎉

Next, move to:

**[Module 3: Functions and Modules](../03_Functions_and_Modules/README.md)**
- Defining functions
- Arguments and parameters
- Lambda functions
- Modules and packages

---

*Control flow mastery = Problem solving mastery! 🎯*
