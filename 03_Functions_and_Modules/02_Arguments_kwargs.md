# 📘 Arguments, *args, and **kwargs

## 📌 Introduction

When calling functions, you need to pass data to them. Python offers incredibly flexible ways to handle function arguments. Understanding these patterns is essential for writing powerful, reusable functions.

**Types of Arguments:**
- **Positional arguments** — Order matters
- **Keyword arguments** — Named, order doesn't matter
- **Default arguments** — Have fallback values
- ***args** — Variable positional arguments
- ****kwargs** — Variable keyword arguments

```
┌─────────────────────────────────────────────────────────────┐
│                  FUNCTION ARGUMENTS                          │
│                                                              │
│  def func(a, b, c=10, *args, **kwargs):                     │
│           │  │   │        │        │                        │
│           │  │   │        │        └── keyword args dict    │
│           │  │   │        └── extra positional args tuple   │
│           │  │   └── default value argument                 │
│           │  └── required positional argument               │
│           └── required positional argument                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. Parameters vs Arguments

| Term | Definition | Example |
|------|------------|---------|
| **Parameter** | Variable in function definition | `def greet(name):` — `name` is parameter |
| **Argument** | Value passed when calling | `greet("Alice")` — `"Alice"` is argument |

### 2. Types of Arguments

| Type | Description | Example Call |
|------|-------------|--------------|
| **Positional** | Passed by position | `func(1, 2, 3)` |
| **Keyword** | Passed by name | `func(a=1, b=2)` |
| **Default** | Has default value | `def func(x=10)` |
| **Variable Positional (*args)** | Any number of positional | `func(1, 2, 3, 4, 5)` |
| **Variable Keyword (**kwargs)** | Any number of keyword | `func(a=1, b=2, c=3)` |

### 3. Argument Order Rule

```python
def func(positional, *args, keyword_only, **kwargs):
    pass

# Order: positional → *args → keyword-only → **kwargs
```

---

## 💻 Code Examples

---

## 1️⃣ Positional Arguments

### 🟢 Basic: Simple Positional Arguments

```python
def describe_pet(animal, name):
    """Describe a pet using positional arguments."""
    print(f"I have a {animal} named {name}.")

# Order matters!
describe_pet("dog", "Buddy")    # Correct
describe_pet("Buddy", "dog")    # Wrong - says "dog named dog"
```

**Output:**
```
I have a dog named Buddy.
I have a Buddy named dog.
```

### 🟡 Intermediate: Mixed Positional Arguments

```python
def calculate_price(quantity, unit_price, tax_rate):
    """Calculate total price with tax."""
    subtotal = quantity * unit_price
    tax = subtotal * tax_rate
    total = subtotal + tax
    return total

# All positional - must remember order!
price = calculate_price(5, 10.99, 0.08)
print(f"Total: ${price:.2f}")
```

---

## 2️⃣ Keyword Arguments

### 🟢 Basic: Using Keyword Arguments

```python
def describe_pet(animal, name):
    """Describe a pet."""
    print(f"I have a {animal} named {name}.")

# Using keywords - order doesn't matter!
describe_pet(animal="dog", name="Buddy")
describe_pet(name="Whiskers", animal="cat")

# Mix positional and keyword
describe_pet("hamster", name="Fluffy")
```

**Output:**
```
I have a dog named Buddy.
I have a cat named Whiskers.
I have a hamster named Fluffy.
```

### 🟡 Intermediate: Keyword Arguments Improve Readability

```python
def create_user(username, email, age, is_active, role):
    """Create a user account."""
    return {
        "username": username,
        "email": email,
        "age": age,
        "is_active": is_active,
        "role": role
    }

# Hard to read with positional
user1 = create_user("john_doe", "john@email.com", 25, True, "admin")

# Much clearer with keywords
user2 = create_user(
    username="jane_doe",
    email="jane@email.com",
    age=30,
    is_active=True,
    role="editor"
)

print(user2)
```

---

## 3️⃣ Default Arguments

### 🟢 Basic: Simple Default Values

```python
def greet(name, greeting="Hello"):
    """Greet someone with optional custom greeting."""
    print(f"{greeting}, {name}!")

greet("Alice")                    # Uses default
greet("Bob", "Good morning")      # Custom greeting
greet("Charlie", greeting="Hi")   # Keyword argument
```

**Output:**
```
Hello, Alice!
Good morning, Bob!
Hi, Charlie!
```

### 🟡 Intermediate: Multiple Default Arguments

```python
def create_profile(name, age=18, city="Unknown", country="USA"):
    """Create a user profile with defaults."""
    return {
        "name": name,
        "age": age,
        "city": city,
        "country": country
    }

# Various ways to call
print(create_profile("Alice"))
print(create_profile("Bob", 25))
print(create_profile("Charlie", 30, "New York"))
print(create_profile("Diana", city="Los Angeles"))  # Skip age
print(create_profile("Eve", country="Canada", age=22))  # Different order
```

**Output:**
```
{'name': 'Alice', 'age': 18, 'city': 'Unknown', 'country': 'USA'}
{'name': 'Bob', 'age': 25, 'city': 'Unknown', 'country': 'USA'}
{'name': 'Charlie', 'age': 30, 'city': 'New York', 'country': 'USA'}
{'name': 'Diana', 'age': 18, 'city': 'Los Angeles', 'country': 'USA'}
{'name': 'Eve', 'age': 22, 'city': 'Unknown', 'country': 'Canada'}
```

### 🔴 Advanced: Avoiding Mutable Default Arguments

```python
# WRONG - Mutable default is shared!
def append_to_list_wrong(item, my_list=[]):
    my_list.append(item)
    return my_list

print(append_to_list_wrong(1))  # [1]
print(append_to_list_wrong(2))  # [1, 2] - Unexpected!
print(append_to_list_wrong(3))  # [1, 2, 3] - Still accumulating!

# CORRECT - Use None as default
def append_to_list_correct(item, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(item)
    return my_list

print(append_to_list_correct(1))  # [1]
print(append_to_list_correct(2))  # [2] - Fresh list!
print(append_to_list_correct(3))  # [3] - Fresh list!
```

---

## 4️⃣ *args (Variable Positional Arguments)

### 🟢 Basic: Accept Any Number of Arguments

```python
def sum_all(*args):
    """Sum any number of arguments."""
    print(f"Arguments received: {args}")
    print(f"Type: {type(args)}")  # tuple
    return sum(args)

# Call with different numbers of arguments
print(f"Sum: {sum_all(1, 2)}")
print(f"Sum: {sum_all(1, 2, 3, 4, 5)}")
print(f"Sum: {sum_all(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)}")
```

**Output:**
```
Arguments received: (1, 2)
Type: <class 'tuple'>
Sum: 3
Arguments received: (1, 2, 3, 4, 5)
Type: <class 'tuple'>
Sum: 15
Arguments received: (10, 20, 30, 40, 50, 60, 70, 80, 90, 100)
Type: <class 'tuple'>
Sum: 550
```

### 🟡 Intermediate: *args with Regular Parameters

```python
def greet_all(greeting, *names):
    """Greet multiple people with same greeting."""
    for name in names:
        print(f"{greeting}, {name}!")

greet_all("Hello", "Alice", "Bob", "Charlie")
print()
greet_all("Good morning", "Diana")
```

**Output:**
```
Hello, Alice!
Hello, Bob!
Hello, Charlie!

Good morning, Diana!
```

### 🟡 Intermediate: Unpacking with *

```python
def add(a, b, c):
    return a + b + c

numbers = [1, 2, 3]

# Without unpacking - Error!
# add(numbers)  # TypeError: missing arguments

# With unpacking - Works!
result = add(*numbers)  # Same as add(1, 2, 3)
print(f"Sum: {result}")

# Combine unpacking
list1 = [1, 2]
list2 = [3, 4, 5]
combined = [*list1, *list2]
print(f"Combined: {combined}")
```

**Output:**
```
Sum: 6
Combined: [1, 2, 3, 4, 5]
```

---

## 5️⃣ **kwargs (Variable Keyword Arguments)

### 🟢 Basic: Accept Any Keyword Arguments

```python
def print_info(**kwargs):
    """Print any keyword arguments received."""
    print(f"Arguments received: {kwargs}")
    print(f"Type: {type(kwargs)}")  # dict
    
    for key, value in kwargs.items():
        print(f"  {key}: {value}")

print_info(name="Alice", age=25, city="NYC")
print()
print_info(a=1, b=2, c=3, d=4, e=5)
```

**Output:**
```
Arguments received: {'name': 'Alice', 'age': 25, 'city': 'NYC'}
Type: <class 'dict'>
  name: Alice
  age: 25
  city: NYC

Arguments received: {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
Type: <class 'dict'>
  a: 1
  b: 2
  c: 3
  d: 4
  e: 5
```

### 🟡 Intermediate: Building Flexible Functions

```python
def create_html_element(tag, content="", **attributes):
    """Create an HTML element with attributes."""
    # Build attribute string
    attr_str = ""
    for key, value in attributes.items():
        # Replace underscores with hyphens (for attributes like data-id)
        key = key.replace("_", "-")
        attr_str += f' {key}="{value}"'
    
    return f"<{tag}{attr_str}>{content}</{tag}>"

# Create various HTML elements
print(create_html_element("p", "Hello, World!"))
print(create_html_element("a", "Click here", href="https://example.com"))
print(create_html_element("div", "Content", id="main", class_="container"))
print(create_html_element("input", type="text", name="email", placeholder="Enter email"))
```

**Output:**
```
<p>Hello, World!</p>
<a href="https://example.com">Click here</a>
<div id="main" class="container">Content</div>
<input type="text" name="email" placeholder="Enter email"></input>
```

### 🟡 Intermediate: Unpacking with **

```python
def greet(name, greeting="Hello", punctuation="!"):
    return f"{greeting}, {name}{punctuation}"

# Using a dictionary
config = {
    "name": "Alice",
    "greeting": "Good morning",
    "punctuation": "!!!"
}

# Unpack dictionary as keyword arguments
message = greet(**config)  # Same as greet(name="Alice", greeting="Good morning", ...)
print(message)

# Merge dictionaries
defaults = {"color": "blue", "size": "medium"}
custom = {"size": "large", "style": "bold"}
merged = {**defaults, **custom}  # custom overrides defaults
print(merged)
```

**Output:**
```
Good morning, Alice!!!
{'color': 'blue', 'size': 'large', 'style': 'bold'}
```

---

## 6️⃣ Combining *args and **kwargs

### 🟡 Intermediate: Full Flexibility

```python
def super_flexible(required, *args, default="value", **kwargs):
    """Demonstrate all argument types."""
    print(f"Required: {required}")
    print(f"*args: {args}")
    print(f"Default: {default}")
    print(f"**kwargs: {kwargs}")
    print()

# Various calls
super_flexible("must have")
super_flexible("must", 1, 2, 3)
super_flexible("must", 1, 2, 3, default="custom")
super_flexible("must", 1, 2, x=10, y=20)
super_flexible("must", 1, 2, 3, default="custom", x=10, y=20)
```

**Output:**
```
Required: must have
*args: ()
Default: value
**kwargs: {}

Required: must
*args: (1, 2, 3)
Default: value
**kwargs: {}

Required: must
*args: (1, 2, 3)
Default: custom
**kwargs: {}

Required: must
*args: (1, 2)
Default: value
**kwargs: {'x': 10, 'y': 20}

Required: must
*args: (1, 2, 3)
Default: custom
**kwargs: {'x': 10, 'y': 20}
```

### 🔴 Advanced: Forwarding Arguments (Decorator Pattern)

```python
def log_call(func):
    """Decorator that logs function calls."""
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        print(f"  args: {args}")
        print(f"  kwargs: {kwargs}")
        result = func(*args, **kwargs)  # Forward all arguments
        print(f"  returned: {result}")
        return result
    return wrapper

@log_call
def add(a, b):
    return a + b

@log_call
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# Usage
add(5, 3)
print()
greet("Alice", greeting="Hi")
```

**Output:**
```
Calling add
  args: (5, 3)
  kwargs: {}
  returned: 8

Calling greet
  args: ('Alice',)
  kwargs: {'greeting': 'Hi'}
  returned: Hi, Alice!
```

---

## 7️⃣ Keyword-Only Arguments

### 🔴 Advanced: Forcing Keyword Arguments

```python
# Everything after * must be keyword-only
def process(data, *, verbose=False, timeout=30):
    """
    Process data with keyword-only options.
    
    Args:
        data: Data to process
        verbose: Print progress (keyword-only)
        timeout: Max seconds (keyword-only)
    """
    if verbose:
        print(f"Processing with timeout={timeout}")
    return f"Processed: {data}"

# Must use keywords for verbose and timeout
result = process("my data")  # OK
result = process("my data", verbose=True)  # OK
result = process("my data", verbose=True, timeout=60)  # OK

# This would fail:
# process("my data", True, 60)  # TypeError!

print(result)
```

### 🔴 Advanced: Positional-Only Arguments (Python 3.8+)

```python
# Everything before / must be positional-only
def greet(name, /, greeting="Hello"):
    """
    Greet someone.
    
    Args:
        name: Person's name (positional-only)
        greeting: Greeting to use
    """
    return f"{greeting}, {name}!"

# Must use positional for name
print(greet("Alice"))  # OK
print(greet("Bob", greeting="Hi"))  # OK
print(greet("Charlie", "Hey"))  # OK

# This would fail:
# greet(name="Diana")  # TypeError!

# Full syntax example
def example(pos_only, /, standard, *, kw_only):
    """
    pos_only: must be positional
    standard: can be either
    kw_only: must be keyword
    """
    pass

# Valid calls:
example(1, 2, kw_only=3)
example(1, standard=2, kw_only=3)
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Positional After Keyword

```python
# Wrong - positional can't follow keyword
def func(a, b, c):
    pass

# func(a=1, 2, 3)  # SyntaxError!
func(1, b=2, c=3)  # OK
func(1, 2, c=3)    # OK
```

### ❌ Mistake 2: Non-default After Default

```python
# Wrong - non-default can't follow default
# def func(a=1, b):  # SyntaxError!
#     pass

# Correct
def func(a, b=1):
    pass
```

### ❌ Mistake 3: Using *args Name Wrong

```python
# *args is a convention, not required
def my_func(*numbers):  # "numbers", not "args"
    return sum(numbers)

# Works fine!
my_func(1, 2, 3)
```

### ❌ Mistake 4: Mutable Default (Repeated)

```python
# NEVER do this
def bad(items=[]):
    items.append(1)
    return items

# ALWAYS do this
def good(items=None):
    if items is None:
        items = []
    items.append(1)
    return items
```

---

## 💡 Pro Tips

### 1. Use * to Require Keyword Arguments
```python
def dangerous_operation(*, confirm=False):
    if not confirm:
        raise ValueError("Must confirm=True")
```

### 2. Use **kwargs for Configuration
```python
def create_connection(host, port, **options):
    timeout = options.get('timeout', 30)
    retry = options.get('retry', 3)
    # ...
```

### 3. Forward Arguments Properly
```python
def wrapper(*args, **kwargs):
    return actual_function(*args, **kwargs)
```

### 4. Document Complex Signatures
```python
def complex_func(a, b, *args, c=1, **kwargs):
    """
    Args:
        a: First required positional
        b: Second required positional
        *args: Additional positional arguments
        c: Keyword argument with default
        **kwargs: Additional keyword arguments
    """
```

---

## 🔍 Real-world Use Cases

### 1. API Request Function
```python
def make_request(url, method="GET", **kwargs):
    headers = kwargs.get('headers', {})
    timeout = kwargs.get('timeout', 30)
    data = kwargs.get('data', None)
    
    print(f"{method} {url}")
    print(f"Headers: {headers}")
    print(f"Timeout: {timeout}")
    print(f"Data: {data}")

make_request(
    "https://api.example.com/users",
    method="POST",
    headers={"Content-Type": "application/json"},
    data={"name": "Alice"}
)
```

### 2. Database Query Builder
```python
def query(table, *columns, **conditions):
    cols = ", ".join(columns) if columns else "*"
    sql = f"SELECT {cols} FROM {table}"
    
    if conditions:
        where = " AND ".join(f"{k}='{v}'" for k, v in conditions.items())
        sql += f" WHERE {where}"
    
    return sql

print(query("users"))
print(query("users", "name", "email"))
print(query("users", "name", age=25, city="NYC"))
```

---

## 💼 Interview Insight

### Q1: What is the difference between *args and **kwargs?
> - `*args`: Collects extra positional arguments as a tuple
> - `**kwargs`: Collects extra keyword arguments as a dictionary

### Q2: What is the order of parameters in a function definition?
> 1. Regular positional
> 2. *args
> 3. Keyword-only (after *)
> 4. **kwargs

### Q3: Can you have both *args and **kwargs?
> Yes! `def func(*args, **kwargs):` accepts any arguments.

### Q4: Why should you avoid mutable default arguments?
> Mutable defaults are shared between calls, causing unexpected behavior.

---

## 🧪 Practice Questions

### Easy:
1. Write a function that takes any number of strings and joins them
2. Write a function with a default greeting parameter
3. Use **kwargs to print all key-value pairs

### Medium:
4. Create a function that calculates average of any number of scores
5. Build a formatted print function using *args and **kwargs
6. Write a function that merges multiple dictionaries

### Hard:
7. Create a decorator that validates function arguments
8. Write a caching function using **kwargs for configuration
9. Implement a function that builds SQL queries

---

## 📖 Summary

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Positional | `func(1, 2)` | Order-based arguments |
| Keyword | `func(a=1)` | Named arguments |
| Default | `def func(x=10)` | Optional with fallback |
| *args | `def func(*args)` | Variable positional |
| **kwargs | `def func(**kwargs)` | Variable keyword |
| Keyword-only | `def func(*, x)` | Force keyword usage |
| Positional-only | `def func(x, /)` | Force positional usage |

---

## ⏭️ Next Topic

**[Recursion](./03_Recursion.md)** — Functions that call themselves!

---

*Master arguments, master functions!* 🎯
