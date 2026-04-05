# 📘 Inheritance in Python

## 📌 Introduction

**Inheritance** allows a class to inherit attributes and methods from another class. This promotes code reuse and establishes relationships between classes.

Think of it like a **family tree** — children inherit traits from parents but can also have their own unique characteristics.

```
┌─────────────────────────────────────────────────────────────┐
│                     INHERITANCE                              │
│                                                              │
│               ┌───────────────┐                             │
│               │    Animal     │  ← Parent/Base/Super        │
│               │   - name      │                             │
│               │   - eat()     │                             │
│               └───────┬───────┘                             │
│                       │                                      │
│           ┌───────────┴───────────┐                         │
│           ↓                       ↓                         │
│   ┌───────────────┐       ┌───────────────┐                │
│   │     Dog       │       │     Cat       │ ← Child/Derived│
│   │   - breed     │       │   - indoor    │                │
│   │   - bark()    │       │   - meow()    │                │
│   └───────────────┘       └───────────────┘                │
│                                                              │
│   Child inherits: name, eat()                               │
│   Child adds: breed/indoor, bark()/meow()                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Terminology

| Term | Meaning |
|------|---------|
| Parent/Base/Super class | The class being inherited from |
| Child/Derived/Sub class | The class that inherits |
| `super()` | Function to call parent methods |
| Method Override | Redefine parent method in child |
| MRO | Method Resolution Order |

### Types of Inheritance

```
1. Single       →  Child(Parent)
2. Multiple     →  Child(Parent1, Parent2)
3. Multilevel   →  Child(Parent) → GrandChild(Child)
4. Hierarchical →  Child1(Parent), Child2(Parent)
5. Hybrid       →  Combination of above
```

---

## 💻 Code Examples

### 🟢 Basic: Single Inheritance

```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def eat(self):
        return f"{self.name} is eating"
    
    def sleep(self):
        return f"{self.name} is sleeping"

# Child class inherits from Animal
class Dog(Animal):
    def bark(self):
        return f"{self.name} says Woof!"

# Child class
class Cat(Animal):
    def meow(self):
        return f"{self.name} says Meow!"

# Create objects
dog = Dog("Buddy")
cat = Cat("Whiskers")

# Inherited methods
print(dog.eat())    # Buddy is eating
print(dog.sleep())  # Buddy is sleeping
print(dog.bark())   # Buddy says Woof!

print(cat.eat())    # Whiskers is eating
print(cat.meow())   # Whiskers says Meow!

# Check inheritance
print(isinstance(dog, Dog))     # True
print(isinstance(dog, Animal))  # True
print(issubclass(Dog, Animal))  # True
```

### 🟢 Basic: Extending `__init__`

```python
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Dog(Animal):
    def __init__(self, name, age, breed):
        # Call parent's __init__
        super().__init__(name, age)
        # Add child-specific attribute
        self.breed = breed
    
    def info(self):
        return f"{self.name} is a {self.age} year old {self.breed}"

dog = Dog("Buddy", 3, "Labrador")
print(dog.name)    # Buddy
print(dog.age)     # 3
print(dog.breed)   # Labrador
print(dog.info())  # Buddy is a 3 year old Labrador
```

### 🟡 Intermediate: Method Overriding

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"
    
    def describe(self):
        return f"I am {self.name}"

class Dog(Animal):
    # Override speak method
    def speak(self):
        return f"{self.name} says Woof!"

class Cat(Animal):
    # Override speak method
    def speak(self):
        return f"{self.name} says Meow!"

class Cow(Animal):
    # Override speak and describe
    def speak(self):
        return f"{self.name} says Moo!"
    
    def describe(self):
        # Call parent's describe and extend
        return super().describe() + " and I give milk"

# Polymorphism in action
animals = [Dog("Buddy"), Cat("Whiskers"), Cow("Bessie")]

for animal in animals:
    print(animal.speak())
    print(animal.describe())
    print()

# Output:
# Buddy says Woof!
# I am Buddy
#
# Whiskers says Meow!
# I am Whiskers
#
# Bessie says Moo!
# I am Bessie and I give milk
```

### 🟡 Intermediate: The `super()` Function

```python
class A:
    def __init__(self):
        print("A init")
    
    def method(self):
        print("A method")

class B(A):
    def __init__(self):
        super().__init__()  # Call A's __init__
        print("B init")
    
    def method(self):
        super().method()    # Call A's method
        print("B method")

b = B()
# Output:
# A init
# B init

b.method()
# Output:
# A method
# B method

# super() with arguments (older style)
class C(A):
    def __init__(self):
        super(C, self).__init__()  # Equivalent to super().__init__()
```

### 🔴 Advanced: Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "I can fly!"

class Swimmable:
    def swim(self):
        return "I can swim!"

class Walkable:
    def walk(self):
        return "I can walk!"

# Multiple inheritance
class Duck(Flyable, Swimmable, Walkable):
    def __init__(self, name):
        self.name = name
    
    def quack(self):
        return f"{self.name} says Quack!"

class Penguin(Swimmable, Walkable):
    def __init__(self, name):
        self.name = name

# Usage
duck = Duck("Donald")
print(duck.fly())    # I can fly!
print(duck.swim())   # I can swim!
print(duck.walk())   # I can walk!
print(duck.quack())  # Donald says Quack!

penguin = Penguin("Pingu")
print(penguin.swim())  # I can swim!
print(penguin.walk())  # I can walk!
# penguin.fly()       # AttributeError - penguins can't fly!
```

### 🔴 Advanced: The Diamond Problem & MRO

```python
class A:
    def method(self):
        print("A.method")

class B(A):
    def method(self):
        print("B.method")
        super().method()

class C(A):
    def method(self):
        print("C.method")
        super().method()

class D(B, C):  # Diamond inheritance
    def method(self):
        print("D.method")
        super().method()

# Method Resolution Order (MRO)
print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

print(D.mro())  # Same result

d = D()
d.method()
# Output:
# D.method
# B.method
# C.method
# A.method

# MRO ensures each class's method is called only once!
```

### 🔴 Advanced: Mixins

```python
# Mixins are small classes that provide specific functionality

class JSONMixin:
    """Add JSON serialization capability"""
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

class LoggingMixin:
    """Add logging capability"""
    def log(self, message):
        print(f"[{self.__class__.__name__}] {message}")

class ValidatorMixin:
    """Add validation capability"""
    def validate(self):
        for key, value in self.__dict__.items():
            if value is None:
                raise ValueError(f"{key} cannot be None")
        return True

# Use mixins
class User(JSONMixin, LoggingMixin, ValidatorMixin):
    def __init__(self, name, email):
        self.name = name
        self.email = email

user = User("Alice", "alice@email.com")
print(user.to_json())    # {"name": "Alice", "email": "alice@email.com"}
user.log("User created") # [User] User created
user.validate()          # True

# Mixins allow adding functionality without deep inheritance
```

### 🔴 Advanced: Abstract Base Class Inheritance

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass
    
    def describe(self):
        return f"Area: {self.area()}, Perimeter: {self.perimeter()}"

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

# Cannot instantiate abstract class
# shape = Shape()  # TypeError!

rect = Rectangle(5, 3)
circle = Circle(7)

print(rect.describe())    # Area: 15, Perimeter: 16
print(circle.describe())  # Area: 153.93..., Perimeter: 43.98...
```

---

## 📊 Inheritance Reference

| Type | Syntax | Description |
|------|--------|-------------|
| Single | `class B(A)` | One parent |
| Multiple | `class C(A, B)` | Multiple parents |
| Multilevel | A → B → C | Chain of inheritance |
| `super()` | `super().method()` | Call parent method |
| Override | Define same method | Replace parent method |
| `isinstance()` | `isinstance(obj, Class)` | Check instance |
| `issubclass()` | `issubclass(Child, Parent)` | Check relationship |
| MRO | `Class.__mro__` | Method Resolution Order |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting `super().__init__()`

```python
# WRONG
class Parent:
    def __init__(self, name):
        self.name = name

class Child(Parent):
    def __init__(self, name, age):
        self.age = age  # Parent's __init__ not called!

child = Child("Alice", 10)
# print(child.name)  # AttributeError!

# CORRECT
class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)  # Initialize parent first
        self.age = age
```

### ❌ Mistake 2: Wrong MRO in Multiple Inheritance

```python
# WRONG - inconsistent MRO
# class A(B, C): where C is parent of B
# This would cause TypeError

# CORRECT - follow MRO rules
# Parent classes should be in order from most specific to most general
class Base:
    pass

class Mixin:
    pass

class Derived(Mixin, Base):  # Mixin before Base
    pass
```

### ❌ Mistake 3: Overusing Inheritance

```python
# WRONG - deep inheritance for unrelated concepts
class Animal:
    pass

class Dog(Animal):
    pass

class RobotDog(Dog):  # Is a RobotDog really an Animal?
    pass

# BETTER - composition
class Robot:
    def process(self):
        return "Processing..."

class RobotDog:
    def __init__(self):
        self.robot = Robot()  # Has-a relationship
        self.name = "RoboDog"
    
    def bark(self):
        return f"{self.name} says Boop!"
```

---

## 💡 Pro Tips

### 1. Prefer Composition Over Inheritance

```python
# Instead of inheriting, include as attribute
class Engine:
    def start(self):
        return "Engine started"

class Car:
    def __init__(self):
        self.engine = Engine()  # Composition
    
    def start(self):
        return self.engine.start()
```

### 2. Use Mixins for Cross-Cutting Concerns

```python
# Mixins are great for adding logging, serialization, validation
class LoggableMixin:
    def log(self, msg):
        print(f"[LOG] {msg}")
```

### 3. Check MRO When Debugging

```python
print(MyClass.__mro__)
# or
print(MyClass.mro())
```

---

## 💼 Interview Insight

### Q1: What is the difference between `is-a` and `has-a`?

> - **is-a**: Inheritance relationship (Dog IS-A Animal)
> - **has-a**: Composition relationship (Car HAS-A Engine)
> - Prefer composition for flexibility

### Q2: What is the Method Resolution Order (MRO)?

> MRO is the order in which Python looks for methods in a class hierarchy. It uses C3 linearization algorithm. You can check it with `ClassName.__mro__` or `ClassName.mro()`.

### Q3: What is the Diamond Problem?

> When class D inherits from B and C, which both inherit from A, there's ambiguity about which path to use. Python's MRO solves this by ensuring each class appears once in a consistent order.

---

## 🔍 Real-world Use Cases

### 1. GUI Framework

```python
class Widget:
    def draw(self):
        pass

class Button(Widget):
    def draw(self):
        return "Drawing button"
    
    def click(self):
        return "Button clicked"

class CheckBox(Widget):
    def draw(self):
        return "Drawing checkbox"
    
    def toggle(self):
        self.checked = not getattr(self, 'checked', False)
```

### 2. Exception Hierarchy

```python
class AppError(Exception):
    """Base application error"""
    pass

class ValidationError(AppError):
    """Data validation failed"""
    pass

class DatabaseError(AppError):
    """Database operation failed"""
    pass

class ConnectionError(DatabaseError):
    """Database connection failed"""
    pass
```

### 3. Django Models

```python
# Django ORM uses inheritance extensively
class Model:
    def save(self):
        pass

class User(Model):
    name = "field"
    email = "field"

class Admin(User):
    permissions = "field"
```

---

## 🧪 Practice Questions

### Easy:
1. Create `Vehicle` class and inherit `Car` and `Bike`
2. Override a method in child class
3. Use `super()` to call parent method

### Medium:
4. Implement multiple inheritance with mixins
5. Create a class hierarchy with 3 levels
6. Handle the diamond problem

### Hard:
7. Design an exception hierarchy for an app
8. Implement a plugin system using inheritance
9. Create a ORM-like model system

---

## 📖 Summary

| Concept | Key Point |
|---------|-----------|
| Single | `class Child(Parent)` |
| Multiple | `class Child(Parent1, Parent2)` |
| `super()` | Call parent methods |
| Override | Replace parent method |
| MRO | Order of method lookup |
| `isinstance()` | Check if object is instance |
| Mixins | Small, focused additions |

---

## ⏭️ Next Topic

**[Polymorphism](./03_Polymorphism.md)** — One interface, many forms!

---

*Inheritance: Stand on the shoulders of giants!* 🏛️
