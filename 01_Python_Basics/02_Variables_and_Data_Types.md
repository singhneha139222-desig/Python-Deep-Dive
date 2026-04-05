# 📘 Variables and Data Types

## 📌 Introduction

In programming, we need to store data to work with it. A **variable** is like a container or a box that holds data in the computer's memory. You give the box a name, and you can put different types of data inside it.

Think of it like this:
- **Variable name** = Label on a box
- **Variable value** = What's inside the box
- **Data type** = Type of content (text, number, etc.)

```
┌─────────────────────────────────────────┐
│  Variable: age                          │
│  ┌─────────────────────────────────┐    │
│  │           25                    │    │
│  └─────────────────────────────────┘    │
│  Type: int (integer)                    │
└─────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Creating Variables

In Python, you create a variable using the **assignment operator** (`=`):

```python
variable_name = value
```

Python is **dynamically typed**, meaning you don't need to declare the data type — Python figures it out automatically!

```python
# Creating variables - Python automatically detects the type
name = "Alice"      # String
age = 25            # Integer
height = 5.6        # Float
is_student = True   # Boolean
```

### 2. Variable Naming Rules

| Rule | Example | Valid? |
|------|---------|--------|
| Must start with letter or underscore | `name`, `_count` | ✅ |
| Cannot start with number | `2name` | ❌ |
| Can contain letters, numbers, underscores | `user_name_2` | ✅ |
| Cannot contain spaces | `user name` | ❌ |
| Cannot use reserved keywords | `class`, `if`, `for` | ❌ |
| Case-sensitive | `Name` ≠ `name` | ✅ |

### 3. Python Reserved Keywords

```python
# These words cannot be used as variable names
import keyword
print(keyword.kwlist)
```

```
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 
 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 
 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 
 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 
 'return', 'try', 'while', 'with', 'yield']
```

### 4. Naming Conventions (Best Practices)

| Convention | Example | When to Use |
|------------|---------|-------------|
| **snake_case** | `user_name`, `total_count` | Variables, functions |
| **UPPERCASE** | `MAX_SIZE`, `PI` | Constants |
| **PascalCase** | `UserProfile`, `BankAccount` | Classes |
| **_leading_underscore** | `_private_var` | Internal use |

### 5. Python Data Types

```
┌────────────────────────────────────────────────────────────────┐
│                     PYTHON DATA TYPES                          │
├────────────────────┬───────────────────────────────────────────┤
│   CATEGORY         │   TYPES                                   │
├────────────────────┼───────────────────────────────────────────┤
│   Numeric          │   int, float, complex                     │
│   Text             │   str                                     │
│   Boolean          │   bool                                    │
│   Sequence         │   list, tuple, range                      │
│   Mapping          │   dict                                    │
│   Set              │   set, frozenset                          │
│   Binary           │   bytes, bytearray, memoryview            │
│   None             │   NoneType                                │
└────────────────────┴───────────────────────────────────────────┘
```

---

## 📊 Data Types Reference Table

| Type | Example | Description | Mutable? |
|------|---------|-------------|----------|
| `int` | `42`, `-10`, `0` | Whole numbers | No |
| `float` | `3.14`, `-2.5` | Decimal numbers | No |
| `complex` | `2+3j` | Complex numbers | No |
| `str` | `"Hello"`, `'Hi'` | Text/String | No |
| `bool` | `True`, `False` | Boolean values | No |
| `list` | `[1, 2, 3]` | Ordered, changeable | Yes |
| `tuple` | `(1, 2, 3)` | Ordered, unchangeable | No |
| `dict` | `{"a": 1}` | Key-value pairs | Yes |
| `set` | `{1, 2, 3}` | Unique values | Yes |
| `NoneType` | `None` | Absence of value | No |

---

## 💻 Code Examples

### 🟢 Basic: Creating Variables

```python
# String - text data (use quotes)
name = "John"
city = 'New York'  # Single or double quotes work

# Integer - whole numbers
age = 25
year = 2024
negative_num = -100

# Float - decimal numbers
price = 99.99
temperature = -5.5
pi = 3.14159

# Boolean - True or False
is_active = True
is_logged_in = False

# Print all variables
print("Name:", name)
print("Age:", age)
print("Price:", price)
print("Active:", is_active)
```

**Output:**
```
Name: John
Age: 25
Price: 99.99
Active: True
```

### 🟢 Basic: Checking Data Types

```python
# Use type() function to check data type
name = "Alice"
age = 30
salary = 50000.50
is_employed = True

print(type(name))        # <class 'str'>
print(type(age))         # <class 'int'>
print(type(salary))      # <class 'float'>
print(type(is_employed)) # <class 'bool'>
```

**Output:**
```
<class 'str'>
<class 'int'>
<class 'float'>
<class 'bool'>
```

### 🟡 Intermediate: Multiple Assignment

```python
# Assign same value to multiple variables
x = y = z = 100
print(x, y, z)  # 100 100 100

# Assign multiple values in one line
a, b, c = 1, 2, 3
print(a, b, c)  # 1 2 3

# Swap values (Python magic!)
a, b = b, a
print(a, b)  # 2 1

# Unpack a list
first, second, third = [10, 20, 30]
print(first, second, third)  # 10 20 30
```

**Output:**
```
100 100 100
1 2 3
2 1
10 20 30
```

### 🟡 Intermediate: Dynamic Typing

```python
# Python allows changing variable types
x = 10           # x is int
print(f"x = {x}, type = {type(x)}")

x = "Hello"      # x is now str
print(f"x = {x}, type = {type(x)}")

x = 3.14         # x is now float
print(f"x = {x}, type = {type(x)}")

x = [1, 2, 3]    # x is now list
print(f"x = {x}, type = {type(x)}")
```

**Output:**
```
x = 10, type = <class 'int'>
x = Hello, type = <class 'str'>
x = 3.14, type = <class 'float'>
x = [1, 2, 3], type = <class 'list'>
```

### 🟡 Intermediate: Working with Strings

```python
# Different ways to create strings
single = 'Hello'
double = "World"
triple = '''This is a
multi-line string'''
raw = r"Path: C:\Users\name"  # Raw string (ignores escape)

print(single)
print(double)
print(triple)
print(raw)

# String operations
name = "Python"
print(len(name))        # Length: 6
print(name.upper())     # PYTHON
print(name.lower())     # python
print(name[0])          # First character: P
print(name[-1])         # Last character: n
print(name[0:3])        # Slicing: Pyt
```

**Output:**
```
Hello
World
This is a
multi-line string
Path: C:\Users\name
6
PYTHON
python
P
n
Pyt
```

### 🟡 Intermediate: Number Types Deep Dive

```python
# Integer - no size limit in Python!
big_number = 99999999999999999999999999999
print(f"Big number: {big_number}")
print(f"Type: {type(big_number)}")

# Float - decimal numbers (limited precision)
pi = 3.14159265358979323846
print(f"Pi: {pi}")  # Note: some precision lost

# Scientific notation
light_speed = 3e8      # 3 × 10^8
tiny = 1.5e-10         # 1.5 × 10^-10
print(f"Light speed: {light_speed}")
print(f"Tiny: {tiny}")

# Complex numbers
c = 2 + 3j
print(f"Complex: {c}")
print(f"Real part: {c.real}")
print(f"Imaginary part: {c.imag}")
```

**Output:**
```
Big number: 99999999999999999999999999999
Type: <class 'int'>
Pi: 3.141592653589793
Light speed: 300000000.0
Tiny: 1.5e-10
Complex: (2+3j)
Real part: 2.0
Imaginary part: 3.0
```

### 🔴 Advanced: Memory and Identity

```python
# id() function shows memory address
a = 10
b = 10
c = a

print(f"a = {a}, id = {id(a)}")
print(f"b = {b}, id = {id(b)}")
print(f"c = {c}, id = {id(c)}")

# Python caches small integers (-5 to 256)
# So a, b, c point to the SAME memory location!
print(f"a is b: {a is b}")  # True
print(f"a is c: {a is c}")  # True

# For larger numbers, different behavior
x = 1000
y = 1000
print(f"\nx = {x}, id = {id(x)}")
print(f"y = {y}, id = {id(y)}")
print(f"x is y: {x is y}")  # May be False (implementation dependent)
print(f"x == y: {x == y}")  # True (values are equal)
```

**Output:**
```
a = 10, id = 140734567890456
b = 10, id = 140734567890456
c = 10, id = 140734567890456
a is b: True
a is c: True

x = 1000, id = 140234567890123
y = 1000, id = 140234567890456
x is y: False
x == y: True
```

### 🔴 Advanced: None Type

```python
# None represents "no value" or "nothing"
result = None

print(result)           # None
print(type(result))     # <class 'NoneType'>

# Common use: function that doesn't return anything
def greet(name):
    print(f"Hello, {name}!")
    # No return statement

return_value = greet("Alice")
print(f"Return value: {return_value}")  # None

# Checking for None
if result is None:
    print("Result is None")
    
# None is NOT the same as 0, "", or False
print(None == 0)      # False
print(None == "")     # False
print(None == False)  # False
```

**Output:**
```
None
<class 'NoneType'>
Hello, Alice!
Return value: None
Result is None
False
False
False
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Using Reserved Keywords

```python
# Wrong - 'class' is a reserved keyword
class = "Mathematics"  # SyntaxError!

# Correct - use a different name
class_name = "Mathematics"
```

### ❌ Mistake 2: Starting with Number

```python
# Wrong
2nd_place = "Silver"  # SyntaxError!

# Correct
second_place = "Silver"
place_2 = "Silver"
```

### ❌ Mistake 3: Confusing `=` and `==`

```python
# = is for assignment
x = 10  # Assigns 10 to x

# == is for comparison
x == 10  # Checks if x equals 10 (returns True/False)
```

### ❌ Mistake 4: Case Sensitivity

```python
Name = "Alice"
name = "Bob"

print(Name)  # Alice
print(name)  # Bob
# These are TWO DIFFERENT variables!
```

### ❌ Mistake 5: Using Variable Before Assignment

```python
print(x)  # NameError: name 'x' is not defined
x = 10
```

---

## 💡 Pro Tips

### 1. Use Meaningful Variable Names
```python
# Bad
x = 1000
y = 0.1

# Good
principal_amount = 1000
interest_rate = 0.1
```

### 2. Use Underscores for Readability
```python
# Hard to read
totalsalesrevenue = 150000

# Easy to read
total_sales_revenue = 150000

# For large numbers
population = 1_000_000_000  # Same as 1000000000
```

### 3. Constants Should Be UPPERCASE
```python
PI = 3.14159
MAX_CONNECTIONS = 100
DATABASE_URL = "localhost:5432"
```

### 4. Check Type Before Operations
```python
value = input("Enter a number: ")  # Returns string!
# print(value + 10)  # Error!
print(int(value) + 10)  # Correct
```

---

## 🔍 Real-world Use Cases

### 1. User Profile System
```python
# Storing user information
user_id = 12345
username = "john_doe"
email = "john@example.com"
age = 28
is_premium = True
account_balance = 150.75
```

### 2. E-commerce Product
```python
# Product information
product_id = "SKU-001"
product_name = "Wireless Mouse"
price = 29.99
stock_quantity = 150
is_available = True
discount_percentage = 10.5
```

### 3. Temperature Converter
```python
# Converting Celsius to Fahrenheit
celsius = 37.5
fahrenheit = (celsius * 9/5) + 32
print(f"{celsius}°C = {fahrenheit}°F")  # 37.5°C = 99.5°F
```

---

## 💼 Interview Insight

### Q1: What is dynamic typing in Python?
> Python determines the variable type at runtime, not at compile time. You don't need to declare types explicitly.

### Q2: What is the difference between `is` and `==`?
> - `==` compares **values** (are they equal?)
> - `is` compares **identity** (same object in memory?)

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (same values)
print(a is b)  # False (different objects)
print(a is c)  # True (same object)
```

### Q3: What is the maximum value of an integer in Python?
> Python 3 integers have **unlimited precision**. There's no maximum value — limited only by available memory.

### Q4: How do you declare a constant in Python?
> Python doesn't have true constants. By convention, use UPPERCASE names:
```python
PI = 3.14159
MAX_SIZE = 100
```

### Q5: What is the difference between mutable and immutable types?
> - **Immutable:** Cannot be changed after creation (int, float, str, tuple)
> - **Mutable:** Can be modified (list, dict, set)

---

## 🔥 Most Asked Questions

**Q: Can variable names have spaces?**
> No. Use underscores instead: `user_name` not `user name`

**Q: What's the difference between `'string'` and `"string"`?**
> No difference. Both create strings. Use one consistently.

**Q: Can I change a variable's type?**
> Yes! Python allows reassignment to any type.

**Q: What is `None`?**
> `None` represents the absence of a value. It's Python's null.

---

## ⚡ Shortcut Tricks

### Quick Variable Swap
```python
a, b = b, a  # Swap without temporary variable
```

### Multiple Assignment
```python
x = y = z = 0  # All equal to 0
```

### Underscore for Large Numbers
```python
million = 1_000_000  # More readable
```

### Check Multiple Types
```python
isinstance(x, (int, float))  # True if x is int OR float
```

---

## 🧪 Practice Questions

### Easy:
1. Create variables to store your name, age, and city
2. Print the type of each variable
3. What is the output of `type(3.14)`?

### Medium:
4. Create two variables `a = 5` and `b = 10`, then swap their values
5. What is the difference between `int` and `float`?
6. How do you create a multi-line string?

### Hard:
7. Explain why `a = 256; b = 256; print(a is b)` gives True but `a = 257; b = 257; print(a is b)` might give False
8. What is integer caching in Python?
9. Write code to demonstrate the difference between `==` and `is`

---

## 🚀 Mini Tasks / Assignments

### Task 1: Personal Information Card
```python
# Create variables for:
# - Full name (string)
# - Age (integer)
# - Height in meters (float)
# - Is student? (boolean)
# Print all with their types
```

### Task 2: Circle Calculator
```python
# Given radius = 5
# Calculate and store:
# - Area (pi * r^2)
# - Circumference (2 * pi * r)
# Use PI = 3.14159 as a constant
```

### Task 3: Type Detective
```python
# For each value, predict the type before running:
mystery1 = 42
mystery2 = 42.0
mystery3 = "42"
mystery4 = True
mystery5 = None
mystery6 = 2 + 3j

# Then verify using type()
```

### Task 4: Memory Explorer
```python
# Create two variables with the same value
# Check if they point to the same memory location
# Try with: 10, 100, 1000, "hello", [1,2,3]
```

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Variable | A named container for storing data |
| Assignment | Use `=` to assign values |
| Naming | snake_case, no spaces, no keywords |
| Data Types | int, float, str, bool, None, etc. |
| type() | Check a variable's data type |
| id() | Check memory address |
| Dynamic Typing | Types determined at runtime |
| `is` vs `==` | Identity vs equality comparison |

---

## ⏭️ Next Topic

Now that you understand variables and data types, let's learn:

**[Input and Output](./03_Input_Output.md)** — How to interact with users by taking input and displaying output.

---

*Remember: Good variable names make code readable. Your future self will thank you!* 🎯
