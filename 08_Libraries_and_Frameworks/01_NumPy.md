# 📘 NumPy - Numerical Python

## 📌 Introduction

**NumPy** (Numerical Python) is the fundamental library for scientific computing in Python. It provides powerful N-dimensional array objects and mathematical functions that are much faster than native Python lists.

Think of NumPy as **Python's calculator on steroids** — it can handle millions of numbers in milliseconds!

```
┌────────────────────────────────────────────────────────────────┐
│                   Python List vs NumPy Array                    │
│                                                                 │
│   Python List:          NumPy Array:                           │
│   [1, 2, 3, 4, 5]       array([1, 2, 3, 4, 5])                │
│                                                                 │
│   - Flexible types       - Fixed type (faster)                 │
│   - Slow for math        - Vectorized operations               │
│   - High memory          - Memory efficient                    │
│   - Loop required        - Broadcast operations                │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Why NumPy?

| Feature | Python List | NumPy Array |
|---------|-------------|-------------|
| Speed | Slow | 10-100x faster |
| Memory | More | Less |
| Operations | Element-by-element | Vectorized |
| Type | Mixed | Homogeneous |
| Broadcasting | No | Yes |

### Installation

```bash
pip install numpy
```

### Importing NumPy

```python
import numpy as np  # Standard convention
```

---

## 💻 Code Examples

### 🟢 Basic: Creating Arrays

```python
import numpy as np

# From Python list
arr1 = np.array([1, 2, 3, 4, 5])
print(arr1)
# Output: [1 2 3 4 5]

# 2D array
arr2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr2d)
# Output:
# [[1 2 3]
#  [4 5 6]]

# Array with zeros
zeros = np.zeros((3, 4))
print(zeros)
# Output:
# [[0. 0. 0. 0.]
#  [0. 0. 0. 0.]
#  [0. 0. 0. 0.]]

# Array with ones
ones = np.ones((2, 3))
print(ones)
# Output:
# [[1. 1. 1.]
#  [1. 1. 1.]]

# Range of numbers
range_arr = np.arange(0, 10, 2)
print(range_arr)
# Output: [0 2 4 6 8]

# Evenly spaced numbers
linspace_arr = np.linspace(0, 1, 5)
print(linspace_arr)
# Output: [0.   0.25 0.5  0.75 1.  ]

# Identity matrix
identity = np.eye(3)
print(identity)
# Output:
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]

# Random numbers
random_arr = np.random.rand(3, 3)
print(random_arr)
# Output: 3x3 array with values between 0 and 1
```

### 🟢 Basic: Array Properties

```python
import numpy as np

arr = np.array([[1, 2, 3], [4, 5, 6]])

# Shape (rows, columns)
print(arr.shape)
# Output: (2, 3)

# Number of dimensions
print(arr.ndim)
# Output: 2

# Total elements
print(arr.size)
# Output: 6

# Data type
print(arr.dtype)
# Output: int64

# Memory size per element
print(arr.itemsize)
# Output: 8 (bytes)

# Total memory
print(arr.nbytes)
# Output: 48 (bytes)
```

### 🟡 Intermediate: Array Operations

```python
import numpy as np

a = np.array([1, 2, 3, 4])
b = np.array([10, 20, 30, 40])

# Element-wise operations
print(a + b)        # [11 22 33 44]
print(a - b)        # [-9 -18 -27 -36]
print(a * b)        # [10 40 90 160]
print(a / b)        # [0.1 0.1 0.1 0.1]
print(a ** 2)       # [1 4 9 16]

# Scalar operations (broadcasting)
print(a * 10)       # [10 20 30 40]
print(a + 100)      # [101 102 103 104]

# Comparison
print(a > 2)        # [False False True True]
print(a == 2)       # [False True False False]

# Mathematical functions
print(np.sqrt(a))   # [1. 1.414 1.732 2.]
print(np.exp(a))    # [2.718 7.389 20.085 54.598]
print(np.log(a))    # [0. 0.693 1.098 1.386]
print(np.sin(a))    # [0.841 0.909 0.141 -0.756]
```

### 🟡 Intermediate: Indexing and Slicing

```python
import numpy as np

# 1D array indexing
arr = np.array([10, 20, 30, 40, 50])
print(arr[0])       # 10
print(arr[-1])      # 50
print(arr[1:4])     # [20 30 40]
print(arr[::2])     # [10 30 50] (every 2nd element)

# 2D array indexing
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

print(arr2d[0, 0])      # 1 (first row, first column)
print(arr2d[1, 2])      # 6 (second row, third column)
print(arr2d[0])         # [1 2 3] (first row)
print(arr2d[:, 0])      # [1 4 7] (first column)
print(arr2d[0:2, 1:3])  # [[2 3] [5 6]] (subarray)

# Boolean indexing
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8])
print(arr[arr > 4])     # [5 6 7 8]
print(arr[arr % 2 == 0])  # [2 4 6 8] (even numbers)

# Fancy indexing
indices = [0, 2, 4]
print(arr[indices])     # [1 3 5]
```

### 🟡 Intermediate: Reshaping and Manipulating

```python
import numpy as np

arr = np.arange(12)
print(arr)
# Output: [ 0  1  2  3  4  5  6  7  8  9 10 11]

# Reshape
reshaped = arr.reshape(3, 4)
print(reshaped)
# Output:
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

# Flatten
flat = reshaped.flatten()
print(flat)
# Output: [ 0  1  2  3  4  5  6  7  8  9 10 11]

# Transpose
transposed = reshaped.T
print(transposed)
# Output:
# [[ 0  4  8]
#  [ 1  5  9]
#  [ 2  6 10]
#  [ 3  7 11]]

# Concatenate
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(np.concatenate([a, b]))
# Output: [1 2 3 4 5 6]

# Stack
print(np.vstack([a, b]))  # Vertical
# Output:
# [[1 2 3]
#  [4 5 6]]

print(np.hstack([a.reshape(3, 1), b.reshape(3, 1)]))  # Horizontal
# Output:
# [[1 4]
#  [2 5]
#  [3 6]]

# Split
arr = np.arange(9)
print(np.split(arr, 3))
# Output: [array([0, 1, 2]), array([3, 4, 5]), array([6, 7, 8])]
```

### 🟡 Intermediate: Statistical Functions

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Basic statistics
print(np.sum(arr))      # 55
print(np.mean(arr))     # 5.5
print(np.median(arr))   # 5.5
print(np.std(arr))      # 2.872 (standard deviation)
print(np.var(arr))      # 8.25 (variance)

print(np.min(arr))      # 1
print(np.max(arr))      # 10
print(np.argmin(arr))   # 0 (index of min)
print(np.argmax(arr))   # 9 (index of max)

# 2D statistics
arr2d = np.array([[1, 2, 3], [4, 5, 6]])

print(np.sum(arr2d))          # 21 (total)
print(np.sum(arr2d, axis=0))  # [5 7 9] (column sums)
print(np.sum(arr2d, axis=1))  # [6 15] (row sums)

print(np.mean(arr2d, axis=0))  # [2.5 3.5 4.5]
print(np.mean(arr2d, axis=1))  # [2. 5.]
```

### 🔴 Advanced: Broadcasting

```python
import numpy as np

# Broadcasting rules:
# 1. Arrays with different shapes can operate together
# 2. Smaller arrays are "broadcast" to match larger ones
# 3. Shapes must be compatible (same or one is 1)

# Example 1: Scalar and array
arr = np.array([1, 2, 3])
print(arr + 10)
# Output: [11 12 13]
# 10 is broadcast to [10, 10, 10]

# Example 2: 1D and 2D
arr2d = np.array([[1, 2, 3], [4, 5, 6]])
arr1d = np.array([10, 20, 30])
print(arr2d + arr1d)
# Output:
# [[11 22 33]
#  [14 25 36]]

# Example 3: Column and row
col = np.array([[1], [2], [3]])  # 3x1
row = np.array([10, 20, 30])     # 1x3
print(col + row)
# Output:
# [[11 21 31]
#  [12 22 32]
#  [13 23 33]]

# Practical: Normalize data (subtract mean, divide by std)
data = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
mean = data.mean(axis=0)
std = data.std(axis=0)
normalized = (data - mean) / std
print(normalized)
```

### 🔴 Advanced: Linear Algebra

```python
import numpy as np

# Matrix creation
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# Matrix multiplication
print(np.dot(A, B))  # or A @ B
# Output:
# [[19 22]
#  [43 50]]

# Transpose
print(A.T)
# Output:
# [[1 3]
#  [2 4]]

# Determinant
print(np.linalg.det(A))
# Output: -2.0

# Inverse
print(np.linalg.inv(A))
# Output:
# [[-2.   1. ]
#  [ 1.5 -0.5]]

# Eigenvalues and eigenvectors
eigenvalues, eigenvectors = np.linalg.eig(A)
print("Eigenvalues:", eigenvalues)
print("Eigenvectors:", eigenvectors)

# Solve linear system: Ax = b
A = np.array([[3, 1], [1, 2]])
b = np.array([9, 8])
x = np.linalg.solve(A, b)
print(x)
# Output: [2. 3.]  (x=2, y=3)
```

### 🔴 Advanced: Performance Comparison

```python
import numpy as np
import time

# Compare list vs numpy
size = 1000000

# Python list
python_list = list(range(size))
start = time.time()
result = [x * 2 for x in python_list]
python_time = time.time() - start

# NumPy array
numpy_array = np.arange(size)
start = time.time()
result = numpy_array * 2
numpy_time = time.time() - start

print(f"Python list: {python_time:.4f} seconds")
print(f"NumPy array: {numpy_time:.4f} seconds")
print(f"NumPy is {python_time/numpy_time:.1f}x faster")
# Output:
# Python list: 0.0800 seconds
# NumPy array: 0.0010 seconds
# NumPy is 80.0x faster
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: View vs Copy

```python
# WRONG - Modifying a view affects original
arr = np.array([1, 2, 3, 4, 5])
view = arr[2:4]
view[0] = 99
print(arr)  # [1 2 99 4 5] - Original changed!

# CORRECT - Use copy for independent array
arr = np.array([1, 2, 3, 4, 5])
copy = arr[2:4].copy()
copy[0] = 99
print(arr)  # [1 2 3 4 5] - Original unchanged
```

### ❌ Mistake 2: Shape Mismatch in Operations

```python
# WRONG - Incompatible shapes
a = np.array([[1, 2, 3]])  # (1, 3)
b = np.array([[1, 2]])     # (1, 2)
# a + b  # ValueError!

# CORRECT - Check shapes first
print(a.shape, b.shape)
# Make sure shapes are compatible
```

---

## 💡 Pro Tips

### 1. Use Vectorization, Avoid Loops

```python
# SLOW
result = []
for x in arr:
    result.append(x ** 2)

# FAST
result = arr ** 2
```

### 2. Use np.where for Conditional Logic

```python
arr = np.array([1, 2, 3, 4, 5])
result = np.where(arr > 2, arr * 10, arr)
print(result)  # [1 2 30 40 50]
```

### 3. Use np.clip for Bounds

```python
arr = np.array([1, 5, 10, 15, 20])
clipped = np.clip(arr, 5, 15)
print(clipped)  # [5 5 10 15 15]
```

---

## 🔍 Real-world Use Cases

1. **Data Science** — Data preprocessing, normalization
2. **Machine Learning** — Feature matrices, weights
3. **Image Processing** — Images as 3D arrays (height, width, RGB)
4. **Financial Analysis** — Stock prices, portfolio optimization
5. **Scientific Computing** — Simulations, numerical analysis

---

## 💼 Interview Insight

### Q1: Why is NumPy faster than Python lists?

> NumPy arrays are stored in contiguous memory with fixed types, enabling vectorized operations using optimized C code. Python lists store references to objects scattered in memory.

### Q2: What is broadcasting?

> Broadcasting is NumPy's ability to perform operations on arrays with different shapes by automatically expanding the smaller array to match the larger one.

---

## 🧪 Practice Questions

### Easy:
1. Create a 3x3 array of zeros
2. Find the mean of `[1, 2, 3, 4, 5]`
3. Reshape a 1D array of 12 elements into 3x4

### Medium:
4. Normalize an array (mean=0, std=1)
5. Find all elements greater than the mean
6. Multiply two matrices

### Hard:
7. Implement linear regression using NumPy
8. Calculate the covariance matrix of a dataset
9. Solve a system of linear equations

---

## 📖 Summary

| Function | Purpose |
|----------|---------|
| `np.array()` | Create array |
| `np.zeros()`, `np.ones()` | Create filled arrays |
| `np.arange()`, `np.linspace()` | Create ranges |
| `arr.reshape()` | Change shape |
| `np.sum()`, `np.mean()` | Aggregations |
| `np.dot()`, `@` | Matrix multiply |
| `np.where()` | Conditional select |

---

## ⏭️ Next Topic

**[Pandas - Data Analysis](02_Pandas.md)** — Learn to work with tabular data!

---

*NumPy: The foundation of data science in Python!* 🔢
