# 📘 If-Else Statements (Conditional Statements)

## 📌 Introduction

**Conditional statements** allow your program to make decisions. They let you execute different code based on whether a condition is True or False.

Think of it like a traffic light:
- **If** the light is green → Go
- **Else if** the light is yellow → Slow down
- **Else** → Stop

```
┌─────────────────────────────────────────────────────────────┐
│                    DECISION MAKING                           │
│                                                              │
│                    ┌─────────────┐                          │
│                    │ Is it       │                          │
│               ┌────│  raining?   │────┐                     │
│               │    └─────────────┘    │                     │
│             Yes                       No                    │
│               │                       │                     │
│       ┌───────▼───────┐       ┌───────▼───────┐            │
│       │ Take umbrella │       │  Wear shorts  │            │
│       └───────────────┘       └───────────────┘            │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. The `if` Statement

The simplest form of conditional:

```python
if condition:
    # Code to execute if condition is True
```

### 2. The `if-else` Statement

When you need two paths:

```python
if condition:
    # Code if True
else:
    # Code if False
```

### 3. The `if-elif-else` Statement

When you have multiple conditions:

```python
if condition1:
    # Code if condition1 is True
elif condition2:
    # Code if condition2 is True
elif condition3:
    # Code if condition3 is True
else:
    # Code if no condition is True
```

### 4. Important Rules

| Rule | Description |
|------|-------------|
| **Indentation** | Code inside `if` must be indented (4 spaces) |
| **Colon** | Each `if`, `elif`, `else` ends with `:` |
| **Boolean result** | Condition must evaluate to True or False |
| **Order matters** | Python checks conditions from top to bottom |
| **One path** | Only ONE block of code executes |

---

## 📊 Condition Evaluation

Any expression can be a condition. Python evaluates it as **truthy** or **falsy**:

| Falsy Values | Truthy Values |
|--------------|---------------|
| `False` | `True` |
| `0`, `0.0` | Any non-zero number |
| `""` (empty string) | Any non-empty string |
| `[]`, `()`, `{}` | Non-empty collections |
| `None` | Everything else |

---

## 💻 Code Examples

### 🟢 Basic: Simple if Statement

```python
age = 18

if age >= 18:
    print("You are an adult.")
    print("You can vote!")

print("Program continues...")
```

**Output:**
```
You are an adult.
You can vote!
Program continues...
```

### 🟢 Basic: if-else Statement

```python
temperature = 25

if temperature > 30:
    print("It's hot outside!")
else:
    print("It's pleasant weather.")

print(f"Current temperature: {temperature}°C")
```

**Output:**
```
It's pleasant weather.
Current temperature: 25°C
```

### 🟢 Basic: if-elif-else Statement

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Your score: {score}")
print(f"Your grade: {grade}")
```

**Output:**
```
Your score: 85
Your grade: B
```

### 🟡 Intermediate: Multiple Conditions with Logical Operators

```python
age = 25
has_license = True
has_car = False

# AND - both must be True
if age >= 18 and has_license:
    print("You can drive!")

# OR - at least one must be True
if has_car or has_license:
    print("You have transportation options.")

# NOT - inverts the condition
if not has_car:
    print("You don't own a car.")

# Combined conditions
if age >= 18 and has_license and not has_car:
    print("You can drive but need to rent a car.")
```

**Output:**
```
You can drive!
You have transportation options.
You don't own a car.
You can drive but need to rent a car.
```

### 🟡 Intermediate: Nested if Statements

```python
num = 15

if num > 0:
    print("Number is positive")
    
    if num % 2 == 0:
        print("Number is even")
    else:
        print("Number is odd")
        
        if num % 5 == 0:
            print("Number is divisible by 5")
else:
    print("Number is zero or negative")
```

**Output:**
```
Number is positive
Number is odd
Number is divisible by 5
```

### 🟡 Intermediate: Ternary Operator (Conditional Expression)

```python
# Regular if-else
age = 20
if age >= 18:
    status = "adult"
else:
    status = "minor"

# Same logic with ternary operator (one line!)
status = "adult" if age >= 18 else "minor"
print(f"Status: {status}")

# More examples
x = 10
y = 20

# Find maximum
max_val = x if x > y else y
print(f"Max: {max_val}")

# Conditional message
is_sunny = True
activity = "Go to beach" if is_sunny else "Stay home"
print(f"Plan: {activity}")

# Nested ternary (use sparingly!)
score = 75
grade = "A" if score >= 90 else "B" if score >= 80 else "C" if score >= 70 else "F"
print(f"Grade: {grade}")
```

**Output:**
```
Status: adult
Max: 20
Plan: Go to beach
Grade: C
```

### 🟡 Intermediate: Checking Multiple Values

```python
# Check if value is in a set of values
day = "Saturday"

# Long way
if day == "Saturday" or day == "Sunday":
    print("It's weekend!")

# Better way - using 'in'
if day in ("Saturday", "Sunday"):
    print("It's weekend!")

# For multiple conditions
fruit = "apple"
if fruit in ["apple", "banana", "orange"]:
    print("Common fruit!")

# Check character type
char = 'A'
if char in 'aeiouAEIOU':
    print("It's a vowel!")
```

**Output:**
```
It's weekend!
It's weekend!
Common fruit!
It's a vowel!
```

### 🔴 Advanced: Match Statement (Python 3.10+)

```python
# Pattern matching (like switch-case in other languages)
def http_status(status):
    match status:
        case 200:
            return "OK"
        case 404:
            return "Not Found"
        case 500:
            return "Internal Server Error"
        case _:  # Default case (underscore matches anything)
            return "Unknown Status"

print(http_status(200))  # OK
print(http_status(404))  # Not Found
print(http_status(999))  # Unknown Status

# Pattern matching with conditions
def describe_point(point):
    match point:
        case (0, 0):
            return "Origin"
        case (0, y):
            return f"On Y-axis at {y}"
        case (x, 0):
            return f"On X-axis at {x}"
        case (x, y) if x == y:
            return f"Diagonal point at ({x}, {y})"
        case (x, y):
            return f"Point at ({x}, {y})"

print(describe_point((0, 0)))   # Origin
print(describe_point((5, 0)))   # On X-axis at 5
print(describe_point((3, 3)))   # Diagonal point at (3, 3)
print(describe_point((2, 5)))   # Point at (2, 5)
```

### 🔴 Advanced: Walrus Operator in Conditions

```python
# Python 3.8+ walrus operator (:=)
# Assign and use in same expression

data = [1, 2, 3, 4, 5]

# Without walrus
n = len(data)
if n > 3:
    print(f"List has {n} items, that's a lot!")

# With walrus operator
if (n := len(data)) > 3:
    print(f"List has {n} items, that's a lot!")

# Useful for input validation
while (user_input := input("Enter 'quit' to exit: ")) != "quit":
    print(f"You entered: {user_input}")

# In list comprehensions
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
results = [y for x in numbers if (y := x ** 2) > 20]
print(results)  # [25, 36, 49, 64, 81, 100]
```

### 🔴 Advanced: Truthiness in Conditions

```python
# Using truthiness for cleaner code

# Instead of:
name = "Alice"
if name != "":
    print(f"Hello, {name}!")

# Use:
if name:  # Non-empty string is truthy
    print(f"Hello, {name}!")

# Instead of:
items = [1, 2, 3]
if len(items) > 0:
    print("List is not empty")

# Use:
if items:  # Non-empty list is truthy
    print("List is not empty")

# Instead of:
value = None
if value is not None:
    print(f"Value: {value}")

# Use (be careful!):
if value:  # None is falsy
    print(f"Value: {value}")
# But 0 and "" are also falsy, so be specific when needed!

# Safe pattern for optional values
result = value or "default"
print(f"Result: {result}")  # "default" if value is falsy
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting the Colon

```python
# Wrong
if age >= 18
    print("Adult")  # SyntaxError!

# Correct
if age >= 18:
    print("Adult")
```

### ❌ Mistake 2: Wrong Indentation

```python
# Wrong
if age >= 18:
print("Adult")  # IndentationError!

# Correct
if age >= 18:
    print("Adult")
```

### ❌ Mistake 3: Using = Instead of ==

```python
# Wrong
if x = 10:  # SyntaxError! (assignment, not comparison)
    print("x is 10")

# Correct
if x == 10:
    print("x is 10")
```

### ❌ Mistake 4: Checking Boolean with ==

```python
# Unnecessary
if is_valid == True:
    print("Valid")

# Better
if is_valid:
    print("Valid")

# Checking for False
if not is_valid:
    print("Invalid")
```

### ❌ Mistake 5: Unreachable Code

```python
# Wrong - second condition never reached
if score >= 60:
    print("Pass")
elif score >= 90:  # This NEVER executes! (already caught by >= 60)
    print("Excellent")

# Correct - check more specific condition first
if score >= 90:
    print("Excellent")
elif score >= 60:
    print("Pass")
else:
    print("Fail")
```

### ❌ Mistake 6: Floating Point Comparison

```python
# Wrong
if 0.1 + 0.2 == 0.3:  # False!
    print("Equal")

# Correct
import math
if math.isclose(0.1 + 0.2, 0.3):
    print("Equal")
```

---

## 💡 Pro Tips

### 1. Use Guard Clauses
```python
# Instead of nested conditions
def process_data(data):
    if data is not None:
        if len(data) > 0:
            if data[0] > 0:
                # Process...
                pass

# Use guard clauses (early returns)
def process_data(data):
    if data is None:
        return
    if len(data) == 0:
        return
    if data[0] <= 0:
        return
    # Process...
```

### 2. Use dict Instead of Many elif
```python
# Instead of:
def get_day_name(day):
    if day == 1:
        return "Monday"
    elif day == 2:
        return "Tuesday"
    # ... more elif

# Use dict:
def get_day_name(day):
    days = {1: "Monday", 2: "Tuesday", 3: "Wednesday", 
            4: "Thursday", 5: "Friday", 6: "Saturday", 7: "Sunday"}
    return days.get(day, "Invalid")
```

### 3. Use any() and all()
```python
conditions = [True, False, True]

# Check if ALL conditions are True
if all(conditions):
    print("All true")

# Check if ANY condition is True
if any(conditions):
    print("At least one true")

# Practical example
ages = [25, 30, 18, 22]
if all(age >= 18 for age in ages):
    print("Everyone is an adult")
```

### 4. Simplify Boolean Returns
```python
# Instead of:
def is_adult(age):
    if age >= 18:
        return True
    else:
        return False

# Use:
def is_adult(age):
    return age >= 18
```

---

## 🔍 Real-world Use Cases

### 1. Login Validation
```python
def login(username, password):
    stored_username = "admin"
    stored_password = "secret123"
    
    if not username:
        return "Username cannot be empty"
    if not password:
        return "Password cannot be empty"
    if username != stored_username:
        return "User not found"
    if password != stored_password:
        return "Incorrect password"
    
    return "Login successful!"

print(login("admin", "secret123"))  # Login successful!
```

### 2. Shopping Cart Discount
```python
def calculate_discount(total, is_member, coupon_code):
    discount = 0
    
    # Member discount
    if is_member:
        discount += 10
    
    # Amount-based discount
    if total >= 100:
        discount += 15
    elif total >= 50:
        discount += 10
    
    # Coupon code
    if coupon_code == "SAVE20":
        discount += 20
    elif coupon_code == "SAVE10":
        discount += 10
    
    # Cap discount at 50%
    discount = min(discount, 50)
    
    final_price = total * (1 - discount/100)
    return final_price, discount

price, disc = calculate_discount(120, True, "SAVE20")
print(f"Discount: {disc}%, Final: ${price}")
```

### 3. Grade Calculator
```python
def calculate_grade(scores):
    if not scores:
        return "No scores provided"
    
    average = sum(scores) / len(scores)
    
    if average >= 90:
        grade, remark = "A", "Excellent!"
    elif average >= 80:
        grade, remark = "B", "Good job!"
    elif average >= 70:
        grade, remark = "C", "Satisfactory"
    elif average >= 60:
        grade, remark = "D", "Needs improvement"
    else:
        grade, remark = "F", "Failed"
    
    return f"Average: {average:.1f}, Grade: {grade}, {remark}"

print(calculate_grade([85, 90, 78, 92]))
```

---

## 💼 Interview Insight

### Q1: What is the difference between `if-elif-else` and multiple `if` statements?
> - `if-elif-else`: Only ONE block executes (first true condition)
> - Multiple `if`: ALL true conditions execute

```python
x = 10
# Only "A" prints
if x > 5:
    print("A")
elif x > 3:
    print("B")  # Skipped!

# Both "A" and "B" print
if x > 5:
    print("A")
if x > 3:
    print("B")
```

### Q2: What is short-circuit evaluation?
> Python stops evaluating a condition as soon as the result is determined.
```python
# check_b() never called if check_a() is False
if check_a() and check_b():
    pass
```

### Q3: Can you have an `if` without an `else`?
> Yes! `else` is optional. Use it only when you need to handle the false case.

### Q4: What is the ternary operator in Python?
```python
result = value_if_true if condition else value_if_false
```

### Q5: How do you check if a variable is None?
```python
if x is None:    # Preferred
if x == None:    # Works but not recommended
if not x:        # Dangerous! Also matches 0, "", [], etc.
```

---

## 🔥 Most Asked Questions

**Q: Can `elif` exist without `if`?**
> No. `elif` must follow an `if` or another `elif`.

**Q: Can `else` exist without `if`?**
> No. `else` must follow an `if` or `elif`.

**Q: How many `elif` can I have?**
> Unlimited. But too many suggests you might need a different approach (like dict mapping).

**Q: Is there a switch statement in Python?**
> Yes! Python 3.10+ has `match-case` (pattern matching).

---

## ⚡ Shortcut Tricks

```python
# Quick max/min
max_val = a if a > b else b
min_val = a if a < b else b

# Default value
name = user_name or "Guest"

# Toggle boolean
flag = not flag

# Clamp value to range
value = max(min_val, min(value, max_val))

# Check multiple conditions
if x in (1, 2, 3):  # Faster than x == 1 or x == 2 or x == 3
```

---

## 🧪 Practice Questions

### Easy:
1. Check if a number is positive, negative, or zero
2. Check if a number is even or odd
3. Find the largest of three numbers

### Medium:
4. Check if a year is a leap year
5. Create a simple calculator (+, -, *, /)
6. Check if a character is a vowel, consonant, digit, or special character

### Hard:
7. Validate a password (min 8 chars, has uppercase, lowercase, digit)
8. Create a tax calculator with multiple tax brackets
9. Build a simple rock-paper-scissors game

---

## 🚀 Mini Tasks / Assignments

### Task 1: BMI Calculator
```python
# Take height (m) and weight (kg)
# Calculate BMI = weight / (height ** 2)
# Categories:
# < 18.5: Underweight
# 18.5-24.9: Normal
# 25-29.9: Overweight
# >= 30: Obese
```

### Task 2: Ticket Pricing
```python
# Movie ticket pricing:
# Age < 5: Free
# Age 5-12: $10
# Age 13-59: $15
# Age 60+: $8
# Weekend adds $2 to all prices
```

### Task 3: Triangle Type Checker
```python
# Given three sides, determine:
# - Is it a valid triangle?
# - If valid: Equilateral, Isosceles, or Scalene?
```

---

## 📖 Summary

| Concept | Syntax | Use Case |
|---------|--------|----------|
| `if` | `if condition:` | Single condition check |
| `if-else` | `if: ... else:` | Two-way decision |
| `if-elif-else` | `if: elif: else:` | Multiple conditions |
| Ternary | `x if cond else y` | One-line condition |
| Nested if | `if: if:` | Complex logic |
| `in` operator | `x in (1,2,3)` | Multiple value check |
| `match-case` | `match x: case:` | Pattern matching (3.10+) |

---

## ⏭️ Next Topic

Now that you can make decisions, let's learn to repeat actions:

**[Loops (for, while)](./02_Loops.md)** — How to execute code repeatedly.

---

*Good decisions make good programs. Master conditional logic!* 🎯
