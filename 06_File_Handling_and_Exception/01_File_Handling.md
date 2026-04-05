# 📘 File Handling in Python

## 📌 Introduction

**File handling** allows Python programs to read from and write to files on disk. It's essential for data persistence, configuration, logging, and data processing.

Think of files like **notebooks** — you can open them, read their contents, write new information, and close them when done.

```
┌─────────────────────────────────────────────────────────────┐
│                    FILE OPERATIONS                           │
│                                                              │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐ │
│   │  Open   │ →  │  Read/  │ →  │  Close  │    │  With   │ │
│   │         │    │  Write  │    │         │ or │(auto)   │ │
│   └─────────┘    └─────────┘    └─────────┘    └─────────┘ │
│                                                              │
│   Text: 'r', 'w', 'a'          Binary: 'rb', 'wb', 'ab'    │
│                                                              │
│   Best Practice: Use 'with' statement for automatic close   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### File Modes

| Mode | Name | Description | Creates? | Truncates? |
|------|------|-------------|----------|------------|
| `r` | Read | Read only | No | No |
| `w` | Write | Write only | Yes | Yes |
| `a` | Append | Append only | Yes | No |
| `r+` | Read+ | Read and write | No | No |
| `w+` | Write+ | Write and read | Yes | Yes |
| `a+` | Append+ | Append and read | Yes | No |
| `rb` | Read Binary | Binary read | No | No |
| `wb` | Write Binary | Binary write | Yes | Yes |

### File Object Methods

| Method | Description |
|--------|-------------|
| `read()` | Read entire file |
| `read(n)` | Read n characters/bytes |
| `readline()` | Read one line |
| `readlines()` | Read all lines as list |
| `write(s)` | Write string |
| `writelines(lst)` | Write list of strings |
| `seek(pos)` | Move to position |
| `tell()` | Get current position |
| `close()` | Close file |

---

## 💻 Code Examples

### 🟢 Basic: Opening and Reading Files

```python
# Method 1: Manual open/close (NOT recommended)
file = open('example.txt', 'r')
content = file.read()
file.close()

# Method 2: Using 'with' (RECOMMENDED)
# Automatically closes file, even if error occurs
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
# File is automatically closed here

# Read entire file
with open('data.txt', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content)

# Read fixed number of characters
with open('data.txt', 'r') as f:
    chunk = f.read(100)  # Read first 100 characters
    print(chunk)
```

### 🟢 Basic: Reading Lines

```python
# Read one line
with open('data.txt', 'r') as f:
    first_line = f.readline()
    second_line = f.readline()
    print(f"First: {first_line.strip()}")
    print(f"Second: {second_line.strip()}")

# Read all lines as list
with open('data.txt', 'r') as f:
    lines = f.readlines()
    print(f"Total lines: {len(lines)}")
    for i, line in enumerate(lines):
        print(f"{i+1}: {line.strip()}")

# Best: Iterate directly (memory efficient)
with open('data.txt', 'r') as f:
    for line_number, line in enumerate(f, 1):
        print(f"{line_number}: {line.strip()}")
```

### 🟢 Basic: Writing to Files

```python
# Write mode - creates new or overwrites existing
with open('output.txt', 'w', encoding='utf-8') as f:
    f.write('Hello, World!\n')
    f.write('This is line 2\n')

# Write multiple lines
lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']
with open('output.txt', 'w') as f:
    f.writelines(lines)

# Append mode - adds to end of file
with open('log.txt', 'a') as f:
    f.write('New log entry\n')

# Write with print()
with open('output.txt', 'w') as f:
    print('Hello', file=f)
    print('World', file=f)
```

### 🟡 Intermediate: File Position

```python
with open('data.txt', 'r') as f:
    # Read first 10 characters
    print(f.read(10))
    
    # Check current position
    print(f"Position: {f.tell()}")  # 10
    
    # Go back to start
    f.seek(0)
    print(f"Position: {f.tell()}")  # 0
    
    # Read again
    print(f.read(5))
    
    # Go to specific position
    f.seek(20)
    print(f.read())  # Rest of file from position 20

# Seek modes
# seek(offset, whence)
# whence: 0 = start (default), 1 = current, 2 = end
with open('data.txt', 'rb') as f:  # Binary mode for seek(1) and seek(2)
    f.seek(0, 2)  # Go to end
    size = f.tell()  # Get file size
    print(f"File size: {size} bytes")
```

### 🟡 Intermediate: Working with Paths

```python
import os
from pathlib import Path

# Check if file exists
if os.path.exists('data.txt'):
    print("File exists!")

# Using pathlib (modern approach)
file_path = Path('data.txt')
if file_path.exists():
    print(f"Size: {file_path.stat().st_size} bytes")

# Get file information
path = Path('example.txt')
print(f"Name: {path.name}")           # example.txt
print(f"Stem: {path.stem}")           # example
print(f"Suffix: {path.suffix}")       # .txt
print(f"Parent: {path.parent}")       # .
print(f"Absolute: {path.absolute()}") # Full path

# Build paths safely
base = Path('/home/user')
file = base / 'documents' / 'report.txt'
print(file)  # /home/user/documents/report.txt

# List files in directory
for file in Path('.').glob('*.txt'):
    print(file)

# Recursive glob
for file in Path('.').rglob('*.py'):
    print(file)
```

### 🟡 Intermediate: Binary Files

```python
# Reading binary file
with open('image.png', 'rb') as f:
    data = f.read()
    print(f"Size: {len(data)} bytes")
    print(f"First 10 bytes: {data[:10]}")

# Writing binary file
with open('output.bin', 'wb') as f:
    f.write(b'\x00\x01\x02\x03\x04')

# Copy binary file
with open('source.png', 'rb') as src:
    with open('copy.png', 'wb') as dst:
        dst.write(src.read())

# Copy large file in chunks
def copy_file(src_path, dst_path, chunk_size=1024*1024):
    with open(src_path, 'rb') as src:
        with open(dst_path, 'wb') as dst:
            while True:
                chunk = src.read(chunk_size)
                if not chunk:
                    break
                dst.write(chunk)
```

### 🟡 Intermediate: CSV Files

```python
import csv

# Writing CSV
data = [
    ['Name', 'Age', 'City'],
    ['Alice', 25, 'New York'],
    ['Bob', 30, 'London'],
    ['Charlie', 35, 'Tokyo']
]

with open('people.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(data)

# Reading CSV
with open('people.csv', 'r', encoding='utf-8') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# CSV with dictionary
with open('people.csv', 'r') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(f"{row['Name']} is {row['Age']} years old")

# Writing with DictWriter
people = [
    {'Name': 'Alice', 'Age': 25, 'City': 'NY'},
    {'Name': 'Bob', 'Age': 30, 'City': 'LA'}
]

with open('people.csv', 'w', newline='') as f:
    fieldnames = ['Name', 'Age', 'City']
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(people)
```

### 🔴 Advanced: JSON Files

```python
import json

# Python object to JSON file
data = {
    "name": "Alice",
    "age": 25,
    "skills": ["Python", "JavaScript"],
    "active": True
}

with open('data.json', 'w') as f:
    json.dump(data, f, indent=2)

# JSON file to Python object
with open('data.json', 'r') as f:
    loaded = json.load(f)
    print(loaded['name'])  # Alice

# JSON with custom encoding
from datetime import datetime

class DateEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

data = {"timestamp": datetime.now()}
json_str = json.dumps(data, cls=DateEncoder)
print(json_str)

# Pretty print JSON
print(json.dumps(data, indent=4, sort_keys=True))
```

### 🔴 Advanced: Temporary Files

```python
import tempfile
import os

# Create temporary file
with tempfile.NamedTemporaryFile(mode='w', delete=False, suffix='.txt') as f:
    f.write('Temporary content')
    temp_path = f.name
    print(f"Created: {temp_path}")

# Use and clean up
try:
    with open(temp_path, 'r') as f:
        print(f.read())
finally:
    os.unlink(temp_path)

# Temporary directory
with tempfile.TemporaryDirectory() as tmpdir:
    filepath = os.path.join(tmpdir, 'test.txt')
    with open(filepath, 'w') as f:
        f.write('Hello!')
    # Directory and contents deleted after 'with' block
```

### 🔴 Advanced: File Locking

```python
import fcntl  # Unix only
import os

# File locking (Unix)
def write_with_lock(filepath, content):
    with open(filepath, 'a') as f:
        try:
            fcntl.flock(f.fileno(), fcntl.LOCK_EX)
            f.write(content)
        finally:
            fcntl.flock(f.fileno(), fcntl.LOCK_UN)

# Cross-platform locking using portalocker
# pip install portalocker
import portalocker

with open('shared.txt', 'a') as f:
    portalocker.lock(f, portalocker.LOCK_EX)
    f.write('Critical data\n')
    portalocker.unlock(f)
```

---

## 📊 File Methods Summary

| Method | Return | Description |
|--------|--------|-------------|
| `open(file, mode)` | file object | Open file |
| `f.read()` | str/bytes | Read all |
| `f.read(n)` | str/bytes | Read n chars |
| `f.readline()` | str | Read line |
| `f.readlines()` | list | All lines |
| `f.write(s)` | int | Write string |
| `f.writelines(lst)` | None | Write list |
| `f.seek(pos)` | int | Move position |
| `f.tell()` | int | Current position |
| `f.close()` | None | Close file |
| `f.closed` | bool | Is closed? |
| `f.name` | str | File name |
| `f.mode` | str | Open mode |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting to Close

```python
# WRONG
file = open('data.txt', 'r')
content = file.read()
# Forgot to close! Resource leak!

# CORRECT
with open('data.txt', 'r') as file:
    content = file.read()
# Automatically closed
```

### ❌ Mistake 2: Encoding Issues

```python
# WRONG - may fail on non-ASCII
with open('data.txt', 'r') as f:
    content = f.read()  # UnicodeDecodeError possible

# CORRECT
with open('data.txt', 'r', encoding='utf-8') as f:
    content = f.read()
```

### ❌ Mistake 3: Writing List Without Join

```python
# WRONG
lines = ['line1', 'line2', 'line3']
with open('out.txt', 'w') as f:
    f.write(lines)  # TypeError!

# CORRECT
with open('out.txt', 'w') as f:
    f.write('\n'.join(lines))

# OR
with open('out.txt', 'w') as f:
    f.writelines(line + '\n' for line in lines)
```

---

## 💡 Pro Tips

### 1. Always Use Context Managers

```python
with open('file.txt') as f:
    # Work with file
    pass
# Automatic cleanup
```

### 2. Use Pathlib for Paths

```python
from pathlib import Path
path = Path('dir') / 'subdir' / 'file.txt'
```

### 3. Read Large Files in Chunks

```python
with open('large.txt') as f:
    while chunk := f.read(8192):
        process(chunk)
```

---

## 💼 Interview Insight

### Q1: What's the difference between `read()` and `readline()`?

> - `read()`: Reads entire file (or n characters)
> - `readline()`: Reads one line at a time
> - For large files, `readline()` or iteration is more memory-efficient

### Q2: Why use `with` statement?

> `with` ensures proper cleanup even if an exception occurs. It calls `__exit__` which closes the file automatically.

### Q3: Difference between text and binary mode?

> - Text mode (`'r'`, `'w'`): Handles encoding, returns strings
> - Binary mode (`'rb'`, `'wb'`): Raw bytes, no encoding

---

## 🧪 Practice Questions

### Easy:
1. Read a file and count the number of lines
2. Write a list of names to a file
3. Copy contents from one file to another

### Medium:
4. Read a CSV and calculate statistics
5. Search for a word in a file and show line numbers
6. Merge multiple text files into one

### Hard:
7. Implement a log file rotator
8. Create a simple file-based database
9. Build a file diff utility

---

## 📖 Summary

| Concept | Best Practice |
|---------|---------------|
| Open files | Use `with` statement |
| Encoding | Specify `encoding='utf-8'` |
| Read large | Use iteration or chunks |
| Paths | Use `pathlib.Path` |
| Binary | Use `'rb'` / `'wb'` modes |
| CSV/JSON | Use `csv` and `json` modules |

---

## ⏭️ Next Topic

**[Exception Handling](./02_Exception_Handling.md)** — Handle errors gracefully!

---

*Files: Your program's memory beyond execution!* 📁
