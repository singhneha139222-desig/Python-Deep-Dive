# 📘 Matplotlib - Data Visualization

## 📌 Introduction

**Matplotlib** is Python's foundational visualization library. It provides complete control over every aspect of a plot, from simple line charts to complex multi-panel figures.

Think of Matplotlib as **Python's drawing canvas** — you can create virtually any visualization you can imagine!

```
┌────────────────────────────────────────────────────────────────┐
│                    MATPLOTLIB ANATOMY                           │
│                                                                 │
│   Figure (Canvas)                                              │
│   ┌────────────────────────────────────────────────────────┐  │
│   │  Axes (Plot Area)                                      │  │
│   │  ┌──────────────────────────────────────────────────┐ │  │
│   │  │           Title                                   │ │  │
│   │  │  ↑                                               │ │  │
│   │  │  │ Y-axis        * *                            │ │  │
│   │  │  │ label       *    *                           │ │  │
│   │  │  │            *      *   ← Line/Plot            │ │  │
│   │  │  │           *        *                         │ │  │
│   │  │  └────────────────────────→                     │ │  │
│   │  │              X-axis label                        │ │  │
│   │  └──────────────────────────────────────────────────┘ │  │
│   │                    Legend                              │  │
│   └────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Two Interfaces

| Interface | Style | Best For |
|-----------|-------|----------|
| pyplot (plt) | MATLAB-like | Quick plots, scripts |
| Object-Oriented | Explicit | Complex figures, control |

### Installation

```bash
pip install matplotlib
```

### Importing

```python
import matplotlib.pyplot as plt
import numpy as np  # Often used together
```

---

## 💻 Code Examples

### 🟢 Basic: Line Plot

```python
import matplotlib.pyplot as plt

# Simple line plot
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('Simple Line Plot')
plt.show()

# Multiple lines
x = [1, 2, 3, 4, 5]
y1 = [1, 4, 9, 16, 25]
y2 = [1, 2, 3, 4, 5]

plt.plot(x, y1, label='Squares')
plt.plot(x, y2, label='Linear')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Multiple Lines')
plt.legend()
plt.grid(True)
plt.show()
```

### 🟢 Basic: Customizing Lines

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

# Line styles and colors
plt.plot(x, np.sin(x), 'r-', label='sin (red solid)')
plt.plot(x, np.cos(x), 'b--', label='cos (blue dashed)')
plt.plot(x, np.sin(x + 1), 'g:', label='sin shifted (green dotted)')

# Detailed customization
plt.plot(x, np.sin(x + 2), 
         color='purple',
         linestyle='-.',
         linewidth=2,
         marker='o',
         markersize=4,
         markevery=10,
         label='Custom')

plt.legend()
plt.show()

# Line style reference:
# '-'  : solid
# '--' : dashed
# ':'  : dotted
# '-.' : dash-dot

# Color reference:
# 'r' : red, 'g' : green, 'b' : blue
# 'c' : cyan, 'm' : magenta, 'y' : yellow
# 'k' : black, 'w' : white
```

### 🟢 Basic: Bar Charts

```python
import matplotlib.pyplot as plt

# Vertical bar chart
categories = ['A', 'B', 'C', 'D', 'E']
values = [23, 45, 56, 78, 32]

plt.bar(categories, values, color='steelblue')
plt.xlabel('Categories')
plt.ylabel('Values')
plt.title('Vertical Bar Chart')
plt.show()

# Horizontal bar chart
plt.barh(categories, values, color='coral')
plt.xlabel('Values')
plt.ylabel('Categories')
plt.title('Horizontal Bar Chart')
plt.show()

# Grouped bar chart
import numpy as np

x = np.arange(len(categories))
width = 0.35

values1 = [23, 45, 56, 78, 32]
values2 = [18, 38, 62, 70, 40]

plt.bar(x - width/2, values1, width, label='2023')
plt.bar(x + width/2, values2, width, label='2024')

plt.xlabel('Categories')
plt.ylabel('Values')
plt.title('Grouped Bar Chart')
plt.xticks(x, categories)
plt.legend()
plt.show()

# Stacked bar chart
plt.bar(categories, values1, label='2023')
plt.bar(categories, values2, bottom=values1, label='2024')
plt.legend()
plt.show()
```

### 🟡 Intermediate: Scatter Plots

```python
import matplotlib.pyplot as plt
import numpy as np

# Basic scatter
x = np.random.rand(50)
y = np.random.rand(50)

plt.scatter(x, y)
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Scatter Plot')
plt.show()

# Scatter with size and color
n = 100
x = np.random.rand(n)
y = np.random.rand(n)
colors = np.random.rand(n)
sizes = 1000 * np.random.rand(n)

plt.scatter(x, y, c=colors, s=sizes, alpha=0.5, cmap='viridis')
plt.colorbar(label='Color Scale')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Scatter with Size and Color')
plt.show()
```

### 🟡 Intermediate: Pie Charts

```python
import matplotlib.pyplot as plt

# Basic pie chart
labels = ['Python', 'Java', 'JavaScript', 'C++', 'Others']
sizes = [40, 25, 20, 10, 5]
colors = ['#ff9999', '#66b3ff', '#99ff99', '#ffcc99', '#ff66b3']
explode = (0.1, 0, 0, 0, 0)  # Explode first slice

plt.pie(sizes, 
        explode=explode, 
        labels=labels, 
        colors=colors,
        autopct='%1.1f%%',  # Show percentages
        shadow=True, 
        startangle=90)

plt.title('Programming Language Popularity')
plt.axis('equal')  # Equal aspect ratio for circle
plt.show()

# Donut chart
fig, ax = plt.subplots()
ax.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=90)
centre_circle = plt.Circle((0, 0), 0.70, fc='white')
ax.add_patch(centre_circle)
plt.title('Donut Chart')
plt.show()
```

### 🟡 Intermediate: Histograms

```python
import matplotlib.pyplot as plt
import numpy as np

# Basic histogram
data = np.random.randn(1000)

plt.hist(data, bins=30, edgecolor='black')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.title('Histogram')
plt.show()

# Customized histogram
plt.hist(data, bins=30, 
         density=True,      # Normalize
         alpha=0.7, 
         color='steelblue',
         edgecolor='black')
plt.xlabel('Value')
plt.ylabel('Density')
plt.title('Normalized Histogram')
plt.show()

# Multiple histograms
data1 = np.random.normal(0, 1, 1000)
data2 = np.random.normal(2, 1.5, 1000)

plt.hist(data1, bins=30, alpha=0.5, label='Dataset 1')
plt.hist(data2, bins=30, alpha=0.5, label='Dataset 2')
plt.legend()
plt.show()
```

### 🟡 Intermediate: Subplots

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)

# 2x2 grid of subplots
fig, axes = plt.subplots(2, 2, figsize=(10, 8))

axes[0, 0].plot(x, np.sin(x), 'r')
axes[0, 0].set_title('Sine')

axes[0, 1].plot(x, np.cos(x), 'b')
axes[0, 1].set_title('Cosine')

axes[1, 0].plot(x, np.tan(x), 'g')
axes[1, 0].set_ylim(-5, 5)
axes[1, 0].set_title('Tangent')

axes[1, 1].plot(x, np.exp(-x), 'm')
axes[1, 1].set_title('Exponential Decay')

plt.tight_layout()
plt.show()

# Shared axes
fig, axes = plt.subplots(2, 1, sharex=True, figsize=(10, 6))
axes[0].plot(x, np.sin(x))
axes[0].set_ylabel('sin(x)')
axes[1].plot(x, np.cos(x))
axes[1].set_ylabel('cos(x)')
axes[1].set_xlabel('x')
plt.show()
```

### 🔴 Advanced: Object-Oriented Approach

```python
import matplotlib.pyplot as plt
import numpy as np

# Create figure and axes explicitly
fig, ax = plt.subplots(figsize=(10, 6))

x = np.linspace(0, 10, 100)

# Plot on the axes
ax.plot(x, np.sin(x), label='sin(x)')
ax.plot(x, np.cos(x), label='cos(x)')

# Customize
ax.set_xlabel('X axis', fontsize=12)
ax.set_ylabel('Y axis', fontsize=12)
ax.set_title('Trigonometric Functions', fontsize=14)
ax.legend(loc='upper right')
ax.grid(True, alpha=0.3)
ax.set_xlim(0, 10)
ax.set_ylim(-1.5, 1.5)

# Add annotations
ax.annotate('Maximum', xy=(np.pi/2, 1), 
            xytext=(np.pi/2 + 1, 1.3),
            arrowprops=dict(arrowstyle='->', color='red'),
            fontsize=10)

# Add text
ax.text(5, 0, 'Center', ha='center', fontsize=10)

plt.tight_layout()
plt.savefig('plot.png', dpi=300, bbox_inches='tight')
plt.show()
```

### 🔴 Advanced: Heatmaps

```python
import matplotlib.pyplot as plt
import numpy as np

# Create data
data = np.random.rand(10, 10)

# Basic heatmap
fig, ax = plt.subplots(figsize=(8, 6))
im = ax.imshow(data, cmap='hot')

# Add colorbar
plt.colorbar(im)

# Add labels
ax.set_xticks(np.arange(10))
ax.set_yticks(np.arange(10))
ax.set_title('Heatmap')
plt.show()

# Correlation heatmap
import pandas as pd

# Create sample data
df = pd.DataFrame(np.random.randn(100, 5), columns=['A', 'B', 'C', 'D', 'E'])
correlation = df.corr()

fig, ax = plt.subplots(figsize=(8, 6))
im = ax.imshow(correlation, cmap='coolwarm', vmin=-1, vmax=1)

# Add labels
ax.set_xticks(range(len(correlation.columns)))
ax.set_yticks(range(len(correlation.columns)))
ax.set_xticklabels(correlation.columns)
ax.set_yticklabels(correlation.columns)

# Add values
for i in range(len(correlation)):
    for j in range(len(correlation)):
        ax.text(j, i, f'{correlation.iloc[i, j]:.2f}', 
                ha='center', va='center')

plt.colorbar(im)
plt.title('Correlation Heatmap')
plt.show()
```

### 🔴 Advanced: 3D Plots

```python
import matplotlib.pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D

# 3D Line plot
fig = plt.figure(figsize=(10, 8))
ax = fig.add_subplot(111, projection='3d')

t = np.linspace(0, 10, 100)
x = np.sin(t)
y = np.cos(t)
z = t

ax.plot3D(x, y, z)
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
ax.set_title('3D Line Plot')
plt.show()

# 3D Surface plot
fig = plt.figure(figsize=(10, 8))
ax = fig.add_subplot(111, projection='3d')

x = np.linspace(-5, 5, 50)
y = np.linspace(-5, 5, 50)
X, Y = np.meshgrid(x, y)
Z = np.sin(np.sqrt(X**2 + Y**2))

surf = ax.plot_surface(X, Y, Z, cmap='viridis', alpha=0.8)
fig.colorbar(surf)
ax.set_title('3D Surface Plot')
plt.show()

# 3D Scatter plot
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

n = 100
x = np.random.rand(n)
y = np.random.rand(n)
z = np.random.rand(n)
colors = np.random.rand(n)

ax.scatter(x, y, z, c=colors, cmap='plasma')
ax.set_title('3D Scatter Plot')
plt.show()
```

### 🔴 Advanced: Styling and Themes

```python
import matplotlib.pyplot as plt
import numpy as np

# List available styles
print(plt.style.available)

# Use a style
plt.style.use('seaborn-v0_8')  # or 'ggplot', 'dark_background', etc.

x = np.linspace(0, 10, 100)

plt.plot(x, np.sin(x), label='sin')
plt.plot(x, np.cos(x), label='cos')
plt.legend()
plt.title('Styled Plot')
plt.show()

# Custom style context
with plt.style.context('dark_background'):
    plt.plot(x, np.sin(x))
    plt.title('Dark Theme')
    plt.show()

# Global customization
plt.rcParams['font.size'] = 12
plt.rcParams['axes.labelsize'] = 14
plt.rcParams['figure.figsize'] = [10, 6]
plt.rcParams['lines.linewidth'] = 2
```

---

## 📊 Plot Type Reference

| Plot Type | Function | Best For |
|-----------|----------|----------|
| Line | `plt.plot()` | Trends over time |
| Bar | `plt.bar()` | Category comparison |
| Scatter | `plt.scatter()` | Relationships |
| Pie | `plt.pie()` | Proportions |
| Histogram | `plt.hist()` | Distributions |
| Heatmap | `plt.imshow()` | Matrices, correlations |
| Box | `plt.boxplot()` | Statistical distributions |
| Violin | `plt.violinplot()` | Distribution + density |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Not Clearing Previous Plots

```python
# WRONG - Plots accumulate
plt.plot([1, 2, 3])
plt.plot([4, 5, 6])  # Both lines shown

# CORRECT - Clear between plots
plt.plot([1, 2, 3])
plt.show()
plt.clf()  # Clear figure
plt.plot([4, 5, 6])
plt.show()
```

### ❌ Mistake 2: Forgetting plt.show()

```python
# WRONG - Nothing displayed
plt.plot([1, 2, 3])

# CORRECT
plt.plot([1, 2, 3])
plt.show()
```

---

## 💡 Pro Tips

### 1. Save High-Quality Figures

```python
plt.savefig('figure.png', dpi=300, bbox_inches='tight', 
            facecolor='white', transparent=False)
```

### 2. Use tight_layout()

```python
fig, axes = plt.subplots(2, 2)
# ... add plots ...
plt.tight_layout()  # Prevents overlap
```

### 3. Interactive Mode

```python
plt.ion()  # Turn on interactive mode
plt.ioff() # Turn off interactive mode
```

---

## 💼 Interview Insight

### Q1: pyplot vs Object-Oriented?

> pyplot is simpler for quick plots; object-oriented gives more control for complex figures with multiple axes.

### Q2: How to save figures programmatically?

> Use `plt.savefig('filename.png', dpi=300)` with format inferred from extension (png, pdf, svg, etc.)

---

## 🧪 Practice Questions

### Easy:
1. Create a line plot with labels
2. Make a bar chart comparing categories
3. Create a scatter plot with colors

### Medium:
4. Create a 2x2 subplot grid
5. Make a pie chart with exploded slice
6. Build a grouped bar chart

### Hard:
7. Create an animated plot
8. Build a dashboard with multiple plots
9. Create a custom visualization

---

## 📖 Summary

| Operation | Code |
|-----------|------|
| Line plot | `plt.plot(x, y)` |
| Bar chart | `plt.bar(x, height)` |
| Scatter | `plt.scatter(x, y)` |
| Histogram | `plt.hist(data, bins)` |
| Subplots | `fig, ax = plt.subplots()` |
| Save | `plt.savefig('file.png')` |
| Show | `plt.show()` |

---

## ⏭️ Next Topic

**[Flask - Web Framework](04_Flask.md)** — Build web applications!

---

*Matplotlib: A picture is worth a thousand data points!* 📊
