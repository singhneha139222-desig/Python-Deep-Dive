# 📘 MySQL - Server Database

## 📌 Introduction

**MySQL** is a powerful, open-source relational database management system (RDBMS). Unlike SQLite, MySQL runs as a server and can handle multiple connections, making it ideal for production applications.

Think of MySQL as a **professional data warehouse** — multiple users can access it simultaneously, and it scales to handle massive datasets!

```
┌────────────────────────────────────────────────────────────────┐
│                      MySQL ARCHITECTURE                         │
│                                                                 │
│   Client Apps                                                  │
│   ┌─────┐ ┌─────┐ ┌─────┐                                     │
│   │ App │ │ App │ │ App │  Multiple connections                │
│   └──┬──┘ └──┬──┘ └──┬──┘                                     │
│      │       │       │                                          │
│      └───────┼───────┘                                          │
│              ▼                                                  │
│   ┌──────────────────────────┐                                 │
│   │     MySQL Server         │                                 │
│   │  ┌────────────────────┐ │                                 │
│   │  │  Connection Pool   │ │  Handles connections            │
│   │  └────────────────────┘ │                                 │
│   │  ┌────────────────────┐ │                                 │
│   │  │   Query Optimizer  │ │  Optimizes queries              │
│   │  └────────────────────┘ │                                 │
│   │  ┌────────────────────┐ │                                 │
│   │  │   Storage Engine   │ │  Manages data                   │
│   │  └────────────────────┘ │                                 │
│   └──────────────────────────┘                                 │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### MySQL vs SQLite

| Feature | MySQL | SQLite |
|---------|-------|--------|
| Type | Server-based | File-based |
| Setup | Requires installation | Built-in |
| Concurrency | Excellent | Limited |
| Scale | Very large | Small to medium |
| Users | Multiple | Single |

### Installation

```bash
# Install MySQL connector for Python
pip install mysql-connector-python

# Alternative: PyMySQL (pure Python)
pip install pymysql
```

### MySQL Server Setup

1. Download from mysql.com
2. Or use Docker: `docker run -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 mysql`

---

## 💻 Code Examples

### 🟢 Basic: Connecting to MySQL

```python
import mysql.connector

# Connect to MySQL server
conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password"
)

print("Connected to MySQL!")

cursor = conn.cursor()

# Show databases
cursor.execute("SHOW DATABASES")
for db in cursor:
    print(db)

conn.close()
```

### 🟢 Basic: Creating Database and Tables

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password"
)
cursor = conn.cursor()

# Create database
cursor.execute("CREATE DATABASE IF NOT EXISTS company")
cursor.execute("USE company")

# Create table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS employees (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100) NOT NULL,
        email VARCHAR(100) UNIQUE,
        department VARCHAR(50),
        salary DECIMAL(10, 2),
        hire_date DATE,
        is_active BOOLEAN DEFAULT TRUE,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    )
''')

print("Database and table created!")

conn.commit()
conn.close()

# MySQL Data Types:
# INT, BIGINT - Integers
# DECIMAL(p,s) - Precise decimals (money)
# VARCHAR(n) - Variable-length string
# TEXT - Long text
# DATE, DATETIME, TIMESTAMP - Date/time
# BOOLEAN - True/False
# BLOB - Binary data
```

### 🟢 Basic: Inserting Data

```python
import mysql.connector
from datetime import date

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor()

# Single insert
cursor.execute('''
    INSERT INTO employees (name, email, department, salary, hire_date)
    VALUES (%s, %s, %s, %s, %s)
''', ('Alice Johnson', 'alice@company.com', 'Engineering', 75000.00, date(2023, 1, 15)))

# Multiple insert
employees = [
    ('Bob Smith', 'bob@company.com', 'Marketing', 65000.00, date(2023, 3, 20)),
    ('Charlie Brown', 'charlie@company.com', 'Engineering', 80000.00, date(2022, 8, 10)),
    ('Diana Prince', 'diana@company.com', 'HR', 60000.00, date(2023, 6, 1))
]

cursor.executemany('''
    INSERT INTO employees (name, email, department, salary, hire_date)
    VALUES (%s, %s, %s, %s, %s)
''', employees)

conn.commit()
print(f"Inserted {cursor.rowcount} rows")
print(f"Last inserted ID: {cursor.lastrowid}")

conn.close()
```

### 🟡 Intermediate: Querying Data

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor()

# Basic select
cursor.execute("SELECT * FROM employees")
rows = cursor.fetchall()

for row in rows:
    print(row)

# Select with conditions
cursor.execute('''
    SELECT name, department, salary
    FROM employees
    WHERE salary > %s AND department = %s
''', (70000, 'Engineering'))

# Order and limit
cursor.execute('''
    SELECT name, salary
    FROM employees
    ORDER BY salary DESC
    LIMIT 5
''')
top_5 = cursor.fetchall()
print("Top 5 salaries:", top_5)

# Using dictionary cursor (returns dicts)
cursor = conn.cursor(dictionary=True)
cursor.execute("SELECT * FROM employees WHERE id = %s", (1,))
employee = cursor.fetchone()
if employee:
    print(f"Name: {employee['name']}, Salary: ${employee['salary']:,.2f}")

# Named tuple cursor
cursor = conn.cursor(named_tuple=True)
cursor.execute("SELECT * FROM employees")
for emp in cursor:
    print(f"{emp.name} works in {emp.department}")

conn.close()
```

### 🟡 Intermediate: Updating and Deleting

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor()

# Update
cursor.execute('''
    UPDATE employees
    SET salary = salary * 1.10
    WHERE department = %s
''', ('Engineering',))
print(f"Updated {cursor.rowcount} employees")

# Conditional update
cursor.execute('''
    UPDATE employees
    SET salary = CASE
        WHEN salary < 60000 THEN salary * 1.15
        WHEN salary < 80000 THEN salary * 1.10
        ELSE salary * 1.05
    END
''')

# Delete
cursor.execute('''
    DELETE FROM employees
    WHERE is_active = %s
''', (False,))
print(f"Deleted {cursor.rowcount} inactive employees")

# Soft delete (recommended)
cursor.execute('''
    UPDATE employees
    SET is_active = FALSE
    WHERE id = %s
''', (5,))

conn.commit()
conn.close()
```

### 🟡 Intermediate: Prepared Statements

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor(prepared=True)

# Prepare statement once
insert_stmt = '''
    INSERT INTO employees (name, email, department, salary, hire_date)
    VALUES (%s, %s, %s, %s, %s)
'''

# Execute multiple times efficiently
employees = [
    ('Eve Wilson', 'eve@company.com', 'Sales', 58000.00, '2024-01-10'),
    ('Frank Miller', 'frank@company.com', 'Engineering', 72000.00, '2024-02-15')
]

for emp in employees:
    cursor.execute(insert_stmt, emp)

conn.commit()
conn.close()
```

### 🔴 Advanced: Joins and Complex Queries

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor(dictionary=True)

# Setup additional tables
cursor.execute('''
    CREATE TABLE IF NOT EXISTS departments (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(50) UNIQUE NOT NULL,
        budget DECIMAL(15, 2)
    )
''')

cursor.execute('''
    CREATE TABLE IF NOT EXISTS projects (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        department_id INT,
        employee_id INT,
        deadline DATE,
        FOREIGN KEY (department_id) REFERENCES departments(id),
        FOREIGN KEY (employee_id) REFERENCES employees(id)
    )
''')
conn.commit()

# INNER JOIN
cursor.execute('''
    SELECT 
        e.name AS employee_name,
        d.name AS department_name,
        e.salary
    FROM employees e
    INNER JOIN departments d ON e.department = d.name
''')

# LEFT JOIN with aggregation
cursor.execute('''
    SELECT 
        d.name AS department,
        COUNT(e.id) AS employee_count,
        AVG(e.salary) AS avg_salary,
        SUM(e.salary) AS total_salary
    FROM departments d
    LEFT JOIN employees e ON d.name = e.department
    GROUP BY d.id, d.name
    ORDER BY total_salary DESC
''')
dept_stats = cursor.fetchall()
for stat in dept_stats:
    print(f"{stat['department']}: {stat['employee_count']} employees, "
          f"Avg: ${stat['avg_salary']:,.2f}")

# Subquery
cursor.execute('''
    SELECT name, salary
    FROM employees
    WHERE salary > (
        SELECT AVG(salary) FROM employees
    )
    ORDER BY salary DESC
''')

# Common Table Expression (CTE)
cursor.execute('''
    WITH dept_avg AS (
        SELECT department, AVG(salary) as avg_sal
        FROM employees
        GROUP BY department
    )
    SELECT e.name, e.department, e.salary, da.avg_sal
    FROM employees e
    JOIN dept_avg da ON e.department = da.department
    WHERE e.salary > da.avg_sal
''')

conn.close()
```

### 🔴 Advanced: Transactions

```python
import mysql.connector
from mysql.connector import Error

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)
cursor = conn.cursor()

def transfer_budget(from_dept, to_dept, amount):
    try:
        # Start transaction
        conn.start_transaction()
        
        # Check source budget
        cursor.execute(
            "SELECT budget FROM departments WHERE name = %s FOR UPDATE",
            (from_dept,)
        )
        result = cursor.fetchone()
        
        if not result or result[0] < amount:
            raise ValueError("Insufficient budget")
        
        # Deduct from source
        cursor.execute(
            "UPDATE departments SET budget = budget - %s WHERE name = %s",
            (amount, from_dept)
        )
        
        # Add to destination
        cursor.execute(
            "UPDATE departments SET budget = budget + %s WHERE name = %s",
            (amount, to_dept)
        )
        
        # Commit transaction
        conn.commit()
        print(f"Transferred ${amount:,.2f} from {from_dept} to {to_dept}")
        
    except Error as e:
        conn.rollback()
        print(f"Transaction failed: {e}")
    except ValueError as e:
        conn.rollback()
        print(f"Transfer failed: {e}")

# Test
transfer_budget('Engineering', 'Marketing', 10000)

conn.close()
```

### 🔴 Advanced: Connection Pooling

```python
import mysql.connector
from mysql.connector import pooling

# Create connection pool
pool = pooling.MySQLConnectionPool(
    pool_name="mypool",
    pool_size=5,  # Number of connections
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)

def get_all_employees():
    conn = pool.get_connection()
    try:
        cursor = conn.cursor(dictionary=True)
        cursor.execute("SELECT * FROM employees")
        return cursor.fetchall()
    finally:
        conn.close()  # Returns to pool

def get_employee(emp_id):
    conn = pool.get_connection()
    try:
        cursor = conn.cursor(dictionary=True)
        cursor.execute("SELECT * FROM employees WHERE id = %s", (emp_id,))
        return cursor.fetchone()
    finally:
        conn.close()

# Usage
employees = get_all_employees()
emp = get_employee(1)
```

### 🔴 Advanced: Database Manager Class

```python
import mysql.connector
from mysql.connector import Error, pooling
from contextlib import contextmanager

class MySQLManager:
    def __init__(self, host, user, password, database, pool_size=5):
        self.pool = pooling.MySQLConnectionPool(
            pool_name="app_pool",
            pool_size=pool_size,
            host=host,
            user=user,
            password=password,
            database=database
        )
    
    @contextmanager
    def get_connection(self):
        conn = self.pool.get_connection()
        try:
            yield conn
        finally:
            conn.close()
    
    @contextmanager
    def get_cursor(self, dictionary=True):
        with self.get_connection() as conn:
            cursor = conn.cursor(dictionary=dictionary)
            try:
                yield cursor
                conn.commit()
            except Error as e:
                conn.rollback()
                raise e
    
    def execute(self, query, params=None):
        with self.get_cursor() as cursor:
            cursor.execute(query, params or ())
            return cursor.rowcount
    
    def fetchall(self, query, params=None):
        with self.get_cursor() as cursor:
            cursor.execute(query, params or ())
            return cursor.fetchall()
    
    def fetchone(self, query, params=None):
        with self.get_cursor() as cursor:
            cursor.execute(query, params or ())
            return cursor.fetchone()
    
    def insert(self, table, data):
        columns = ', '.join(data.keys())
        placeholders = ', '.join(['%s'] * len(data))
        query = f"INSERT INTO {table} ({columns}) VALUES ({placeholders})"
        
        with self.get_cursor() as cursor:
            cursor.execute(query, list(data.values()))
            return cursor.lastrowid

# Usage
db = MySQLManager(
    host="localhost",
    user="root",
    password="your_password",
    database="company"
)

# Insert
new_id = db.insert("employees", {
    "name": "Grace Lee",
    "email": "grace@company.com",
    "department": "Engineering",
    "salary": 78000.00
})
print(f"Inserted employee with ID: {new_id}")

# Query
employees = db.fetchall("SELECT * FROM employees WHERE department = %s", ("Engineering",))
for emp in employees:
    print(f"{emp['name']}: ${emp['salary']:,.2f}")
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Using Wrong Placeholder

```python
# WRONG - SQLite style
cursor.execute("SELECT * FROM users WHERE id = ?", (1,))

# CORRECT - MySQL style
cursor.execute("SELECT * FROM users WHERE id = %s", (1,))
```

### ❌ Mistake 2: Not Using Connection Pooling

```python
# WRONG - Creates new connection every time
def get_data():
    conn = mysql.connector.connect(...)
    # ... operations ...
    conn.close()

# CORRECT - Use connection pool
pool = pooling.MySQLConnectionPool(...)
def get_data():
    conn = pool.get_connection()
    # ... operations ...
    conn.close()  # Returns to pool
```

---

## 💡 Pro Tips

### 1. Use Indexes for Better Performance

```sql
CREATE INDEX idx_department ON employees(department);
CREATE INDEX idx_email ON employees(email);
```

### 2. Use EXPLAIN to Optimize Queries

```python
cursor.execute("EXPLAIN SELECT * FROM employees WHERE department = 'Engineering'")
print(cursor.fetchall())
```

### 3. Configure Connection Timeout

```python
conn = mysql.connector.connect(
    connection_timeout=10,
    autocommit=False,
    ...
)
```

---

## 💼 Interview Insight

### Q1: MySQL vs PostgreSQL?

> MySQL is faster for read-heavy operations and simpler to set up. PostgreSQL has better support for complex queries, JSON, and data integrity. Choose based on your needs.

### Q2: How do you prevent SQL injection?

> Always use parameterized queries with placeholders (%s) instead of string formatting. Never interpolate user input directly into SQL.

---

## 🧪 Practice Questions

### Easy:
1. Connect to MySQL and create a database
2. Create a table with various data types
3. Insert and query data

### Medium:
4. Write JOIN queries across tables
5. Implement transaction handling
6. Create a connection pool

### Hard:
7. Build a complete database manager class
8. Optimize queries using indexes
9. Implement data migrations

---

## 📖 Summary

| Operation | MySQL Syntax |
|-----------|-------------|
| Connect | `mysql.connector.connect(...)` |
| Create DB | `CREATE DATABASE name` |
| Create Table | `CREATE TABLE name (...)` |
| Insert | `INSERT INTO ... VALUES (%s, ...)` |
| Select | `SELECT * FROM ... WHERE ...` |
| Join | `SELECT ... FROM t1 JOIN t2 ON ...` |
| Transaction | `start_transaction()`, `commit()`, `rollback()` |

---

## ⏭️ Next Topic

**[CRUD Operations](03_CRUD.md)** — Master all database operations!

---

*MySQL: Where professional data lives!* 🗄️
