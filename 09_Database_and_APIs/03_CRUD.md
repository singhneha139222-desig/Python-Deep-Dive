# 📘 CRUD Operations

## 📌 Introduction

**CRUD** stands for **Create, Read, Update, Delete** — the four basic operations for managing data in any storage system. These operations form the foundation of virtually all database applications.

```
┌────────────────────────────────────────────────────────────────┐
│                      CRUD OPERATIONS                            │
│                                                                 │
│   ┌──────────────┐   ┌──────────────┐                          │
│   │   CREATE     │   │    READ      │                          │
│   │   INSERT     │   │   SELECT     │                          │
│   │   Add new    │   │   Retrieve   │                          │
│   │   records    │   │   records    │                          │
│   └──────────────┘   └──────────────┘                          │
│                                                                 │
│   ┌──────────────┐   ┌──────────────┐                          │
│   │   UPDATE     │   │   DELETE     │                          │
│   │   Modify     │   │   Remove     │                          │
│   │   existing   │   │   records    │                          │
│   │   records    │   │              │                          │
│   └──────────────┘   └──────────────┘                          │
│                                                                 │
│   HTTP Methods: POST   GET         PUT/PATCH    DELETE         │
│   SQL:          INSERT SELECT      UPDATE       DELETE         │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### CRUD Mapping

| Operation | SQL Command | HTTP Method | Description |
|-----------|-------------|-------------|-------------|
| Create | INSERT | POST | Add new records |
| Read | SELECT | GET | Retrieve records |
| Update | UPDATE | PUT/PATCH | Modify records |
| Delete | DELETE | DELETE | Remove records |

---

## 💻 Code Examples

### 🟢 Basic: Complete CRUD Example

```python
import sqlite3
from datetime import datetime

class Database:
    def __init__(self, db_name):
        self.db_name = db_name
        self._init_db()
    
    def _get_connection(self):
        conn = sqlite3.connect(self.db_name)
        conn.row_factory = sqlite3.Row
        return conn
    
    def _init_db(self):
        with self._get_connection() as conn:
            conn.execute('''
                CREATE TABLE IF NOT EXISTS users (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    name TEXT NOT NULL,
                    email TEXT UNIQUE NOT NULL,
                    age INTEGER,
                    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
                    updated_at TEXT
                )
            ''')

# Initialize database
db = Database('crud_example.db')
```

### 🟢 Basic: CREATE Operation

```python
class Database:
    # ... previous code ...
    
    def create_user(self, name, email, age=None):
        """
        CREATE - Insert a new user
        Returns: ID of created user
        """
        with self._get_connection() as conn:
            cursor = conn.execute('''
                INSERT INTO users (name, email, age)
                VALUES (?, ?, ?)
            ''', (name, email, age))
            conn.commit()
            return cursor.lastrowid
    
    def create_users_bulk(self, users):
        """
        CREATE - Bulk insert multiple users
        users: list of tuples (name, email, age)
        Returns: Number of users created
        """
        with self._get_connection() as conn:
            cursor = conn.executemany('''
                INSERT INTO users (name, email, age)
                VALUES (?, ?, ?)
            ''', users)
            conn.commit()
            return cursor.rowcount

# Usage
db = Database('crud_example.db')

# Create single user
user_id = db.create_user("Alice", "alice@example.com", 25)
print(f"Created user with ID: {user_id}")

# Create multiple users
new_users = [
    ("Bob", "bob@example.com", 30),
    ("Charlie", "charlie@example.com", 35),
    ("Diana", "diana@example.com", 28)
]
count = db.create_users_bulk(new_users)
print(f"Created {count} users")
```

### 🟡 Intermediate: READ Operation

```python
class Database:
    # ... previous code ...
    
    def get_user(self, user_id):
        """
        READ - Get single user by ID
        """
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT * FROM users WHERE id = ?', (user_id,)
            )
            row = cursor.fetchone()
            return dict(row) if row else None
    
    def get_user_by_email(self, email):
        """
        READ - Get user by email
        """
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT * FROM users WHERE email = ?', (email,)
            )
            row = cursor.fetchone()
            return dict(row) if row else None
    
    def get_all_users(self, limit=None, offset=0):
        """
        READ - Get all users with pagination
        """
        with self._get_connection() as conn:
            if limit:
                cursor = conn.execute(
                    'SELECT * FROM users LIMIT ? OFFSET ?', (limit, offset)
                )
            else:
                cursor = conn.execute('SELECT * FROM users')
            return [dict(row) for row in cursor.fetchall()]
    
    def search_users(self, name_query=None, min_age=None, max_age=None):
        """
        READ - Search users with filters
        """
        query = 'SELECT * FROM users WHERE 1=1'
        params = []
        
        if name_query:
            query += ' AND name LIKE ?'
            params.append(f'%{name_query}%')
        
        if min_age is not None:
            query += ' AND age >= ?'
            params.append(min_age)
        
        if max_age is not None:
            query += ' AND age <= ?'
            params.append(max_age)
        
        with self._get_connection() as conn:
            cursor = conn.execute(query, params)
            return [dict(row) for row in cursor.fetchall()]
    
    def count_users(self):
        """
        READ - Count total users
        """
        with self._get_connection() as conn:
            cursor = conn.execute('SELECT COUNT(*) FROM users')
            return cursor.fetchone()[0]

# Usage
db = Database('crud_example.db')

# Get single user
user = db.get_user(1)
print(f"User: {user}")

# Get all users with pagination
page1 = db.get_all_users(limit=10, offset=0)   # First 10
page2 = db.get_all_users(limit=10, offset=10)  # Next 10

# Search users
results = db.search_users(name_query="Ali", min_age=20, max_age=30)
print(f"Search results: {results}")

# Count
total = db.count_users()
print(f"Total users: {total}")
```

### 🟡 Intermediate: UPDATE Operation

```python
class Database:
    # ... previous code ...
    
    def update_user(self, user_id, **kwargs):
        """
        UPDATE - Modify user fields
        Only updates provided fields
        """
        if not kwargs:
            return 0
        
        # Build dynamic update query
        fields = ', '.join(f'{k} = ?' for k in kwargs.keys())
        values = list(kwargs.values())
        values.append(datetime.now().isoformat())  # updated_at
        values.append(user_id)
        
        with self._get_connection() as conn:
            cursor = conn.execute(f'''
                UPDATE users
                SET {fields}, updated_at = ?
                WHERE id = ?
            ''', values)
            conn.commit()
            return cursor.rowcount
    
    def update_user_email(self, user_id, new_email):
        """
        UPDATE - Change user's email
        """
        return self.update_user(user_id, email=new_email)
    
    def increment_age(self, user_id):
        """
        UPDATE - Increment age by 1
        """
        with self._get_connection() as conn:
            cursor = conn.execute('''
                UPDATE users
                SET age = age + 1, updated_at = ?
                WHERE id = ?
            ''', (datetime.now().isoformat(), user_id))
            conn.commit()
            return cursor.rowcount
    
    def bulk_update_age(self, user_ids, new_age):
        """
        UPDATE - Update multiple users at once
        """
        placeholders = ','.join('?' * len(user_ids))
        with self._get_connection() as conn:
            cursor = conn.execute(f'''
                UPDATE users
                SET age = ?, updated_at = ?
                WHERE id IN ({placeholders})
            ''', [new_age, datetime.now().isoformat()] + user_ids)
            conn.commit()
            return cursor.rowcount

# Usage
db = Database('crud_example.db')

# Update single field
db.update_user(1, name="Alice Smith")

# Update multiple fields
db.update_user(1, name="Alice Johnson", age=26)

# Specific update methods
db.update_user_email(1, "alice.new@example.com")
db.increment_age(1)

# Bulk update
db.bulk_update_age([1, 2, 3], 30)
```

### 🟡 Intermediate: DELETE Operation

```python
class Database:
    # ... previous code ...
    
    def delete_user(self, user_id):
        """
        DELETE - Remove user by ID (hard delete)
        """
        with self._get_connection() as conn:
            cursor = conn.execute(
                'DELETE FROM users WHERE id = ?', (user_id,)
            )
            conn.commit()
            return cursor.rowcount > 0
    
    def delete_users_by_ids(self, user_ids):
        """
        DELETE - Remove multiple users
        """
        placeholders = ','.join('?' * len(user_ids))
        with self._get_connection() as conn:
            cursor = conn.execute(
                f'DELETE FROM users WHERE id IN ({placeholders})', user_ids
            )
            conn.commit()
            return cursor.rowcount
    
    def delete_inactive_users(self, days_inactive=30):
        """
        DELETE - Remove users inactive for X days
        """
        with self._get_connection() as conn:
            cursor = conn.execute('''
                DELETE FROM users
                WHERE updated_at < datetime('now', ? || ' days')
                OR (updated_at IS NULL AND created_at < datetime('now', ? || ' days'))
            ''', (f'-{days_inactive}', f'-{days_inactive}'))
            conn.commit()
            return cursor.rowcount

# Usage
db = Database('crud_example.db')

# Delete single user
deleted = db.delete_user(5)
print(f"Deleted: {deleted}")

# Delete multiple users
count = db.delete_users_by_ids([6, 7, 8])
print(f"Deleted {count} users")

# Delete old users
count = db.delete_inactive_users(90)
print(f"Deleted {count} inactive users")
```

### 🔴 Advanced: Soft Delete Pattern

```python
class Database:
    def _init_db(self):
        with self._get_connection() as conn:
            conn.execute('''
                CREATE TABLE IF NOT EXISTS users (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    name TEXT NOT NULL,
                    email TEXT UNIQUE NOT NULL,
                    age INTEGER,
                    is_active BOOLEAN DEFAULT 1,
                    deleted_at TEXT,
                    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
                    updated_at TEXT
                )
            ''')
    
    def soft_delete_user(self, user_id):
        """
        SOFT DELETE - Mark as inactive instead of removing
        """
        with self._get_connection() as conn:
            cursor = conn.execute('''
                UPDATE users
                SET is_active = 0, deleted_at = ?
                WHERE id = ? AND is_active = 1
            ''', (datetime.now().isoformat(), user_id))
            conn.commit()
            return cursor.rowcount > 0
    
    def restore_user(self, user_id):
        """
        RESTORE - Reactivate soft-deleted user
        """
        with self._get_connection() as conn:
            cursor = conn.execute('''
                UPDATE users
                SET is_active = 1, deleted_at = NULL, updated_at = ?
                WHERE id = ?
            ''', (datetime.now().isoformat(), user_id))
            conn.commit()
            return cursor.rowcount > 0
    
    def get_all_users(self, include_deleted=False):
        """
        READ - Get users (optionally including deleted)
        """
        query = 'SELECT * FROM users'
        if not include_deleted:
            query += ' WHERE is_active = 1'
        
        with self._get_connection() as conn:
            cursor = conn.execute(query)
            return [dict(row) for row in cursor.fetchall()]
    
    def permanently_delete_user(self, user_id):
        """
        HARD DELETE - Actually remove from database
        """
        with self._get_connection() as conn:
            cursor = conn.execute(
                'DELETE FROM users WHERE id = ?', (user_id,)
            )
            conn.commit()
            return cursor.rowcount > 0

# Usage
db = Database('crud_example.db')

# Soft delete (recommended)
db.soft_delete_user(1)  # Still in DB, just marked inactive

# Get active users only
active_users = db.get_all_users()

# Get all users including deleted
all_users = db.get_all_users(include_deleted=True)

# Restore deleted user
db.restore_user(1)

# Permanently delete (careful!)
db.permanently_delete_user(1)
```

### 🔴 Advanced: Complete Repository Pattern

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass
from typing import Optional, List
from datetime import datetime
import sqlite3

@dataclass
class User:
    id: Optional[int] = None
    name: str = ""
    email: str = ""
    age: Optional[int] = None
    is_active: bool = True
    created_at: Optional[str] = None
    updated_at: Optional[str] = None

class UserRepository:
    """
    Repository pattern for User CRUD operations
    Abstracts database operations from business logic
    """
    
    def __init__(self, db_path: str):
        self.db_path = db_path
        self._init_db()
    
    def _get_connection(self):
        conn = sqlite3.connect(self.db_path)
        conn.row_factory = sqlite3.Row
        return conn
    
    def _init_db(self):
        with self._get_connection() as conn:
            conn.execute('''
                CREATE TABLE IF NOT EXISTS users (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    name TEXT NOT NULL,
                    email TEXT UNIQUE NOT NULL,
                    age INTEGER,
                    is_active BOOLEAN DEFAULT 1,
                    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
                    updated_at TEXT
                )
            ''')
    
    def _row_to_user(self, row) -> User:
        if row is None:
            return None
        return User(
            id=row['id'],
            name=row['name'],
            email=row['email'],
            age=row['age'],
            is_active=bool(row['is_active']),
            created_at=row['created_at'],
            updated_at=row['updated_at']
        )
    
    # CREATE
    def create(self, user: User) -> User:
        with self._get_connection() as conn:
            cursor = conn.execute('''
                INSERT INTO users (name, email, age)
                VALUES (?, ?, ?)
            ''', (user.name, user.email, user.age))
            conn.commit()
            user.id = cursor.lastrowid
            return user
    
    # READ
    def find_by_id(self, user_id: int) -> Optional[User]:
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT * FROM users WHERE id = ? AND is_active = 1',
                (user_id,)
            )
            return self._row_to_user(cursor.fetchone())
    
    def find_by_email(self, email: str) -> Optional[User]:
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT * FROM users WHERE email = ? AND is_active = 1',
                (email,)
            )
            return self._row_to_user(cursor.fetchone())
    
    def find_all(self, limit: int = 100, offset: int = 0) -> List[User]:
        with self._get_connection() as conn:
            cursor = conn.execute('''
                SELECT * FROM users WHERE is_active = 1
                ORDER BY id LIMIT ? OFFSET ?
            ''', (limit, offset))
            return [self._row_to_user(row) for row in cursor.fetchall()]
    
    # UPDATE
    def update(self, user: User) -> bool:
        with self._get_connection() as conn:
            cursor = conn.execute('''
                UPDATE users
                SET name = ?, email = ?, age = ?, updated_at = ?
                WHERE id = ? AND is_active = 1
            ''', (user.name, user.email, user.age, 
                  datetime.now().isoformat(), user.id))
            conn.commit()
            return cursor.rowcount > 0
    
    # DELETE
    def delete(self, user_id: int) -> bool:
        with self._get_connection() as conn:
            cursor = conn.execute('''
                UPDATE users SET is_active = 0, updated_at = ?
                WHERE id = ?
            ''', (datetime.now().isoformat(), user_id))
            conn.commit()
            return cursor.rowcount > 0
    
    def exists(self, user_id: int) -> bool:
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT 1 FROM users WHERE id = ? AND is_active = 1',
                (user_id,)
            )
            return cursor.fetchone() is not None
    
    def count(self) -> int:
        with self._get_connection() as conn:
            cursor = conn.execute(
                'SELECT COUNT(*) FROM users WHERE is_active = 1'
            )
            return cursor.fetchone()[0]

# Usage
repo = UserRepository('app.db')

# Create
new_user = User(name="Alice", email="alice@example.com", age=25)
created = repo.create(new_user)
print(f"Created user ID: {created.id}")

# Read
user = repo.find_by_id(1)
print(f"Found: {user}")

all_users = repo.find_all(limit=10)
print(f"Total: {len(all_users)} users")

# Update
user.name = "Alice Smith"
user.age = 26
repo.update(user)

# Delete
repo.delete(user.id)
print(f"Deleted user {user.id}")
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Not Validating Before Update

```python
# WRONG - Blindly updates
def update_user(self, user_id, **kwargs):
    # What if user doesn't exist?
    self.execute("UPDATE users SET ...", ...)

# CORRECT - Check first
def update_user(self, user_id, **kwargs):
    if not self.find_by_id(user_id):
        raise ValueError(f"User {user_id} not found")
    self.execute("UPDATE users SET ...", ...)
```

### ❌ Mistake 2: Hard Delete by Default

```python
# WRONG - Data is lost forever
def delete_user(self, user_id):
    self.execute("DELETE FROM users WHERE id = ?", (user_id,))

# CORRECT - Use soft delete
def delete_user(self, user_id):
    self.execute("UPDATE users SET is_active = 0 WHERE id = ?", (user_id,))
```

---

## 💡 Pro Tips

### 1. Always Use Transactions for Multiple Operations

```python
def transfer_data(self, from_id, to_id):
    conn = self._get_connection()
    try:
        conn.execute("BEGIN")
        # Multiple operations...
        conn.execute("COMMIT")
    except:
        conn.execute("ROLLBACK")
        raise
```

### 2. Return Affected Rows

```python
def update_user(self, user_id, **kwargs):
    # Return rowcount to know if update succeeded
    return cursor.rowcount > 0
```

---

## 💼 Interview Insight

### Q1: What's the difference between PUT and PATCH?

> PUT replaces the entire resource (requires all fields), while PATCH partially updates specific fields. For updates, PATCH is usually more efficient.

### Q2: Why use soft delete?

> Soft delete preserves data for auditing, allows recovery, maintains referential integrity, and provides better data history tracking.

---

## 🧪 Practice Questions

### Easy:
1. Implement basic CRUD for a "products" table
2. Add pagination to read operations
3. Implement search with filters

### Medium:
4. Add soft delete to your CRUD operations
5. Implement bulk operations (create many, delete many)
6. Add transaction support

### Hard:
7. Build a complete repository pattern
8. Implement optimistic locking for updates
9. Add audit logging for all CRUD operations

---

## 📖 Summary

| Operation | SQL | HTTP | Use Case |
|-----------|-----|------|----------|
| Create | INSERT | POST | Add new records |
| Read | SELECT | GET | Retrieve records |
| Update | UPDATE | PUT/PATCH | Modify records |
| Delete | DELETE | DELETE | Remove records |

---

## ⏭️ Next Topic

**[REST API Principles](04_REST_API.md)** — Design professional APIs!

---

*CRUD: The foundation of all data operations!* 📝
