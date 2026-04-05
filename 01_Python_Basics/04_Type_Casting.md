# 📘 Type Casting

## 📌 Introduction

**Type casting** (or type conversion) is the process of converting one data type to another. In programming, you often need to change data types to perform specific operations.

Think of it like currency conversion:
- You have **dollars** but the shop only accepts **euros**
- You need to **convert** your dollars to euros
- Similarly, you might have a **string** "42" but need an **integer** 42

```
┌─────────────────────────────────────────────────────────────┐
│                    TYPE CASTING                              │
│                                                              │
│   "42" (string) ──────────> 42 (integer)                    │
│         str      int()      int                              │
│                                                              │
│   42 (integer) ──────────> "42" (string)                    │
│       int        str()      str                              │
│                                                              │
│   42 (integer) ──────────> 42.0 (float)                     │
│       int       float()    float                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Types of Type Casting

| Type | Description | How it Works |
|------|-------------|--------------|
| **Implicit** | Automatic conversion | Python does it automatically |
| **Explicit** | Manual conversion | You use conversion functions |

### 2. Implicit Type Casting (Type Coercion)

Python automatically converts smaller data types to larger ones to prevent data loss.

```
int → float → complex
```

```python
# Python automatically converts int to float
x = 5       # int
y = 2.0     # float
result = x + y  # Python converts 5 to 5.0, then adds
print(result)   # 7.0 (float)
```

### 3. Explicit Type Casting

You manually convert using built-in functions:

| Function | Converts To | Example |
|----------|-------------|---------|
| `int()` | Integer | `int("42")` → `42` |
| `float()` | Float | `float("3.14")` → `3.14` |
| `str()` | String | `str(42)` → `"42"` |
| `bool()` | Boolean | `bool(1)` → `True` |
| `list()` | List | `list("abc")` → `['a','b','c']` |
| `tuple()` | Tuple | `tuple([1,2])` → `(1, 2)` |
| `set()` | Set | `set([1,1,2])` → `{1, 2}` |
| `dict()` | Dictionary | `dict([('a',1)])` → `{'a': 1}` |
| `ord()` | ASCII value | `ord('A')` → `65` |
| `chr()` | Character | `chr(65)` → `'A'` |
| `hex()` | Hexadecimal | `hex(255)` → `'0xff'` |
| `oct()` | Octal | `oct(8)` → `'0o10'` |
| `bin()` | Binary | `bin(5)` → `'0b101'` |

---

## 📊 Type Casting Compatibility Chart

```
┌─────────────────────────────────────────────────────────────────────┐
│               CAN CONVERT FROM → TO?                                │
├──────────┬────────┬────────┬────────┬────────┬────────┬────────────┤
│ FROM/TO  │  int   │ float  │  str   │  bool  │  list  │    dict    │
├──────────┼────────┼────────┼────────┼────────┼────────┼────────────┤
│   int    │   -    │   ✅   │   ✅   │   ✅   │   ❌   │     ❌     │
│  float   │   ✅*  │   -    │   ✅   │   ✅   │   ❌   │     ❌     │
│   str    │   ✅** │  ✅**  │   -    │   ✅   │   ✅   │     ❌     │
│   bool   │   ✅   │   ✅   │   ✅   │   -    │   ❌   │     ❌     │
│   list   │   ❌   │   ❌   │   ✅   │   ✅   │   -    │     ❌     │
│  tuple   │   ❌   │   ❌   │   ✅   │   ✅   │   ✅   │     ❌     │
└──────────┴────────┴────────┴────────┴────────┴────────┴────────────┘

* Truncates decimal (3.9 → 3)
** Only if string contains valid number
```

---

## 💻 Code Examples

### 🟢 Basic: Integer Conversions

```python
# String to Integer
age_str = "25"
age_int = int(age_str)
print(f"String: '{age_str}' → Integer: {age_int}")
print(f"Now we can calculate: {age_int + 5}")

# Float to Integer (truncates, doesn't round!)
price = 99.99
price_int = int(price)  # Removes decimal part
print(f"Float: {price} → Integer: {price_int}")

# Boolean to Integer
print(f"True → {int(True)}")   # 1
print(f"False → {int(False)}") # 0
```

**Output:**
```
String: '25' → Integer: 25
Now we can calculate: 30
Float: 99.99 → Integer: 99
True → 1
False → 0
```

### 🟢 Basic: Float Conversions

```python
# String to Float
price_str = "99.99"
price = float(price_str)
print(f"String: '{price_str}' → Float: {price}")

# Integer to Float
count = 42
count_float = float(count)
print(f"Integer: {count} → Float: {count_float}")

# Boolean to Float
print(f"True → {float(True)}")   # 1.0
print(f"False → {float(False)}") # 0.0
```

**Output:**
```
String: '99.99' → Float: 99.99
Integer: 42 → Float: 42.0
True → 1.0
False → 0.0
```

### 🟢 Basic: String Conversions

```python
# Integer to String
age = 25
age_str = str(age)
print(f"Integer: {age} → String: '{age_str}'")
print(f"Type: {type(age_str)}")

# Float to String
pi = 3.14159
pi_str = str(pi)
print(f"Float: {pi} → String: '{pi_str}'")

# Boolean to String
print(f"True → '{str(True)}'")
print(f"False → '{str(False)}'")

# List to String
my_list = [1, 2, 3]
list_str = str(my_list)
print(f"List: {my_list} → String: '{list_str}'")
```

**Output:**
```
Integer: 25 → String: '25'
Type: <class 'str'>
Float: 3.14159 → String: '3.14159'
True → 'True'
False → 'False'
List: [1, 2, 3] → String: '[1, 2, 3]'
```

### 🟡 Intermediate: Boolean Conversions

```python
# What values are "Truthy" and "Falsy"?

# Falsy values (convert to False)
print("=== Falsy Values ===")
print(f"bool(0) = {bool(0)}")
print(f"bool(0.0) = {bool(0.0)}")
print(f"bool('') = {bool('')}")  # Empty string
print(f"bool([]) = {bool([])}")  # Empty list
print(f"bool({{}}) = {bool({})}")  # Empty dict
print(f"bool(None) = {bool(None)}")

# Truthy values (convert to True)
print("\n=== Truthy Values ===")
print(f"bool(1) = {bool(1)}")
print(f"bool(-1) = {bool(-1)}")  # Any non-zero number
print(f"bool(3.14) = {bool(3.14)}")
print(f"bool('Hello') = {bool('Hello')}")  # Non-empty string
print(f"bool([1,2,3]) = {bool([1,2,3])}")  # Non-empty list
print(f"bool(' ') = {bool(' ')}")  # Space is not empty!
```

**Output:**
```
=== Falsy Values ===
bool(0) = False
bool(0.0) = False
bool('') = False
bool([]) = False
bool({}) = False
bool(None) = False

=== Truthy Values ===
bool(1) = True
bool(-1) = True
bool(3.14) = True
bool('Hello') = True
bool([1,2,3]) = True
bool(' ') = True
```

### 🟡 Intermediate: Collection Conversions

```python
# String to List
text = "Python"
char_list = list(text)
print(f"'{text}' → {char_list}")

# List to Tuple
my_list = [1, 2, 3]
my_tuple = tuple(my_list)
print(f"{my_list} (list) → {my_tuple} (tuple)")

# Tuple to List
my_tuple = (4, 5, 6)
my_list = list(my_tuple)
print(f"{my_tuple} (tuple) → {my_list} (list)")

# List to Set (removes duplicates)
numbers = [1, 2, 2, 3, 3, 3]
unique = set(numbers)
print(f"{numbers} → {unique}")

# Set to List
back_to_list = list(unique)
print(f"{unique} → {back_to_list}")

# String split to List
sentence = "Hello World Python"
words = sentence.split()
print(f"'{sentence}' → {words}")

# List join to String
words = ['Hello', 'World']
joined = ' '.join(words)
print(f"{words} → '{joined}'")
```

**Output:**
```
'Python' → ['P', 'y', 't', 'h', 'o', 'n']
[1, 2, 3] (list) → (1, 2, 3) (tuple)
(4, 5, 6) (tuple) → [4, 5, 6] (list)
[1, 2, 2, 3, 3, 3] → {1, 2, 3}
{1, 2, 3} → [1, 2, 3]
'Hello World Python' → ['Hello', 'World', 'Python']
['Hello', 'World'] → 'Hello World'
```

### 🟡 Intermediate: Implicit Type Casting

```python
# Python automatically handles type conversion
print("=== Implicit Type Casting ===")

# int + float = float
x = 5       # int
y = 2.5     # float
result = x + y
print(f"int + float: {x} + {y} = {result} (type: {type(result).__name__})")

# int + complex = complex
a = 10            # int
b = 3 + 2j        # complex
result = a + b
print(f"int + complex: {a} + {b} = {result} (type: {type(result).__name__})")

# bool + int = int (True=1, False=0)
flag = True
number = 5
result = flag + number
print(f"bool + int: {flag} + {number} = {result} (type: {type(result).__name__})")

# NOTE: str + int does NOT work!
# Python won't implicitly convert here
# "5" + 5  # TypeError!
```

**Output:**
```
=== Implicit Type Casting ===
int + float: 5 + 2.5 = 7.5 (type: float)
int + complex: 10 + (3+2j) = (13+2j) (type: complex)
bool + int: True + 5 = 6 (type: int)
```

### 🟡 Intermediate: ASCII and Character Conversions

```python
# ord() - Character to ASCII value
print(f"'A' → {ord('A')}")   # 65
print(f"'a' → {ord('a')}")   # 97
print(f"'0' → {ord('0')}")   # 48

# chr() - ASCII value to Character
print(f"65 → '{chr(65)}'")   # A
print(f"97 → '{chr(97)}'")   # a
print(f"48 → '{chr(48)}'")   # 0

# Practical: Generate alphabet
alphabet = [chr(i) for i in range(65, 91)]
print(f"A-Z: {alphabet}")
```

**Output:**
```
'A' → 65
'a' → 97
'0' → 48
65 → 'A'
97 → 'a'
48 → '0'
A-Z: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
```

### 🔴 Advanced: Number Base Conversions

```python
# Decimal to other bases
decimal = 255

print(f"Decimal: {decimal}")
print(f"Binary:  {bin(decimal)}")   # 0b11111111
print(f"Octal:   {oct(decimal)}")   # 0o377
print(f"Hex:     {hex(decimal)}")   # 0xff

# Other bases to Decimal
binary = "0b11111111"
octal = "0o377"
hexa = "0xff"

print(f"\nBack to decimal:")
print(f"{binary} → {int(binary, 2)}")
print(f"{octal} → {int(octal, 8)}")
print(f"{hexa} → {int(hexa, 16)}")

# Direct conversion (without prefix)
print(f"\nDirect base conversion:")
print(f"'1010' (base 2) → {int('1010', 2)}")   # 10
print(f"'FF' (base 16) → {int('FF', 16)}")     # 255
print(f"'77' (base 8) → {int('77', 8)}")       # 63
```

**Output:**
```
Decimal: 255
Binary:  0b11111111
Octal:   0o377
Hex:     0xff

Back to decimal:
0b11111111 → 255
0o377 → 255
0xff → 255

Direct base conversion:
'1010' (base 2) → 10
'FF' (base 16) → 255
'77' (base 8) → 63
```

### 🔴 Advanced: Safe Type Casting

```python
def safe_int(value, default=0):
    """Safely convert to int, return default on failure."""
    try:
        return int(value)
    except (ValueError, TypeError):
        return default

def safe_float(value, default=0.0):
    """Safely convert to float, return default on failure."""
    try:
        return float(value)
    except (ValueError, TypeError):
        return default

# Testing safe conversion
print(safe_int("42"))        # 42
print(safe_int("hello"))     # 0 (default)
print(safe_int(None))        # 0 (default)
print(safe_int("", -1))      # -1 (custom default)

print(safe_float("3.14"))    # 3.14
print(safe_float("invalid")) # 0.0 (default)
```

**Output:**
```
42
0
0
-1
3.14
0.0
```

### 🔴 Advanced: Custom Type Conversion with Classes

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius
    
    def __int__(self):
        """Called when int(temperature) is used."""
        return int(self.celsius)
    
    def __float__(self):
        """Called when float(temperature) is used."""
        return float(self.celsius)
    
    def __str__(self):
        """Called when str(temperature) is used."""
        return f"{self.celsius}°C"
    
    def __bool__(self):
        """Called when bool(temperature) is used."""
        return self.celsius != 0

# Usage
temp = Temperature(37.5)
print(f"int(temp) = {int(temp)}")       # 37
print(f"float(temp) = {float(temp)}")   # 37.5
print(f"str(temp) = {str(temp)}")       # 37.5°C
print(f"bool(temp) = {bool(temp)}")     # True

zero_temp = Temperature(0)
print(f"bool(0°C) = {bool(zero_temp)}") # False
```

**Output:**
```
int(temp) = 37
float(temp) = 37.5
str(temp) = 37.5°C
bool(temp) = True
bool(0°C) = False
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Converting Invalid Strings

```python
# Wrong
int("hello")      # ValueError!
int("3.14")       # ValueError! (has decimal)
float("12,345")   # ValueError! (comma instead of dot)

# Correct
int("42")         # ✅ Works
int(float("3.14")) # ✅ Convert to float first, then int
float("3.14")     # ✅ Works
```

### ❌ Mistake 2: Confusing Truncation with Rounding

```python
# int() truncates (removes decimal), doesn't round!
print(int(3.9))   # 3, NOT 4
print(int(-3.9))  # -3, NOT -4

# For rounding, use round()
print(round(3.9)) # 4
print(round(3.5)) # 4 (banker's rounding)
```

### ❌ Mistake 3: Expecting str() to Join Lists

```python
# Wrong
my_list = [1, 2, 3]
result = str(my_list)
print(result)  # "[1, 2, 3]" - includes brackets!

# Correct - to get "1 2 3"
result = ' '.join(map(str, my_list))
print(result)  # "1 2 3"
```

### ❌ Mistake 4: Bool Conversion Surprises

```python
# These might surprise you!
print(bool("False"))  # True! (non-empty string)
print(bool("0"))      # True! (non-empty string)
print(bool([0]))      # True! (non-empty list)
print(bool(0.0001))   # True! (non-zero number)
```

### ❌ Mistake 5: Not Handling Conversion Errors

```python
# Dangerous
age = int(input("Enter age: "))  # Crashes if user types "abc"

# Safe
try:
    age = int(input("Enter age: "))
except ValueError:
    print("Invalid input! Using default age 0")
    age = 0
```

---

## 💡 Pro Tips

### 1. Check Type Before Converting
```python
def smart_convert(value):
    if isinstance(value, str) and value.isdigit():
        return int(value)
    elif isinstance(value, str):
        try:
            return float(value)
        except ValueError:
            return value
    return value
```

### 2. Use Ternary for Quick Conversion
```python
value = "42"
result = int(value) if value.isdigit() else 0
```

### 3. Convert List of Strings to Integers
```python
str_numbers = ["1", "2", "3", "4", "5"]
int_numbers = list(map(int, str_numbers))
# Or
int_numbers = [int(x) for x in str_numbers]
```

### 4. Format Float Precision
```python
# Convert float to string with specific precision
pi = 3.14159265
formatted = f"{pi:.2f}"  # "3.14"
```

### 5. Use `isinstance()` for Type Checking
```python
value = 42
if isinstance(value, (int, float)):
    print("It's a number!")
```

---

## 🔍 Real-world Use Cases

### 1. Processing User Input
```python
# Calculator that handles different input types
def calculate(num1, num2, operation):
    # Convert to float to handle both int and float inputs
    n1 = float(num1)
    n2 = float(num2)
    
    if operation == '+':
        result = n1 + n2
    elif operation == '-':
        result = n1 - n2
    
    # Return as int if it's a whole number
    return int(result) if result == int(result) else result

print(calculate("10", "3", "+"))  # 13
print(calculate("10", "3.5", "+"))  # 13.5
```

### 2. Data Cleaning
```python
# Clean data from CSV (strings to appropriate types)
raw_data = ["42", "3.14", "True", "None", "hello"]

def clean_value(val):
    val = val.strip()
    if val.lower() == "true":
        return True
    elif val.lower() == "false":
        return False
    elif val.lower() == "none":
        return None
    elif val.isdigit() or (val.startswith('-') and val[1:].isdigit()):
        return int(val)
    else:
        try:
            return float(val)
        except ValueError:
            return val

cleaned = [clean_value(x) for x in raw_data]
print(cleaned)  # [42, 3.14, True, None, 'hello']
```

### 3. Binary/Hex for Color Codes
```python
# Convert hex color to RGB
hex_color = "#FF5733"

# Remove # and convert each pair
r = int(hex_color[1:3], 16)
g = int(hex_color[3:5], 16)
b = int(hex_color[5:7], 16)

print(f"Color: {hex_color} = RGB({r}, {g}, {b})")
# Color: #FF5733 = RGB(255, 87, 51)
```

---

## 💼 Interview Insight

### Q1: What is the difference between implicit and explicit type casting?
> - **Implicit:** Python automatically converts (int→float)
> - **Explicit:** Programmer manually converts using functions like `int()`, `str()`

### Q2: What happens when you do `int(3.9)`?
> It **truncates** (cuts off) the decimal, returning `3`. It does NOT round. Use `round()` for rounding.

### Q3: What are truthy and falsy values in Python?
> **Falsy:** `0`, `0.0`, `""`, `[]`, `{}`, `()`, `None`, `False`
> **Truthy:** Everything else (non-zero numbers, non-empty collections)

### Q4: How do you convert a list of strings to integers?
```python
strings = ["1", "2", "3"]
integers = list(map(int, strings))
# or
integers = [int(x) for x in strings]
```

### Q5: Can you convert any string to int?
> No. Only strings that represent valid integers can be converted. `int("hello")` raises `ValueError`.

---

## 🔥 Most Asked Questions

**Q: Why does `int("3.14")` fail?**
> `int()` expects a string that represents a whole number. Convert to float first: `int(float("3.14"))`

**Q: What's the difference between `str(list)` and `''.join(list)`?**
> - `str([1,2,3])` → `"[1, 2, 3]"` (includes brackets)
> - `''.join(['a','b','c'])` → `"abc"` (joins elements)

**Q: How does Python handle bool arithmetic?**
> `True` = 1, `False` = 0. So `True + True = 2`

**Q: Can you convert dict to list?**
> Yes, but you only get keys: `list({"a":1, "b":2})` → `['a', 'b']`

---

## ⚡ Shortcut Tricks

### Quick Boolean Check
```python
# Instead of
if len(my_list) > 0:
# Use
if my_list:  # Truthy check
```

### One-liner Type Conversion
```python
# Multiple integers from input
a, b, c = map(int, input().split())
```

### Safe Division Result
```python
# Get int if whole, float otherwise
result = a / b
result = int(result) if result == int(result) else result
```

### Quick Digit Check
```python
"42".isdigit()      # True
"42.5".isdigit()    # False (has dot)
"-42".isdigit()     # False (has minus)
```

---

## 🧪 Practice Questions

### Easy:
1. Convert the string "100" to an integer and add 50 to it
2. Convert the integer 42 to a string and concatenate " years"
3. What is `bool([])`? Why?

### Medium:
4. Convert a float 99.99 to an integer. What value do you get?
5. Convert the list `[1, 2, 3]` to a tuple, then to a set
6. Convert the hex string "FF" to decimal

### Hard:
7. Write a function that takes any input and returns its type-converted form (string numbers → int/float)
8. Explain why `bool("False")` is `True`
9. Convert RGB(255, 128, 0) to hex color code

---

## 🚀 Mini Tasks / Assignments

### Task 1: Input Type Validator
```python
# Create a function that:
# - Takes user input
# - Tries to convert to int, then float
# - Returns the appropriate type
# - Returns string if conversion fails
```

### Task 2: Binary Calculator
```python
# Create a calculator that:
# - Takes two binary numbers as strings ("1010", "1100")
# - Converts to decimal, performs addition
# - Returns result in binary
```

### Task 3: Grade Converter
```python
# Convert percentage to grade:
# 90-100 → "A", 80-89 → "B", etc.
# Handle string input like "85%"
```

### Task 4: Color Code Converter
```python
# Create functions to convert:
# - Hex to RGB: "#FF5733" → (255, 87, 51)
# - RGB to Hex: (255, 87, 51) → "#FF5733"
```

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Implicit Casting | Python auto-converts (int→float) |
| Explicit Casting | Manual: `int()`, `float()`, `str()` |
| int() | Truncates decimals (3.9→3) |
| round() | Rounds to nearest (3.9→4) |
| bool() | Falsy: 0, "", [], None |
| Truthy | Non-zero, non-empty values |
| ord()/chr() | Character ↔ ASCII |
| bin()/hex()/oct() | Number ↔ Base conversion |

---

## ⏭️ Next Topic

Now you can convert between data types! Next, let's learn:

**[Operators](./05_Operators.md)** — How to perform operations on data.

---

*Type casting is essential for data processing. Master it!* 🔄
