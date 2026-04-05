# 📘 Python MCQs

## 📌 Introduction

Test your Python knowledge with these multiple choice questions covering all major topics.

---

## 📝 Section 1: Python Basics

### Q1. What is the output of `print(type([]))`?
a) `<class 'tuple'>`  
b) `<class 'list'>`  
c) `<class 'array'>`  
d) `<class 'set'>`

**Answer: b) `<class 'list'>`**

---

### Q2. Which of the following is immutable in Python?
a) List  
b) Dictionary  
c) Tuple  
d) Set

**Answer: c) Tuple**

> Tuples are immutable - once created, their elements cannot be changed.

---

### Q3. What is the output of `print(2 ** 3 ** 2)`?
a) 64  
b) 512  
c) 256  
d) 81

**Answer: b) 512**

> Exponentiation is right-associative: `2 ** (3 ** 2)` = `2 ** 9` = 512

---

### Q4. What does `bool([])` return?
a) `True`  
b) `False`  
c) `None`  
d) `Error`

**Answer: b) `False`**

> Empty sequences (list, string, tuple) are falsy in Python.

---

### Q5. What is the output of `print("hello"[1:4])`?
a) `hel`  
b) `ell`  
c) `ello`  
d) `hell`

**Answer: b) `ell`**

> Slicing [1:4] includes indices 1, 2, 3 (excludes 4).

---

## 📝 Section 2: Data Structures

### Q6. What is the time complexity of accessing an element in a dictionary by key?
a) O(n)  
b) O(log n)  
c) O(1) average  
d) O(n²)

**Answer: c) O(1) average**

> Dictionaries use hash tables for O(1) average-case lookup.

---

### Q7. Which method removes and returns the last element from a list?
a) `remove()`  
b) `delete()`  
c) `pop()`  
d) `discard()`

**Answer: c) `pop()`**

---

### Q8. What is the output of `{1, 2, 3} & {2, 3, 4}`?
a) `{1, 2, 3, 4}`  
b) `{2, 3}`  
c) `{1, 4}`  
d) `{}`

**Answer: b) `{2, 3}`**

> `&` is the intersection operator for sets.

---

### Q9. What does `dict.get('key', 'default')` return if 'key' doesn't exist?
a) `None`  
b) `KeyError`  
c) `'default'`  
d) `False`

**Answer: c) `'default'`**

> `get()` returns the default value if key is not found.

---

### Q10. Which of these creates a dictionary?
a) `{1, 2, 3}`  
b) `{1: 'a', 2: 'b'}`  
c) `[1: 'a', 2: 'b']`  
d) `(1: 'a', 2: 'b')`

**Answer: b) `{1: 'a', 2: 'b'}`**

---

## 📝 Section 3: Functions

### Q11. What is the output?
```python
def func(a, b=[]):
    b.append(a)
    return b

print(func(1))
print(func(2))
```
a) `[1]` and `[2]`  
b) `[1]` and `[1, 2]`  
c) `[1, 2]` and `[1, 2]`  
d) Error

**Answer: b) `[1]` and `[1, 2]`**

> Default mutable arguments are shared across calls - a common Python gotcha!

---

### Q12. What does `*args` represent?
a) Keyword arguments  
b) Positional arguments as a tuple  
c) Required arguments  
d) Default arguments

**Answer: b) Positional arguments as a tuple**

---

### Q13. What is a lambda function?
a) A recursive function  
b) A generator function  
c) An anonymous function  
d) A class method

**Answer: c) An anonymous function**

---

### Q14. What is the output of `(lambda x: x * 2)(5)`?
a) `x * 2`  
b) `10`  
c) `5`  
d) Error

**Answer: b) `10`**

---

### Q15. Which keyword is used to create a generator?
a) `return`  
b) `yield`  
c) `generate`  
d) `iter`

**Answer: b) `yield`**

---

## 📝 Section 4: OOP

### Q16. What is encapsulation?
a) Inheriting from multiple classes  
b) Hiding internal details  
c) Method overloading  
d) Creating objects

**Answer: b) Hiding internal details**

---

### Q17. What does `super()` do?
a) Creates a superclass  
b) Calls parent class methods  
c) Defines abstract methods  
d) Creates multiple instances

**Answer: b) Calls parent class methods**

---

### Q18. Which method is called when an object is created?
a) `__new__`  
b) `__create__`  
c) `__init__`  
d) `__start__`

**Answer: c) `__init__`**

> Note: `__new__` is called before `__init__` but `__init__` is the initializer.

---

### Q19. What is the output?
```python
class A:
    def __init__(self):
        self.x = 1

class B(A):
    def __init__(self):
        super().__init__()
        self.y = 2

b = B()
print(b.x, b.y)
```
a) Error  
b) `1 2`  
c) `None None`  
d) `2 1`

**Answer: b) `1 2`**

---

### Q20. What does `@staticmethod` decorator do?
a) Makes method private  
b) Creates class method  
c) Creates method without self  
d) Makes method abstract

**Answer: c) Creates method without self**

---

## 📝 Section 5: File Handling & Exceptions

### Q21. Which mode opens a file for both reading and writing?
a) `'r'`  
b) `'w'`  
c) `'r+'`  
d) `'a'`

**Answer: c) `'r+'`**

---

### Q22. What happens if you open a non-existent file in 'r' mode?
a) Creates new file  
b) `FileNotFoundError`  
c) Returns `None`  
d) Opens empty file

**Answer: b) `FileNotFoundError`**

---

### Q23. What is the correct way to handle exceptions?
a) `try... except... else... finally`  
b) `catch... throw... finally`  
c) `try... catch... finally`  
d) `handle... except... else`

**Answer: a) `try... except... else... finally`**

---

### Q24. Which block always executes?
a) `try`  
b) `except`  
c) `else`  
d) `finally`

**Answer: d) `finally`**

---

### Q25. What does `with open('file.txt') as f:` ensure?
a) File is opened in binary  
b) File is automatically closed  
c) File is read-only  
d) File is encrypted

**Answer: b) File is automatically closed**

---

## 📝 Section 6: Advanced Python

### Q26. What is a decorator?
a) A class attribute  
b) A function that modifies another function  
c) A loop structure  
d) An exception handler

**Answer: b) A function that modifies another function**

---

### Q27. What does `@property` decorator do?
a) Makes attribute private  
b) Creates getter method  
c) Deletes attribute  
d) Makes class singleton

**Answer: b) Creates getter method**

---

### Q28. What is the GIL in Python?
a) Global Import Lock  
b) Global Interpreter Lock  
c) General Input Library  
d) Generator Instance Lock

**Answer: b) Global Interpreter Lock**

> GIL prevents multiple threads from executing Python bytecode simultaneously.

---

### Q29. Which is faster for iteration?
a) List  
b) Generator  
c) Same speed  
d) Depends on size

**Answer: b) Generator**

> Generators don't store all values in memory, making them more memory-efficient.

---

### Q30. What is the output?
```python
def gen():
    yield 1
    yield 2
    yield 3

g = gen()
print(next(g), next(g))
```
a) `1 1`  
b) `1 2`  
c) `2 3`  
d) Error

**Answer: b) `1 2`**

---

## 📝 Section 7: Libraries

### Q31. Which library is used for data analysis with DataFrames?
a) NumPy  
b) Pandas  
c) Matplotlib  
d) SciPy

**Answer: b) Pandas**

---

### Q32. What does `numpy.array([1,2,3]).shape` return?
a) `3`  
b) `(3,)`  
c) `[3]`  
d) `1x3`

**Answer: b) `(3,)`**

---

### Q33. Which function creates a figure in Matplotlib?
a) `plt.graph()`  
b) `plt.figure()`  
c) `plt.create()`  
d) `plt.canvas()`

**Answer: b) `plt.figure()`**

---

### Q34. In Flask, what decorator defines a route?
a) `@route`  
b) `@path`  
c) `@app.route`  
d) `@url`

**Answer: c) `@app.route`**

---

### Q35. What HTTP method is typically used to create a resource?
a) GET  
b) POST  
c) PUT  
d) DELETE

**Answer: b) POST**

---

## 📝 Section 8: Database & APIs

### Q36. Which Python module is used for SQLite?
a) `mysql`  
b) `sqlite3`  
c) `sql`  
d) `database`

**Answer: b) `sqlite3`**

---

### Q37. What does CRUD stand for?
a) Create, Read, Update, Delete  
b) Create, Retrieve, Update, Drop  
c) Copy, Read, Update, Delete  
d) Create, Read, Upload, Delete

**Answer: a) Create, Read, Update, Delete**

---

### Q38. Which library is commonly used for HTTP requests?
a) `http`  
b) `requests`  
c) `urllib`  
d) `fetch`

**Answer: b) `requests`**

> While `urllib` is built-in, `requests` is the standard for simplicity.

---

### Q39. What status code indicates success?
a) 100  
b) 200  
c) 300  
d) 400

**Answer: b) 200**

---

### Q40. Which SQL clause is used to filter results?
a) SELECT  
b) FROM  
c) WHERE  
d) ORDER BY

**Answer: c) WHERE**

---

## 📝 Section 9: Miscellaneous

### Q41. What is PEP 8?
a) Python Enhancement Protocol  
b) Python Style Guide  
c) Python Error Prevention  
d) Python Extension Package

**Answer: b) Python Style Guide**

---

### Q42. What is `__name__` when a script is run directly?
a) `'module'`  
b) `'script'`  
c) `'__main__'`  
d) The filename

**Answer: c) `'__main__'`**

---

### Q43. Which is NOT a valid variable name?
a) `_var`  
b) `var_1`  
c) `1_var`  
d) `var1`

**Answer: c) `1_var`**

> Variable names cannot start with a number.

---

### Q44. What does `//` operator do?
a) Division  
b) Floor division  
c) Modulo  
d) Comment

**Answer: b) Floor division**

---

### Q45. What is the output of `print(list(range(0, 10, 3)))`?
a) `[0, 3, 6, 9]`  
b) `[0, 3, 6, 9, 10]`  
c) `[3, 6, 9]`  
d) `[0, 1, 2, 3]`

**Answer: a) `[0, 3, 6, 9]`**

---

## 📊 Score Card

| Score | Level |
|-------|-------|
| 40-45 | Expert 🏆 |
| 30-39 | Advanced 🥇 |
| 20-29 | Intermediate 🥈 |
| 10-19 | Beginner 🥉 |
| 0-9 | Need Practice 📚 |

---

## ⏭️ Next: Interview Questions

Prepare for interviews with **[Interview Questions](06_Interview_Questions.md)**!

---

*Knowledge is power!* 💡
