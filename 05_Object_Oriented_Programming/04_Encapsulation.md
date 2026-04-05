# 📘 Encapsulation in Python

## 📌 Introduction

**Encapsulation** is bundling data and methods together while controlling access to the internal state. It's about hiding implementation details and exposing only what's necessary.

Think of it like a **car's dashboard** — you see the speedometer and controls (public interface), but the engine internals are hidden (private implementation).

```
┌─────────────────────────────────────────────────────────────┐
│                     ENCAPSULATION                            │
│                                                              │
│   ┌─────────────────────────────────────┐                   │
│   │            BankAccount              │                   │
│   │ ┌─────────────────────────────────┐ │                   │
│   │ │     PRIVATE (Hidden)            │ │                   │
│   │ │  __balance = 1000               │ │                   │
│   │ │  __pin = "1234"                 │ │                   │
│   │ │  __validate_transaction()       │ │                   │
│   │ └─────────────────────────────────┘ │                   │
│   │                                      │                   │
│   │ ┌─────────────────────────────────┐ │                   │
│   │ │     PUBLIC (Accessible)         │ │                   │
│   │ │  deposit()                      │ │                   │
│   │ │  withdraw()                     │ │                   │
│   │ │  get_balance()                  │ │                   │
│   │ └─────────────────────────────────┘ │                   │
│   └─────────────────────────────────────┘                   │
│                                                              │
│   Hide complexity, expose simplicity                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Access Modifiers in Python

| Convention | Syntax | Meaning | Accessibility |
|------------|--------|---------|---------------|
| Public | `name` | Open to all | Everywhere |
| Protected | `_name` | Internal use | Class and subclasses (convention) |
| Private | `__name` | Strictly internal | Class only (name mangling) |

### Python's Philosophy

> "We're all consenting adults here."

Python uses **conventions**, not strict enforcement. The underscore prefix signals intent, but doesn't truly prevent access.

---

## 💻 Code Examples

### 🟢 Basic: Public Attributes

```python
class Person:
    def __init__(self, name, age):
        # Public attributes - accessible everywhere
        self.name = name
        self.age = age

person = Person("Alice", 25)

# Direct access (public)
print(person.name)  # Alice
print(person.age)   # 25

# Direct modification (no protection!)
person.age = -5     # No validation!
print(person.age)   # -5 (invalid but allowed)
```

### 🟢 Basic: Protected Attributes (Convention)

```python
class Person:
    def __init__(self, name, age):
        # Protected attributes - single underscore
        self._name = name  # "Don't touch directly" signal
        self._age = age
    
    def get_info(self):
        return f"{self._name}, {self._age} years old"

person = Person("Alice", 25)

# Still accessible (just a convention)
print(person._name)  # Alice - works but discouraged

# The underscore signals: "I'm internal, use methods instead"
print(person.get_info())  # Alice, 25 years old
```

### 🟢 Basic: Private Attributes (Name Mangling)

```python
class Person:
    def __init__(self, name, age):
        # Private attributes - double underscore
        self.__name = name
        self.__age = age
    
    def get_info(self):
        return f"{self.__name}, {self.__age} years old"
    
    def set_age(self, age):
        if age >= 0:
            self.__age = age
        else:
            raise ValueError("Age cannot be negative")

person = Person("Alice", 25)

# Cannot access directly
# print(person.__name)  # AttributeError!

# Access through methods
print(person.get_info())  # Alice, 25 years old

# Still accessible via name mangling (but don't do this!)
print(person._Person__name)  # Alice (mangled name)

# Name mangling: __name becomes _ClassName__name
print(dir(person))
# ['_Person__age', '_Person__name', 'get_info', 'set_age', ...]
```

### 🟡 Intermediate: Property Decorator

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    # Getter
    @property
    def celsius(self):
        """Get temperature in Celsius"""
        return self._celsius
    
    # Setter
    @celsius.setter
    def celsius(self, value):
        """Set temperature in Celsius with validation"""
        if value < -273.15:
            raise ValueError("Below absolute zero!")
        self._celsius = value
    
    # Deleter (rarely used)
    @celsius.deleter
    def celsius(self):
        print("Deleting temperature...")
        del self._celsius
    
    # Computed property (read-only)
    @property
    def fahrenheit(self):
        """Get temperature in Fahrenheit"""
        return self._celsius * 9/5 + 32
    
    @property
    def kelvin(self):
        """Get temperature in Kelvin"""
        return self._celsius + 273.15

# Usage - looks like attribute access, but uses methods
temp = Temperature(25)

print(temp.celsius)     # 25 (getter)
print(temp.fahrenheit)  # 77.0 (computed)
print(temp.kelvin)      # 298.15 (computed)

temp.celsius = 30       # Uses setter
print(temp.celsius)     # 30

# Validation kicks in
# temp.celsius = -300   # ValueError: Below absolute zero!

# Cannot set read-only properties
# temp.fahrenheit = 100  # AttributeError: can't set attribute
```

### 🟡 Intermediate: Getters and Setters Pattern

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.__owner = owner
        self.__balance = balance
        self.__transaction_history = []
    
    # Getter for balance
    @property
    def balance(self):
        return self.__balance
    
    # No setter for balance - use deposit/withdraw instead
    
    # Getter for owner
    @property
    def owner(self):
        return self.__owner
    
    # Read-only history
    @property
    def history(self):
        return self.__transaction_history.copy()  # Return copy!
    
    # Controlled operations
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount
        self.__transaction_history.append(f"Deposit: +${amount}")
        return self.__balance
    
    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self.__balance:
            raise ValueError("Insufficient funds")
        self.__balance -= amount
        self.__transaction_history.append(f"Withdrawal: -${amount}")
        return self.__balance

# Usage
account = BankAccount("Alice", 100)

print(account.balance)  # 100
print(account.owner)    # Alice

account.deposit(50)     # 150
account.withdraw(30)    # 120

print(account.history)
# ['Deposit: +$50', 'Withdrawal: -$30']

# Cannot directly modify balance
# account.balance = 1000000  # AttributeError!
# account.__balance = 1000000  # Creates new attribute, doesn't modify!
```

### 🟡 Intermediate: Protected in Inheritance

```python
class Animal:
    def __init__(self, name):
        self._name = name        # Protected - accessible to subclasses
        self.__secret = "hidden" # Private - not accessible
    
    def _internal_method(self):  # Protected method
        return f"Internal: {self._name}"
    
    def __private_method(self):  # Private method
        return "This is private"
    
    def public_method(self):
        return self.__private_method()  # Can call own private

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self._breed = breed
    
    def info(self):
        # Can access protected
        return f"{self._name} is a {self._breed}"
    
    def internal_info(self):
        # Can call protected method
        return self._internal_method()
    
    def try_private(self):
        # Cannot access parent's private
        # return self.__secret  # AttributeError!
        pass

dog = Dog("Buddy", "Labrador")
print(dog.info())           # Buddy is a Labrador
print(dog.internal_info())  # Internal: Buddy
print(dog._name)            # Buddy (accessible but discouraged)
```

### 🔴 Advanced: Descriptor Protocol

```python
class ValidatedAttribute:
    """Descriptor that validates on set"""
    
    def __init__(self, name, validator):
        self.name = name
        self.validator = validator
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return obj.__dict__.get(self.name)
    
    def __set__(self, obj, value):
        if not self.validator(value):
            raise ValueError(f"Invalid value for {self.name}: {value}")
        obj.__dict__[self.name] = value

# Validators
def positive(x):
    return x > 0

def non_empty_string(s):
    return isinstance(s, str) and len(s) > 0

class Product:
    name = ValidatedAttribute('name', non_empty_string)
    price = ValidatedAttribute('price', positive)
    quantity = ValidatedAttribute('quantity', lambda x: x >= 0)
    
    def __init__(self, name, price, quantity=0):
        self.name = name
        self.price = price
        self.quantity = quantity

# Usage
product = Product("Apple", 1.50, 100)
print(product.name)    # Apple
print(product.price)   # 1.5

# Validation
# Product("", 1.50)    # ValueError: Invalid value for name
# Product("Apple", -1) # ValueError: Invalid value for price
```

### 🔴 Advanced: `__slots__` for Controlled Attributes

```python
class Point:
    __slots__ = ['x', 'y']  # Only these attributes allowed
    
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(3, 4)
print(p.x, p.y)  # 3 4

# Cannot add new attributes
# p.z = 5  # AttributeError: 'Point' object has no attribute 'z'

# Benefits:
# 1. Prevents accidental attribute creation
# 2. Reduces memory usage
# 3. Faster attribute access

import sys
class WithSlots:
    __slots__ = ['x', 'y']
    def __init__(self):
        self.x = 1
        self.y = 2

class WithoutSlots:
    def __init__(self):
        self.x = 1
        self.y = 2

# Memory comparison
print(sys.getsizeof(WithSlots()))    # ~56 bytes
print(sys.getsizeof(WithoutSlots())) # ~56 bytes + dict overhead
```

### 🔴 Advanced: Property Factory

```python
def validated_property(name, validator, error_msg):
    """Factory function to create validated properties"""
    storage_name = f'__{name}'
    
    @property
    def prop(self):
        return getattr(self, storage_name, None)
    
    @prop.setter
    def prop(self, value):
        if not validator(value):
            raise ValueError(error_msg)
        setattr(self, storage_name, value)
    
    return prop

class User:
    # Create validated properties using factory
    age = validated_property(
        'age',
        lambda x: isinstance(x, int) and 0 <= x <= 150,
        "Age must be between 0 and 150"
    )
    
    email = validated_property(
        'email',
        lambda x: isinstance(x, str) and '@' in x,
        "Invalid email format"
    )
    
    def __init__(self, age, email):
        self.age = age
        self.email = email

user = User(25, "alice@example.com")
print(user.age)    # 25
print(user.email)  # alice@example.com

# Validation
# user.age = 200   # ValueError: Age must be between 0 and 150
# user.email = "invalid"  # ValueError: Invalid email format
```

---

## 📊 Encapsulation Summary

| Level | Syntax | Name Mangling | Access |
|-------|--------|---------------|--------|
| Public | `name` | No | Anywhere |
| Protected | `_name` | No | By convention: class + subclasses |
| Private | `__name` | Yes | Only within class |

### Property Patterns

| Pattern | Use Case |
|---------|----------|
| `@property` | Read-only or computed attributes |
| `@x.setter` | Validated attribute setting |
| Getter only | Truly read-only data |
| Private + property | Protected internal state |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Exposing Mutable Objects

```python
# WRONG
class Team:
    def __init__(self):
        self._members = []
    
    @property
    def members(self):
        return self._members  # Returns actual list!

team = Team()
team.members.append("Alice")  # Modified internal state!

# CORRECT
class Team:
    def __init__(self):
        self._members = []
    
    @property
    def members(self):
        return self._members.copy()  # Return copy
    
    def add_member(self, name):
        self._members.append(name)
```

### ❌ Mistake 2: Thinking Private is Truly Private

```python
class Secret:
    def __init__(self):
        self.__password = "secret123"

s = Secret()
# print(s.__password)  # AttributeError

# But still accessible via mangling!
print(s._Secret__password)  # secret123

# Don't rely on __ for security!
```

### ❌ Mistake 3: Overusing Properties

```python
# WRONG - property for simple access
class Point:
    def __init__(self, x, y):
        self._x = x
        self._y = y
    
    @property
    def x(self):
        return self._x
    
    @x.setter
    def x(self, value):
        self._x = value  # No validation, just overhead!

# CORRECT - just use public if no validation needed
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

---

## 💡 Pro Tips

### 1. Use Properties for Computed Values

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    @property
    def area(self):
        return self.width * self.height  # Computed on access
```

### 2. Return Copies of Mutable Data

```python
@property
def items(self):
    return list(self._items)  # Return copy
```

### 3. Document Access Intentions

```python
class API:
    """
    Public: url, get_data()
    Protected: _parse(), _validate()
    Private: __token (internal only)
    """
```

---

## 💼 Interview Insight

### Q1: What is encapsulation?

> Encapsulation is bundling data and methods that operate on that data within a class, while restricting direct access to some of the object's components. It helps maintain data integrity and hide implementation details.

### Q2: Difference between `_` and `__` prefix?

> - Single underscore (`_name`): Convention for protected. Still accessible, signals "internal use."
> - Double underscore (`__name`): Name mangling occurs. Becomes `_ClassName__name`. Harder to access accidentally.

### Q3: How to create a read-only property?

```python
class Example:
    def __init__(self, value):
        self._value = value
    
    @property
    def value(self):
        return self._value
    # No setter = read-only
```

---

## 🧪 Practice Questions

### Easy:
1. Create a `Person` class with validated age
2. Implement a read-only `id` property
3. Use `@property` for temperature conversion

### Medium:
4. Build a `Password` class with strength validation
5. Create an `Inventory` with protected stock levels
6. Implement a `Counter` with min/max limits

### Hard:
7. Build a complete `BankAccount` with transaction limits
8. Create a `Configuration` class with typed properties
9. Implement a `CreditCard` with masked display

---

## 📖 Summary

| Concept | Implementation |
|---------|----------------|
| Public | `self.name` |
| Protected | `self._name` |
| Private | `self.__name` |
| Property | `@property` |
| Setter | `@name.setter` |
| Validation | Check in setter |
| Read-only | Property without setter |

---

## ⏭️ Next Topic

**[Abstraction](./05_Abstraction.md)** — Defining interfaces and contracts!

---

*Encapsulation: Protect what matters!* 🔐
