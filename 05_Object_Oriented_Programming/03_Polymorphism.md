# 📘 Polymorphism in Python

## 📌 Introduction

**Polymorphism** means "many forms." It allows objects of different classes to be treated as objects of a common type, with each responding differently to the same method call.

Think of it like a **universal remote** — the "power" button works on any device (TV, AC, sound system), but each device responds differently.

```
┌─────────────────────────────────────────────────────────────┐
│                     POLYMORPHISM                             │
│                                                              │
│   Same interface, different behaviors                        │
│                                                              │
│      speak()              speak()              speak()       │
│         ↓                    ↓                    ↓         │
│   ┌─────────┐          ┌─────────┐          ┌─────────┐    │
│   │   Dog   │          │   Cat   │          │   Cow   │    │
│   │ "Woof!" │          │ "Meow!" │          │ "Moo!"  │    │
│   └─────────┘          └─────────┘          └─────────┘    │
│                                                              │
│   One method name, different implementations                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Types of Polymorphism

| Type | Description | Python Implementation |
|------|-------------|----------------------|
| Duck Typing | If it walks like a duck... | Default behavior |
| Method Overriding | Child replaces parent method | Inheritance |
| Operator Overloading | `+` means different things | Magic methods |
| Method Overloading | Same name, different params | Not native (use defaults) |

### Duck Typing Philosophy

> "If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck."

Python doesn't care about the TYPE of an object, only if it has the required METHOD.

---

## 💻 Code Examples

### 🟢 Basic: Method Overriding (Classic Polymorphism)

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some generic sound"

class Dog(Animal):
    def speak(self):  # Override
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):  # Override
        return f"{self.name} says Meow!"

class Cow(Animal):
    def speak(self):  # Override
        return f"{self.name} says Moo!"

# Polymorphism in action
animals = [Dog("Buddy"), Cat("Whiskers"), Cow("Bessie")]

for animal in animals:
    print(animal.speak())  # Same method, different behavior

# Output:
# Buddy says Woof!
# Whiskers says Meow!
# Bessie says Moo!

# Function that works with any animal
def make_all_speak(animals):
    for animal in animals:
        print(animal.speak())

make_all_speak(animals)
```

### 🟢 Basic: Duck Typing

```python
# No inheritance needed!
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class Robot:
    def speak(self):
        return "Beep boop!"

# All have speak() method - that's all that matters
def make_sound(entity):
    return entity.speak()

# Works with any object that has speak()
print(make_sound(Dog()))    # Woof!
print(make_sound(Cat()))    # Meow!
print(make_sound(Robot()))  # Beep boop!

# Even works with custom objects
class Alien:
    def speak(self):
        return "Greetings, earthling!"

print(make_sound(Alien()))  # Greetings, earthling!
```

### 🟢 Basic: Polymorphism with Built-in Functions

```python
# len() is polymorphic
print(len("Hello"))      # 5 (string)
print(len([1, 2, 3]))    # 3 (list)
print(len({"a": 1}))     # 1 (dict)

# + is polymorphic (operator overloading)
print(5 + 3)             # 8 (addition)
print("Hello" + "World") # HelloWorld (concatenation)
print([1, 2] + [3, 4])   # [1, 2, 3, 4] (list concat)

# iter() is polymorphic
for item in [1, 2, 3]:      # List iteration
    print(item)

for char in "abc":          # String iteration
    print(char)

for key in {"a": 1, "b": 2}: # Dict iteration
    print(key)
```

### 🟡 Intermediate: Operator Overloading

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # Addition
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    # Subtraction
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    # Multiplication (scalar)
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
    
    # Right multiplication (scalar * vector)
    def __rmul__(self, scalar):
        return self.__mul__(scalar)
    
    # Equality
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    # String representation
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    # Absolute value (magnitude)
    def __abs__(self):
        return (self.x**2 + self.y**2)**0.5
    
    # Negation
    def __neg__(self):
        return Vector(-self.x, -self.y)

# Usage
v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(v1 + v2)      # Vector(4, 6)
print(v1 - v2)      # Vector(2, 2)
print(v1 * 2)       # Vector(6, 8)
print(3 * v1)       # Vector(9, 12)
print(abs(v1))      # 5.0
print(-v1)          # Vector(-3, -4)
print(v1 == Vector(3, 4))  # True
```

### 🟡 Intermediate: Comparison Operators

```python
from functools import total_ordering

@total_ordering  # Fills in other comparisons from __eq__ and __lt__
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
    
    def __eq__(self, other):
        return self.grade == other.grade
    
    def __lt__(self, other):
        return self.grade < other.grade
    
    def __str__(self):
        return f"{self.name}: {self.grade}"

students = [
    Student("Alice", 85),
    Student("Bob", 92),
    Student("Charlie", 78)
]

# Sorting works because we defined comparison
students.sort()
for s in students:
    print(s)
# Output:
# Charlie: 78
# Alice: 85
# Bob: 92

# All comparisons work
alice = Student("Alice", 85)
bob = Student("Bob", 92)
print(alice < bob)   # True
print(alice > bob)   # False (from total_ordering)
print(alice <= bob)  # True (from total_ordering)
```

### 🟡 Intermediate: Protocol-Based Polymorphism

```python
# Iterable protocol - implement __iter__
class Countdown:
    def __init__(self, start):
        self.start = start
    
    def __iter__(self):
        n = self.start
        while n > 0:
            yield n
            n -= 1

for num in Countdown(5):
    print(num)  # 5, 4, 3, 2, 1

# Context manager protocol - implement __enter__ and __exit__
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()

# Callable protocol - implement __call__
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, value):
        return value * self.factor

double = Multiplier(2)
triple = Multiplier(3)

print(double(5))   # 10
print(triple(5))   # 15
```

### 🔴 Advanced: Abstract Methods for Polymorphism

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass
    
    # Concrete method using abstract methods
    def describe(self):
        return f"Area: {self.area():.2f}, Perimeter: {self.perimeter():.2f}"

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

# Polymorphic function
def print_shape_info(shape: Shape):
    print(f"Type: {type(shape).__name__}")
    print(shape.describe())
    print()

shapes = [Rectangle(5, 3), Circle(7)]

for shape in shapes:
    print_shape_info(shape)

# Output:
# Type: Rectangle
# Area: 15.00, Perimeter: 16.00
#
# Type: Circle
# Area: 153.94, Perimeter: 43.98
```

### 🔴 Advanced: Simulating Method Overloading

```python
# Python doesn't have true method overloading
# But we can simulate it with:

# 1. Default arguments
class Calculator:
    def add(self, a, b=0, c=0):
        return a + b + c

calc = Calculator()
print(calc.add(5))        # 5
print(calc.add(5, 3))     # 8
print(calc.add(5, 3, 2))  # 10

# 2. *args
class Calculator2:
    def add(self, *args):
        return sum(args)

calc2 = Calculator2()
print(calc2.add(1))           # 1
print(calc2.add(1, 2))        # 3
print(calc2.add(1, 2, 3, 4))  # 10

# 3. singledispatch for type-based dispatch
from functools import singledispatch

@singledispatch
def process(data):
    return f"Unknown type: {type(data)}"

@process.register(int)
def _(data):
    return f"Processing integer: {data * 2}"

@process.register(str)
def _(data):
    return f"Processing string: {data.upper()}"

@process.register(list)
def _(data):
    return f"Processing list of {len(data)} items"

print(process(5))           # Processing integer: 10
print(process("hello"))     # Processing string: HELLO
print(process([1, 2, 3]))   # Processing list of 3 items
```

### 🔴 Advanced: Method Dispatch with singledispatchmethod

```python
from functools import singledispatchmethod

class Formatter:
    @singledispatchmethod
    def format(self, data):
        return str(data)
    
    @format.register(int)
    def _(self, data):
        return f"{data:,}"  # Number with commas
    
    @format.register(float)
    def _(self, data):
        return f"{data:.2f}"  # 2 decimal places
    
    @format.register(list)
    def _(self, data):
        return " | ".join(str(x) for x in data)
    
    @format.register(dict)
    def _(self, data):
        return ", ".join(f"{k}={v}" for k, v in data.items())

fmt = Formatter()
print(fmt.format(1000000))       # 1,000,000
print(fmt.format(3.14159))       # 3.14
print(fmt.format([1, 2, 3]))     # 1 | 2 | 3
print(fmt.format({"a": 1}))      # a=1
print(fmt.format("hello"))       # hello
```

---

## 📊 Operator Overloading Reference

| Operator | Magic Method | Example |
|----------|-------------|---------|
| `+` | `__add__` | `a + b` |
| `-` | `__sub__` | `a - b` |
| `*` | `__mul__` | `a * b` |
| `/` | `__truediv__` | `a / b` |
| `//` | `__floordiv__` | `a // b` |
| `%` | `__mod__` | `a % b` |
| `**` | `__pow__` | `a ** b` |
| `==` | `__eq__` | `a == b` |
| `!=` | `__ne__` | `a != b` |
| `<` | `__lt__` | `a < b` |
| `<=` | `__le__` | `a <= b` |
| `>` | `__gt__` | `a > b` |
| `>=` | `__ge__` | `a >= b` |
| `[]` | `__getitem__` | `a[key]` |
| `in` | `__contains__` | `x in a` |
| `len()` | `__len__` | `len(a)` |
| `str()` | `__str__` | `str(a)` |
| `bool()` | `__bool__` | `bool(a)` |
| `()` | `__call__` | `a()` |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Type Checking Instead of Duck Typing

```python
# WRONG - checking types
def process(data):
    if isinstance(data, list):
        return sum(data)
    elif isinstance(data, str):
        return len(data)
    else:
        raise TypeError("Unsupported type")

# CORRECT - duck typing
def process(data):
    try:
        return sum(data)  # Try as iterable
    except TypeError:
        return len(data)  # Try as sizeable

# BEST - use protocols/interfaces
from typing import Protocol

class Summable(Protocol):
    def __iter__(self): ...

def process_summable(data: Summable):
    return sum(data)
```

### ❌ Mistake 2: Forgetting `__radd__` for Commutative Operations

```python
# INCOMPLETE
class Money:
    def __init__(self, amount):
        self.amount = amount
    
    def __add__(self, other):
        return Money(self.amount + other.amount)

m = Money(100)
# m + 50  # Works if 50 is Money, fails if int

# COMPLETE
class Money:
    def __init__(self, amount):
        self.amount = amount
    
    def __add__(self, other):
        if isinstance(other, Money):
            return Money(self.amount + other.amount)
        return Money(self.amount + other)
    
    def __radd__(self, other):  # For: 50 + Money(100)
        return self.__add__(other)
```

### ❌ Mistake 3: Breaking the Liskov Substitution Principle

```python
# WRONG - child breaks parent's contract
class Bird:
    def fly(self):
        return "Flying!"

class Penguin(Bird):
    def fly(self):
        raise NotImplementedError("Penguins can't fly!")  # Breaks contract!

# CORRECT - redesign hierarchy
class Bird:
    def move(self):
        pass

class FlyingBird(Bird):
    def move(self):
        return "Flying!"

class Penguin(Bird):
    def move(self):
        return "Swimming!"
```

---

## 💡 Pro Tips

### 1. Use `hasattr()` for Safe Duck Typing

```python
def safe_speak(entity):
    if hasattr(entity, 'speak'):
        return entity.speak()
    return "Cannot speak"
```

### 2. Use `@total_ordering` for Comparisons

```python
from functools import total_ordering

@total_ordering
class Item:
    def __eq__(self, other): ...
    def __lt__(self, other): ...
    # All other comparisons auto-generated!
```

### 3. Make Classes Callable When Appropriate

```python
class Greeting:
    def __init__(self, template):
        self.template = template
    
    def __call__(self, name):
        return self.template.format(name=name)

hello = Greeting("Hello, {name}!")
print(hello("Alice"))  # Hello, Alice!
```

---

## 💼 Interview Insight

### Q1: What is polymorphism in Python?

> Polymorphism allows objects of different types to be treated uniformly through a common interface. Python achieves this through:
> - Duck typing (structural polymorphism)
> - Method overriding (inheritance-based)
> - Operator overloading (magic methods)

### Q2: What is duck typing?

> Duck typing means Python doesn't check an object's type, only whether it has the required methods/attributes. "If it walks like a duck and quacks like a duck, it's a duck."

### Q3: Can you have method overloading in Python?

> Python doesn't support traditional method overloading (same name, different signatures). Instead:
> - Use default arguments
> - Use `*args` and `**kwargs`
> - Use `@singledispatch` for type-based dispatch

---

## 🧪 Practice Questions

### Easy:
1. Create classes with a common `draw()` method
2. Overload `+` for a custom class
3. Implement `__str__` and `__repr__`

### Medium:
4. Create a `Fraction` class with full arithmetic
5. Implement a `Matrix` class with operators
6. Use `singledispatch` for type handling

### Hard:
7. Build a plugin system using polymorphism
8. Implement a full numeric class (like `complex`)
9. Create a polymorphic data serializer

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Duck Typing | Check behavior, not type |
| Override | Child redefines parent method |
| Operators | `__add__`, `__eq__`, etc. |
| Protocols | Implement required methods |
| `singledispatch` | Type-based function dispatch |
| Abstract Methods | Define interface contracts |

---

## ⏭️ Next Topic

**[Encapsulation](./04_Encapsulation.md)** — Protecting your data!

---

*Polymorphism: Many forms, one interface!* 🎭
