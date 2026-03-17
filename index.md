---
layout: default
title: PYTHON203 Quick Reference
---

# PYTHON203 — Quick Reference Cheat Sheets
*Data Manipulation and Visualisation in Python — Intersect Australia*

---

## 📦 Setup & Imports

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline   # Jupyter only — display plots in notebook
```

---

## 🐼 Day 1: Pandas

### Loading Data
```python
df = pd.read_csv("file.csv")
df = pd.read_csv("file.csv", index_col=0)  # Use first column as index
```

### First Look
```python
df.head()          # First 5 rows
df.tail()          # Last 5 rows
df.shape           # (rows, columns)
df.dtypes          # Data type of each column
df.columns         # Column names
df.columns.values  # Column names as array
df.describe()      # Summary statistics
```

### Selecting Data
```python
df['column']             # Single column
df[['col1', 'col2']]     # Multiple columns

# iloc — by position (integer index)
df.iloc[0]               # First row
df.iloc[0:3, 0:4]        # First 3 rows, first 4 columns

# loc — by label
df.loc[[0, 10], :]                    # Rows 0 and 10, all columns
df.loc[[0], ['country', 'continent']] # Row 0, specific columns
```

### Filtering
```python
df[df.continent == 'Americas']
df[df.gdpPercap < 2000]

# Multiple conditions — always use brackets!
df[(df.gdpPercap < 15000) & (df.continent == 'Europe')]

# Match a list of values
df[df['continent'].isin(['Asia', 'Oceania'])]
```

### Summary Statistics
```python
df['col'].describe()   # Full summary
df['col'].min()
df['col'].max()
df['col'].mean()
df['col'].std()
df['col'].count()
pd.unique(df['continent'])   # Unique values
```

### Grouping
```python
grouped = df.groupby('continent')
grouped.describe()
grouped.mean()

# Multiple columns
df.groupby(['continent', 'country']).mean()
```

### Combining DataFrames
```python
pd.concat([df1, df2])                          # Stack rows
pd.merge(df1, df2, on='key')                   # Inner join
pd.merge(df1, df2, on='key', how='left')       # Left join
```

### Cleaning
```python
df.dropna()                  # Drop rows with NaN
df.fillna(0)                 # Fill NaN with 0
df.isnull().sum()            # Count NaN per column
df['col'].astype(float)      # Convert type
```

---

## 📊 Day 2: Visualisation

### Seaborn Plot Types

```python
# Scatterplot
sns.scatterplot(x='col1', y='col2', data=df,
    hue='continent',        # Colour by category
    size='pop',             # Size by numeric column
    sizes=(20, 2000),       # Min/max marker size
    alpha=0.8,              # Transparency
    palette='Set1')

# Barplot
sns.barplot(x='continent', y='lifeExp', data=df)

# Histogram
sns.histplot(data=df, x='col', bins=30)

# Density / KDE
sns.kdeplot(data=df, x='col')

# Boxplot
sns.boxplot(x='continent', y='lifeExp', data=df)

# Heatmap
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')

# Line plot
sns.lineplot(x='year', y='value', data=df)

# FacetGrid (small multiples)
g = sns.FacetGrid(df, col='continent')
g.map(sns.histplot, 'lifeExp')

# Joint plot
sns.jointplot(x='gdpPercap', y='lifeExp', data=df)
```

### Matplotlib Customisation

```python
plt.figure(figsize=(14, 10))    # Set size BEFORE seaborn call

plt.xlabel('X Label', fontsize=16)
plt.ylabel('Y Label', fontsize=16)
plt.title('My Title', fontsize=18)

plt.xlim(0, 160000)
plt.ylim(40, 90)
plt.xticks([0, 50000, 100000, 150000])
plt.yticks(range(40, 90, 10))
plt.tick_params(axis='both', labelsize=14)
plt.grid(True)

plt.text(x, y, 'Some text', size=12, backgroundcolor='white')

plt.legend(loc='lower right', framealpha=0.8, ncol=2, fontsize=14)

plt.savefig('plot.png', dpi=300, transparent=True)
plt.show()
```

### Colour Palettes
```python
palette='Set1'       # Bold distinct colours
palette='Set2'       # Softer distinct colours
palette='Blues'      # Sequential
palette='coolwarm'   # Diverging (good for correlations)
palette='viridis'    # Perceptually uniform

# Custom dictionary
palette=dict(Africa='black', Asia='yellow',
             Americas='red', Europe='blue', Oceania='green')
```

---

## 🔑 Quick Reference Table

| Task | Code |
|------|------|
| Load CSV | `pd.read_csv('file.csv')` |
| Shape | `df.shape` |
| First rows | `df.head()` |
| Column types | `df.dtypes` |
| Filter rows | `df[df.col > value]` |
| Select column | `df['col']` |
| Group by | `df.groupby('col').mean()` |
| Merge | `pd.merge(df1, df2, on='key')` |
| Scatterplot | `sns.scatterplot(x=, y=, data=)` |
| Barplot | `sns.barplot(x=, y=, data=)` |
| Boxplot | `sns.boxplot(x=, y=, data=)` |
| Save figure | `plt.savefig('name.png', dpi=300)` |

---

## ⚠️ Common Gotchas

- **Multiple conditions:** always use `&` / `\|` with brackets: `(cond1) & (cond2)`
- **iloc vs loc:** `iloc` = integer position, `loc` = label/index name
- **Figure size:** set `plt.figure(figsize=...)` *before* the seaborn call
- **Path errors:** check working directory with `import os; os.getcwd()`
- **NaN errors:** use `df.dropna()` or `df.fillna()` before plotting

---

*Course material: [PYTHON203 — Intersect Australia](https://intersectaustralia.github.io/training/PYTHON203/)*
