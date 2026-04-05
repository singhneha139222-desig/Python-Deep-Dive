# 📘 Operators in Python

## 📌 Introduction

**Operators** are special symbols that perform operations on values (called **operands**). They are the building blocks of any calculation or logic in programming.

Think of operators like mathematical symbols you learned in school:
- `+` adds numbers
- `-` subtracts numbers
- `>` checks if something is greater

But Python has many MORE operators beyond basic math!

```
┌─────────────────────────────────────────────────────────────┐
│                    PYTHON OPERATORS                          │
│                                                              │
│   Operand    Operator    Operand    =    Result             │
│      5          +           3       =      8                │
│      ↓          ↓           ↓              ↓                │
│   (value)   (symbol)     (value)       (output)             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Types of Operators in Python

| Category | Purpose | Examples |
|----------|---------|----------|
| **Arithmetic** | Math operations | `+`, `-`, `*`, `/`, `//`, `%`, `**` |
| **Comparison** | Compare values | `==`, `!=`, `>`, `<`, `>=`, `<=` |
| **Logical** | Combine conditions | `and`, `or`, `not` |
| **Assignment** | Assign values | `=`, `+=`, `-=`, `*=`, etc. |
| **Bitwise** | Binary operations | `&`, `\|`, `^`, `~`, `<<`, `>>` |
| **Membership** | Check membership | `in`, `not in` |
| **Identity** | Check identity | `is`, `is not` |

---

## 📊 Operator Precedence (Priority)

Operations are performed in this order (highest to lowest):

| Priority | Operator | Description |
|----------|----------|-------------|
| 1 (Highest) | `()` | Parentheses |
| 2 | `**` | Exponentiation |
| 3 | `~`, `+`, `-` | Unary operators |
| 4 | `*`, `/`, `//`, `%` | Multiplication/Division |
| 5 | `+`, `-` | Addition/Subtraction |
| 6 | `<<`, `>>` | Bitwise shifts |
| 7 | `&` | Bitwise AND |
| 8 | `^` | Bitwise XOR |
| 9 | `\|` | Bitwise OR |
| 10 | `==`, `!=`, `<`, `>`, `<=`, `>=`, `is`, `in` | Comparisons |
| 11 | `not` | Logical NOT |
| 12 | `and` | Logical AND |
| 13 (Lowest) | `or` | Logical OR |

**Memory Trick:** **PEMDAS** + Comparison + Logical
- **P**arentheses
- **E**xponents
- **M**ultiplication/**D**ivision
- **A**ddition/**S**ubtraction

---

## 💻 Code Examples

---

## 1️⃣ Arithmetic Operators

### 🟢 Basic: Arithmetic Operations

```python
a = 15
b = 4

# Addition
print(f"{a} + {b} = {a + b}")      # 19

# Subtraction
print(f"{a} - {b} = {a - b}")      # 11

# Multiplication
print(f"{a} * {b} = {a * b}")      # 60

# Division (always returns float)
print(f"{a} / {b} = {a / b}")      # 3.75

# Floor Division (returns integer, rounds down)
print(f"{a} // {b} = {a // b}")    # 3

# Modulus (remainder)
print(f"{a} % {b} = {a % b}")      # 3

# Exponentiation (power)
print(f"{a} ** {b} = {a ** b}")    # 50625 (15^4)
```

**Output:**
```
15 + 4 = 19
15 - 4 = 11
15 * 4 = 60
15 / 4 = 3.75
15 // 4 = 3
15 % 4 = 3
15 ** 4 = 50625
```

### 🟡 Intermediate: Division Edge Cases

```python
# Regular division vs Floor division
print("=== Division Comparison ===")
print(f"7 / 2 = {7 / 2}")       # 3.5 (float)
print(f"7 // 2 = {7 // 2}")     # 3 (int, floor)

# Negative floor division (rounds toward negative infinity!)
print(f"-7 // 2 = {-7 // 2}")   # -4 (not -3!)
print(f"7 // -2 = {7 // -2}")   # -4

# Modulus with negatives
print(f"\n=== Modulus with Negatives ===")
print(f"7 % 3 = {7 % 3}")       # 1
print(f"-7 % 3 = {-7 % 3}")     # 2 (not -1!)
print(f"7 % -3 = {7 % -3}")     # -2

# Zero cases
print(f"\n=== Zero Cases ===")
print(f"0 / 5 = {0 / 5}")       # 0.0
print(f"0 // 5 = {0 // 5}")     # 0
# print(f"5 / 0 = {5 / 0}")     # ZeroDivisionError!
```

**Output:**
```
=== Division Comparison ===
7 / 2 = 3.5
7 // 2 = 3
-7 // 2 = -4
7 // -2 = -4

=== Modulus with Negatives ===
7 % 3 = 1
-7 % 3 = 2
7 % -3 = -2

=== Zero Cases ===
0 / 5 = 0.0
0 // 5 = 0
```

### 🔴 Advanced: divmod() and pow()

```python
# divmod() returns (quotient, remainder)
quotient, remainder = divmod(17, 5)
print(f"17 divided by 5: quotient={quotient}, remainder={remainder}")

# pow() function vs ** operator
print(f"2 ** 10 = {2 ** 10}")           # 1024
print(f"pow(2, 10) = {pow(2, 10)}")     # 1024

# pow() with modulo (efficient for large numbers)
# pow(base, exp, mod) = (base ** exp) % mod
print(f"pow(2, 10, 100) = {pow(2, 10, 100)}")  # 24
# This is much faster than (2 ** 10) % 100 for large numbers
```

**Output:**
```
17 divided by 5: quotient=3, remainder=2
2 ** 10 = 1024
pow(2, 10) = 1024
pow(2, 10, 100) = 24
```

---

## 2️⃣ Comparison Operators

### 🟢 Basic: Comparison Operations

```python
a = 10
b = 20

# Equal to
print(f"{a} == {b} : {a == b}")    # False

# Not equal to
print(f"{a} != {b} : {a != b}")    # True

# Greater than
print(f"{a} > {b} : {a > b}")      # False

# Less than
print(f"{a} < {b} : {a < b}")      # True

# Greater than or equal
print(f"{a} >= {b} : {a >= b}")    # False

# Less than or equal
print(f"{a} <= {b} : {a <= b}")    # True
```

**Output:**
```
10 == 20 : False
10 != 20 : True
10 > 20 : False
10 < 20 : True
10 >= 20 : False
10 <= 20 : True
```

### 🟡 Intermediate: Chained Comparisons

```python
# Python allows chaining comparisons!
x = 5

# Traditional way
print(x > 0 and x < 10)  # True

# Pythonic way (chained)
print(0 < x < 10)        # True (x is between 0 and 10)

# More examples
age = 25
print(18 <= age <= 65)   # True (working age)

score = 85
print(80 <= score < 90)  # True (Grade B)

# You can chain multiple comparisons
a, b, c = 1, 2, 3
print(a < b < c)         # True
print(a < b > c)         # False (b is not > c)
```

**Output:**
```
True
True
True
True
True
False
```

### 🔴 Advanced: Comparing Different Types

```python
# Comparing strings (lexicographic order)
print(f"'apple' < 'banana': {'apple' < 'banana'}")  # True
print(f"'Apple' < 'apple': {'Apple' < 'apple'}")    # True (uppercase < lowercase)

# Comparing lists (element by element)
print(f"[1, 2] < [1, 3]: {[1, 2] < [1, 3]}")        # True
print(f"[1, 2] < [1, 2, 3]: {[1, 2] < [1, 2, 3]}")  # True (shorter)

# Float comparison pitfall
print(f"0.1 + 0.2 == 0.3: {0.1 + 0.2 == 0.3}")      # False! (floating point)
print(f"Actual: 0.1 + 0.2 = {0.1 + 0.2}")

# Safe float comparison
import math
print(f"math.isclose: {math.isclose(0.1 + 0.2, 0.3)}")  # True
```

**Output:**
```
'apple' < 'banana': True
'Apple' < 'apple': True
[1, 2] < [1, 3]: True
[1, 2] < [1, 2, 3]: True
0.1 + 0.2 == 0.3: False
Actual: 0.1 + 0.2 = 0.30000000000000004
math.isclose: True
```

---

## 3️⃣ Logical Operators

### 🟢 Basic: and, or, not

```python
# AND - Both must be True
print(f"True and True: {True and True}")     # True
print(f"True and False: {True and False}")   # False
print(f"False and False: {False and False}") # False

# OR - At least one must be True
print(f"\nTrue or True: {True or True}")     # True
print(f"True or False: {True or False}")     # True
print(f"False or False: {False or False}")   # False

# NOT - Inverts the value
print(f"\nnot True: {not True}")             # False
print(f"not False: {not False}")             # True
```

**Output:**
```
True and True: True
True and False: False
False and False: False

True or True: True
True or False: True
False or False: False

not True: False
not False: True
```

### 🟡 Intermediate: Practical Conditions

```python
age = 25
has_license = True
has_car = False

# Complex conditions
can_drive = age >= 18 and has_license
print(f"Can drive: {can_drive}")  # True

can_travel = has_car or has_license
print(f"Can travel: {can_travel}")  # True

is_minor = not (age >= 18)
print(f"Is minor: {is_minor}")  # False

# Grade checker
score = 85
is_passing = score >= 60
is_honors = score >= 90
is_good = score >= 70 and score < 90
print(f"Good grade: {is_good}")  # True
```

**Output:**
```
Can drive: True
Can travel: True
Is minor: False
Good grade: True
```

### 🔴 Advanced: Short-circuit Evaluation

```python
# Python uses short-circuit evaluation
# and: stops at first False
# or: stops at first True

def check_a():
    print("Checking A")
    return False

def check_b():
    print("Checking B")
    return True

# AND: check_b is never called because check_a is False
print("Testing AND:")
result = check_a() and check_b()
print(f"Result: {result}\n")

# OR: check_b is never called because check_a returning True would skip
print("Testing OR:")
result = check_b() or check_a()
print(f"Result: {result}")

# Practical use: safe operations
user = None
# This won't crash even if user is None
name = user and user.name  # Returns None, doesn't access .name
print(f"\nSafe access: {name}")
```

**Output:**
```
Testing AND:
Checking A
Result: False

Testing OR:
Checking B
Result: True

Safe access: None
```

### 🔴 Advanced: Logical Operators Return Values (not just True/False!)

```python
# and/or return actual values, not just True/False!

# AND returns first falsy or last value
print(5 and 10)      # 10 (last value, both truthy)
print(0 and 10)      # 0 (first falsy)
print("" and "hi")   # "" (first falsy)

# OR returns first truthy or last value
print(5 or 10)       # 5 (first truthy)
print(0 or 10)       # 10 (first truthy)
print("" or "hi")    # "hi" (first truthy)

# Practical: default values
name = "" or "Guest"
print(f"Name: {name}")  # Guest

value = None
result = value or "default"
print(f"Result: {result}")  # default
```

**Output:**
```
10
0

5
10
hi
Name: Guest
Result: default
```

---

## 4️⃣ Assignment Operators

### 🟢 Basic: Simple Assignment

```python
# Simple assignment
x = 10
print(f"x = {x}")

# Multiple assignment
a, b, c = 1, 2, 3
print(f"a={a}, b={b}, c={c}")

# Same value to multiple variables
x = y = z = 0
print(f"x={x}, y={y}, z={z}")
```

**Output:**
```
x = 10
a=1, b=2, c=3
x=0, y=0, z=0
```

### 🟡 Intermediate: Compound Assignment

```python
x = 10

# Addition assignment
x += 5    # x = x + 5
print(f"After x += 5: {x}")      # 15

# Subtraction assignment
x -= 3    # x = x - 3
print(f"After x -= 3: {x}")      # 12

# Multiplication assignment
x *= 2    # x = x * 2
print(f"After x *= 2: {x}")      # 24

# Division assignment
x /= 4    # x = x / 4
print(f"After x /= 4: {x}")      # 6.0

# Floor division assignment
x //= 2   # x = x // 2
print(f"After x //= 2: {x}")     # 3.0

# Modulus assignment
x %= 2    # x = x % 2
print(f"After x %= 2: {x}")      # 1.0

# Power assignment
x = 2
x **= 3   # x = x ** 3
print(f"After x **= 3: {x}")     # 8
```

**Output:**
```
After x += 5: 15
After x -= 3: 12
After x *= 2: 24
After x /= 4: 6.0
After x //= 2: 3.0
After x %= 2: 1.0
After x **= 3: 8
```

### 🔴 Advanced: Walrus Operator `:=` (Python 3.8+)

```python
# Walrus operator assigns AND returns the value

# Without walrus operator
numbers = [1, 2, 3, 4, 5]
n = len(numbers)
if n > 3:
    print(f"List has {n} items")

# With walrus operator (more concise)
if (n := len(numbers)) > 3:
    print(f"List has {n} items")

# Useful in while loops
data = input("Enter data (empty to quit): ")
while data:
    print(f"Processing: {data}")
    data = input("Enter data (empty to quit): ")

# With walrus operator
while (data := input("Enter data (empty to quit): ")):
    print(f"Processing: {data}")

# In list comprehensions
results = [y for x in range(10) if (y := x ** 2) > 20]
print(results)  # [25, 36, 49, 64, 81]
```

---

## 5️⃣ Bitwise Operators

### 🟢 Basic: Understanding Binary

```python
# Binary representation
a = 5   # Binary: 0101
b = 3   # Binary: 0011

print(f"a = {a} = {bin(a)}")  # 0b101
print(f"b = {b} = {bin(b)}")  # 0b11
```

### 🟡 Intermediate: Bitwise Operations

```python
a = 5   # Binary: 0101
b = 3   # Binary: 0011

# AND: 1 if both bits are 1
print(f"{a} & {b} = {a & b}")    # 1 (0001)

# OR: 1 if any bit is 1
print(f"{a} | {b} = {a | b}")    # 7 (0111)

# XOR: 1 if bits are different
print(f"{a} ^ {b} = {a ^ b}")    # 6 (0110)

# NOT: inverts all bits
print(f"~{a} = {~a}")            # -6

# Left shift: multiply by 2^n
print(f"{a} << 1 = {a << 1}")    # 10 (1010)
print(f"{a} << 2 = {a << 2}")    # 20 (10100)

# Right shift: divide by 2^n
print(f"{a} >> 1 = {a >> 1}")    # 2 (010)
```

**Output:**
```
5 & 3 = 1
5 | 3 = 7
5 ^ 3 = 6
~5 = -6
5 << 1 = 10
5 << 2 = 20
5 >> 1 = 2
```

### 🔴 Advanced: Bitwise Tricks

```python
# Check if number is even or odd
n = 7
is_odd = n & 1  # 1 if odd, 0 if even
print(f"{n} is {'odd' if is_odd else 'even'}")

# Swap without temp variable
a, b = 5, 10
a = a ^ b
b = a ^ b
a = a ^ b
print(f"Swapped: a={a}, b={b}")  # a=10, b=5

# Multiply by 2 using left shift
x = 5
print(f"{x} * 2 = {x << 1}")     # 10

# Divide by 2 using right shift
x = 10
print(f"{x} / 2 = {x >> 1}")     # 5

# Set, clear, toggle bits
flags = 0b0000
flags |= 0b0001   # Set bit 0
print(f"After set: {bin(flags)}")     # 0b1
flags &= ~0b0001  # Clear bit 0
print(f"After clear: {bin(flags)}")   # 0b0
flags ^= 0b0010   # Toggle bit 1
print(f"After toggle: {bin(flags)}")  # 0b10
```

---

## 6️⃣ Membership Operators

### 🟢 Basic: in and not in

```python
# Check if element exists in sequence

# In a string
text = "Hello, World!"
print(f"'World' in text: {'World' in text}")    # True
print(f"'Python' in text: {'Python' in text}")  # False

# In a list
fruits = ["apple", "banana", "cherry"]
print(f"'banana' in fruits: {'banana' in fruits}")  # True
print(f"'mango' not in fruits: {'mango' not in fruits}")  # True

# In a tuple
numbers = (1, 2, 3, 4, 5)
print(f"3 in numbers: {3 in numbers}")  # True

# In a dictionary (checks keys!)
person = {"name": "Alice", "age": 25}
print(f"'name' in person: {'name' in person}")      # True
print(f"'Alice' in person: {'Alice' in person}")    # False (Alice is value, not key)
```

**Output:**
```
'World' in text: True
'Python' in text: False
'banana' in fruits: True
'mango' not in fruits: True
3 in numbers: True
'name' in person: True
'Alice' in person: False
```

### 🟡 Intermediate: Performance Considerations

```python
import time

# List vs Set for membership testing
large_list = list(range(1000000))
large_set = set(range(1000000))

# List membership (O(n))
start = time.time()
result = 999999 in large_list
print(f"List search: {time.time() - start:.6f}s")

# Set membership (O(1))
start = time.time()
result = 999999 in large_set
print(f"Set search: {time.time() - start:.6f}s")

# Takeaway: Use sets for frequent membership checks!
```

---

## 7️⃣ Identity Operators

### 🟢 Basic: is and is not

```python
# is checks if two variables point to SAME OBJECT (memory)
# == checks if values are EQUAL

a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(f"a == b: {a == b}")      # True (same values)
print(f"a is b: {a is b}")      # False (different objects)
print(f"a is c: {a is c}")      # True (same object)

# Check memory addresses
print(f"\nid(a): {id(a)}")
print(f"id(b): {id(b)}")
print(f"id(c): {id(c)}")
```

**Output:**
```
a == b: True
a is b: False
a is c: True

id(a): 140234567890123
id(b): 140234567890456
id(c): 140234567890123
```

### 🟡 Intermediate: Integer Caching

```python
# Python caches small integers (-5 to 256)
a = 100
b = 100
print(f"100: a is b = {a is b}")  # True (cached)

a = 1000
b = 1000
print(f"1000: a is b = {a is b}") # False (not cached)

# Always use == for value comparison!
# Use 'is' only for None, True, False

x = None
if x is None:  # Correct
    print("x is None")
    
# if x == None:  # Works, but 'is' is preferred for None
```

### 🔴 Advanced: None Checking

```python
# is None vs == None

def process(value):
    # Correct way
    if value is None:
        return "No value provided"
    return f"Processing: {value}"

# Why 'is None' is better:
# Some objects override __eq__ and might equal None unexpectedly
class Weird:
    def __eq__(self, other):
        return True  # Equals everything!

w = Weird()
print(w == None)   # True (dangerous!)
print(w is None)   # False (safe!)
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: = vs ==

```python
x = 10
# if x = 10:  # SyntaxError! (assignment, not comparison)
if x == 10:   # Correct (comparison)
    print("x is 10")
```

### ❌ Mistake 2: Division Always Returns Float

```python
result = 10 / 2
print(result)       # 5.0, not 5!
print(type(result)) # float

# Use // for integer division
result = 10 // 2
print(result)       # 5
print(type(result)) # int
```

### ❌ Mistake 3: Negative Floor Division

```python
# Floor division rounds toward NEGATIVE infinity
print(-7 // 2)  # -4, not -3!
```

### ❌ Mistake 4: Floating Point Comparison

```python
# Never use == for floats!
print(0.1 + 0.2 == 0.3)  # False!

# Use math.isclose instead
import math
print(math.isclose(0.1 + 0.2, 0.3))  # True
```

### ❌ Mistake 5: Confusing `is` with `==`

```python
# Use == for value comparison
# Use 'is' only for None, True, False, singletons

a = [1, 2, 3]
b = [1, 2, 3]
print(a is b)  # False! Use a == b instead
```

---

## 💡 Pro Tips

### 1. Use Chained Comparisons
```python
# Instead of
if x > 0 and x < 100:
# Use
if 0 < x < 100:
```

### 2. Use `or` for Default Values
```python
name = user_input or "Guest"
```

### 3. Use `//` for Integer Results
```python
pages = total_items // items_per_page
```

### 4. Use `%` to Check Divisibility
```python
is_even = number % 2 == 0
```

### 5. Use Bitwise for Performance
```python
# Fast multiplication/division by powers of 2
x * 2  ==  x << 1
x // 2  ==  x >> 1
```

---

## 🔍 Real-world Use Cases

### 1. Discount Calculator
```python
price = 100
discount_percent = 20
discount_amount = price * discount_percent / 100
final_price = price - discount_amount
print(f"Final price: ${final_price}")  # $80.0
```

### 2. Pagination
```python
total_items = 95
items_per_page = 10
total_pages = (total_items + items_per_page - 1) // items_per_page
print(f"Total pages: {total_pages}")  # 10
```

### 3. Leap Year Checker
```python
year = 2024
is_leap = (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)
print(f"{year} is leap year: {is_leap}")  # True
```

---

## 💼 Interview Insight

### Q1: What is operator precedence?
> The order in which operators are evaluated. `*` and `/` before `+` and `-`. Use parentheses to control order.

### Q2: What's the difference between `/` and `//`?
> `/` is true division (always returns float), `//` is floor division (returns int, rounds down).

### Q3: What's the difference between `is` and `==`?
> `==` compares values, `is` compares object identity (same memory location).

### Q4: What is short-circuit evaluation?
> `and` stops at first False, `or` stops at first True. Remaining expressions aren't evaluated.

### Q5: What does `%` return for negative numbers?
> Python's `%` always returns result with the same sign as the divisor. `-7 % 3 = 2` (not -1).

---

## 🔥 Most Asked Questions

**Q: Why does `0.1 + 0.2 != 0.3`?**
> Floating point precision limits. Use `math.isclose()` for float comparison.

**Q: Can I use `^` for power?**
> No! `^` is XOR in Python. Use `**` for power.

**Q: What's the walrus operator?**
> `:=` assigns a value AND returns it. `if (n := len(x)) > 5:`

---

## ⚡ Shortcut Tricks

```python
# Check even/odd
is_even = n % 2 == 0
is_odd = n & 1  # Faster!

# Swap values
a, b = b, a

# Toggle boolean
flag = not flag

# Ceiling division
ceil_div = (a + b - 1) // b

# Absolute value
abs_val = n if n >= 0 else -n
# or just use abs(n)
```

---

## 🧪 Practice Questions

### Easy:
1. Calculate `5 + 3 * 2` and explain the result
2. Check if 15 is divisible by both 3 and 5
3. What is `17 % 5`?

### Medium:
4. Calculate the number of pages needed for 87 items with 10 per page
5. Check if a year is a leap year
6. Explain why `-7 // 2 = -4` and not `-3`

### Hard:
7. Use bitwise operators to check if a number is a power of 2
8. Implement a function that swaps two numbers without a temp variable
9. Explain short-circuit evaluation with an example

---

## 🚀 Mini Tasks / Assignments

### Task 1: Calculator
```python
# Build a calculator that takes two numbers and an operator
# Handle: +, -, *, /, //, %, **
```

### Task 2: Age Validator
```python
# Check if age is:
# - Valid (0-120)
# - Adult (18+)
# - Senior (65+)
```

### Task 3: Grade Calculator
```python
# Calculate grade based on score:
# 90-100: A, 80-89: B, 70-79: C, 60-69: D, <60: F
```

---

## 📖 Summary

| Category | Operators |
|----------|-----------|
| Arithmetic | `+`, `-`, `*`, `/`, `//`, `%`, `**` |
| Comparison | `==`, `!=`, `>`, `<`, `>=`, `<=` |
| Logical | `and`, `or`, `not` |
| Assignment | `=`, `+=`, `-=`, `*=`, `/=`, etc. |
| Bitwise | `&`, `\|`, `^`, `~`, `<<`, `>>` |
| Membership | `in`, `not in` |
| Identity | `is`, `is not` |

---

## ⏭️ Next Module

Congratulations! You've completed **Module 1: Python Basics**! 🎉

Next, move to:

**[Module 2: Control Flow](../02_Control_Flow/README.md)** — Learn if-else statements, loops, and program flow control.

---

*Operators are the tools of programming. Master them!* 🛠️
