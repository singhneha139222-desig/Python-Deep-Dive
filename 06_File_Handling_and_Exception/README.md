# 📘 Module 6: File Handling and Exceptions

Welcome to **Module 6**! This module covers two essential skills: working with files and handling errors gracefully. These are crucial for building robust, real-world applications.

---

## 📌 Overview of This Module

Programs need to interact with the file system and handle unexpected situations. This module teaches you to read/write files and manage errors professionally.

```
┌─────────────────────────────────────────────────────────────┐
│              FILE HANDLING & EXCEPTIONS                      │
│                                                              │
│   FILE HANDLING                    EXCEPTION HANDLING        │
│   ┌─────────────────────┐         ┌─────────────────────┐   │
│   │  Open → Read/Write  │         │   try:              │   │
│   │         ↓           │         │       risky_code()  │   │
│   │       Close         │         │   except:           │   │
│   └─────────────────────┘         │       handle_error()│   │
│                                   │   finally:          │   │
│   Modes: r, w, a, rb, wb          │       cleanup()     │   │
│   Best: with statement            └─────────────────────┘   │
│                                                              │
│   "Better to ask forgiveness than permission" - EAFP        │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 What You Will Learn

| Skill | Description |
|-------|-------------|
| ✅ File Operations | Open, read, write, close files |
| ✅ File Modes | Text, binary, append modes |
| ✅ Context Managers | Automatic resource cleanup |
| ✅ Exception Handling | try/except/finally blocks |
| ✅ Error Types | Built-in exception hierarchy |
| ✅ Custom Exceptions | Create domain-specific errors |

---

## 🗂️ Subtopics in This Module

| File | Topic | Description |
|------|-------|-------------|
| `01_File_Handling.md` | **File Handling** | Open, read, write, close, modes, paths |
| `02_Exception_Handling.md` | **Exception Handling** | try/except, finally, else, raising |
| `03_Custom_Exceptions.md` | **Custom Exceptions** | Creating your own exception classes |

---

## 🚀 Recommended Learning Path

```
Step 1: 01_File_Handling.md
        ↓
        Read, write, manage files
        
Step 2: 02_Exception_Handling.md
        ↓
        Handle errors gracefully
        
Step 3: 03_Custom_Exceptions.md
        ↓
        Create domain-specific exceptions
```

**Estimated Time:** 4-5 hours (with practice)

---

## 📊 Quick Comparison

### File Modes

| Mode | Description | Create? | Truncate? | Position |
|------|-------------|---------|-----------|----------|
| `r` | Read | No | No | Start |
| `w` | Write | Yes | Yes | Start |
| `a` | Append | Yes | No | End |
| `r+` | Read/Write | No | No | Start |
| `w+` | Read/Write | Yes | Yes | Start |
| `a+` | Read/Append | Yes | No | End |

### Common Exceptions

| Exception | When |
|-----------|------|
| `FileNotFoundError` | File doesn't exist |
| `PermissionError` | No access rights |
| `ValueError` | Wrong value type |
| `TypeError` | Wrong operation type |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |

---

## 💡 Tips Before Starting

### File Handling Best Practices:

1. **Always use `with`** — Automatic cleanup
2. **Specify encoding** — Use `encoding='utf-8'`
3. **Check paths** — Use `pathlib` or `os.path`
4. **Handle errors** — Wrap file ops in try/except

### Exception Handling Best Practices:

1. **Be specific** — Catch specific exceptions
2. **Don't silence** — Log or re-raise errors
3. **Clean up** — Use `finally` for cleanup
4. **Fail fast** — Validate inputs early

---

## 🎯 Module Objectives

By the end of this module, you should be able to:

- [ ] Read and write text and binary files
- [ ] Use context managers for safe file handling
- [ ] Handle common exceptions appropriately
- [ ] Create and raise custom exceptions
- [ ] Build robust error-handling strategies
- [ ] Use logging instead of print for errors

---

## 🔗 Quick Links

1. [File Handling](./01_File_Handling.md)
2. [Exception Handling](./02_Exception_Handling.md)
3. [Custom Exceptions](./03_Custom_Exceptions.md)

---

## 📝 Quick Reference

```python
# FILE HANDLING
# Read entire file
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# Write to file
with open('file.txt', 'w', encoding='utf-8') as f:
    f.write('Hello, World!')

# Read line by line
with open('file.txt', 'r') as f:
    for line in f:
        print(line.strip())

# EXCEPTION HANDLING
try:
    result = risky_operation()
except ValueError as e:
    print(f"Value error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
else:
    print("Success!")
finally:
    print("Cleanup")

# CUSTOM EXCEPTION
class ValidationError(Exception):
    def __init__(self, field, message):
        self.field = field
        super().__init__(f"{field}: {message}")

raise ValidationError("email", "Invalid format")
```

---

## ⏭️ What's Next?

After completing this module, you will move to:

**[Module 7: Advanced Python](../07_Advanced_Python/README.md)**
- Decorators
- Generators
- Context Managers
- Threading and Multiprocessing

---

> 💪 **Remember:** Good error handling separates amateur code from professional code!

---

*Happy Learning! 🚀*
