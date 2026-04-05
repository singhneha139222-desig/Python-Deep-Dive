# 📘 Functions in Python

## 📌 Introduction

A **function** is a reusable block of code that performs a specific task. Instead of writing the same code over and over, you put it in a function and call it whenever needed.

Think of a function like a **vending machine**:
- You put in input (money, selection)
- The machine processes it
- You get output (snack)

```
┌─────────────────────────────────────────────────────────────┐
│                    FUNCTION CONCEPT                          │
│                                                              │
│   ┌───────────┐                           ┌───────────┐     │
│   │  INPUTS   │    ┌──────────────┐       │  OUTPUT   │     │
│   │(parameters)│───►│   FUNCTION   │──────►│ (return)  │     │
│   │           │    │ (your code)  │       │           │     │
│   └───────────┘    └──────────────┘       └───────────┘     │
│                                                              │
│   greet("Alice")  ─────►  def greet(name):   ─────►  "Hello, Alice!"  │
│                            return f"Hello, {name}!"                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Function Syntax

```python
def function_name(parameters):
    """Docstring: describes what the function does."""
    # Function body (code)
    return result  # Optional
```

### 2. Key Terminology

| Term | Description | Example |
|------|-------------|---------|
| **Define** | Create a function | `def greet():` |
| **Call** | Execute/use a function | `greet()` |
| **Parameter** | Variable in function definition | `def greet(name):` |
| **Argument** | Value passed when calling | `greet("Alice")` |
| **Return** | Value sent back from function | `return result` |
| **Docstring** | Documentation string | `"""Description"""` |

### 3. Function Rules

| Rule | Description |
|------|-------------|
| Name | Use lowercase with underscores (`calculate_tax`) |
| Define before use | Function must be defined before calling it |
| Indentation | Function body must be indented |
| Return | Without `return`, function returns `None` |

---

## 💻 Code Examples

### 🟢 Basic: Simple Function (No Parameters)

```python
# Define a function
def say_hello():
    """Print a greeting message."""
    print("Hello, World!")

# Call the function
say_hello()
say_hello()
say_hello()
```

**Output:**
```
Hello, World!
Hello, World!
Hello, World!
```

### 🟢 Basic: Function with Parameters

```python
def greet(name):
    """Greet a person by name."""
    print(f"Hello, {name}!")

# Call with different arguments
greet("Alice")
greet("Bob")
greet("Charlie")
```

**Output:**
```
Hello, Alice!
Hello, Bob!
Hello, Charlie!
```

### 🟢 Basic: Function with Return Value

```python
def add(a, b):
    """Return the sum of two numbers."""
    result = a + b
    return result

# Use the returned value
sum1 = add(5, 3)
print(f"5 + 3 = {sum1}")

sum2 = add(10, 20)
print(f"10 + 20 = {sum2}")

# Use directly in expressions
print(f"Total: {add(sum1, sum2)}")
```

**Output:**
```
5 + 3 = 8
10 + 20 = 30
Total: 38
```

### 🟢 Basic: Multiple Parameters

```python
def introduce(name, age, city):
    """Introduce a person."""
    print(f"Hi, I'm {name}!")
    print(f"I'm {age} years old.")
    print(f"I live in {city}.")

introduce("Alice", 25, "New York")
print()
introduce("Bob", 30, "Los Angeles")
```

**Output:**
```
Hi, I'm Alice!
I'm 25 years old.
I live in New York.

Hi, I'm Bob!
I'm 30 years old.
I live in Los Angeles.
```

### 🟡 Intermediate: Return Multiple Values

```python
def get_stats(numbers):
    """Calculate and return multiple statistics."""
    total = sum(numbers)
    count = len(numbers)
    average = total / count
    minimum = min(numbers)
    maximum = max(numbers)
    
    return total, count, average, minimum, maximum

# Unpack returned tuple
data = [10, 20, 30, 40, 50]
total, count, avg, min_val, max_val = get_stats(data)

print(f"Data: {data}")
print(f"Sum: {total}")
print(f"Count: {count}")
print(f"Average: {avg}")
print(f"Min: {min_val}")
print(f"Max: {max_val}")
```

**Output:**
```
Data: [10, 20, 30, 40, 50]
Sum: 150
Count: 5
Average: 30.0
Min: 10
Max: 50
```

### 🟡 Intermediate: Docstrings and Help

```python
def calculate_bmi(weight, height):
    """
    Calculate Body Mass Index (BMI).
    
    Parameters:
        weight (float): Weight in kilograms
        height (float): Height in meters
    
    Returns:
        float: BMI value
        str: BMI category
    
    Example:
        >>> bmi, category = calculate_bmi(70, 1.75)
        >>> print(f"BMI: {bmi}, Category: {category}")
        BMI: 22.86, Category: Normal
    """
    bmi = weight / (height ** 2)
    
    if bmi < 18.5:
        category = "Underweight"
    elif bmi < 25:
        category = "Normal"
    elif bmi < 30:
        category = "Overweight"
    else:
        category = "Obese"
    
    return round(bmi, 2), category

# Use the function
bmi, category = calculate_bmi(70, 1.75)
print(f"BMI: {bmi}, Category: {category}")

# View documentation
print("\n--- Documentation ---")
print(calculate_bmi.__doc__)

# Or use help()
# help(calculate_bmi)
```

**Output:**
```
BMI: 22.86, Category: Normal

--- Documentation ---

    Calculate Body Mass Index (BMI).
    
    Parameters:
        weight (float): Weight in kilograms
        height (float): Height in meters
    
    Returns:
        float: BMI value
        str: BMI category
    
    Example:
        >>> bmi, category = calculate_bmi(70, 1.75)
        >>> print(f"BMI: {bmi}, Category: {category}")
        BMI: 22.86, Category: Normal
```

### 🟡 Intermediate: Early Return

```python
def get_grade(score):
    """Return grade based on score."""
    # Guard clauses with early returns
    if score < 0 or score > 100:
        return "Invalid score"
    
    if score >= 90:
        return "A"
    if score >= 80:
        return "B"
    if score >= 70:
        return "C"
    if score >= 60:
        return "D"
    
    return "F"

# Test
scores = [95, 82, 71, 65, 45, 150, -5]
for score in scores:
    grade = get_grade(score)
    print(f"Score: {score} → Grade: {grade}")
```

**Output:**
```
Score: 95 → Grade: A
Score: 82 → Grade: B
Score: 71 → Grade: C
Score: 65 → Grade: D
Score: 45 → Grade: F
Score: 150 → Grade: Invalid score
Score: -5 → Grade: Invalid score
```

### 🟡 Intermediate: Functions with None Return

```python
def print_header(text, char="="):
    """Print a formatted header. Returns None."""
    line = char * len(text)
    print(line)
    print(text)
    print(line)
    # No return statement = returns None

# Call function
result = print_header("Welcome to Python")
print(f"Return value: {result}")  # None

# Check for None
def find_item(items, target):
    """Find item in list. Return None if not found."""
    for item in items:
        if item == target:
            return item
    return None  # Explicit None return

fruits = ["apple", "banana", "cherry"]
result = find_item(fruits, "banana")
print(f"\nFound: {result}")

result = find_item(fruits, "mango")
print(f"Found: {result}")  # None

# Safe handling
if (found := find_item(fruits, "mango")) is not None:
    print(f"Processing: {found}")
else:
    print("Item not found")
```

**Output:**
```
=================
Welcome to Python
=================
Return value: None

Found: banana
Found: None
Item not found
```

### 🔴 Advanced: Function Annotations (Type Hints)

```python
def calculate_total(
    price: float,
    quantity: int,
    discount: float = 0.0
) -> float:
    """
    Calculate total price with optional discount.
    
    Args:
        price: Unit price
        quantity: Number of items
        discount: Discount percentage (0-100)
    
    Returns:
        Total price after discount
    """
    subtotal: float = price * quantity
    discount_amount: float = subtotal * (discount / 100)
    total: float = subtotal - discount_amount
    
    return round(total, 2)

# Usage
total = calculate_total(29.99, 3)
print(f"Total (no discount): ${total}")

total = calculate_total(29.99, 3, 10)
print(f"Total (10% off): ${total}")

# Type hints are for documentation, not enforced!
# This still works (but IDE may warn):
total = calculate_total("not a number", "oops")  # Will error at runtime
```

### 🔴 Advanced: Functions as First-Class Objects

```python
# Functions are objects - can be assigned, passed, returned

def greet(name):
    return f"Hello, {name}!"

def farewell(name):
    return f"Goodbye, {name}!"

# Assign function to variable
say = greet
print(say("Alice"))  # Hello, Alice!

say = farewell
print(say("Alice"))  # Goodbye, Alice!

# Store functions in list
greetings = [greet, farewell]
for func in greetings:
    print(func("Bob"))

# Pass function as argument
def apply_greeting(greeting_func, name):
    return greeting_func(name)

result = apply_greeting(greet, "Charlie")
print(result)

# Return function from function
def get_greeting_function(formal=False):
    def formal_greet(name):
        return f"Good day, {name}."
    
    def casual_greet(name):
        return f"Hey, {name}!"
    
    return formal_greet if formal else casual_greet

greeter = get_greeting_function(formal=True)
print(greeter("Diana"))

greeter = get_greeting_function(formal=False)
print(greeter("Diana"))
```

**Output:**
```
Hello, Alice!
Goodbye, Alice!
Hello, Bob!
Goodbye, Bob!
Hello, Charlie!
Good day, Diana.
Hey, Diana!
```

### 🔴 Advanced: Closures

```python
def create_counter(start=0):
    """Create a counter function that remembers its state."""
    count = start
    
    def counter():
        nonlocal count  # Access variable from enclosing scope
        count += 1
        return count
    
    return counter

# Create two independent counters
counter1 = create_counter()
counter2 = create_counter(100)

print("Counter 1:", counter1())  # 1
print("Counter 1:", counter1())  # 2
print("Counter 1:", counter1())  # 3

print("Counter 2:", counter2())  # 101
print("Counter 2:", counter2())  # 102

print("Counter 1:", counter1())  # 4 (independent!)

# Practical example: multiplier factory
def create_multiplier(factor):
    """Create a function that multiplies by a fixed factor."""
    def multiplier(x):
        return x * factor
    return multiplier

double = create_multiplier(2)
triple = create_multiplier(3)

print(f"\nDouble 5: {double(5)}")  # 10
print(f"Triple 5: {triple(5)}")    # 15
```

**Output:**
```
Counter 1: 1
Counter 1: 2
Counter 1: 3
Counter 2: 101
Counter 2: 102
Counter 1: 4

Double 5: 10
Triple 5: 15
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting to Call the Function

```python
def greet():
    print("Hello!")

# Wrong - just references function
greet  # Nothing happens!

# Correct - call with parentheses
greet()  # Hello!
```

### ❌ Mistake 2: Modifying Mutable Default Arguments

```python
# DANGEROUS!
def add_item(item, items=[]):
    items.append(item)
    return items

print(add_item("a"))  # ['a']
print(add_item("b"))  # ['a', 'b'] - WHAT?!

# The default list is shared between calls!

# CORRECT
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

print(add_item("a"))  # ['a']
print(add_item("b"))  # ['b'] - Correct!
```

### ❌ Mistake 3: Forgetting Return

```python
def add(a, b):
    result = a + b
    # Forgot return!

sum = add(5, 3)
print(sum)  # None (not 8!)

# Correct
def add(a, b):
    return a + b
```

### ❌ Mistake 4: Wrong Argument Order

```python
def power(base, exponent):
    return base ** exponent

# Confusing order
print(power(2, 10))  # 1024 (2^10)
print(power(10, 2))  # 100 (10^2) - Different!

# Use keyword arguments when order is unclear
print(power(base=2, exponent=10))  # Clear!
```

### ❌ Mistake 5: Printing Instead of Returning

```python
# Wrong - prints but doesn't return
def calculate_tax(amount):
    tax = amount * 0.1
    print(tax)  # Just prints

result = calculate_tax(100)
total = result + 100  # Error! result is None

# Correct
def calculate_tax(amount):
    tax = amount * 0.1
    return tax

result = calculate_tax(100)
total = result + 100  # 110
```

---

## 💡 Pro Tips

### 1. Single Responsibility
```python
# Bad - does too much
def process_user(name, email, password):
    validate_email(email)
    hash_password(password)
    save_to_database(...)
    send_email(...)

# Good - each function does one thing
def validate_user(name, email, password):
    return is_valid_email(email) and is_valid_password(password)

def create_user(name, email, password):
    return {"name": name, "email": email, "password": hash_password(password)}
```

### 2. Use Descriptive Names
```python
# Bad
def calc(x, y):
    return x * y * 0.0825

# Good
def calculate_sales_tax(price, quantity, tax_rate=0.0825):
    return price * quantity * tax_rate
```

### 3. Write Good Docstrings
```python
def process_data(data: list) -> dict:
    """
    Process raw data and return summary statistics.
    
    Args:
        data: List of numerical values
        
    Returns:
        Dictionary with 'mean', 'sum', 'count' keys
        
    Raises:
        ValueError: If data is empty
        
    Example:
        >>> process_data([1, 2, 3])
        {'mean': 2.0, 'sum': 6, 'count': 3}
    """
```

### 4. Keep Functions Small
```python
# If a function is more than 20-30 lines, consider breaking it up
```

---

## 🔍 Real-world Use Cases

### 1. User Authentication
```python
def validate_password(password):
    """Check if password meets requirements."""
    if len(password) < 8:
        return False, "Password must be at least 8 characters"
    if not any(c.isupper() for c in password):
        return False, "Password must contain uppercase letter"
    if not any(c.isdigit() for c in password):
        return False, "Password must contain a digit"
    return True, "Password is valid"

is_valid, message = validate_password("Hello123")
print(f"Valid: {is_valid}, Message: {message}")
```

### 2. Data Processing Pipeline
```python
def clean_text(text):
    return text.strip().lower()

def remove_punctuation(text):
    import string
    return text.translate(str.maketrans('', '', string.punctuation))

def tokenize(text):
    return text.split()

def process_text(raw_text):
    """Process text through pipeline."""
    cleaned = clean_text(raw_text)
    no_punct = remove_punctuation(cleaned)
    tokens = tokenize(no_punct)
    return tokens

result = process_text("  Hello, World! How are you?  ")
print(result)  # ['hello', 'world', 'how', 'are', 'you']
```

---

## 💼 Interview Insight

### Q1: What is the difference between parameters and arguments?
> - **Parameters:** Variables in function definition
> - **Arguments:** Actual values passed when calling

### Q2: What does a function return if there's no return statement?
> It returns `None`.

### Q3: Can a function return multiple values?
> Yes, using tuple (comma-separated values).

### Q4: What is a docstring?
> A string literal as the first statement in a function that documents it.

### Q5: What is function scope?
> Variables defined inside a function are local to that function.

---

## 🔥 Most Asked Questions

**Q: Can I have a function inside a function?**
> Yes! It's called a nested function or inner function.

**Q: What is function overloading?**
> Python doesn't have traditional overloading. Use default arguments or *args instead.

**Q: When should I use print() vs return?**
> Use `return` when you need the value later. Use `print()` only for output/debugging.

---

## ⚡ Shortcut Tricks

```python
# Swap return values
def divide(a, b):
    return a // b, a % b  # quotient, remainder

q, r = divide(17, 5)

# Conditional return
def abs_value(x):
    return x if x >= 0 else -x

# Default to function result
def greet(name=None):
    return f"Hello, {name or 'Guest'}!"
```

---

## 🧪 Practice Questions

### Easy:
1. Write a function that returns the square of a number
2. Write a function that checks if a number is even
3. Write a function that reverses a string

### Medium:
4. Write a function that finds the factorial of a number
5. Write a function that returns the nth Fibonacci number
6. Write a function that checks if a string is a palindrome

### Hard:
7. Write a function that returns all prime factors of a number
8. Write a function that merges two sorted lists
9. Write a function that validates an email address

---

## 🚀 Mini Tasks / Assignments

### Task 1: Temperature Converter
```python
# Create two functions:
# - celsius_to_fahrenheit(celsius)
# - fahrenheit_to_celsius(fahrenheit)
```

### Task 2: Calculator Functions
```python
# Create functions for:
# - add, subtract, multiply, divide
# - Handle division by zero
```

### Task 3: String Utilities
```python
# Create a module with:
# - count_vowels(text)
# - count_words(text)
# - reverse_words(text)
```

---

## 📖 Summary

| Concept | Description |
|---------|-------------|
| `def` | Keyword to define function |
| Parameters | Input variables in definition |
| Arguments | Values passed when calling |
| `return` | Send value back from function |
| Docstring | Documentation string |
| Scope | Variables are local to function |
| First-class | Functions are objects |
| Closure | Function with remembered environment |

---

## ⏭️ Next Topic

Now let's explore advanced arguments:

**[Arguments and *args/**kwargs](./02_Arguments_kwargs.md)**

---

*Functions are the building blocks of programs. Build well!* 🧱
