# 📘 Input and Output

## 📌 Introduction

Every useful program needs to **communicate with users**. This communication happens through:

1. **Output** — The program displays information to the user (using `print()`)
2. **Input** — The user provides information to the program (using `input()`)

Think of it like a conversation:
- **Output:** The computer speaks to you
- **Input:** You speak to the computer

```
┌─────────────────────────────────────────────────────────────┐
│                     PROGRAM FLOW                             │
│                                                              │
│   ┌──────────┐    Input     ┌──────────┐    Output    ┌──────────┐
│   │   User   │ ──────────> │  Program │ ──────────> │  Screen  │
│   └──────────┘   input()    └──────────┘   print()    └──────────┘
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. The `print()` Function

The `print()` function displays output to the console.

**Syntax:**
```python
print(value1, value2, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
```

| Parameter | Default | Description |
|-----------|---------|-------------|
| `value` | - | What to print (can be multiple) |
| `sep` | `' '` (space) | Separator between values |
| `end` | `'\n'` (newline) | What to print at the end |
| `file` | `sys.stdout` | Where to output (usually screen) |
| `flush` | `False` | Force flush the stream |

### 2. The `input()` Function

The `input()` function takes user input from the keyboard.

**Syntax:**
```python
variable = input(prompt)
```

> ⚠️ **Important:** `input()` ALWAYS returns a **string**, even if the user types a number!

### 3. Escape Sequences

Special characters that start with backslash (`\`):

| Escape | Description | Example Output |
|--------|-------------|----------------|
| `\n` | New line | Line 1<br>Line 2 |
| `\t` | Tab | Hello   World |
| `\\` | Backslash | C:\Users |
| `\'` | Single quote | It's Python |
| `\"` | Double quote | He said "Hi" |
| `\r` | Carriage return | (returns to start) |
| `\b` | Backspace | Removes character |

### 4. String Formatting Methods

Python provides multiple ways to format strings:

| Method | Syntax | Python Version |
|--------|--------|----------------|
| f-strings | `f"Hello {name}"` | 3.6+ (Recommended) |
| `.format()` | `"Hello {}".format(name)` | 2.6+ |
| `%` operator | `"Hello %s" % name` | Legacy |

---

## 💻 Code Examples

### 🟢 Basic: Simple Print

```python
# Print a string
print("Hello, World!")

# Print numbers
print(42)
print(3.14)

# Print boolean
print(True)

# Print multiple values
print("Age:", 25)
```

**Output:**
```
Hello, World!
42
3.14
True
Age: 25
```

### 🟢 Basic: Print with Variables

```python
name = "Alice"
age = 25
city = "New York"

# Method 1: Comma separated (adds spaces automatically)
print("Name:", name)
print("Age:", age)
print("City:", city)

# Method 2: String concatenation
print("Hello, " + name + "!")

# Method 3: f-string (recommended)
print(f"Hello, {name}! You are {age} years old.")
```

**Output:**
```
Name: Alice
Age: 25
City: New York
Hello, Alice!
Hello, Alice! You are 25 years old.
```

### 🟢 Basic: Simple Input

```python
# Get user's name
name = input("Enter your name: ")
print("Hello,", name)

# Get user's age
age = input("Enter your age: ")
print("You are", age, "years old")

# Note: age is a STRING, not integer!
print("Type of age:", type(age))
```

**Output:**
```
Enter your name: John
Hello, John
Enter your age: 25
You are 25 years old
Type of age: <class 'str'>
```

### 🟡 Intermediate: Print Parameters

```python
# Using 'sep' parameter (separator)
print("apple", "banana", "cherry", sep=", ")
print("2024", "04", "02", sep="-")
print("192", "168", "1", "1", sep=".")

# Using 'end' parameter
print("Loading", end="")
print(".", end="")
print(".", end="")
print(".", end="")
print(" Done!")

# Combining sep and end
print("A", "B", "C", sep="-", end="!\n")
```

**Output:**
```
apple, banana, cherry
2024-04-02
192.168.1.1
Loading.... Done!
A-B-C!
```

### 🟡 Intermediate: Escape Sequences

```python
# New line
print("Line 1\nLine 2\nLine 3")

# Tab
print("Name\tAge\tCity")
print("Alice\t25\tNYC")
print("Bob\t30\tLA")

# Backslash
print("Path: C:\\Users\\Documents")

# Quotes inside strings
print("He said \"Hello!\"")
print('It\'s a beautiful day')

# Raw string (ignores escape sequences)
print(r"C:\new\folder")  # Prints literally
```

**Output:**
```
Line 1
Line 2
Line 3
Name    Age     City
Alice   25      NYC
Bob     30      LA
Path: C:\Users\Documents
He said "Hello!"
It's a beautiful day
C:\new\folder
```

### 🟡 Intermediate: String Formatting

```python
name = "Alice"
age = 25
salary = 50000.756

# Method 1: f-strings (Python 3.6+) - RECOMMENDED
print(f"Name: {name}, Age: {age}")
print(f"Salary: ${salary:.2f}")  # 2 decimal places

# Method 2: .format() method
print("Name: {}, Age: {}".format(name, age))
print("Name: {n}, Age: {a}".format(n=name, a=age))
print("Name: {0}, Age: {1}".format(name, age))

# Method 3: % operator (old style)
print("Name: %s, Age: %d" % (name, age))
print("Salary: %.2f" % salary)
```

**Output:**
```
Name: Alice, Age: 25
Salary: $50000.76
Name: Alice, Age: 25
Name: Alice, Age: 25
Name: Alice, Age: 25
Name: Alice, Age: 25
Salary: 50000.76
```

### 🟡 Intermediate: f-string Formatting Options

```python
# Number formatting
number = 42
pi = 3.14159265
big_num = 1234567890

print(f"Padded: {number:05d}")       # 00042 (5 digits, zero-padded)
print(f"Pi: {pi:.2f}")               # 3.14 (2 decimal places)
print(f"Pi: {pi:.4f}")               # 3.1416 (4 decimal places)
print(f"With commas: {big_num:,}")   # 1,234,567,890
print(f"Scientific: {big_num:.2e}")  # 1.23e+09

# Alignment
text = "Hello"
print(f"|{text:>10}|")  # Right align
print(f"|{text:<10}|")  # Left align
print(f"|{text:^10}|")  # Center align
print(f"|{text:*^10}|") # Center with fill character

# Percentage
ratio = 0.756
print(f"Progress: {ratio:.1%}")  # 75.6%
```

**Output:**
```
Padded: 00042
Pi: 3.14
Pi: 3.1416
With commas: 1,234,567,890
Scientific: 1.23e+09
|     Hello|
|Hello     |
|  Hello   |
|**Hello***|
Progress: 75.6%
```

### 🟡 Intermediate: Input with Type Conversion

```python
# Get integer input
age = int(input("Enter your age: "))
print(f"Next year you'll be {age + 1}")

# Get float input
height = float(input("Enter your height in meters: "))
print(f"Height in cm: {height * 100}")

# Get multiple inputs in one line
x, y = input("Enter two numbers (space separated): ").split()
x, y = int(x), int(y)
print(f"Sum: {x + y}")

# Alternative: using map()
a, b = map(int, input("Enter two numbers: ").split())
print(f"Product: {a * b}")
```

**Output:**
```
Enter your age: 25
Next year you'll be 26
Enter your height in meters: 1.75
Height in cm: 175.0
Enter two numbers (space separated): 10 20
Sum: 30
Enter two numbers: 5 6
Product: 30
```

### 🔴 Advanced: Multi-line Strings

```python
# Triple quotes for multi-line
message = """Dear User,

Welcome to Python Programming!

Best regards,
Python Team"""

print(message)

# Formatted multi-line
name = "Alice"
course = "Python"
email = f"""
╔══════════════════════════════════════╗
║     Welcome, {name:^20}   ║
║     Course: {course:^20}  ║
╚══════════════════════════════════════╝
"""
print(email)
```

**Output:**
```
Dear User,

Welcome to Python Programming!

Best regards,
Python Team

╔══════════════════════════════════════╗
║     Welcome,         Alice           ║
║     Course:         Python           ║
╚══════════════════════════════════════╝
```

### 🔴 Advanced: Print to File

```python
# Print to a file instead of console
with open("output.txt", "w") as file:
    print("This goes to file", file=file)
    print("So does this", file=file)

# Verify by reading
with open("output.txt", "r") as file:
    print("File contents:")
    print(file.read())
```

**Output:**
```
File contents:
This goes to file
So does this
```

### 🔴 Advanced: Flush Parameter

```python
import time

# Without flush - output may be buffered
print("Downloading", end="")
for i in range(5):
    time.sleep(0.5)
    print(".", end="", flush=True)  # flush=True forces immediate output
print(" Complete!")
```

**Output (appears progressively):**
```
Downloading..... Complete!
```

### 🔴 Advanced: repr() vs str()

```python
text = "Hello\nWorld"

# str() - human-readable
print(str(text))
# Output:
# Hello
# World

# repr() - developer-readable (shows escape characters)
print(repr(text))
# Output: 'Hello\nWorld'

# In f-strings
print(f"str: {text}")
print(f"repr: {text!r}")
```

**Output:**
```
Hello
World
'Hello\nWorld'
str: Hello
World
repr: 'Hello\nWorld'
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting input() Returns String

```python
# Wrong
age = input("Enter age: ")
new_age = age + 1  # TypeError! Can't add str and int

# Correct
age = int(input("Enter age: "))
new_age = age + 1  # Works!
```

### ❌ Mistake 2: String Concatenation with Non-strings

```python
# Wrong
name = "Age: " + 25  # TypeError!

# Correct options
name = "Age: " + str(25)
name = f"Age: {25}"
print("Age:", 25)  # print handles different types
```

### ❌ Mistake 3: Forgetting Commas in print()

```python
# Wrong - SyntaxError
print("Hello" "World")  # Actually concatenates! Prints: HelloWorld

# Correct
print("Hello", "World")  # Prints: Hello World
```

### ❌ Mistake 4: Wrong Escape Sequence

```python
# Wrong - \n creates newline
path = "C:\new\folder"  # '\n' is newline, '\f' is form feed!

# Correct
path = "C:\\new\\folder"   # Escaped backslashes
path = r"C:\new\folder"    # Raw string
```

### ❌ Mistake 5: Invalid Type Conversion

```python
# Wrong
num = int(input("Enter number: "))  
# If user enters "hello" → ValueError!

# Better - with error handling
try:
    num = int(input("Enter number: "))
except ValueError:
    print("Invalid number!")
```

---

## 💡 Pro Tips

### 1. Use f-strings for Formatting
```python
# Old way (avoid)
print("Hello %s, you have %d messages" % (name, count))

# New way (preferred)
print(f"Hello {name}, you have {count} messages")
```

### 2. Debug with f-strings
```python
x = 10
y = 20
# Quick debugging
print(f"{x=}, {y=}")  # Output: x=10, y=20 (Python 3.8+)
```

### 3. Read Multiple Lines
```python
# Read until empty line
lines = []
while True:
    line = input()
    if line == "":
        break
    lines.append(line)
```

### 4. Use `*` to Unpack
```python
numbers = [1, 2, 3, 4, 5]
print(*numbers)  # 1 2 3 4 5
print(*numbers, sep=", ")  # 1, 2, 3, 4, 5
```

### 5. Pretty Print Complex Data
```python
import pprint
data = {"name": "Alice", "scores": [95, 87, 92], "info": {"age": 25}}
pprint.pprint(data)
```

---

## 🔍 Real-world Use Cases

### 1. Simple Calculator
```python
print("=== Simple Calculator ===")
num1 = float(input("Enter first number: "))
num2 = float(input("Enter second number: "))
operation = input("Enter operation (+, -, *, /): ")

if operation == "+":
    result = num1 + num2
elif operation == "-":
    result = num1 - num2
elif operation == "*":
    result = num1 * num2
elif operation == "/":
    result = num1 / num2 if num2 != 0 else "Error: Division by zero"

print(f"Result: {result}")
```

### 2. User Registration Form
```python
print("╔═══════════════════════════════════╗")
print("║       USER REGISTRATION           ║")
print("╚═══════════════════════════════════╝")

username = input("Username: ")
email = input("Email: ")
age = int(input("Age: "))

print("\n--- Registration Summary ---")
print(f"Username: {username}")
print(f"Email: {email}")
print(f"Age: {age}")
print("Registration successful!")
```

### 3. Progress Bar
```python
import time

total = 20
for i in range(total + 1):
    percent = (i / total) * 100
    bar = "█" * i + "░" * (total - i)
    print(f"\rProgress: |{bar}| {percent:.0f}%", end="", flush=True)
    time.sleep(0.1)
print("\nComplete!")
```

---

## 💼 Interview Insight

### Q1: What's the difference between print() and return?
> - `print()` displays output to console (for users)
> - `return` sends a value back from a function (for the program)
> - `print()` doesn't affect program flow; `return` does

### Q2: How do you take multiple inputs in one line?
```python
# Method 1: split()
a, b = input("Enter two values: ").split()

# Method 2: map() for type conversion
x, y = map(int, input("Enter two numbers: ").split())
```

### Q3: What is the difference between `input()` in Python 2 vs Python 3?
> - Python 2: `input()` evaluates the input as Python code (dangerous!)
> - Python 2: `raw_input()` returns string (like Python 3's input())
> - Python 3: `input()` always returns string (safer)

### Q4: How do you format numbers in print?
```python
# Decimal places
print(f"{3.14159:.2f}")  # 3.14

# Comma separator
print(f"{1000000:,}")  # 1,000,000

# Percentage
print(f"{0.756:.1%}")  # 75.6%
```

### Q5: Explain `end` and `sep` parameters in print().
> - `sep`: Separator between multiple values (default: space)
> - `end`: What to print at the end (default: newline)

---

## 🔥 Most Asked Questions

**Q: Why does input() always return string?**
> Safety! If it auto-converted, "10" could become 10 or "10", causing confusion. Explicit conversion is clearer.

**Q: How to clear the screen in Python?**
```python
import os
os.system('cls' if os.name == 'nt' else 'clear')
```

**Q: How to get password input (hidden)?**
```python
import getpass
password = getpass.getpass("Enter password: ")
```

**Q: How to read from file?**
```python
with open("file.txt", "r") as f:
    content = f.read()
```

---

## ⚡ Shortcut Tricks

### Quick Multiple Print
```python
print("Hello\n" * 3)
# Hello
# Hello
# Hello
```

### One-liner Input List
```python
nums = list(map(int, input().split()))
```

### Print Without Newline
```python
print("Same", end=" ")
print("line")  # Output: Same line
```

### Debug Variables Quickly
```python
x, y = 10, 20
print(f"{x=}, {y=}")  # x=10, y=20
```

### Unpack and Print
```python
print(*range(5))  # 0 1 2 3 4
```

---

## 🧪 Practice Questions

### Easy:
1. Print your name and age on separate lines
2. Use `\t` to create a table of 3 columns
3. Take user's name as input and say "Hello, [name]!"

### Medium:
4. Take two numbers and print their sum, difference, product, and quotient
5. Format a price with 2 decimal places and a dollar sign
6. Create a simple greeting that uses the current date (hint: `datetime` module)

### Hard:
7. Create a progress bar that updates on a single line
8. Read a list of numbers from user in one line and calculate average
9. Create a receipt formatter that aligns item names left and prices right

---

## 🚀 Mini Tasks / Assignments

### Task 1: Personal ID Card
```python
# Create an ID card that looks like:
# ╔════════════════════════════╗
# ║  Name: [NAME]              ║
# ║  Age: [AGE]                ║
# ║  ID: [ID NUMBER]           ║
# ╚════════════════════════════╝
```

### Task 2: Temperature Converter
```python
# Take temperature in Celsius from user
# Convert to Fahrenheit: F = (C × 9/5) + 32
# Display result with 2 decimal places
```

### Task 3: Invoice Generator
```python
# Take item name, quantity, and unit price as input
# Calculate total
# Print a formatted invoice with proper alignment
```

### Task 4: Mad Libs Game
```python
# Ask user for: noun, verb, adjective, place
# Create a funny story using these inputs
# Example: "The [adjective] [noun] decided to [verb] at the [place]."
```

---

## 📖 Summary

| Concept | Syntax | Description |
|---------|--------|-------------|
| Print | `print(value)` | Display output |
| Input | `input(prompt)` | Get user input (string) |
| sep | `print(a, b, sep=",")` | Custom separator |
| end | `print(a, end="")` | Custom ending |
| f-string | `f"Hello {name}"` | Modern formatting |
| Escape | `\n`, `\t`, `\\` | Special characters |
| Type conversion | `int(input())` | Convert input |

---

## ⏭️ Next Topic

Now you can interact with users! Next, let's learn:

**[Type Casting](./04_Type_Casting.md)** — Converting between different data types.

---

*Practice makes perfect! Try building small programs that interact with users.* 💬
