# 📘 Modules and Packages

## 📌 Introduction

As your programs grow, you'll want to organize code into separate files. **Modules** and **packages** help you:

- **Organize** code logically
- **Reuse** code across projects
- **Share** code with others
- **Avoid** name conflicts

```
┌─────────────────────────────────────────────────────────────┐
│                    STRUCTURE                                 │
│                                                              │
│   Module = Single Python file (.py)                         │
│   Package = Folder containing modules + __init__.py         │
│   Library = Collection of packages (e.g., NumPy, Pandas)    │
│                                                              │
│   my_project/                                                │
│   ├── main.py              ← Your main script               │
│   ├── utils.py             ← A module                       │
│   └── mypackage/           ← A package                      │
│       ├── __init__.py      ← Makes it a package            │
│       ├── module1.py       ← Module in package             │
│       └── module2.py       ← Module in package             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### 1. What is a Module?

A **module** is simply a Python file (`.py`) containing functions, classes, and variables.

```python
# math_utils.py (this is a module)
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

PI = 3.14159
```

### 2. What is a Package?

A **package** is a folder containing multiple modules and a special `__init__.py` file.

### 3. Import Methods

| Method | Syntax | Usage |
|--------|--------|-------|
| Import module | `import math` | `math.sqrt(16)` |
| Import specific | `from math import sqrt` | `sqrt(16)` |
| Import with alias | `import numpy as np` | `np.array([1,2])` |
| Import all | `from math import *` | `sqrt(16)` (not recommended) |

---

## 💻 Code Examples

### 🟢 Basic: Importing Built-in Modules

```python
# Method 1: Import entire module
import math

print(f"Square root of 16: {math.sqrt(16)}")
print(f"Value of PI: {math.pi}")
print(f"Ceiling of 4.2: {math.ceil(4.2)}")

# Method 2: Import specific functions
from math import sqrt, pi, ceil

print(f"Square root of 25: {sqrt(25)}")
print(f"PI value: {pi}")

# Method 3: Import with alias
import datetime as dt

today = dt.date.today()
print(f"Today: {today}")

# Method 4: Import specific with alias
from datetime import datetime as dt_time

now = dt_time.now()
print(f"Current time: {now}")
```

**Output:**
```
Square root of 16: 4.0
Value of PI: 3.141592653589793
Ceiling of 4.2: 5
Square root of 25: 5.0
PI value: 3.141592653589793
Today: 2024-01-15
Current time: 2024-01-15 10:30:45.123456
```

### 🟢 Basic: Common Built-in Modules

```python
# random module
import random

print(f"Random number 1-10: {random.randint(1, 10)}")
print(f"Random choice: {random.choice(['apple', 'banana', 'cherry'])}")
print(f"Random float: {random.random()}")

# os module
import os

print(f"Current directory: {os.getcwd()}")
print(f"Files in current dir: {os.listdir('.')[:5]}")

# sys module
import sys

print(f"Python version: {sys.version}")
print(f"Platform: {sys.platform}")

# json module
import json

data = {"name": "Alice", "age": 25}
json_string = json.dumps(data)
print(f"JSON: {json_string}")
```

### 🟡 Intermediate: Creating Your Own Module

**Step 1: Create the module file**

```python
# myutils.py

"""
My utility functions module.
Contains helper functions for string and math operations.
"""

def reverse_string(s):
    """Reverse a string."""
    return s[::-1]

def is_palindrome(s):
    """Check if string is palindrome."""
    clean = s.lower().replace(" ", "")
    return clean == clean[::-1]

def factorial(n):
    """Calculate factorial."""
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Constants
VERSION = "1.0.0"
AUTHOR = "Your Name"

# This only runs when module is executed directly
if __name__ == "__main__":
    print("Testing myutils module...")
    print(f"reverse_string('hello'): {reverse_string('hello')}")
    print(f"is_palindrome('radar'): {is_palindrome('radar')}")
    print(f"factorial(5): {factorial(5)}")
```

**Step 2: Use the module**

```python
# main.py (in same directory)

import myutils

# Use functions
print(myutils.reverse_string("Python"))  # nohtyP
print(myutils.is_palindrome("A man a plan a canal Panama"))  # True
print(myutils.factorial(6))  # 720

# Access constants
print(f"Module version: {myutils.VERSION}")

# Or import specific items
from myutils import reverse_string, VERSION

print(reverse_string("Hello"))  # olleH
print(VERSION)  # 1.0.0
```

### 🟡 Intermediate: Understanding `__name__` and `__main__`

```python
# module_demo.py

print(f"Module name is: {__name__}")

def main():
    """Main function that runs tests."""
    print("Running main function...")
    print("All tests passed!")

# This block only runs when file is executed directly
# NOT when imported as a module
if __name__ == "__main__":
    main()
```

```python
# When run directly: python module_demo.py
# Output:
# Module name is: __main__
# Running main function...
# All tests passed!

# When imported: import module_demo
# Output:
# Module name is: module_demo
# (main() is NOT called)
```

### 🟡 Intermediate: Creating a Package

**Package structure:**
```
mypackage/
├── __init__.py
├── math_ops.py
├── string_ops.py
└── data_ops.py
```

**mypackage/__init__.py**
```python
"""
MyPackage - A collection of utility functions.
"""

# This runs when package is imported
print("Initializing mypackage...")

# Version info
__version__ = "1.0.0"
__author__ = "Your Name"

# Import commonly used functions to package level
from .math_ops import add, subtract
from .string_ops import reverse

# Define what gets exported with "from mypackage import *"
__all__ = ['add', 'subtract', 'reverse']
```

**mypackage/math_ops.py**
```python
"""Math operations module."""

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

**mypackage/string_ops.py**
```python
"""String operations module."""

def reverse(s):
    return s[::-1]

def capitalize_words(s):
    return ' '.join(word.capitalize() for word in s.split())

def count_vowels(s):
    return sum(1 for c in s.lower() if c in 'aeiou')
```

**Using the package:**
```python
# Method 1: Import package
import mypackage
print(mypackage.__version__)
result = mypackage.add(5, 3)

# Method 2: Import module from package
from mypackage import math_ops
result = math_ops.multiply(4, 5)

# Method 3: Import function from module
from mypackage.string_ops import reverse
print(reverse("hello"))

# Method 4: Import from package level (defined in __init__.py)
from mypackage import add, reverse
print(add(10, 20))
print(reverse("Python"))
```

### 🔴 Advanced: Module Search Path

```python
import sys

# Python searches for modules in these directories
print("Module search paths:")
for path in sys.path:
    print(f"  {path}")

# Add custom path
sys.path.append('/path/to/my/modules')

# Or use PYTHONPATH environment variable
```

### 🔴 Advanced: Relative Imports (Inside Packages)

```
myproject/
├── main.py
└── mypackage/
    ├── __init__.py
    ├── module_a.py
    └── subpackage/
        ├── __init__.py
        └── module_b.py
```

```python
# In mypackage/subpackage/module_b.py

# Relative imports (use dots)
from . import another_module        # Same directory
from .. import module_a             # Parent directory
from ..module_a import some_func    # From parent's module
```

### 🔴 Advanced: Installing Packages with pip

```bash
# Install from PyPI
pip install numpy
pip install pandas matplotlib

# Install specific version
pip install numpy==1.21.0

# Install from requirements file
pip install -r requirements.txt

# List installed packages
pip list

# Show package info
pip show numpy

# Uninstall
pip uninstall numpy

# Upgrade
pip install --upgrade numpy

# Install in development mode (editable)
pip install -e .
```

**Creating requirements.txt:**
```
# requirements.txt
numpy>=1.21.0
pandas==2.0.0
requests>=2.28.0,<3.0.0
matplotlib
```

### 🔴 Advanced: Virtual Environments

```bash
# Create virtual environment
python -m venv myenv

# Activate (Windows)
myenv\Scripts\activate

# Activate (Mac/Linux)
source myenv/bin/activate

# Deactivate
deactivate

# Freeze installed packages
pip freeze > requirements.txt
```

---

## 📚 Essential Built-in Modules

### os Module
```python
import os

# File/directory operations
os.getcwd()              # Current directory
os.listdir('.')          # List files
os.mkdir('new_folder')   # Create directory
os.path.exists('file.txt')  # Check if exists
os.path.join('dir', 'file.txt')  # Join paths
os.environ['HOME']       # Environment variables
```

### sys Module
```python
import sys

sys.version              # Python version
sys.platform             # Operating system
sys.argv                 # Command line arguments
sys.path                 # Module search paths
sys.exit(0)              # Exit program
```

### datetime Module
```python
from datetime import datetime, date, timedelta

now = datetime.now()     # Current datetime
today = date.today()     # Current date
tomorrow = today + timedelta(days=1)
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
parsed = datetime.strptime("2024-01-15", "%Y-%m-%d")
```

### json Module
```python
import json

# To JSON string
data = {"name": "Alice", "age": 25}
json_str = json.dumps(data, indent=2)

# From JSON string
parsed = json.loads(json_str)

# To/from file
with open('data.json', 'w') as f:
    json.dump(data, f)

with open('data.json', 'r') as f:
    loaded = json.load(f)
```

### collections Module
```python
from collections import Counter, defaultdict, namedtuple, deque

# Counter
count = Counter("hello")  # {'l': 2, 'h': 1, 'e': 1, 'o': 1}

# defaultdict
dd = defaultdict(list)
dd['key'].append(1)

# namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)

# deque (efficient appends/pops from both ends)
d = deque([1, 2, 3])
d.appendleft(0)
d.append(4)
```

### itertools Module
```python
from itertools import count, cycle, repeat, chain, combinations, permutations

# Infinite iterators
for i in count(10):     # 10, 11, 12, ... (infinite!)
    if i > 15: break
    print(i)

# Combinations and permutations
list(combinations('ABC', 2))  # [('A','B'), ('A','C'), ('B','C')]
list(permutations('AB', 2))   # [('A','B'), ('B','A')]

# Chain iterables
list(chain([1,2], [3,4]))  # [1, 2, 3, 4]
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Circular Imports

```python
# module_a.py
from module_b import func_b  # Imports module_b
def func_a():
    return func_b()

# module_b.py
from module_a import func_a  # Imports module_a (circular!)
def func_b():
    return func_a()

# SOLUTION: Import inside function or restructure
```

### ❌ Mistake 2: Shadowing Module Names

```python
# DON'T name your file: math.py, random.py, json.py
# It will shadow the built-in modules!

# Your math.py will be imported instead of built-in math!
import math  # Imports YOUR math.py, not Python's math
```

### ❌ Mistake 3: Using `from module import *`

```python
# BAD - pollutes namespace, unclear what's imported
from math import *
from numpy import *  # May override math functions!

# GOOD - explicit is better
from math import sqrt, pi
import numpy as np
```

### ❌ Mistake 4: Forgetting `__init__.py`

```python
# In Python 3.3+, __init__.py is optional but recommended
# Without it, you might have issues with imports

# Always create __init__.py for clarity
```

---

## 💡 Pro Tips

### 1. Use Type Hints with Modules
```python
# mymodule.py
from typing import List, Optional

def process_items(items: List[str]) -> Optional[str]:
    if not items:
        return None
    return items[0]
```

### 2. Lazy Import for Performance
```python
def heavy_function():
    # Import only when needed
    import numpy as np  # Heavy module
    return np.array([1, 2, 3])
```

### 3. Use `__all__` for Clean Exports
```python
# mymodule.py
__all__ = ['public_function', 'PublicClass']

def public_function():
    pass

def _private_function():
    pass
```

### 4. Document Your Modules
```python
"""
Module Name
===========

Description of what this module does.

Usage:
    from mymodule import function
    result = function(arg)

Functions:
    function(arg) - Does something useful
"""
```

---

## 🔍 Real-world Use Cases

### 1. Configuration Module
```python
# config.py
import os

DEBUG = os.environ.get('DEBUG', 'False').lower() == 'true'
DATABASE_URL = os.environ.get('DATABASE_URL', 'sqlite:///app.db')
SECRET_KEY = os.environ.get('SECRET_KEY', 'dev-key')
MAX_CONNECTIONS = int(os.environ.get('MAX_CONNECTIONS', '10'))
```

### 2. Utility Package
```python
# utils/__init__.py
from .string_utils import clean, normalize
from .file_utils import read_json, write_json
from .date_utils import parse_date, format_date

__all__ = ['clean', 'normalize', 'read_json', 'write_json', 
           'parse_date', 'format_date']
```

### 3. Plugin System
```python
import importlib

def load_plugin(plugin_name):
    """Dynamically load a plugin module."""
    module = importlib.import_module(f'plugins.{plugin_name}')
    return module.Plugin()
```

---

## 💼 Interview Insight

### Q1: What is the difference between a module and a package?
> Module is a single `.py` file. Package is a directory containing multiple modules and `__init__.py`.

### Q2: What is `__init__.py` for?
> It marks a directory as a Python package and can initialize package-level code/imports.

### Q3: What is `if __name__ == "__main__":`?
> Code that only runs when the file is executed directly, not when imported.

### Q4: How does Python find modules?
> Searches in: current directory → PYTHONPATH → installation-dependent default paths.

### Q5: What is the difference between relative and absolute imports?
> Absolute: `from mypackage.module import func`
> Relative: `from .module import func` (within same package)

---

## 🧪 Practice Questions

### Easy:
1. Import the `random` module and generate a random number
2. Create a simple module with two functions and import them
3. Use `from module import` to import specific functions

### Medium:
4. Create a package with two modules and use `__init__.py`
5. Use `if __name__ == "__main__"` properly
6. Handle circular import issues

### Hard:
7. Create a plugin system using dynamic imports
8. Implement lazy loading for heavy modules
9. Create a requirements.txt and set up virtual environment

---

## 📖 Summary

| Concept | Description |
|---------|-------------|
| Module | Single `.py` file |
| Package | Directory with `__init__.py` |
| `import` | Import entire module |
| `from import` | Import specific items |
| `as` | Create alias |
| `__name__` | Module's name (`__main__` if run directly) |
| `__all__` | Define public API |
| pip | Package installer |
| venv | Virtual environment |

---

## ⏭️ Next Module

Congratulations! You've completed **Module 3: Functions and Modules**! 🎉

Next, move to:

**[Module 4: Data Structures](../04_Data_Structures/README.md)**
- Lists
- Tuples
- Sets
- Dictionaries

---

*Modules: Write once, use everywhere!* 📦
