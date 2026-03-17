---
layout: default
title: PYTHON203 Quick Reference
---

# PYTHON203 — Quick Reference Cheat Sheets
*Data Manipulation and Visualisation in Python*

---

## 📦 SETUP & IMPORTS

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline   # Jupyter only — display plots in notebook
```

---

## 🐼 DAY 1: PANDAS CHEAT SHEET

### Loading Data
```python
df = pd.read_csv("file.csv")          # Load CSV
df = pd.read_csv("file.csv", index_col=0)  # Use first column as index
```

### First Look at Your Data
```python
df.head()          # First 5 rows
df.tail()          # Last 5 rows
df.shape           # (rows, columns)
df.dtypes          # Data type of each column
df.columns         # Column names
df.columns.values  # Column names as array
df.describe()      # Summary statistics for all numeric columns
```

### Selecting Data
```python
# Select a column
df['column']
df.column           # Same thing

# Select multiple columns
df[['col1', 'col2']]

# iloc — by position (integer index)
df.iloc[0]          # First row
df.iloc[0:3, 0:4]   # First 3 rows, first 4 columns

# loc — by label
df.loc[[0, 10], :]                        # Rows 0 and 10, all columns
df.loc[[0], ['country', 'continent']]     # Row 0, specific columns
```

### Filtering / Subsetting
```python
# Single condition
df[df.continent == 'Americas']
df[df.gdpPercap < 2000]

# Multiple conditions (use & and | with brackets!)
df[(df.gdpPercap < 15000) & (df.continent == 'Europe')]

# Match a list of values
df[df['continent'].isin(['Asia', 'Oceania'])]
```

### Summary Statistics
```python
df['col'].describe()   # Count, mean, std, min, max, quartiles
df['col'].min()
df['col'].max()
df['col'].mean()
df['col'].std()
df['col'].count()

# Unique values
pd.unique(df['continent'])
```

### Grouping
```python
grouped = df.groupby('continent')
grouped.describe()
grouped.mean()

# Group by multiple columns
df.groupby(['continent', 'country']).mean()
```

### Combining DataFrames
```python
# Concatenate (stack rows)
pd.concat([df1, df2])

# Merge (join on a key)
pd.merge(df1, df2, on='key_column')
pd.merge(df1, df2, left_on='col1', right_on='col2')

# Types: 'inner' (default), 'left', 'right', 'outer'
pd.merge(df1, df2, on='key', how='left')
```

### Data Types & Cleaning
```python
df['col'].astype(float)       # Convert type
df.dropna()                   # Drop rows with any NaN
df.fillna(0)                  # Fill NaN with 0
df.isnull().sum()             # Count NaN per column
```

---

## 📊 DAY 2: VISUALISATION CHEAT SHEET

### Seaborn — Quick Plots

```python
# Scatterplot
sns.scatterplot(x='col1', y='col2', data=df)
sns.scatterplot(x='col1', y='col2', data=df,
    hue='continent',      # Colour by category
    size='pop',           # Size by numeric value
    sizes=(20, 2000),     # Min/max marker size
    markers='o',
    s=140,
    alpha=0.8,
    edgecolor='white',
    linewidth=1,
    palette='Set1')       # Colour palette

# Barplot
sns.barplot(x='continent', y='lifeExp', data=df)
sns.barplot(x='continent', y='lifeExp', data=df, hue='sex')

# Histogram
sns.histplot(data=df, x='col', bins=30)

# Density / KDE plot
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

# Joint plot (scatter + distributions)
sns.jointplot(x='gdpPercap', y='lifeExp', data=df, kind='scatter')
```

### Matplotlib — Customising Plots

```python
# Figure size (always set BEFORE seaborn call)
plt.figure(figsize=(14, 10))

# Axis labels
plt.xlabel('GDP Per Capita [$USD]', fontsize=16)
plt.ylabel('Life Expectancy [years]', fontsize=16)

# Title
plt.title('My Plot Title', fontsize=18)

# Axis limits
plt.xlim(0, 160000)
plt.ylim(40, 90)

# Ticks
plt.xticks([0, 50000, 100000, 150000])
plt.yticks(range(40, 90, 10))
plt.tick_params(axis='both', labelsize=14)

# Grid
plt.grid(True)

# Add text to plot
plt.text(x_pos, y_pos, 'My text', size=12, backgroundcolor='white')

# Legend
plt.legend(loc='lower right', framealpha=0.8,
           edgecolor='white', ncol=2,
           fontsize=14, title='Legend Title')

# Save figure
plt.savefig('my_plot.png', dpi=300, transparent=True)

# Show plot
plt.show()
```

### Common Colour Palettes
```python
# Named palettes
palette='Set1'       # Bold distinct colours
palette='Set2'       # Softer distinct colours
palette='Blues'      # Sequential blue
palette='coolwarm'   # Diverging (good for correlations)
palette='viridis'    # Perceptually uniform

# Custom dict palette
palette=dict(Africa='black', Asia='yellow',
             Americas='red', Europe='blue', Oceania='green')
```

---

## 🔑 KEY CONCEPTS QUICK REFERENCE

| Concept | Code |
|---|---|
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

## ⚠️ COMMON GOTCHAS

- **Multiple conditions:** always use `&` / `|` with brackets: `(cond1) & (cond2)`
- **iloc vs loc:** `iloc` = integer position, `loc` = label/index
- **Figure size:** set `plt.figure(figsize=...)` *before* the seaborn call
- **Path errors:** if `pd.read_csv` fails, check your working directory with `import os; os.getcwd()`
- **NaN errors:** use `df.dropna()` or `df.fillna()` before plotting

---
*Generated for PYTHON203 — Intersect Australia*
