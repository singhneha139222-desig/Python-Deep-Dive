# 📘 Classes and Objects in Python

## 📌 Introduction

A **class** is a blueprint for creating objects. An **object** is an instance of a class with its own data.

Think of it like this:
- **Class** = Cookie cutter (defines the shape)
- **Object** = Individual cookie (actual thing you can use)

```
┌─────────────────────────────────────────────────────────────┐
│                    CLASS vs OBJECT                           │
│                                                              │
│   CLASS (Blueprint)          OBJECT (Instance)              │
│   ┌─────────────────┐        ┌─────────────────┐           │
│   │     Dog         │        │   my_dog        │           │
│   ├─────────────────┤   →    ├─────────────────┤           │
│   │ name            │        │ name = "Buddy"  │           │
│   │ age             │        │ age = 3         │           │
│   ├─────────────────┤        ├─────────────────┤           │
│   │ bark()          │        │ bark()          │           │
│   │ eat()           │        │ eat()           │           │
│   └─────────────────┘        └─────────────────┘           │
│                                                              │
│   One class → Many objects                                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Class Components

| Component | Description | Example |
|-----------|-------------|---------|
| Class | Blueprint/template | `class Dog:` |
| Object | Instance of class | `my_dog = Dog()` |
| Attribute | Data/variables | `self.name = "Buddy"` |
| Method | Functions in class | `def bark(self):` |
| Constructor | Initializer | `__init__()` |
| `self` | Reference to instance | First param in methods |

### The `self` Parameter

`self` refers to the current instance of the class. It's automatically passed but must be explicitly declared.

```python
class Dog:
    def bark(self):  # self is the instance
        print(f"{self.name} barks!")

dog1 = Dog()
dog1.name = "Buddy"
dog1.bark()  # self = dog1
```

---

## 💻 Code Examples

### 🟢 Basic: Creating a Simple Class

```python
# Define a class
class Dog:
    pass  # Empty class

# Create objects (instances)
dog1 = Dog()
dog2 = Dog()

# Objects are independent
print(dog1)  # <__main__.Dog object at 0x...>
print(dog1 == dog2)  # False - different objects

# Add attributes dynamically
dog1.name = "Buddy"
dog1.age = 3
dog2.name = "Max"
dog2.age = 5

print(f"{dog1.name} is {dog1.age} years old")  # Buddy is 3 years old
```

### 🟢 Basic: The `__init__` Constructor

```python
class Dog:
    # Constructor - called when object is created
    def __init__(self, name, age):
        self.name = name  # Instance attribute
        self.age = age
        print(f"Created dog: {name}")

# Create objects with arguments
dog1 = Dog("Buddy", 3)  # Output: Created dog: Buddy
dog2 = Dog("Max", 5)    # Output: Created dog: Max

print(dog1.name)  # Buddy
print(dog2.age)   # 5
```

### 🟢 Basic: Instance Methods

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    # Instance method
    def bark(self):
        return f"{self.name} says Woof!"
    
    def get_info(self):
        return f"{self.name} is {self.age} years old"
    
    def have_birthday(self):
        self.age += 1
        return f"{self.name} is now {self.age}!"

# Using methods
dog = Dog("Buddy", 3)
print(dog.bark())         # Buddy says Woof!
print(dog.get_info())     # Buddy is 3 years old
print(dog.have_birthday())  # Buddy is now 4!
print(dog.get_info())     # Buddy is 4 years old
```

### 🟡 Intermediate: Class vs Instance Attributes

```python
class Dog:
    # Class attribute - shared by ALL instances
    species = "Canis familiaris"
    count = 0
    
    def __init__(self, name, age):
        # Instance attributes - unique to each instance
        self.name = name
        self.age = age
        Dog.count += 1  # Increment class attribute
    
    def describe(self):
        return f"{self.name} is a {Dog.species}"

# Create dogs
dog1 = Dog("Buddy", 3)
dog2 = Dog("Max", 5)

# Class attributes
print(Dog.species)      # Canis familiaris
print(Dog.count)        # 2
print(dog1.species)     # Canis familiaris (accessed via instance)

# Instance attributes
print(dog1.name)        # Buddy
print(dog2.name)        # Max

# Modifying class attribute
Dog.species = "Canine"
print(dog1.species)     # Canine (all instances affected)
print(dog2.species)     # Canine

# Be careful with mutable class attributes!
class BadExample:
    items = []  # Shared mutable - dangerous!

class GoodExample:
    def __init__(self):
        self.items = []  # Each instance gets own list
```

### 🟡 Intermediate: Methods Types

```python
class MyClass:
    class_var = "I'm a class variable"
    
    def __init__(self, value):
        self.instance_var = value
    
    # Instance method - has access to instance (self)
    def instance_method(self):
        return f"Instance method, value: {self.instance_var}"
    
    # Class method - has access to class (cls)
    @classmethod
    def class_method(cls):
        return f"Class method, class_var: {cls.class_var}"
    
    # Static method - no access to class or instance
    @staticmethod
    def static_method(x, y):
        return f"Static method, sum: {x + y}"

# Usage
obj = MyClass(42)

# Instance method
print(obj.instance_method())  # Instance method, value: 42

# Class method
print(MyClass.class_method())  # Class method, class_var: I'm a class variable
print(obj.class_method())      # Also works on instance

# Static method
print(MyClass.static_method(3, 5))  # Static method, sum: 8
print(obj.static_method(3, 5))      # Also works on instance
```

### 🟡 Intermediate: Alternative Constructors

```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    
    def __str__(self):
        return f"{self.year}-{self.month:02d}-{self.day:02d}"
    
    # Alternative constructor from string
    @classmethod
    def from_string(cls, date_string):
        year, month, day = map(int, date_string.split("-"))
        return cls(year, month, day)
    
    # Alternative constructor for today
    @classmethod
    def today(cls):
        import datetime
        today = datetime.date.today()
        return cls(today.year, today.month, today.day)

# Different ways to create Date objects
d1 = Date(2024, 6, 15)
d2 = Date.from_string("2024-12-25")
d3 = Date.today()

print(d1)  # 2024-06-15
print(d2)  # 2024-12-25
print(d3)  # Today's date
```

### 🔴 Advanced: Magic Methods (Dunder Methods)

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation for users
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    
    # String representation for developers
    def __repr__(self):
        return f"Point(x={self.x}, y={self.y})"
    
    # Addition
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    # Subtraction
    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)
    
    # Equality
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    # Less than (for sorting)
    def __lt__(self, other):
        return (self.x, self.y) < (other.x, other.y)
    
    # Length/magnitude
    def __len__(self):
        return int((self.x**2 + self.y**2)**0.5)
    
    # Boolean value
    def __bool__(self):
        return self.x != 0 or self.y != 0

# Usage
p1 = Point(3, 4)
p2 = Point(1, 2)

print(str(p1))      # Point(3, 4)
print(repr(p1))     # Point(x=3, y=4)
print(p1 + p2)      # Point(4, 6)
print(p1 - p2)      # Point(2, 2)
print(p1 == p2)     # False
print(len(p1))      # 5 (magnitude)
print(bool(p1))     # True
print(bool(Point(0, 0)))  # False
```

### 🔴 Advanced: Property Decorators

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    # Getter
    @property
    def radius(self):
        return self._radius
    
    # Setter
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
    
    # Deleter
    @radius.deleter
    def radius(self):
        print("Deleting radius")
        del self._radius
    
    # Computed property (read-only)
    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2
    
    @property
    def diameter(self):
        return self._radius * 2

# Usage
c = Circle(5)
print(c.radius)    # 5 (getter)
print(c.area)      # 78.54... (computed)
print(c.diameter)  # 10

c.radius = 10      # Setter
print(c.area)      # 314.15...

# c.radius = -5    # ValueError: Radius cannot be negative
# c.area = 50      # AttributeError: can't set (no setter)
```

### 🔴 Advanced: Data Classes (Python 3.7+)

```python
from dataclasses import dataclass, field
from typing import List

# Without dataclass
class PersonOld:
    def __init__(self, name, age, email):
        self.name = name
        self.age = age
        self.email = email
    
    def __repr__(self):
        return f"Person(name={self.name}, age={self.age}, email={self.email})"
    
    def __eq__(self, other):
        return (self.name, self.age, self.email) == (other.name, other.age, other.email)

# With dataclass - much cleaner!
@dataclass
class Person:
    name: str
    age: int
    email: str = "no@email.com"  # Default value

# Auto-generated: __init__, __repr__, __eq__
p1 = Person("Alice", 25)
p2 = Person("Alice", 25)

print(p1)          # Person(name='Alice', age=25, email='no@email.com')
print(p1 == p2)    # True

# Advanced dataclass
@dataclass(frozen=True)  # Immutable
class Point:
    x: float
    y: float

@dataclass
class Student:
    name: str
    grades: List[int] = field(default_factory=list)  # Mutable default
    
    @property
    def average(self):
        return sum(self.grades) / len(self.grades) if self.grades else 0

s = Student("Alice")
s.grades.extend([90, 85, 92])
print(s.average)  # 89.0
```

---

## 📊 Magic Methods Reference

| Method | Trigger | Example |
|--------|---------|---------|
| `__init__` | Object creation | `obj = MyClass()` |
| `__str__` | `str()`, `print()` | `print(obj)` |
| `__repr__` | `repr()`, debugging | `repr(obj)` |
| `__len__` | `len()` | `len(obj)` |
| `__bool__` | `bool()`, conditions | `if obj:` |
| `__eq__` | `==` | `obj1 == obj2` |
| `__lt__` | `<` | `obj1 < obj2` |
| `__add__` | `+` | `obj1 + obj2` |
| `__getitem__` | `[]` | `obj[key]` |
| `__setitem__` | `[] =` | `obj[key] = val` |
| `__iter__` | `for x in obj` | Iteration |
| `__call__` | `()` | `obj()` |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting `self`

```python
# WRONG
class Dog:
    def bark():  # Missing self!
        return "Woof!"

# dog.bark()  # TypeError!

# CORRECT
class Dog:
    def bark(self):
        return "Woof!"
```

### ❌ Mistake 2: Modifying Class Attribute via Instance

```python
# WRONG
class Counter:
    count = 0

c1 = Counter()
c1.count += 1  # Creates instance attribute, doesn't modify class!
print(Counter.count)  # 0 (unchanged)

# CORRECT
class Counter:
    count = 0
    
    def increment(self):
        Counter.count += 1  # Explicitly use class name
```

### ❌ Mistake 3: Mutable Default Arguments

```python
# WRONG
class Team:
    def __init__(self, members=[]):  # Dangerous!
        self.members = members

t1 = Team()
t1.members.append("Alice")
t2 = Team()
print(t2.members)  # ['Alice'] - Shared!

# CORRECT
class Team:
    def __init__(self, members=None):
        self.members = members if members else []
```

---

## 💡 Pro Tips

### 1. Use `__slots__` for Memory Efficiency

```python
class Point:
    __slots__ = ['x', 'y']  # Restricts attributes, saves memory
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

### 2. Use `@classmethod` for Factory Methods

```python
class User:
    @classmethod
    def from_json(cls, json_str):
        data = json.loads(json_str)
        return cls(**data)
```

### 3. Use Properties Instead of Getters/Setters

```python
# Instead of: obj.get_value(), obj.set_value(x)
# Use: obj.value (property)
```

---

## 💼 Interview Insight

### Q1: What is `self` in Python?

> `self` is a reference to the current instance of the class. It's used to access instance attributes and methods. Python passes it automatically but it must be declared explicitly.

### Q2: Difference between class and instance attributes?

> - **Class attributes**: Shared by all instances, defined outside `__init__`
> - **Instance attributes**: Unique to each instance, defined in `__init__` using `self`

### Q3: What is the purpose of `__init__`?

> `__init__` is the constructor method that initializes a new object. It's called automatically when an object is created. It's not the object creator (that's `__new__`), but the initializer.

---

## 🧪 Practice Questions

### Easy:
1. Create a `Rectangle` class with `width` and `height`
2. Add a method to calculate area
3. Create a `BankAccount` with deposit/withdraw methods

### Medium:
4. Create a `Book` class with comparison methods
5. Implement a `Stack` class with push/pop
6. Create a `Temperature` class that converts C to F

### Hard:
7. Implement a `Vector` class with full operator overloading
8. Create a `LinkedList` class with iteration support
9. Build a `Person` class with proper comparison and hashing

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Class | Blueprint with `class ClassName:` |
| Object | Instance with `obj = ClassName()` |
| `__init__` | Constructor for initialization |
| `self` | Reference to current instance |
| Methods | Functions that operate on instance |
| Attributes | Data stored in object |
| Magic Methods | Special `__method__` names |
| Properties | Controlled attribute access |

---

## ⏭️ Next Topic

**[Inheritance](./02_Inheritance.md)** — Building class hierarchies!

---

*Classes: The building blocks of OOP!* 🏗️
