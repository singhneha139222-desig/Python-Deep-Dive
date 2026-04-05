# 📘 Module 5: Object-Oriented Programming (OOP)

Welcome to **Module 5**! Object-Oriented Programming is a paradigm that organizes code around objects rather than functions and logic. It's essential for building scalable, maintainable applications.

---

## 📌 Overview of This Module

OOP is based on the concept of "objects" which contain data (attributes) and code (methods). Python is an object-oriented language — everything is an object!

```
┌─────────────────────────────────────────────────────────────┐
│              THE FOUR PILLARS OF OOP                         │
│                                                              │
│   ┌─────────────────┐   ┌─────────────────┐                │
│   │  ENCAPSULATION  │   │   INHERITANCE   │                │
│   │                 │   │                 │                │
│   │ Hide internal   │   │ Reuse code from │                │
│   │ data, expose    │   │ parent classes  │                │
│   │ public methods  │   │                 │                │
│   └─────────────────┘   └─────────────────┘                │
│                                                              │
│   ┌─────────────────┐   ┌─────────────────┐                │
│   │  POLYMORPHISM   │   │   ABSTRACTION   │                │
│   │                 │   │                 │                │
│   │ Same interface, │   │ Hide complexity │                │
│   │ different       │   │ show essentials │                │
│   │ implementations │   │                 │                │
│   └─────────────────┘   └─────────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

| Skill | Description |
|-------|-------------|
| ✅ Classes & Objects | Create blueprints and instances |
| ✅ Inheritance | Build hierarchies, reuse code |
| ✅ Polymorphism | One interface, many forms |
| ✅ Encapsulation | Protect data, control access |
| ✅ Abstraction | Define contracts, hide details |
| ✅ Magic Methods | Customize object behavior |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_Classes_Objects.md` | **Classes & Objects** | Creating classes, __init__, methods, attributes |
| `02_Inheritance.md` | **Inheritance** | Single, multiple, super(), MRO |
| `03_Polymorphism.md` | **Polymorphism** | Method overriding, duck typing, operator overloading |
| `04_Encapsulation.md` | **Encapsulation** | Public, private, protected, properties |
| `05_Abstraction.md` | **Abstraction** | Abstract classes, interfaces, ABC |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_Classes_Objects.md
        ↓
        Foundation - understand classes and instances
        
Step 2: 02_Inheritance.md
        ↓
        Build on existing classes
        
Step 3: 03_Polymorphism.md
        ↓
        Flexible interfaces
        
Step 4: 04_Encapsulation.md
        ↓
        Data protection
        
Step 5: 05_Abstraction.md
        ↓
        Design contracts
```

**Estimated Time:** 8-10 hours (with practice)

---

## 📊 OOP Concepts Overview

| Concept | What | Why | How |
|---------|------|-----|-----|
| Class | Blueprint | Define structure | `class MyClass:` |
| Object | Instance | Use the blueprint | `obj = MyClass()` |
| Inheritance | Parent-child | Code reuse | `class Child(Parent):` |
| Polymorphism | Many forms | Flexibility | Override methods |
| Encapsulation | Data hiding | Protection | `_private`, properties |
| Abstraction | Essential only | Simplicity | Abstract classes |

---

## 💡 Tips Before Starting

### Key Mindset Shifts:

1. **Think in Objects** — Real-world entities become classes
2. **Attributes = Nouns** — What the object HAS
3. **Methods = Verbs** — What the object DOES
4. **Encapsulate** — Hide implementation, expose interface

### Real-World Analogies:

```
CLASS = Blueprint of a house
OBJECT = Actual house built from blueprint

CLASS = Cookie cutter
OBJECT = Individual cookie

CLASS = Car design
OBJECT = Your specific car
```

---

## 🎯 Module Objectives

By the end of this module, you should be able to:

- [ ] Design classes with attributes and methods
- [ ] Implement inheritance hierarchies
- [ ] Apply polymorphism for flexible code
- [ ] Use encapsulation to protect data
- [ ] Create abstract classes and interfaces
- [ ] Write Pythonic OOP code

---

## 🔗 Quick Links

1. [Classes and Objects](./01_Classes_Objects.md)
2. [Inheritance](./02_Inheritance.md)
3. [Polymorphism](./03_Polymorphism.md)
4. [Encapsulation](./04_Encapsulation.md)
5. [Abstraction](./05_Abstraction.md)

---

## 📝 Quick Reference

```python
# CLASS DEFINITION
class Dog:
    species = "Canine"  # Class attribute
    
    def __init__(self, name, age):  # Constructor
        self.name = name  # Instance attribute
        self.age = age
    
    def bark(self):  # Instance method
        return f"{self.name} says Woof!"

# OBJECT CREATION
my_dog = Dog("Buddy", 3)
print(my_dog.bark())  # Buddy says Woof!

# INHERITANCE
class Bulldog(Dog):
    def bark(self):  # Override
        return f"{self.name} says Woof Woof!"

# ENCAPSULATION
class BankAccount:
    def __init__(self, balance):
        self._balance = balance  # Protected
    
    @property
    def balance(self):
        return self._balance

# ABSTRACTION
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

---

## 🆚 Procedural vs OOP

```python
# PROCEDURAL APPROACH
def create_dog(name, age):
    return {"name": name, "age": age}

def dog_bark(dog):
    return f"{dog['name']} says Woof!"

dog1 = create_dog("Buddy", 3)
print(dog_bark(dog1))

# OOP APPROACH
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def bark(self):
        return f"{self.name} says Woof!"

dog1 = Dog("Buddy", 3)
print(dog1.bark())

# OOP: Data and behavior together!
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 6: File Handling and Exceptions](../06_File_Handling_and_Exception/README.md)**
- Reading and writing files
- Exception handling
- Custom exceptions

---

> 💪 **Remember:** OOP is a way of thinking. Start simple, practice often, and the concepts will click!

---

*Happy Learning! 🚀*
