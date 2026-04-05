# 📘 Module 9: Database and APIs

## 📌 Overview

This module covers **database operations** and **API integration** — essential skills for building real-world applications. You'll learn to store data persistently and communicate with external services.

```
┌────────────────────────────────────────────────────────────────────┐
│                   DATA PERSISTENCE & APIs                          │
│                                                                    │
│   ┌────────────────────────────────────────────────────────────┐  │
│   │                    DATABASES                                │  │
│   │                                                             │  │
│   │   ┌───────────┐         ┌───────────┐                      │  │
│   │   │  SQLite   │         │   MySQL   │                      │  │
│   │   │  (File)   │         │ (Server)  │                      │  │
│   │   └─────┬─────┘         └─────┬─────┘                      │  │
│   │         │                     │                             │  │
│   │         └─────────┬───────────┘                             │  │
│   │                   ▼                                         │  │
│   │            ┌───────────┐                                   │  │
│   │            │   CRUD    │  Create, Read, Update, Delete     │  │
│   │            └───────────┘                                   │  │
│   └────────────────────────────────────────────────────────────┘  │
│                                                                    │
│   ┌────────────────────────────────────────────────────────────┐  │
│   │                       APIs                                  │  │
│   │                                                             │  │
│   │   Your App  ←──REST API──→  External Services              │  │
│   │      │                           │                          │  │
│   │   requests                  JSON/XML                        │  │
│   │   library                   responses                       │  │
│   └────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

By the end of this module, you will be able to:

✅ Work with SQLite databases (file-based)  
✅ Connect to MySQL databases (server-based)  
✅ Perform CRUD operations (Create, Read, Update, Delete)  
✅ Understand REST API principles  
✅ Consume external APIs with the requests library  

---

## 🗂️ Subtopics

| File | Topic | Description |
|------|-------|-------------|
| [01_SQLite.md](01_SQLite.md) | SQLite | Lightweight file-based database |
| [02_MySQL.md](02_MySQL.md) | MySQL | Production-ready server database |
| [03_CRUD.md](03_CRUD.md) | CRUD Operations | Create, Read, Update, Delete |
| [04_REST_API.md](04_REST_API.md) | REST API | API design principles |
| [05_Requests_Library.md](05_Requests_Library.md) | Requests | HTTP client for API calls |

---

## 🚀 Learning Path

```
┌───────────────────────────────────────────────────────────────┐
│                    RECOMMENDED ORDER                           │
│                                                                │
│   DATABASE TRACK:                                             │
│   SQLite → MySQL → CRUD                                       │
│     ↓       ↓        ↓                                        │
│   Basics  Server   Operations                                 │
│                                                                │
│   API TRACK:                                                  │
│   REST API → Requests Library                                 │
│      ↓           ↓                                            │
│   Concepts    Practice                                        │
│                                                                │
│   Complete path: 01 → 02 → 03 → 04 → 05                      │
└───────────────────────────────────────────────────────────────┘
```

---

## 💡 Tips Before Starting

### Database Selection

| Database | Use Case |
|----------|----------|
| SQLite | Learning, small apps, prototypes |
| MySQL | Production, multi-user, scalable |
| PostgreSQL | Complex queries, data integrity |
| MongoDB | Flexible schemas, documents |

### Prerequisites

- ✅ Python basics
- ✅ Data structures (dictionaries)
- ✅ Functions and modules
- ✅ Basic OOP

### Installation

```bash
# SQLite - Built into Python
import sqlite3

# MySQL
pip install mysql-connector-python

# Requests
pip install requests
```

---

## 🎯 Module Objectives Checklist

```
□ Create and connect to SQLite database
□ Write SQL queries (SELECT, INSERT, UPDATE, DELETE)
□ Connect to MySQL server
□ Design database schemas
□ Implement CRUD operations
□ Understand REST API methods
□ Make HTTP requests with Python
□ Handle API responses and errors
```

---

## 📊 Database vs API Comparison

| Aspect | Database | API |
|--------|----------|-----|
| Purpose | Store data | Transfer data |
| Location | Your server | External service |
| Protocol | SQL | HTTP |
| Format | Tables | JSON/XML |
| Access | Direct | Remote calls |

---

## 🔗 Quick Reference

### SQLite
```python
import sqlite3
conn = sqlite3.connect('database.db')
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
```

### MySQL
```python
import mysql.connector
conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="pass",
    database="mydb"
)
```

### Requests
```python
import requests
response = requests.get('https://api.example.com/data')
data = response.json()
```

---

## ⏭️ What's Next?

After completing this module, you'll be ready for **[Module 10: Projects and Interview Prep](../10_Projects_and_Interview_Prep/README.md)** — Apply everything you've learned!

---

## ⬅️ Previous Module

**[Module 8: Libraries and Frameworks](../08_Libraries_and_Frameworks/README.md)**

---

*Databases store your data, APIs connect your world!* 💾
