# 📘 Abstraction in Python

## 📌 Introduction

**Abstraction** is hiding complex implementation details and showing only the essential features. It defines WHAT something does without specifying HOW it does it.

Think of it like a **TV remote** — you press "Volume Up" without knowing the electronics inside. You know WHAT it does, not HOW.

```
┌─────────────────────────────────────────────────────────────┐
│                     ABSTRACTION                              │
│                                                              │
│   ┌─────────────────────────────────────┐                   │
│   │        Abstract Class: Shape        │                   │
│   │  ┌───────────────────────────────┐  │                   │
│   │  │   abstract area()             │  │  WHAT to do       │
│   │  │   abstract perimeter()        │  │  (contract)       │
│   │  └───────────────────────────────┘  │                   │
│   └──────────────────┬──────────────────┘                   │
│                      │                                       │
│          ┌───────────┴───────────┐                          │
│          ↓                       ↓                          │
│   ┌─────────────┐         ┌─────────────┐                   │
│   │  Rectangle  │         │   Circle    │                   │
│   │ area() = w*h│         │ area() = πr²│  HOW to do        │
│   │ perim() = ..│         │ perim() = ..│  (implementation) │
│   └─────────────┘         └─────────────┘                   │
│                                                              │
│   "What" is defined at top, "How" is defined below          │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Abstraction vs Encapsulation

| Aspect | Abstraction | Encapsulation |
|--------|-------------|---------------|
| Focus | What it does | How to protect data |
| Purpose | Hide complexity | Hide implementation |
| Level | Design level | Implementation level |
| Tool | Abstract classes | Access modifiers |

### Python's ABC Module

Python uses the `abc` (Abstract Base Classes) module for abstraction:

```python
from abc import ABC, abstractmethod
```

---

## 💻 Code Examples

### 🟢 Basic: Abstract Base Class

```python
from abc import ABC, abstractmethod

# Abstract class - cannot be instantiated
class Animal(ABC):
    def __init__(self, name):
        self.name = name
    
    @abstractmethod
    def speak(self):
        """All animals must implement this"""
        pass
    
    @abstractmethod
    def move(self):
        """All animals must implement this"""
        pass
    
    # Concrete method (not abstract)
    def describe(self):
        return f"I am {self.name}"

# Cannot create Animal directly
# animal = Animal("Generic")  # TypeError!

# Concrete class - implements all abstract methods
class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"
    
    def move(self):
        return f"{self.name} runs on four legs"

class Bird(Animal):
    def speak(self):
        return f"{self.name} says Chirp!"
    
    def move(self):
        return f"{self.name} flies with wings"

# Create concrete instances
dog = Dog("Buddy")
bird = Bird("Tweety")

print(dog.speak())     # Buddy says Woof!
print(dog.move())      # Buddy runs on four legs
print(dog.describe())  # I am Buddy

print(bird.speak())    # Tweety says Chirp!
print(bird.move())     # Tweety flies with wings
```

### 🟢 Basic: Incomplete Implementation Error

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass

# INCOMPLETE - missing perimeter()
class BadRectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    # Missing perimeter!

# Cannot instantiate incomplete class
# bad = BadRectangle(5, 3)  # TypeError!

# COMPLETE - all methods implemented
class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

rect = Rectangle(5, 3)
print(f"Area: {rect.area()}")        # Area: 15
print(f"Perimeter: {rect.perimeter()}")  # Perimeter: 16
```

### 🟡 Intermediate: Abstract Properties

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    def __init__(self, brand):
        self._brand = brand
    
    @property
    @abstractmethod
    def wheels(self):
        """Number of wheels - must be implemented"""
        pass
    
    @property
    @abstractmethod
    def max_speed(self):
        """Maximum speed in km/h - must be implemented"""
        pass
    
    def info(self):
        return f"{self._brand}: {self.wheels} wheels, max {self.max_speed} km/h"

class Car(Vehicle):
    @property
    def wheels(self):
        return 4
    
    @property
    def max_speed(self):
        return 200

class Motorcycle(Vehicle):
    @property
    def wheels(self):
        return 2
    
    @property
    def max_speed(self):
        return 180

class Truck(Vehicle):
    def __init__(self, brand, is_heavy):
        super().__init__(brand)
        self._is_heavy = is_heavy
    
    @property
    def wheels(self):
        return 18 if self._is_heavy else 6
    
    @property
    def max_speed(self):
        return 100 if self._is_heavy else 120

# Usage
car = Car("Toyota")
bike = Motorcycle("Honda")
truck = Truck("Volvo", is_heavy=True)

print(car.info())   # Toyota: 4 wheels, max 200 km/h
print(bike.info())  # Honda: 2 wheels, max 180 km/h
print(truck.info()) # Volvo: 18 wheels, max 100 km/h
```

### 🟡 Intermediate: Abstract Class with Implementation

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    """Base class for data processing pipelines"""
    
    def __init__(self, data):
        self._data = data
        self._processed = None
    
    # Abstract methods - MUST be implemented
    @abstractmethod
    def validate(self):
        """Validate the input data"""
        pass
    
    @abstractmethod
    def transform(self):
        """Transform the data"""
        pass
    
    # Concrete methods - CAN be used as-is or overridden
    def preprocess(self):
        """Optional preprocessing"""
        return self._data
    
    def postprocess(self):
        """Optional postprocessing"""
        return self._processed
    
    # Template method pattern
    def process(self):
        """Main processing pipeline"""
        self._data = self.preprocess()
        if not self.validate():
            raise ValueError("Validation failed")
        self._processed = self.transform()
        return self.postprocess()

class NumberProcessor(DataProcessor):
    def validate(self):
        return all(isinstance(x, (int, float)) for x in self._data)
    
    def transform(self):
        return [x * 2 for x in self._data]

class StringProcessor(DataProcessor):
    def validate(self):
        return all(isinstance(x, str) for x in self._data)
    
    def transform(self):
        return [s.upper() for s in self._data]
    
    # Override postprocess
    def postprocess(self):
        return " ".join(self._processed)

# Usage
nums = NumberProcessor([1, 2, 3, 4, 5])
print(nums.process())  # [2, 4, 6, 8, 10]

strings = StringProcessor(["hello", "world"])
print(strings.process())  # HELLO WORLD
```

### 🔴 Advanced: Interface-like Patterns

```python
from abc import ABC, abstractmethod

# Pure interface - no implementation
class Drawable(ABC):
    @abstractmethod
    def draw(self):
        pass

class Resizable(ABC):
    @abstractmethod
    def resize(self, factor):
        pass

class Serializable(ABC):
    @abstractmethod
    def to_dict(self):
        pass
    
    @abstractmethod
    def from_dict(cls, data):
        pass

# Multiple interface implementation
class UIElement(Drawable, Resizable, Serializable):
    def __init__(self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
    
    def draw(self):
        return f"Drawing at ({self.x}, {self.y})"
    
    def resize(self, factor):
        self.width *= factor
        self.height *= factor
        return f"Resized to {self.width}x{self.height}"
    
    def to_dict(self):
        return {
            "x": self.x, "y": self.y,
            "width": self.width, "height": self.height
        }
    
    @classmethod
    def from_dict(cls, data):
        return cls(data["x"], data["y"], data["width"], data["height"])

element = UIElement(10, 20, 100, 50)
print(element.draw())       # Drawing at (10, 20)
print(element.resize(1.5))  # Resized to 150x75
print(element.to_dict())    # {'x': 10, 'y': 20, 'width': 150.0, 'height': 75.0}
```

### 🔴 Advanced: Protocol (Structural Subtyping)

```python
from typing import Protocol, runtime_checkable

# Protocol - structural typing (duck typing with type hints)
@runtime_checkable
class Closeable(Protocol):
    def close(self) -> None:
        ...

@runtime_checkable
class Reader(Protocol):
    def read(self, size: int = -1) -> str:
        ...

class Readable(Protocol):
    def read(self) -> str:
        ...
    
    def readline(self) -> str:
        ...

# Classes don't need to explicitly inherit!
class FileWrapper:
    def __init__(self, filename):
        self.filename = filename
        self._file = open(filename, 'r')
    
    def read(self, size=-1):
        return self._file.read(size)
    
    def readline(self):
        return self._file.readline()
    
    def close(self):
        self._file.close()

class StringReader:
    def __init__(self, content):
        self.content = content
        self.pos = 0
    
    def read(self, size=-1):
        if size == -1:
            result = self.content[self.pos:]
            self.pos = len(self.content)
        else:
            result = self.content[self.pos:self.pos + size]
            self.pos += size
        return result
    
    def close(self):
        pass  # Nothing to close

# Both work with protocols without inheriting
def process_reader(reader: Reader):
    return reader.read()

def close_resource(resource: Closeable):
    resource.close()

# Check protocol compliance at runtime
sr = StringReader("Hello, World!")
print(isinstance(sr, Closeable))  # True
print(isinstance(sr, Reader))     # True
```

### 🔴 Advanced: Abstract Factory Pattern

```python
from abc import ABC, abstractmethod

# Abstract Products
class Button(ABC):
    @abstractmethod
    def render(self):
        pass
    
    @abstractmethod
    def click(self):
        pass

class TextBox(ABC):
    @abstractmethod
    def render(self):
        pass
    
    @abstractmethod
    def get_value(self):
        pass

# Abstract Factory
class GUIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button:
        pass
    
    @abstractmethod
    def create_textbox(self) -> TextBox:
        pass

# Concrete Products - Windows
class WindowsButton(Button):
    def render(self):
        return "[Windows Button]"
    
    def click(self):
        return "Windows button clicked"

class WindowsTextBox(TextBox):
    def render(self):
        return "|Windows TextBox|"
    
    def get_value(self):
        return "Windows text"

# Concrete Products - Mac
class MacButton(Button):
    def render(self):
        return "(Mac Button)"
    
    def click(self):
        return "Mac button clicked"

class MacTextBox(TextBox):
    def render(self):
        return "[Mac TextBox]"
    
    def get_value(self):
        return "Mac text"

# Concrete Factories
class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()
    
    def create_textbox(self):
        return WindowsTextBox()

class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()
    
    def create_textbox(self):
        return MacTextBox()

# Client code - works with any factory
def create_ui(factory: GUIFactory):
    button = factory.create_button()
    textbox = factory.create_textbox()
    
    print(button.render())
    print(textbox.render())
    print(button.click())

# Usage
print("Windows UI:")
create_ui(WindowsFactory())

print("\nMac UI:")
create_ui(MacFactory())
```

---

## 📊 Abstract Class Components

| Component | Decorator | Description |
|-----------|-----------|-------------|
| Abstract Method | `@abstractmethod` | Must be implemented |
| Abstract Property | `@property` + `@abstractmethod` | Property that must be implemented |
| Concrete Method | None | Can be used as-is |
| Class Method | `@classmethod` + `@abstractmethod` | Abstract class method |
| Static Method | `@staticmethod` + `@abstractmethod` | Abstract static method |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting ABC Inheritance

```python
from abc import abstractmethod

# WRONG - without ABC, @abstractmethod does nothing
class Shape:
    @abstractmethod
    def area(self):
        pass

s = Shape()  # Works! No error!

# CORRECT
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

# s = Shape()  # TypeError!
```

### ❌ Mistake 2: Not Implementing All Abstract Methods

```python
from abc import ABC, abstractmethod

class Base(ABC):
    @abstractmethod
    def method1(self):
        pass
    
    @abstractmethod
    def method2(self):
        pass

# WRONG - incomplete
class Incomplete(Base):
    def method1(self):
        return "method1"
    # Missing method2!

# i = Incomplete()  # TypeError!

# CORRECT - complete
class Complete(Base):
    def method1(self):
        return "method1"
    
    def method2(self):
        return "method2"
```

### ❌ Mistake 3: Calling super() in Abstract Method

```python
from abc import ABC, abstractmethod

class Base(ABC):
    @abstractmethod
    def process(self):
        pass  # No implementation to call!

class Child(Base):
    def process(self):
        super().process()  # Does nothing (pass)
        return "processed"

# Better approach
class BetterBase(ABC):
    @abstractmethod
    def process(self):
        """Subclass must implement processing logic"""
        raise NotImplementedError  # Explicit!
```

---

## 💡 Pro Tips

### 1. Use Abstract Classes for Shared Implementation

```python
from abc import ABC, abstractmethod

class Logger(ABC):
    def log(self, message):
        # Shared logic
        formatted = f"[{self.get_level()}] {message}"
        self.write(formatted)
    
    @abstractmethod
    def get_level(self):
        pass
    
    @abstractmethod
    def write(self, message):
        pass
```

### 2. Document Abstract Methods Well

```python
@abstractmethod
def process(self, data: dict) -> dict:
    """
    Process input data and return results.
    
    Args:
        data: Input dictionary with 'items' key
        
    Returns:
        Dictionary with 'processed_items' key
        
    Raises:
        ValueError: If data is invalid
    """
    pass
```

### 3. Use Protocol for Duck Typing with Type Hints

```python
from typing import Protocol

class Sortable(Protocol):
    def __lt__(self, other) -> bool: ...

def sort_items(items: list[Sortable]):
    return sorted(items)
```

---

## 💼 Interview Insight

### Q1: What is the difference between Abstract Class and Interface?

> **Abstract Class:**
> - Can have both abstract and concrete methods
> - Can have instance variables
> - Supports single inheritance (in many languages)
> 
> **Interface (Protocol in Python):**
> - Only method signatures (no implementation)
> - Defines a contract
> - Supports multiple implementation

### Q2: When to use ABC vs Protocol?

> - Use **ABC** when you want to enforce inheritance and provide shared implementation
> - Use **Protocol** when you only care about structure (duck typing with type hints)

### Q3: Can abstract class have a constructor?

> Yes! Abstract classes can have `__init__`. It's often used to initialize shared attributes that concrete classes will use.

---

## 🧪 Practice Questions

### Easy:
1. Create an abstract `Shape` class with `area()` and `perimeter()`
2. Implement `Circle` and `Square` from `Shape`
3. Create an abstract `Animal` with `speak()` method

### Medium:
4. Design a payment system with abstract `PaymentProcessor`
5. Create a file parser hierarchy (JSON, XML, CSV)
6. Implement a notification system (Email, SMS, Push)

### Hard:
7. Build an abstract plugin system
8. Design a database connection interface with adapters
9. Create a complete UI component framework

---

## 📖 Summary

| Concept | Description |
|---------|-------------|
| ABC | Abstract Base Class |
| `@abstractmethod` | Method that must be implemented |
| Concrete method | Regular method in abstract class |
| Protocol | Structural typing (duck typing) |
| Interface | Pure abstract class |
| Factory | Create objects without specifying class |

---

## ⏭️ Next Module

**[Module 6: File Handling and Exceptions](../06_File_Handling_and_Exception/README.md)** — Working with files and handling errors!

---

*Abstraction: Define the what, hide the how!* 🎭
