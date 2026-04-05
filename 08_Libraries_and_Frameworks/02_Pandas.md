# 📘 Pandas - Data Analysis Library

## 📌 Introduction

**Pandas** is Python's most powerful data analysis library. It provides data structures and tools for working with structured (tabular) data efficiently.

Think of Pandas as **Excel on steroids** — it can handle millions of rows, perform complex operations, and automate everything!

```
┌────────────────────────────────────────────────────────────────┐
│                      PANDAS DATA STRUCTURES                     │
│                                                                 │
│   Series (1D):                                                 │
│   ┌───────┬───────┐                                           │
│   │ Index │ Value │                                           │
│   ├───────┼───────┤                                           │
│   │   0   │  10   │                                           │
│   │   1   │  20   │                                           │
│   │   2   │  30   │                                           │
│   └───────┴───────┘                                           │
│                                                                 │
│   DataFrame (2D):                                              │
│   ┌───────┬────────┬─────┬────────┐                           │
│   │       │  Name  │ Age │ Salary │                           │
│   ├───────┼────────┼─────┼────────┤                           │
│   │   0   │ Alice  │  25 │ 50000  │                           │
│   │   1   │  Bob   │  30 │ 60000  │                           │
│   │   2   │Charlie │  35 │ 70000  │                           │
│   └───────┴────────┴─────┴────────┘                           │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Two Main Data Structures

| Structure | Dimensions | Description |
|-----------|------------|-------------|
| Series | 1D | Like a column or array with labels |
| DataFrame | 2D | Table with rows and columns |

### Installation

```bash
pip install pandas
```

### Importing Pandas

```python
import pandas as pd  # Standard convention
```

---

## 💻 Code Examples

### 🟢 Basic: Creating Series

```python
import pandas as pd

# From list
s = pd.Series([10, 20, 30, 40])
print(s)
# Output:
# 0    10
# 1    20
# 2    30
# 3    40
# dtype: int64

# With custom index
s = pd.Series([10, 20, 30], index=['a', 'b', 'c'])
print(s)
# Output:
# a    10
# b    20
# c    30

# From dictionary
data = {'apple': 100, 'banana': 200, 'cherry': 150}
s = pd.Series(data)
print(s)
# Output:
# apple     100
# banana    200
# cherry    150

# Accessing values
print(s['apple'])      # 100
print(s[['apple', 'cherry']])  # Multiple values
```

### 🟢 Basic: Creating DataFrames

```python
import pandas as pd

# From dictionary
data = {
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
}
df = pd.DataFrame(data)
print(df)
# Output:
#       Name  Age  Salary
# 0    Alice   25   50000
# 1      Bob   30   60000
# 2  Charlie   35   70000

# From list of dictionaries
data = [
    {'Name': 'Alice', 'Age': 25},
    {'Name': 'Bob', 'Age': 30}
]
df = pd.DataFrame(data)

# With custom index
df = pd.DataFrame(data, index=['emp1', 'emp2'])
print(df)
# Output:
#        Name  Age
# emp1  Alice   25
# emp2    Bob   30

# From NumPy array
import numpy as np
arr = np.array([[1, 2, 3], [4, 5, 6]])
df = pd.DataFrame(arr, columns=['A', 'B', 'C'])
print(df)
# Output:
#    A  B  C
# 0  1  2  3
# 1  4  5  6
```

### 🟢 Basic: Reading and Writing Files

```python
import pandas as pd

# Read CSV
df = pd.read_csv('data.csv')

# Read with options
df = pd.read_csv('data.csv', 
                 sep=',',           # Delimiter
                 header=0,          # Row for column names
                 index_col='id',    # Column for index
                 usecols=['name', 'age'],  # Specific columns
                 nrows=100)         # Limit rows

# Read Excel
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')

# Read JSON
df = pd.read_json('data.json')

# Read SQL
import sqlite3
conn = sqlite3.connect('database.db')
df = pd.read_sql('SELECT * FROM users', conn)

# Write files
df.to_csv('output.csv', index=False)
df.to_excel('output.xlsx', index=False)
df.to_json('output.json')
```

### 🟡 Intermediate: DataFrame Inspection

```python
import pandas as pd

df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Age': [25, 30, 35, 28, 32],
    'Salary': [50000, 60000, 70000, 55000, 65000],
    'Department': ['IT', 'HR', 'IT', 'Sales', 'HR']
})

# First/last rows
print(df.head(2))    # First 2 rows
print(df.tail(2))    # Last 2 rows

# Shape
print(df.shape)      # (5, 4) - rows, columns

# Column names
print(df.columns)    # Index(['Name', 'Age', 'Salary', 'Department'])

# Data types
print(df.dtypes)
# Name          object
# Age            int64
# Salary         int64
# Department    object

# Info summary
print(df.info())

# Statistical summary
print(df.describe())
# Output:
#             Age        Salary
# count   5.00000      5.000000
# mean   30.00000  60000.000000
# std     3.80789   7905.694150
# min    25.00000  50000.000000
# 25%    28.00000  55000.000000
# 50%    30.00000  60000.000000
# 75%    32.00000  65000.000000
# max    35.00000  70000.000000
```

### 🟡 Intermediate: Selection and Indexing

```python
import pandas as pd

df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
})

# Select single column (returns Series)
print(df['Name'])

# Select multiple columns (returns DataFrame)
print(df[['Name', 'Age']])

# Select rows by index label (loc)
print(df.loc[0])              # First row
print(df.loc[0:1])            # Rows 0 and 1
print(df.loc[0, 'Name'])      # Specific cell

# Select rows by position (iloc)
print(df.iloc[0])             # First row
print(df.iloc[0:2])           # Rows 0 and 1
print(df.iloc[0, 0])          # First cell

# Boolean indexing
print(df[df['Age'] > 25])     # Filter by condition
print(df[df['Name'] == 'Alice'])

# Multiple conditions
print(df[(df['Age'] > 25) & (df['Salary'] > 55000)])
print(df[(df['Age'] < 30) | (df['Salary'] > 65000)])

# Query method (cleaner syntax)
print(df.query('Age > 25 and Salary > 55000'))
```

### 🟡 Intermediate: Data Manipulation

```python
import pandas as pd

df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
})

# Add new column
df['Bonus'] = df['Salary'] * 0.1
df['Status'] = 'Active'
print(df)

# Modify column
df['Salary'] = df['Salary'] * 1.1  # 10% raise

# Apply function
df['Age_Category'] = df['Age'].apply(lambda x: 'Young' if x < 30 else 'Senior')

# Rename columns
df = df.rename(columns={'Name': 'Employee', 'Salary': 'Pay'})

# Drop columns
df = df.drop(columns=['Bonus'])

# Drop rows
df = df.drop(index=[0])

# Sort values
df = df.sort_values('Age', ascending=False)
df = df.sort_values(['Department', 'Salary'], ascending=[True, False])

# Reset index
df = df.reset_index(drop=True)
```

### 🟡 Intermediate: Handling Missing Data

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, np.nan, 8],
    'C': [9, 10, 11, 12]
})

# Check for missing values
print(df.isnull())
print(df.isnull().sum())    # Count per column

# Drop missing values
df_dropped = df.dropna()              # Drop rows with any NaN
df_dropped = df.dropna(subset=['A'])  # Drop if NaN in column A

# Fill missing values
df_filled = df.fillna(0)                    # Fill with 0
df_filled = df.fillna(df.mean())            # Fill with column mean
df_filled = df.fillna(method='ffill')       # Forward fill
df_filled = df.fillna(method='bfill')       # Backward fill

# Fill specific columns
df['A'] = df['A'].fillna(df['A'].mean())
df['B'] = df['B'].fillna(df['B'].median())

# Interpolate
df_interp = df.interpolate()
```

### 🔴 Advanced: GroupBy Operations

```python
import pandas as pd

df = pd.DataFrame({
    'Department': ['IT', 'HR', 'IT', 'Sales', 'HR', 'Sales'],
    'Employee': ['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Frank'],
    'Salary': [50000, 45000, 55000, 48000, 47000, 52000],
    'Experience': [3, 5, 4, 2, 6, 3]
})

# Group by single column
grouped = df.groupby('Department')

# Aggregation
print(grouped['Salary'].mean())
# Department
# HR       46000.0
# IT       52500.0
# Sales    50000.0

# Multiple aggregations
print(grouped['Salary'].agg(['mean', 'min', 'max', 'count']))

# Group by multiple columns
print(df.groupby(['Department', 'Experience'])['Salary'].mean())

# Custom aggregation
agg_result = grouped.agg({
    'Salary': ['mean', 'sum'],
    'Experience': 'max',
    'Employee': 'count'
})
print(agg_result)

# Transform (keep same shape)
df['Dept_Avg_Salary'] = grouped['Salary'].transform('mean')
df['Salary_Diff'] = df['Salary'] - df['Dept_Avg_Salary']

# Filter groups
high_paying = grouped.filter(lambda x: x['Salary'].mean() > 50000)
```

### 🔴 Advanced: Merging and Joining

```python
import pandas as pd

# Employee data
employees = pd.DataFrame({
    'emp_id': [1, 2, 3, 4],
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'dept_id': [101, 102, 101, 103]
})

# Department data
departments = pd.DataFrame({
    'dept_id': [101, 102, 104],
    'dept_name': ['IT', 'HR', 'Sales']
})

# Inner join (only matching)
merged = pd.merge(employees, departments, on='dept_id')
print(merged)
#    emp_id     name  dept_id dept_name
# 0       1    Alice      101        IT
# 1       3  Charlie      101        IT
# 2       2      Bob      102        HR

# Left join (all from left)
merged = pd.merge(employees, departments, on='dept_id', how='left')

# Right join (all from right)
merged = pd.merge(employees, departments, on='dept_id', how='right')

# Outer join (all from both)
merged = pd.merge(employees, departments, on='dept_id', how='outer')

# Join on different column names
merged = pd.merge(employees, departments, 
                  left_on='dept_id', right_on='dept_id')

# Concatenate DataFrames
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

# Vertical concatenation
combined = pd.concat([df1, df2], ignore_index=True)

# Horizontal concatenation
combined = pd.concat([df1, df2], axis=1)
```

### 🔴 Advanced: Pivot Tables and Crosstabs

```python
import pandas as pd

df = pd.DataFrame({
    'Date': ['2024-01-01', '2024-01-01', '2024-01-02', '2024-01-02'],
    'Product': ['A', 'B', 'A', 'B'],
    'Region': ['North', 'North', 'South', 'South'],
    'Sales': [100, 150, 200, 250]
})

# Pivot table
pivot = pd.pivot_table(df, 
                       values='Sales', 
                       index='Date', 
                       columns='Product', 
                       aggfunc='sum')
print(pivot)
# Product        A    B
# Date                  
# 2024-01-01   100  150
# 2024-01-02   200  250

# Multiple aggregations
pivot = pd.pivot_table(df, 
                       values='Sales', 
                       index='Date', 
                       columns='Product', 
                       aggfunc=['sum', 'mean'])

# Crosstab (frequency table)
ct = pd.crosstab(df['Product'], df['Region'])
print(ct)

# Melt (unpivot)
melted = pd.melt(df, id_vars=['Date'], value_vars=['Sales'])

# Stack/Unstack
stacked = df.set_index(['Date', 'Product'])['Sales'].unstack()
```

### 🔴 Advanced: Time Series

```python
import pandas as pd

# Create date range
dates = pd.date_range('2024-01-01', periods=10, freq='D')
df = pd.DataFrame({'Value': range(10)}, index=dates)

# Resample (aggregate by time period)
monthly = df.resample('M').sum()
weekly = df.resample('W').mean()

# Rolling window
df['Rolling_Mean'] = df['Value'].rolling(window=3).mean()
df['Rolling_Sum'] = df['Value'].rolling(window=3).sum()

# Shift (lag)
df['Previous'] = df['Value'].shift(1)
df['Next'] = df['Value'].shift(-1)

# Percent change
df['Pct_Change'] = df['Value'].pct_change()

# Date components
df['Year'] = df.index.year
df['Month'] = df.index.month
df['Day'] = df.index.day
df['DayOfWeek'] = df.index.dayofweek

# Filter by date
df_filtered = df['2024-01-05':'2024-01-08']
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: SettingWithCopyWarning

```python
# WRONG - Modifies a copy, not original
df[df['Age'] > 25]['Status'] = 'Senior'

# CORRECT - Use .loc
df.loc[df['Age'] > 25, 'Status'] = 'Senior'
```

### ❌ Mistake 2: Forgetting to Assign Result

```python
# WRONG - Doesn't modify df
df.dropna()
df.sort_values('Age')

# CORRECT - Assign or use inplace
df = df.dropna()
df.sort_values('Age', inplace=True)
```

---

## 💡 Pro Tips

### 1. Use Method Chaining

```python
result = (df
    .query('Age > 25')
    .groupby('Department')
    .agg({'Salary': 'mean'})
    .sort_values('Salary', ascending=False)
    .reset_index())
```

### 2. Optimize Memory

```python
# Check memory usage
print(df.memory_usage(deep=True))

# Convert types
df['Age'] = df['Age'].astype('int8')
df['Status'] = df['Status'].astype('category')
```

---

## 💼 Interview Insight

### Q1: Series vs DataFrame?

> Series is a 1D labeled array, while DataFrame is a 2D labeled table (collection of Series with shared index).

### Q2: loc vs iloc?

> `loc` uses labels/names for selection, `iloc` uses integer positions. loc includes the endpoint in slices, iloc doesn't.

---

## 🧪 Practice Questions

### Easy:
1. Create a DataFrame from a dictionary
2. Select rows where Age > 25
3. Calculate mean salary per department

### Medium:
4. Handle missing values in a dataset
5. Merge two DataFrames on a common column
6. Create a pivot table

### Hard:
7. Perform time series analysis
8. Optimize memory for large dataset
9. Build a complete data pipeline

---

## 📖 Summary

| Operation | Code |
|-----------|------|
| Read CSV | `pd.read_csv('file.csv')` |
| Select column | `df['col']` or `df.col` |
| Filter rows | `df[df['col'] > value]` |
| Group & aggregate | `df.groupby('col').agg()` |
| Merge | `pd.merge(df1, df2, on='key')` |
| Pivot | `pd.pivot_table()` |
| Handle NaN | `df.fillna()`, `df.dropna()` |

---

## ⏭️ Next Topic

**[Matplotlib - Data Visualization](03_Matplotlib.md)** — Create stunning charts!

---

*Pandas: Your data's best friend!* 🐼
