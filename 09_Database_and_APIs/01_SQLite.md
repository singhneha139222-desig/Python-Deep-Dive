# 📘 SQLite - Lightweight Database

## 📌 Introduction

**SQLite** is a self-contained, serverless, file-based relational database. It comes built into Python's standard library, making it perfect for learning, prototyping, and small applications.

Think of SQLite as a **database in a single file** — no server setup, no configuration, just create and go!

```
┌────────────────────────────────────────────────────────────────┐
│                      SQLite ARCHITECTURE                        │
│                                                                 │
│   Your Python App                                              │
│        │                                                        │
│        ▼                                                        │
│   ┌─────────────────┐                                          │
│   │   sqlite3       │  Python's built-in module                │
│   │   module        │                                          │
│   └────────┬────────┘                                          │
│            │                                                    │
│            ▼                                                    │
│   ┌─────────────────┐                                          │
│   │  database.db    │  Single file stores everything           │
│   │                 │  - Tables                                │
│   │                 │  - Data                                  │
│   │                 │  - Indexes                               │
│   └─────────────────┘                                          │
│                                                                 │
│   No server needed! Just file operations.                      │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### SQLite vs Server Databases

| Feature | SQLite | MySQL/PostgreSQL |
|---------|--------|------------------|
| Setup | None | Server required |
| Storage | Single file | Server storage |
| Concurrency | Limited | High |
| Size limit | ~140 TB | Larger |
| Best for | Small apps, learning | Production |

### SQL Basics

SQL (Structured Query Language) is used to interact with databases:

| Command | Purpose |
|---------|---------|
| CREATE | Create tables |
| SELECT | Read data |
| INSERT | Add data |
| UPDATE | Modify data |
| DELETE | Remove data |
| DROP | Delete tables |

---

## 💻 Code Examples

### 🟢 Basic: Connecting to Database

```python
import sqlite3

# Create/connect to database
# Creates file if doesn't exist
conn = sqlite3.connect('example.db')

# Create cursor object
cursor = conn.cursor()

print("Database connected!")

# Always close connection when done
conn.close()

# Using context manager (recommended)
with sqlite3.connect('example.db') as conn:
    cursor = conn.cursor()
    # Do operations
    # Auto-commits and closes
```

### 🟢 Basic: Creating Tables

```python
import sqlite3

conn = sqlite3.connect('school.db')
cursor = conn.cursor()

# Create table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS students (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        age INTEGER,
        grade REAL,
        enrolled_date TEXT
    )
''')

# Multiple columns with constraints
cursor.execute('''
    CREATE TABLE IF NOT EXISTS courses (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT UNIQUE NOT NULL,
        credits INTEGER DEFAULT 3,
        department TEXT
    )
''')

# Foreign key relationship
cursor.execute('''
    CREATE TABLE IF NOT EXISTS enrollments (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        student_id INTEGER NOT NULL,
        course_id INTEGER NOT NULL,
        semester TEXT,
        FOREIGN KEY (student_id) REFERENCES students(id),
        FOREIGN KEY (course_id) REFERENCES courses(id)
    )
''')

conn.commit()
print("Tables created!")
conn.close()

# Data types in SQLite:
# INTEGER - Whole numbers
# REAL - Floating point numbers
# TEXT - String/text
# BLOB - Binary data
# NULL - No value
```

### 🟢 Basic: Inserting Data

```python
import sqlite3

conn = sqlite3.connect('school.db')
cursor = conn.cursor()

# Insert single row
cursor.execute('''
    INSERT INTO students (name, age, grade)
    VALUES ('Alice', 20, 85.5)
''')

# Insert with parameters (safe - prevents SQL injection)
student_data = ('Bob', 22, 90.0)
cursor.execute('''
    INSERT INTO students (name, age, grade)
    VALUES (?, ?, ?)
''', student_data)

# Insert multiple rows
students = [
    ('Charlie', 19, 78.5),
    ('Diana', 21, 92.0),
    ('Eve', 20, 88.0)
]
cursor.executemany('''
    INSERT INTO students (name, age, grade)
    VALUES (?, ?, ?)
''', students)

# Commit changes
conn.commit()
print(f"Inserted {cursor.rowcount} rows")
print(f"Last row ID: {cursor.lastrowid}")

conn.close()
```

### 🟡 Intermediate: Querying Data

```python
import sqlite3

conn = sqlite3.connect('school.db')
cursor = conn.cursor()

# Select all
cursor.execute('SELECT * FROM students')
all_students = cursor.fetchall()
print("All students:", all_students)

# Select specific columns
cursor.execute('SELECT name, grade FROM students')
names_grades = cursor.fetchall()

# Fetch one row
cursor.execute('SELECT * FROM students')
first = cursor.fetchone()
print("First student:", first)

# Fetch many rows
cursor.execute('SELECT * FROM students')
some = cursor.fetchmany(3)  # Get 3 rows

# Using WHERE clause
cursor.execute('SELECT * FROM students WHERE age > ?', (20,))
older_students = cursor.fetchall()
print("Students over 20:", older_students)

# Multiple conditions
cursor.execute('''
    SELECT * FROM students 
    WHERE age >= ? AND grade >= ?
''', (20, 85))

# LIKE for pattern matching
cursor.execute("SELECT * FROM students WHERE name LIKE 'A%'")
a_names = cursor.fetchall()  # Names starting with 'A'

# ORDER BY
cursor.execute('SELECT * FROM students ORDER BY grade DESC')
by_grade = cursor.fetchall()

# LIMIT
cursor.execute('SELECT * FROM students ORDER BY grade DESC LIMIT 3')
top_3 = cursor.fetchall()

# Get column names
cursor.execute('SELECT * FROM students')
column_names = [description[0] for description in cursor.description]
print("Columns:", column_names)

conn.close()
```

### 🟡 Intermediate: Updating and Deleting

```python
import sqlite3

conn = sqlite3.connect('school.db')
cursor = conn.cursor()

# Update single record
cursor.execute('''
    UPDATE students 
    SET grade = ? 
    WHERE name = ?
''', (95.0, 'Alice'))
print(f"Updated {cursor.rowcount} rows")

# Update multiple records
cursor.execute('''
    UPDATE students 
    SET grade = grade + 5 
    WHERE grade < 80
''')

# Conditional update
cursor.execute('''
    UPDATE students 
    SET grade = CASE
        WHEN grade >= 90 THEN grade + 2
        WHEN grade >= 80 THEN grade + 3
        ELSE grade + 5
    END
''')

# Delete specific record
cursor.execute('DELETE FROM students WHERE name = ?', ('Eve',))
print(f"Deleted {cursor.rowcount} rows")

# Delete multiple records
cursor.execute('DELETE FROM students WHERE grade < ?', (60,))

# Delete all records (be careful!)
# cursor.execute('DELETE FROM students')

conn.commit()
conn.close()
```

### 🟡 Intermediate: Row Factory

```python
import sqlite3

conn = sqlite3.connect('school.db')

# Return rows as dictionaries
def dict_factory(cursor, row):
    return {col[0]: row[idx] for idx, col in enumerate(cursor.description)}

conn.row_factory = dict_factory
cursor = conn.cursor()

cursor.execute('SELECT * FROM students')
students = cursor.fetchall()

for student in students:
    print(f"Name: {student['name']}, Grade: {student['grade']}")

# Built-in Row object
conn2 = sqlite3.connect('school.db')
conn2.row_factory = sqlite3.Row
cursor2 = conn2.cursor()

cursor2.execute('SELECT * FROM students')
row = cursor2.fetchone()

# Access by name or index
print(row['name'])     # By column name
print(row[1])          # By index
print(dict(row))       # Convert to dict
print(row.keys())      # Get column names

conn.close()
conn2.close()
```

### 🔴 Advanced: Transactions

```python
import sqlite3

conn = sqlite3.connect('bank.db')
cursor = conn.cursor()

# Create accounts table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS accounts (
        id INTEGER PRIMARY KEY,
        name TEXT,
        balance REAL
    )
''')

# Insert test data
cursor.execute("INSERT OR IGNORE INTO accounts VALUES (1, 'Alice', 1000)")
cursor.execute("INSERT OR IGNORE INTO accounts VALUES (2, 'Bob', 500)")
conn.commit()

def transfer_money(from_id, to_id, amount):
    try:
        # Start transaction (implicit)
        
        # Check balance
        cursor.execute('SELECT balance FROM accounts WHERE id = ?', (from_id,))
        balance = cursor.fetchone()[0]
        
        if balance < amount:
            raise ValueError("Insufficient funds")
        
        # Deduct from sender
        cursor.execute('''
            UPDATE accounts 
            SET balance = balance - ? 
            WHERE id = ?
        ''', (amount, from_id))
        
        # Add to receiver
        cursor.execute('''
            UPDATE accounts 
            SET balance = balance + ? 
            WHERE id = ?
        ''', (amount, to_id))
        
        # Commit transaction
        conn.commit()
        print(f"Transferred ${amount} successfully")
        
    except Exception as e:
        # Rollback on error
        conn.rollback()
        print(f"Transfer failed: {e}")

# Test transfer
transfer_money(1, 2, 200)

# Check balances
cursor.execute('SELECT * FROM accounts')
print(cursor.fetchall())

conn.close()
```

### 🔴 Advanced: Complex Queries

```python
import sqlite3

conn = sqlite3.connect('school.db')
cursor = conn.cursor()

# Setup tables
cursor.executescript('''
    CREATE TABLE IF NOT EXISTS departments (
        id INTEGER PRIMARY KEY,
        name TEXT
    );
    
    CREATE TABLE IF NOT EXISTS teachers (
        id INTEGER PRIMARY KEY,
        name TEXT,
        department_id INTEGER,
        salary REAL
    );
    
    INSERT OR IGNORE INTO departments VALUES (1, 'Math');
    INSERT OR IGNORE INTO departments VALUES (2, 'Science');
    
    INSERT OR IGNORE INTO teachers VALUES (1, 'Dr. Smith', 1, 75000);
    INSERT OR IGNORE INTO teachers VALUES (2, 'Prof. Jones', 2, 80000);
    INSERT OR IGNORE INTO teachers VALUES (3, 'Dr. Brown', 1, 70000);
''')

# JOIN - Combine tables
cursor.execute('''
    SELECT teachers.name, departments.name as department, salary
    FROM teachers
    JOIN departments ON teachers.department_id = departments.id
''')
print("Teachers with departments:", cursor.fetchall())

# LEFT JOIN - Include all from left table
cursor.execute('''
    SELECT teachers.name, departments.name
    FROM teachers
    LEFT JOIN departments ON teachers.department_id = departments.id
''')

# Aggregate functions
cursor.execute('''
    SELECT 
        COUNT(*) as total,
        AVG(salary) as avg_salary,
        MAX(salary) as max_salary,
        MIN(salary) as min_salary,
        SUM(salary) as total_salary
    FROM teachers
''')
print("Statistics:", cursor.fetchone())

# GROUP BY
cursor.execute('''
    SELECT department_id, COUNT(*), AVG(salary)
    FROM teachers
    GROUP BY department_id
''')
print("By department:", cursor.fetchall())

# HAVING (filter groups)
cursor.execute('''
    SELECT department_id, AVG(salary) as avg_sal
    FROM teachers
    GROUP BY department_id
    HAVING avg_sal > 70000
''')

# Subquery
cursor.execute('''
    SELECT name, salary
    FROM teachers
    WHERE salary > (SELECT AVG(salary) FROM teachers)
''')
print("Above average salary:", cursor.fetchall())

conn.close()
```

### 🔴 Advanced: Database Manager Class

```python
import sqlite3
from contextlib import contextmanager

class DatabaseManager:
    def __init__(self, db_name):
        self.db_name = db_name
    
    @contextmanager
    def get_connection(self):
        conn = sqlite3.connect(self.db_name)
        conn.row_factory = sqlite3.Row
        try:
            yield conn
        finally:
            conn.close()
    
    def execute(self, query, params=None):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            if params:
                cursor.execute(query, params)
            else:
                cursor.execute(query)
            conn.commit()
            return cursor
    
    def fetchall(self, query, params=None):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            if params:
                cursor.execute(query, params)
            else:
                cursor.execute(query)
            return [dict(row) for row in cursor.fetchall()]
    
    def fetchone(self, query, params=None):
        with self.get_connection() as conn:
            cursor = conn.cursor()
            if params:
                cursor.execute(query, params)
            else:
                cursor.execute(query)
            row = cursor.fetchone()
            return dict(row) if row else None

# Usage
db = DatabaseManager('app.db')

# Create table
db.execute('''
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT UNIQUE,
        email TEXT
    )
''')

# Insert
db.execute(
    'INSERT INTO users (username, email) VALUES (?, ?)',
    ('john', 'john@example.com')
)

# Query
users = db.fetchall('SELECT * FROM users')
print(users)

user = db.fetchone('SELECT * FROM users WHERE username = ?', ('john',))
print(user)
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: SQL Injection

```python
# WRONG - Vulnerable to SQL injection
username = "admin'; DROP TABLE users; --"
cursor.execute(f"SELECT * FROM users WHERE name = '{username}'")

# CORRECT - Use parameterized queries
cursor.execute("SELECT * FROM users WHERE name = ?", (username,))
```

### ❌ Mistake 2: Not Committing Changes

```python
# WRONG - Changes not saved
cursor.execute("INSERT INTO users VALUES (1, 'John')")
# Forgot conn.commit()

# CORRECT
cursor.execute("INSERT INTO users VALUES (1, 'John')")
conn.commit()  # Save changes
```

### ❌ Mistake 3: Not Closing Connection

```python
# WRONG
conn = sqlite3.connect('db.sqlite')
# ... operations ...
# Forgot to close

# CORRECT - Use context manager
with sqlite3.connect('db.sqlite') as conn:
    # ... operations ...
    pass  # Auto-closes
```

---

## 💡 Pro Tips

### 1. Use executescript for Multiple Statements

```python
cursor.executescript('''
    DROP TABLE IF EXISTS old_table;
    CREATE TABLE new_table (id INTEGER, name TEXT);
    INSERT INTO new_table VALUES (1, 'Test');
''')
```

### 2. Enable Foreign Keys

```python
cursor.execute("PRAGMA foreign_keys = ON")
```

### 3. Backup Database

```python
import shutil
shutil.copy('database.db', 'backup.db')
```

---

## 💼 Interview Insight

### Q1: What is SQL injection?

> SQL injection is when malicious SQL is inserted into queries via user input. Prevent it by using parameterized queries with placeholders (?) instead of string formatting.

### Q2: When would you use SQLite vs MySQL?

> SQLite for small apps, embedded systems, or development. MySQL for production with multiple users, complex queries, and data integrity requirements.

---

## 🧪 Practice Questions

### Easy:
1. Create a database with a "products" table
2. Insert 5 products and query all
3. Update product prices

### Medium:
4. Create related tables with foreign keys
5. Write a JOIN query
6. Implement transaction handling

### Hard:
7. Build a database manager class
8. Create a data migration script
9. Implement connection pooling

---

## 📖 Summary

| Operation | SQL Command |
|-----------|-------------|
| Create table | `CREATE TABLE name (...)` |
| Insert | `INSERT INTO table VALUES (...)` |
| Select | `SELECT * FROM table WHERE ...` |
| Update | `UPDATE table SET ... WHERE ...` |
| Delete | `DELETE FROM table WHERE ...` |
| Join | `SELECT * FROM t1 JOIN t2 ON ...` |

---

## ⏭️ Next Topic

**[MySQL - Server Database](02_MySQL.md)** — Production-ready databases!

---

*SQLite: Your data in a file!* 📁
