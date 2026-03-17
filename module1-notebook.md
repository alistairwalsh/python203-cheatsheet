# Module 1: Python Essentials for Data Work
*A live coding notebook — 20-30 minutes*

---

> **Instructor note:** This replaces the current Module 1 which is just a list of links. The goal is NOT a comprehensive Python intro — it's a targeted 25-minute session covering exactly what students need to understand Pandas. Keep the pace brisk. Students who already know Python will appreciate the Pandas framing; students who don't will get just enough.

---

## What we're covering

Before we can work with data in Python, we need four building blocks. Let's work through them quickly, because the real fun starts in the next module.

---

## 1. Lists — ordered collections of things

A list is a sequence of values. You've probably seen these before.

```python
# A list of country names
countries = ['Australia', 'Brazil', 'China', 'Denmark']

# Access by position (starts at 0!)
print(countries[0])    # Australia
print(countries[-1])   # Denmark (last item)

# Slice a range
print(countries[1:3])  # ['Brazil', 'China']

# Length
print(len(countries))  # 4

# Loop through
for country in countries:
    print(country)
```

**Why it matters for Pandas:** DataFrame columns are basically fancy lists. When you write `df['country']`, you're getting back a list-like structure.

---

## 2. Dictionaries — labelled collections

A dictionary maps **keys** to **values**. Think of it like a lookup table.

```python
# Country data as a dictionary
australia = {
    'country': 'Australia',
    'continent': 'Oceania',
    'lifeExp': 82.3,
    'gdpPercap': 45000
}

# Access by key
print(australia['continent'])   # Oceania
print(australia['lifeExp'])     # 82.3

# Add a new key
australia['pop'] = 25000000
print(australia)

# Keys and values
print(australia.keys())
print(australia.values())
```

**Why it matters for Pandas:** A DataFrame is essentially a list of dictionaries — each row is a record, each key is a column name. This is exactly what Pandas is built on.

```python
# A list of dictionaries = a table
data = [
    {'country': 'Australia', 'continent': 'Oceania', 'lifeExp': 82.3},
    {'country': 'Brazil',    'continent': 'Americas', 'lifeExp': 75.7},
    {'country': 'China',     'continent': 'Asia',     'lifeExp': 76.4},
]

# This is basically what pd.read_csv() gives you — let's see:
import pandas as pd
df = pd.DataFrame(data)
print(df)
```

---

## 3. Functions — reusable blocks of code

Functions let you name a piece of code and run it again later with different inputs.

```python
# Basic function
def greet(name):
    return "Hello, " + name + "!"

print(greet("Alistair"))
print(greet("everyone"))
```

```python
# A function relevant to data work
def above_average(value, average):
    """Returns True if value is above the average."""
    return value > average

print(above_average(82.3, 72.0))   # True
print(above_average(55.1, 72.0))   # False
```

**The docstring** (the triple-quoted text inside a function) is how Python documents what a function does. Get into the habit!

```python
def life_expectancy_category(lifeExp):
    """Classify life expectancy as low, medium, or high."""
    if lifeExp < 60:
        return 'low'
    elif lifeExp < 75:
        return 'medium'
    else:
        return 'high'

# Try it
print(life_expectancy_category(55))   # low
print(life_expectancy_category(72))   # medium
print(life_expectancy_category(82))   # high
```

**Why it matters for Pandas:** You'll apply functions across entire columns using `.apply()`. This is one of the most powerful things you can do with data.

```python
# Preview — we'll do this properly later
life_expectancies = [82.3, 75.7, 55.1, 68.4, 91.0]
categories = [life_expectancy_category(x) for x in life_expectancies]
print(categories)
```

---

## 4. A taste of DataFrames

Now let's put it all together and get a preview of what we're building toward.

```python
import pandas as pd

# Load real data
df = pd.read_csv("./Data/gapminder_2016.csv")

# A DataFrame is a table — rows and columns
print(type(df))      # <class 'pandas.core.frame.DataFrame'>
print(df.shape)      # (190, 5) — 190 rows, 5 columns

# Column names — just like dictionary keys
print(df.columns.values)

# First few rows
df.head()
```

Notice: each column is like a list, and each row is like a dictionary. That's all a DataFrame is — a smart, powerful version of what we just built by hand.

---

## ✅ Challenge Exercise (5 minutes)

Write a function called `gdp_category` that:
- Takes a GDP per capita value as input
- Returns `'low'` if below 5000
- Returns `'middle'` if between 5000 and 20000
- Returns `'high'` if above 20000

Test it with a few values.

**Bonus:** Can you apply it to a list of 5 different GDP values using a loop?

```python
# Your code here

```

### Solution

```python
def gdp_category(gdp):
    """Classify GDP per capita as low, middle, or high."""
    if gdp < 5000:
        return 'low'
    elif gdp <= 20000:
        return 'middle'
    else:
        return 'high'

# Test it
print(gdp_category(1996))    # low (Afghanistan)
print(gdp_category(11154))   # middle (Albania)
print(gdp_category(48185))   # high (Andorra)

# Bonus: apply to a list
gdp_values = [1996, 11154, 48185, 3500, 55000]
categories = [gdp_category(g) for g in gdp_values]
print(categories)
```

---

## What's next?

Now that we have these building blocks, we're going to load a real dataset — 190 countries, with life expectancy, GDP, and population — and start answering questions with it.

By the end of today, you'll be able to load any CSV, filter it, group it, calculate statistics, and combine datasets from multiple sources. By tomorrow, you'll be able to turn that data into publication-quality charts.

Let's go. ➡️ [Module 2: Getting Started with Pandas](./02-pandas-partI)
