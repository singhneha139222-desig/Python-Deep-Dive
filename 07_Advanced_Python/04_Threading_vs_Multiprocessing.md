# 📘 Threading vs Multiprocessing in Python

## 📌 Introduction

**Concurrency** is about dealing with multiple tasks at once. Python offers several approaches: **threading** for I/O-bound tasks and **multiprocessing** for CPU-bound tasks.

Think of it like a restaurant:
- **Threading** = One chef handling multiple orders (switching between tasks)
- **Multiprocessing** = Multiple chefs working independently

```
┌─────────────────────────────────────────────────────────────┐
│            THREADING vs MULTIPROCESSING                      │
│                                                              │
│   THREADING                      MULTIPROCESSING             │
│   ┌─────────────────────┐       ┌─────────────────────┐    │
│   │ Process             │       │ Process 1 │Process 2│    │
│   │ ┌───┐ ┌───┐ ┌───┐  │       │ ┌───────┐│┌───────┐│    │
│   │ │T1 │ │T2 │ │T3 │  │       │ │ Code  │││ Code  ││    │
│   │ └───┘ └───┘ └───┘  │       │ │ Data  │││ Data  ││    │
│   │   Shared Memory     │       │ └───────┘│└───────┘│    │
│   └─────────────────────┘       └─────────────────────┘    │
│                                                              │
│   ✓ I/O-bound               ✓ CPU-bound                    │
│   ✓ Lightweight             ✓ True parallelism             │
│   ✗ GIL limits CPU          ✗ Higher memory                │
│   ✓ Shared memory           ✗ Process overhead             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### The GIL (Global Interpreter Lock)

The GIL allows only one thread to execute Python bytecode at a time. This affects threading but not multiprocessing.

| Feature | Threading | Multiprocessing |
|---------|-----------|-----------------|
| GIL | Blocked | Bypassed |
| Best for | I/O-bound | CPU-bound |
| Memory | Shared | Separate |
| Overhead | Low | High |
| Communication | Easy | IPC needed |

### When to Use What

| Task Type | Example | Solution |
|-----------|---------|----------|
| I/O-bound | HTTP requests, file I/O | Threading, asyncio |
| CPU-bound | Data processing, calculations | Multiprocessing |
| Mixed | Web scraping | Combine both |

---

## 💻 Code Examples

### 🟢 Basic: Threading

```python
import threading
import time

def task(name, delay):
    print(f"Task {name} starting")
    time.sleep(delay)
    print(f"Task {name} completed")

# Create threads
t1 = threading.Thread(target=task, args=("A", 2))
t2 = threading.Thread(target=task, args=("B", 1))

# Start threads
t1.start()
t2.start()

# Wait for completion
t1.join()
t2.join()

print("All tasks done")

# Output (concurrent):
# Task A starting
# Task B starting
# Task B completed (after 1s)
# Task A completed (after 2s)
# All tasks done
```

### 🟢 Basic: Multiprocessing

```python
import multiprocessing
import time

def task(name, delay):
    print(f"Task {name} starting (PID: {multiprocessing.current_process().pid})")
    time.sleep(delay)
    print(f"Task {name} completed")

if __name__ == "__main__":  # Required on Windows!
    # Create processes
    p1 = multiprocessing.Process(target=task, args=("A", 2))
    p2 = multiprocessing.Process(target=task, args=("B", 1))
    
    # Start processes
    p1.start()
    p2.start()
    
    # Wait for completion
    p1.join()
    p2.join()
    
    print("All tasks done")
```

### 🟡 Intermediate: Thread Pool

```python
from concurrent.futures import ThreadPoolExecutor
import requests
import time

def fetch_url(url):
    response = requests.get(url)
    return f"{url}: {len(response.content)} bytes"

urls = [
    "https://www.python.org",
    "https://www.google.com",
    "https://www.github.com",
    "https://www.stackoverflow.com"
]

# Sequential (slow)
start = time.time()
for url in urls:
    print(fetch_url(url))
print(f"Sequential: {time.time() - start:.2f}s")

# Threaded (fast)
start = time.time()
with ThreadPoolExecutor(max_workers=4) as executor:
    results = executor.map(fetch_url, urls)
    for result in results:
        print(result)
print(f"Threaded: {time.time() - start:.2f}s")
```

### 🟡 Intermediate: Process Pool

```python
from concurrent.futures import ProcessPoolExecutor
import time

def cpu_intensive(n):
    """CPU-bound task"""
    return sum(i * i for i in range(n))

numbers = [10_000_000, 10_000_000, 10_000_000, 10_000_000]

if __name__ == "__main__":
    # Sequential
    start = time.time()
    results = [cpu_intensive(n) for n in numbers]
    print(f"Sequential: {time.time() - start:.2f}s")
    
    # Parallel with processes
    start = time.time()
    with ProcessPoolExecutor(max_workers=4) as executor:
        results = list(executor.map(cpu_intensive, numbers))
    print(f"Parallel: {time.time() - start:.2f}s")
```

### 🟡 Intermediate: Thread Synchronization

```python
import threading

# Without lock - race condition
counter = 0

def increment_unsafe():
    global counter
    for _ in range(100000):
        counter += 1

threads = [threading.Thread(target=increment_unsafe) for _ in range(5)]
for t in threads: t.start()
for t in threads: t.join()
print(f"Unsafe counter: {counter}")  # Less than 500000!

# With lock - safe
counter = 0
lock = threading.Lock()

def increment_safe():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

threads = [threading.Thread(target=increment_safe) for _ in range(5)]
for t in threads: t.start()
for t in threads: t.join()
print(f"Safe counter: {counter}")  # Exactly 500000
```

### 🟡 Intermediate: Thread-Safe Queue

```python
import threading
import queue
import time

# Producer-Consumer pattern
task_queue = queue.Queue()
results = []

def producer():
    for i in range(10):
        task_queue.put(i)
        print(f"Produced: {i}")
        time.sleep(0.1)
    task_queue.put(None)  # Signal to stop

def consumer():
    while True:
        item = task_queue.get()
        if item is None:
            break
        result = item * item
        results.append(result)
        print(f"Consumed: {item} -> {result}")
        task_queue.task_done()

producer_thread = threading.Thread(target=producer)
consumer_thread = threading.Thread(target=consumer)

producer_thread.start()
consumer_thread.start()

producer_thread.join()
consumer_thread.join()

print(f"Results: {results}")
```

### 🔴 Advanced: Process Communication

```python
from multiprocessing import Process, Queue, Pipe
import time

# Using Queue
def producer_queue(q):
    for i in range(5):
        q.put(i)
        print(f"Produced: {i}")
    q.put(None)

def consumer_queue(q):
    while True:
        item = q.get()
        if item is None:
            break
        print(f"Consumed: {item}")

if __name__ == "__main__":
    q = Queue()
    p1 = Process(target=producer_queue, args=(q,))
    p2 = Process(target=consumer_queue, args=(q,))
    
    p1.start()
    p2.start()
    p1.join()
    p2.join()

# Using Pipe
def sender(conn):
    for i in range(5):
        conn.send(i)
    conn.send(None)
    conn.close()

def receiver(conn):
    while True:
        item = conn.recv()
        if item is None:
            break
        print(f"Received: {item}")

if __name__ == "__main__":
    parent_conn, child_conn = Pipe()
    p1 = Process(target=sender, args=(child_conn,))
    p2 = Process(target=receiver, args=(parent_conn,))
    
    p1.start()
    p2.start()
    p1.join()
    p2.join()
```

### 🔴 Advanced: Shared Memory

```python
from multiprocessing import Process, Value, Array
import ctypes

def increment_shared(counter, lock):
    for _ in range(10000):
        with lock:
            counter.value += 1

def modify_array(arr):
    for i in range(len(arr)):
        arr[i] = arr[i] * 2

if __name__ == "__main__":
    from multiprocessing import Lock
    
    # Shared counter
    counter = Value('i', 0)  # 'i' = integer
    lock = Lock()
    
    processes = [
        Process(target=increment_shared, args=(counter, lock))
        for _ in range(4)
    ]
    
    for p in processes: p.start()
    for p in processes: p.join()
    
    print(f"Counter: {counter.value}")  # 40000
    
    # Shared array
    arr = Array('d', [1.0, 2.0, 3.0, 4.0])  # 'd' = double
    p = Process(target=modify_array, args=(arr,))
    p.start()
    p.join()
    print(f"Array: {list(arr)}")  # [2.0, 4.0, 6.0, 8.0]
```

### 🔴 Advanced: asyncio (Bonus)

```python
import asyncio

async def fetch_data(name, delay):
    print(f"Fetching {name}...")
    await asyncio.sleep(delay)  # Non-blocking sleep
    print(f"Fetched {name}")
    return f"Data from {name}"

async def main():
    # Run concurrently
    tasks = [
        fetch_data("API1", 2),
        fetch_data("API2", 1),
        fetch_data("API3", 3)
    ]
    
    results = await asyncio.gather(*tasks)
    print(f"Results: {results}")

# Run async main
asyncio.run(main())

# Output (concurrent):
# Fetching API1...
# Fetching API2...
# Fetching API3...
# Fetched API2 (after 1s)
# Fetched API1 (after 2s)
# Fetched API3 (after 3s)
# Total time: ~3s (not 6s)
```

---

## 📊 Comparison Table

| Feature | Threading | Multiprocessing | asyncio |
|---------|-----------|-----------------|---------|
| Best for | I/O-bound | CPU-bound | Many I/O |
| Memory | Shared | Separate | Single-threaded |
| GIL | Blocked | Bypassed | Single-threaded |
| Overhead | Low | High | Lowest |
| Complexity | Medium | High | High |
| Debugging | Harder | Easier | Medium |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Using Threading for CPU Tasks

```python
# WRONG - GIL limits parallelism
import threading

def cpu_task(n):
    return sum(i*i for i in range(n))

# This won't be faster with threads due to GIL!
threads = [threading.Thread(target=cpu_task, args=(10000000,)) for _ in range(4)]

# CORRECT - Use multiprocessing
from multiprocessing import Pool

with Pool(4) as p:
    results = p.map(cpu_task, [10000000] * 4)
```

### ❌ Mistake 2: Race Conditions

```python
# WRONG - race condition
counter = 0
def unsafe_increment():
    global counter
    counter += 1  # Not atomic!

# CORRECT - use lock
from threading import Lock
lock = Lock()

def safe_increment():
    global counter
    with lock:
        counter += 1
```

### ❌ Mistake 3: Missing if __name__ Guard

```python
# WRONG on Windows - will spawn infinitely
from multiprocessing import Process

def worker():
    print("Working")

p = Process(target=worker)
p.start()

# CORRECT
if __name__ == "__main__":
    p = Process(target=worker)
    p.start()
```

---

## 💡 Pro Tips

### 1. Use concurrent.futures for Simple Cases

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# Easy interface for both
with ThreadPoolExecutor() as executor:
    results = executor.map(func, items)
```

### 2. Choose Based on Task Type

```python
# I/O-bound: ThreadPoolExecutor
# CPU-bound: ProcessPoolExecutor
# Many I/O: asyncio
```

### 3. Avoid Sharing State When Possible

```python
# Instead of shared state, return results
def process(item):
    return item * 2

with ProcessPoolExecutor() as ex:
    results = list(ex.map(process, items))
```

---

## 💼 Interview Insight

### Q1: What is the GIL?

> The Global Interpreter Lock is a mutex that allows only one thread to execute Python bytecode at a time in CPython. It simplifies memory management but limits true parallelism in multi-threaded programs.

### Q2: When to use threading vs multiprocessing?

> - **Threading**: I/O-bound tasks (network requests, file I/O)
> - **Multiprocessing**: CPU-bound tasks (calculations, data processing)
> - **asyncio**: Many concurrent I/O operations

### Q3: How to share data between processes?

> Use `multiprocessing.Queue`, `Pipe`, `Value`, `Array`, or `Manager`. These provide IPC (Inter-Process Communication) mechanisms.

---

## 🧪 Practice Questions

### Easy:
1. Create threads that print numbers concurrently
2. Use ThreadPoolExecutor to download multiple URLs
3. Create a simple producer-consumer with threading

### Medium:
4. Implement parallel file processing with processes
5. Create a thread-safe counter class
6. Build a simple task queue system

### Hard:
7. Implement a parallel map-reduce
8. Create a web crawler with asyncio
9. Build a process pool with custom scheduling

---

## 📖 Summary

| Concept | Use Case |
|---------|----------|
| Threading | I/O-bound, shared memory |
| Multiprocessing | CPU-bound, true parallelism |
| asyncio | Many I/O operations |
| ThreadPoolExecutor | Simple thread management |
| ProcessPoolExecutor | Simple process management |
| Lock | Thread synchronization |
| Queue | Thread-safe communication |

---

## ⏭️ Next Topic

**[Context Managers](./05_Context_Managers.md)** — Resource management with `with`!

---

*Concurrency: Do more, faster!* ⚡
