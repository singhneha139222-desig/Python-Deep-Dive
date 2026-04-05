# 📘 Module 8: Libraries and Frameworks

## 📌 Overview

This module introduces Python's most powerful **libraries and frameworks** used in data science, web development, and visualization. These tools transform Python from a general-purpose language into a specialized powerhouse.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PYTHON ECOSYSTEM                                  │
│                                                                      │
│   ┌──────────────────────────────────────────────────────────────┐ │
│   │                     DATA SCIENCE                              │ │
│   │   ┌─────────┐   ┌─────────┐   ┌────────────┐                │ │
│   │   │  NumPy  │ → │ Pandas  │ → │ Matplotlib │                │ │
│   │   │ Arrays  │   │  Data   │   │   Charts   │                │ │
│   │   └─────────┘   └─────────┘   └────────────┘                │ │
│   └──────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   ┌──────────────────────────────────────────────────────────────┐ │
│   │                   WEB DEVELOPMENT                             │ │
│   │   ┌─────────┐                ┌─────────┐                     │ │
│   │   │  Flask  │   (Simple)     │ FastAPI │  (Modern)          │ │
│   │   │Micro-web│                │Async API│                     │ │
│   │   └─────────┘                └─────────┘                     │ │
│   └──────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

By the end of this module, you will be able to:

✅ Perform numerical computations with NumPy arrays  
✅ Analyze and manipulate data using Pandas DataFrames  
✅ Create professional visualizations with Matplotlib  
✅ Build web applications with Flask  
✅ Create modern APIs with FastAPI  

---

## 🗂️ Subtopics

| File | Topic | Description |
|------|-------|-------------|
| [01_NumPy.md](01_NumPy.md) | NumPy | Arrays, broadcasting, linear algebra |
| [02_Pandas.md](02_Pandas.md) | Pandas | DataFrames, data cleaning, analysis |
| [03_Matplotlib.md](03_Matplotlib.md) | Matplotlib | Plots, charts, visualization |
| [04_Flask.md](04_Flask.md) | Flask | Micro web framework, routing, templates |
| [05_FastAPI.md](05_FastAPI.md) | FastAPI | Modern APIs, async, automatic docs |

---

## 🚀 Learning Path

```
┌───────────────────────────────────────────────────────────────┐
│                    RECOMMENDED ORDER                           │
│                                                                │
│   DATA SCIENCE TRACK:                                         │
│   NumPy → Pandas → Matplotlib                                 │
│     ↓       ↓         ↓                                       │
│   Arrays  Tables    Charts                                    │
│                                                                │
│   WEB DEVELOPMENT TRACK:                                      │
│   Flask → FastAPI                                             │
│     ↓        ↓                                                │
│   Basics  Modern                                              │
│                                                                │
│   Or complete all: 01 → 02 → 03 → 04 → 05                    │
└───────────────────────────────────────────────────────────────┘
```

---

## 💡 Tips Before Starting

### Installation

```bash
# Data Science
pip install numpy pandas matplotlib

# Web Development
pip install flask fastapi uvicorn
```

### Prerequisites

- ✅ Python basics (variables, functions, loops)
- ✅ Data structures (lists, dictionaries)
- ✅ OOP concepts (classes, objects)
- ✅ File handling

### Best Practices

1. **Practice with real data** — Download datasets from Kaggle
2. **Build small projects** — Apply each library to real problems
3. **Read documentation** — Official docs are excellent
4. **Use Jupyter Notebooks** — Great for data exploration

---

## 🎯 Module Objectives Checklist

```
□ Create and manipulate NumPy arrays
□ Load and analyze data with Pandas
□ Create line, bar, scatter, and pie charts
□ Build a Flask web application
□ Create a REST API with FastAPI
□ Understand when to use each library
```

---

## 📊 Library Comparison

| Library | Best For | Type |
|---------|----------|------|
| NumPy | Numerical computing | Data Science |
| Pandas | Data analysis | Data Science |
| Matplotlib | Visualization | Data Science |
| Flask | Simple web apps | Web Dev |
| FastAPI | Modern APIs | Web Dev |

---

## 🔗 Quick Reference

### NumPy
```python
import numpy as np
arr = np.array([1, 2, 3])
arr.mean(), arr.sum(), arr.shape
```

### Pandas
```python
import pandas as pd
df = pd.read_csv('data.csv')
df.head(), df.describe(), df.groupby()
```

### Matplotlib
```python
import matplotlib.pyplot as plt
plt.plot(x, y)
plt.show()
```

### Flask
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello!"
```

### FastAPI
```python
from fastapi import FastAPI
app = FastAPI()

@app.get('/')
def home():
    return {"message": "Hello!"}
```

---

## ⏭️ What's Next?

After mastering these libraries, move to **[Module 9: Database and APIs](../09_Database_and_APIs/README.md)** to learn data persistence and API integration!

---

## ⬅️ Previous Module

**[Module 7: Advanced Python](../07_Advanced_Python/README.md)**

---

*Libraries transform Python from a language into a solution!* 📚
