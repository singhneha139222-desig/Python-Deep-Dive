# 📘 Module 2: Control Flow

Welcome to **Module 2** of your Python journey! In this module, you will learn how to control the flow of your programs — making decisions and repeating actions.

---

## 📌 Overview of This Module

In real life, we make decisions constantly:
- **If** it's raining, take an umbrella
- **While** you're hungry, keep eating
- **For** each item in your list, check it off

Programs work the same way! **Control flow** determines:
1. **Which** code runs (conditional statements)
2. **How many times** code runs (loops)
3. **When** to skip or stop (break, continue, pass)

---

## 📚 What You Will Learn

After completing this module, you will be able to:

| Skill | Description |
|-------|-------------|
| ✅ Write conditions | Use if, elif, else statements |
| ✅ Make decisions | Execute different code based on conditions |
| ✅ Use loops | Repeat code with for and while loops |
| ✅ Control loops | Use break, continue, and pass |
| ✅ Nested structures | Combine conditions and loops |
| ✅ Handle edge cases | Write robust, error-free code |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_If_Else.md` | **Conditional Statements** | if, elif, else, nested conditions, ternary operator |
| `02_Loops.md` | **Loops** | for loops, while loops, range(), nested loops |
| `03_Break_Continue_Pass.md` | **Loop Control** | break, continue, pass, else with loops |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_If_Else.md
        ↓
        Learn to make decisions in code
        
Step 2: 02_Loops.md
        ↓
        Learn to repeat code efficiently
        
Step 3: 03_Break_Continue_Pass.md
        ↓
        Learn to control loop behavior
```

**Estimated Time:** 3-5 hours (with practice)

---

## 💡 Tips Before Starting

### Key Concepts to Remember:

1. **Indentation is CRITICAL in Python**
   - Use 4 spaces (or 1 tab) consistently
   - Indentation defines code blocks
   
2. **Conditions evaluate to True or False**
   - Any expression can be used as a condition
   - Remember truthy and falsy values
   
3. **Loops can be infinite**
   - Always ensure your loop has an exit condition
   - Test with small data first

### Visual: Control Flow Concept

```
                    ┌──────────────────┐
                    │   Program Start  │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
               ┌────│   Condition?     │────┐
               │    └──────────────────┘    │
             True                         False
               │                            │
        ┌──────▼──────┐              ┌──────▼──────┐
        │  Code Block │              │  Code Block │
        │     (if)    │              │   (else)    │
        └──────┬──────┘              └──────┬──────┘
               │                            │
               └─────────────┬──────────────┘
                             │
                    ┌────────▼─────────┐
                    │    Continue...   │
                    └──────────────────┘
```

---

## 📊 Module Progress Tracker

```
[ ] 01_If_Else.md
    [ ] Read theory
    [ ] Completed code examples
    [ ] Solved practice questions
    
[ ] 02_Loops.md
    [ ] Read theory
    [ ] Completed code examples
    [ ] Solved practice questions
    
[ ] 03_Break_Continue_Pass.md
    [ ] Read theory
    [ ] Completed code examples
    [ ] Solved practice questions
```

---

## 🎯 Module Objectives Checklist

By the end of this module, you should be able to answer:

- [ ] What is the difference between `if` and `elif`?
- [ ] How does Python determine code blocks?
- [ ] What is the ternary operator?
- [ ] When should you use `for` vs `while` loops?
- [ ] What does `range()` do?
- [ ] When would you use `break` vs `continue`?
- [ ] What is the purpose of `pass`?
- [ ] Can loops have an `else` clause?

---

## 🔗 Quick Links to Subtopics

1. [If-Else Statements](./01_If_Else.md)
2. [Loops (for, while)](./02_Loops.md)
3. [Break, Continue, Pass](./03_Break_Continue_Pass.md)

---

## 📝 Quick Reference Card

```python
# IF-ELSE
if condition:
    # code
elif another_condition:
    # code
else:
    # code

# TERNARY OPERATOR
result = "Yes" if condition else "No"

# FOR LOOP
for item in sequence:
    # code

for i in range(5):      # 0, 1, 2, 3, 4
    print(i)

# WHILE LOOP
while condition:
    # code

# LOOP CONTROL
for i in range(10):
    if i == 3:
        continue    # Skip this iteration
    if i == 7:
        break       # Exit the loop
    print(i)

# PASS (placeholder)
if condition:
    pass  # Do nothing (for now)
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 3: Functions and Modules](../03_Functions_and_Modules/README.md)**
- Functions
- Arguments and Parameters
- Lambda Functions
- Modules and Packages

---

> 💪 **Remember:** Control flow is the logic of your program. Master it, and you can solve any programming problem!

---

*Happy Learning! 🚀*
