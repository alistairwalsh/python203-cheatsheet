# PYTHON203 — Formative Exercises
**Format:** Software Carpentry challenge style  
**Duration:** 5–10 minutes each  
**Usage:** Stop live coding, post the challenge, give learners time to work individually or in pairs, then discuss as a group.

---

## How to Use These Exercises

**For instructors:**
- Post the challenge text in the chat (Zoom) or on a shared document (in-person)
- Tell learners: "Work in your notebook. Green sticky when done, red if stuck."
- Set a timer for 5–7 minutes
- When most have green stickies (or timer expires), live-code the solution and discuss
- Ask: "Who got a different answer? What did you try?" — wrong answers are the most useful teaching moments

**For learners:**
- These are not tests. They are practice.
- It is completely normal to get stuck. That's the point.
- Copy the solution into your notebook after the discussion — it's part of your notes.

---

## Module 1 Exercise: Python Recap

### Challenge 1.1 — Writing a Function

**Objective:** Practice defining and calling a Python function.

**The challenge:**

You have a list of life expectancy values from a survey:

```python
life_exp = [72.3, 68.1, 75.9, 80.2, 65.4, 71.0, 77.8]
```

1. Write a function called `calculate_mean` that takes a list of numbers and returns the mean (average).
2. Call your function with `life_exp` and print the result.
3. **Bonus:** What happens if you pass an empty list? Add a check that prints a helpful message if the list is empty.

**Solution:**

```python
def calculate_mean(values):
    if len(values) == 0:
        print("Warning: empty list, cannot calculate mean")
        return None
    return sum(values) / len(values)

life_exp = [72.3, 68.1, 75.9, 80.2, 65.4, 71.0, 77.8]
mean_le = calculate_mean(life_exp)
print(f"Mean life expectancy: {mean_le:.2f}")
```

**Expected output:** `Mean life expectancy: 72.96`

**Key learning point:** Functions let us reuse logic without copy-pasting code. We'll use this pattern extensively when building plotting functions later.

---

### Challenge 1.2 — Dictionaries to DataFrames

**Objective:** Understand that a DataFrame is a dictionary of lists.

**The challenge:**

You have survey data in a Python dictionary:

```python
survey = {
    'country': ['Australia', 'Canada', 'Germany', 'Japan'],
    'year': [2007, 2007, 2007, 2007],
    'life_exp': [81.2, 80.7, 79.4, 82.6],
    'pop': [20434176, 33390141, 82400996, 127467972]
}
```

1. Import pandas as `pd`
2. Convert the dictionary to a DataFrame using `pd.DataFrame()`
3. Print the result
4. What does `type(survey)` give you? What does `type(pd.DataFrame(survey))` give you?

**Solution:**

```python
import pandas as pd

survey = {
    'country': ['Australia', 'Canada', 'Germany', 'Japan'],
    'year': [2007, 2007, 2007, 2007],
    'life_exp': [81.2, 80.7, 79.4, 82.6],
    'pop': [20434176, 33390141, 82400996, 127467972]
}

df = pd.DataFrame(survey)
print(df)
print(type(survey))       # <class 'dict'>
print(type(df))           # <class 'pandas.core.frame.DataFrame'>
```

**Key learning point:** A DataFrame is conceptually a dictionary of equal-length lists. Each key becomes a column name. This mental model helps when things go wrong.

---

## Module 2 Exercise: Loading and Exploring Data

### Challenge 2.1 — Exploring the Gapminder Dataset

**Objective:** Use basic DataFrame inspection methods to understand an unfamiliar dataset.

**Setup:** The gapminder dataset has been loaded as `df`.

```python
import pandas as pd
df = pd.read_csv('gapminder_all.csv')
```

**The challenge:**

Answer the following questions using only pandas methods (no manual counting):

1. How many rows and columns does the dataset have?
2. What are the column names?
3. What is the data type of the `pop` column?
4. What are the minimum and maximum values of `life_exp`?
5. How many **unique countries** are in the dataset?

**Solution:**

```python
# 1. Shape
print(df.shape)           # (rows, columns)

# 2. Column names
print(df.columns.tolist())

# 3. Data type of pop
print(df['pop'].dtype)

# 4. Min and max life expectancy
print(df['life_exp'].min(), df['life_exp'].max())

# 5. Unique countries
print(df['country'].nunique())
```

**Key learning point:** `df.shape`, `df.columns`, `df.dtypes`, `.min()`, `.max()`, and `.nunique()` are your first tools for understanding any new dataset. Run these before you do anything else.

---

### Challenge 2.2 — Filtering Rows

**Objective:** Use boolean indexing to select rows meeting a condition.

**The challenge:**

Using the gapminder dataset:

1. Select only rows where the continent is `'Asia'`
2. From those rows, select only the year `1997`
3. How many rows does the result have?
4. **Bonus:** What is the mean life expectancy for Asian countries in 1997?

**Solution:**

```python
# Step 1: Asia only
asia = df[df['continent'] == 'Asia']

# Step 2: Asia in 1997
asia_1997 = df[(df['continent'] == 'Asia') & (df['year'] == 1997)]

# Step 3: Count rows
print(asia_1997.shape[0])   # or len(asia_1997)

# Bonus: Mean life expectancy
print(asia_1997['life_exp'].mean())
```

**Key learning point:** `df[condition]` selects rows. Combine conditions with `&` (and) or `|` (or) — not Python's `and`/`or`. Each condition must be wrapped in parentheses.

---

## Module 3 Exercise: Indexing and Subsetting

### Challenge 3.1 — loc vs iloc

**Objective:** Distinguish between label-based and integer-based indexing.

**The challenge:**

```python
# Reset index to make this interesting
europe_2007 = df[(df['continent'] == 'Europe') & (df['year'] == 2007)].reset_index(drop=True)
```

1. Use `.iloc` to select the first 3 rows and the first 2 columns
2. Use `.loc` to select rows where `country` starts with 'A' — hint: use `.str.startswith()`
3. Use `.loc` to select the columns `country` and `life_exp` for all rows
4. What is the difference in result if you use `df.iloc[0]` vs `df.loc[0]`?

**Solution:**

```python
# 1. First 3 rows, first 2 columns
print(europe_2007.iloc[:3, :2])

# 2. Countries starting with 'A'
print(europe_2007.loc[europe_2007['country'].str.startswith('A')])

# 3. Select specific columns
print(europe_2007.loc[:, ['country', 'life_exp']])

# 4. iloc[0] returns first row regardless of index label
#    loc[0]  returns the row WHERE INDEX LABEL == 0
```

**Key learning point:** `.iloc` uses position (like a list). `.loc` uses label (like a dictionary). After filtering, row indices are preserved — so `.loc[5]` gets the row labelled 5, not the 5th row. This is a common source of bugs.

---

### Challenge 3.2 — Boolean Indexing with Multiple Conditions

**Objective:** Filter data using two conditions simultaneously.

**The challenge:**

Find all European countries in 2007 where life expectancy is above 75.

1. Write the filter using `&`
2. Write the same filter using `.query()` — which do you find more readable?
3. Sort the result by life expectancy, highest first
4. Print only the `country` and `life_exp` columns

**Solution:**

```python
# Method 1: Boolean indexing
result = df[
    (df['continent'] == 'Europe') & 
    (df['year'] == 2007) & 
    (df['life_exp'] > 75)
]

# Method 2: query (note: column names with spaces need backticks)
result_q = df.query("continent == 'Europe' and year == 2007 and life_exp > 75")

# Sort and select columns
result_sorted = result.sort_values('life_exp', ascending=False)[['country', 'life_exp']]
print(result_sorted)
```

**Key learning point:** `.query()` is often more readable for complex filters. Both produce the same result. Choose whichever makes your code clearer to a future reader (who might be you, in six months).

---

## Module 4 Exercise: Groupby and Aggregation

### Challenge 4.1 — GroupBy Basics

**Objective:** Compute summary statistics by group.

**The challenge:**

1. Calculate the **mean GDP per capita** for each continent in the year 2007
2. Sort the result from highest to lowest GDP
3. **Bonus:** Calculate both the mean AND the median GDP per capita per continent in one step

**Solution:**

```python
# 1 & 2: Mean GDP per continent, sorted
result = (df[df['year'] == 2007]
          .groupby('continent')['gdpPercap']
          .mean()
          .sort_values(ascending=False))
print(result)

# Bonus: Multiple aggregations
result_multi = (df[df['year'] == 2007]
                .groupby('continent')['gdpPercap']
                .agg(['mean', 'median'])
                .sort_values('mean', ascending=False))
print(result_multi)
```

**Key learning point:** `groupby` splits the data into groups, applies a function, and combines the result. The `.agg()` method lets you apply multiple functions at once. This split-apply-combine pattern is one of pandas' most powerful features.

---

### Challenge 4.2 — GroupBy with Multiple Columns

**Objective:** Group by more than one column and interpret the result.

**The challenge:**

1. Calculate the mean life expectancy by **both** continent and year
2. For which continent did mean life expectancy increase the most between 1952 and 2007?
3. **Hint:** You'll need to filter for those two years, then use `.groupby(['continent', 'year'])`, then do some arithmetic

**Solution:**

```python
# 1. Mean life expectancy by continent and year
by_cont_year = (df.groupby(['continent', 'year'])['life_exp']
                .mean()
                .reset_index())

# 2. Change from 1952 to 2007
early = by_cont_year[by_cont_year['year'] == 1952].set_index('continent')['life_exp']
late  = by_cont_year[by_cont_year['year'] == 2007].set_index('continent')['life_exp']
change = (late - early).sort_values(ascending=False)
print(change)
```

**Key learning point:** Groupby results are themselves DataFrames. You can chain further operations on them. `.reset_index()` converts the group labels back to regular columns — useful when you want to continue working with the result.

---

## Module 5 Exercise: Combining DataFrames

### Challenge 5.1 — Concatenating DataFrames

**Objective:** Stack DataFrames with the same columns.

**The challenge:**

Imagine you have gapminder data split into two files — one for 1952 and one for 2007:

```python
gap_1952 = df[df['year'] == 1952].copy()
gap_2007 = df[df['year'] == 2007].copy()
```

1. Combine these two DataFrames into one using `pd.concat()`
2. Verify the result has the expected number of rows
3. What does `ignore_index=True` do? Try it with and without.

**Solution:**

```python
gap_1952 = df[df['year'] == 1952].copy()
gap_2007 = df[df['year'] == 2007].copy()

# Combine
combined = pd.concat([gap_1952, gap_2007])
print(combined.shape)   # Should be 2 * len(gap_1952) rows

# With ignore_index: resets the row index to 0, 1, 2, ...
combined_reset = pd.concat([gap_1952, gap_2007], ignore_index=True)
print(combined_reset.index)  # 0 to N-1
```

**Key learning point:** `pd.concat()` stacks DataFrames vertically (by default). Without `ignore_index=True`, the original row indices are preserved — so you might have duplicate index values. Always check your row count after concatenation.

---

### Challenge 5.2 — Merging DataFrames

**Objective:** Join two DataFrames on a shared key column.

**The challenge:**

You have the gapminder data and a separate metadata table:

```python
metadata = pd.DataFrame({
    'country': ['Afghanistan', 'Albania', 'Algeria', 'Australia', 'Austria'],
    'region': ['South Asia', 'Southern Europe', 'Northern Africa', 
               'Australia/NZ', 'Western Europe'],
    'income_group': ['Low', 'Upper middle', 'Lower middle', 'High', 'High']
})
```

1. Merge the 2007 gapminder data with this metadata table on `country`
2. How many rows does the result have? Why?
3. Change to a left join — how does the row count change?

**Solution:**

```python
gap_2007 = df[df['year'] == 2007].copy()

# Inner join (default) — only countries present in BOTH tables
merged_inner = pd.merge(gap_2007, metadata, on='country')
print(merged_inner.shape)  # Only 5 rows — only countries in metadata

# Left join — all gapminder countries, NaN for those not in metadata
merged_left = pd.merge(gap_2007, metadata, on='country', how='left')
print(merged_left.shape)   # All gapminder 2007 rows
print(merged_left['region'].isna().sum())  # Countries missing from metadata
```

**Key learning point:** The default merge is an **inner join** — it keeps only rows with matches in both tables. A **left join** keeps all rows from the left table. Understanding join types prevents silent data loss.

---

## New Module Exercise: Data Cleaning

### Challenge DC.1 — Diagnosing a Dirty Dataset

**Objective:** Identify and count quality issues in a DataFrame before deciding how to handle them.

**Setup:** Load the dirty gapminder file:

```python
dirty = pd.read_csv('gapminder_dirty.csv')
```

**The challenge:**

Run a full diagnostic on the dirty DataFrame:

1. How many missing values are in each column? (Use `df.isnull().sum()`)
2. What are the data types of each column? Are any surprising?
3. Are there any duplicate rows?
4. How many unique values does the `continent` column have? Print them — do any look wrong?

**Solution:**

```python
# 1. Missing values per column
print(dirty.isnull().sum())

# 2. Data types
print(dirty.dtypes)

# 3. Duplicate rows
print(f"Duplicate rows: {dirty.duplicated().sum()}")

# 4. Continent values
print(dirty['continent'].nunique())
print(dirty['continent'].unique())
# Look for: 'africa' vs 'Africa', 'europe ' (trailing space), etc.
```

**Key learning point:** Always run a diagnostic before any analysis. `isnull().sum()`, `dtypes`, `duplicated().sum()`, and `.unique()` on categorical columns will find 90% of data quality problems.

---

### Challenge DC.2 — Fixing the Dirty Dataset

**Objective:** Apply cleaning operations to fix identified problems.

**The challenge:**

Based on your diagnostic, fix the dirty gapminder DataFrame:

1. Drop rows where `life_exp` is missing
2. Fill missing `gdpPercap` values with the **median** GDP per capita for that continent
3. Convert the `year` column to integer (if it loaded as float)
4. Strip whitespace from the `continent` column and standardise capitalisation
5. Remove duplicate rows
6. Rename the column `life_exp` to `life_expectancy`

**Solution:**

```python
clean = dirty.copy()

# 1. Drop rows with missing life expectancy
clean = clean.dropna(subset=['life_exp'])

# 2. Fill missing GDP with continent median
clean['gdpPercap'] = clean.groupby('continent')['gdpPercap'].transform(
    lambda x: x.fillna(x.median())
)

# 3. Convert year to int
clean['year'] = clean['year'].astype(int)

# 4. Standardise continent strings
clean['continent'] = clean['continent'].str.strip().str.title()

# 5. Remove duplicates
clean = clean.drop_duplicates()

# 6. Rename column
clean = clean.rename(columns={'life_exp': 'life_expectancy'})

print(clean.isnull().sum())   # Should be 0
print(clean.dtypes)
print(clean.shape)
```

**Key learning point:** Each cleaning step should be a separate operation. Use `.copy()` at the start to avoid modifying the original. After cleaning, re-run your diagnostic to confirm the fixes worked.

---

## Module 6 Exercise: Scatter Plot

### Challenge 6.1 — Recreating and Modifying a Scatter Plot

**Objective:** Reproduce a plot from code and make a targeted modification.

**Setup:** The 2007 gapminder data has been loaded as `gap_2007`.

**The challenge:**

1. Reproduce the class scatter plot (GDP per capita vs life expectancy, coloured by continent, size proportional to population)
2. **Modify it:** Instead of colouring by continent, colour by a new variable `high_life_exp` which is `True` if life expectancy is above 70, `False` otherwise
3. Add a title: "Countries above and below 70 years life expectancy, 2007"
4. **Bonus:** Save the figure to a file called `scatter_life70.png` at 150 DPI

**Solution:**

```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# Create the binary variable
gap_2007 = gap_2007.copy()
gap_2007['high_life_exp'] = gap_2007['life_exp'] > 70

fig, ax = plt.subplots(figsize=(10, 6))

sns.scatterplot(
    data=gap_2007,
    x='gdpPercap',
    y='life_exp',
    hue='high_life_exp',
    size='pop',
    sizes=(20, 500),
    alpha=0.7,
    ax=ax
)

ax.set_xscale('log')
ax.set_xlabel('GDP per Capita (log scale)', fontsize=12)
ax.set_ylabel('Life Expectancy (years)', fontsize=12)
ax.set_title('Countries above and below 70 years life expectancy, 2007', fontsize=14)
ax.axhline(y=70, color='red', linestyle='--', alpha=0.5, label='70 year threshold')
ax.legend(title='Life Exp > 70')

# Bonus: save
fig.savefig('scatter_life70.png', dpi=150, bbox_inches='tight')
plt.show()
```

**Key learning point:** `hue` accepts any column — categorical or boolean. Continuous columns are also valid (they produce a colour gradient). `ax.axhline()` adds a reference line — useful for showing thresholds or targets.

---

## Module 7 Exercise: Barplot and Functions

### Challenge 7.1 — Extending the Plot Function

**Objective:** Modify a parameterised plotting function to add a new option.

**Setup:** The class has written a function `plot_continent_lifeexp(year)` that produces a barplot of mean life expectancy per continent for a given year.

**The challenge:**

1. Add a `palette` parameter to the function with a default of `'Blues_d'`
2. Add a `save` parameter (default `False`) — if `True`, save the figure to `barplot_{year}.png`
3. Call the function three times: for 1952, 1977, and 2007
4. **Bonus:** Add an `assert` statement that checks the year is one of the valid years in the dataset

**Solution:**

```python
def plot_continent_lifeexp(df, year, palette='Blues_d', save=False):
    """
    Plot mean life expectancy per continent for a given year.
    
    Parameters
    ----------
    df : DataFrame
        The gapminder dataset
    year : int
        Year to plot (must be present in df)
    palette : str
        Seaborn palette name (default: 'Blues_d')
    save : bool
        If True, save figure to barplot_{year}.png
    """
    assert year in df['year'].unique(), \
        f"Year {year} not found. Valid years: {sorted(df['year'].unique())}"
    
    year_data = df[df['year'] == year]
    continent_means = year_data.groupby('continent')['life_exp'].mean().reset_index()
    continent_means = continent_means.sort_values('life_exp', ascending=False)
    
    fig, ax = plt.subplots(figsize=(8, 5))
    
    sns.barplot(
        data=continent_means,
        x='continent',
        y='life_exp',
        palette=palette,
        ax=ax
    )
    
    ax.set_title(f'Mean Life Expectancy by Continent, {year}', fontsize=14)
    ax.set_xlabel('Continent', fontsize=12)
    ax.set_ylabel('Mean Life Expectancy (years)', fontsize=12)
    ax.set_ylim(0, 85)
    
    if save:
        fig.savefig(f'barplot_{year}.png', dpi=150, bbox_inches='tight')
    
    plt.show()

# Call it
for year in [1952, 1977, 2007]:
    plot_continent_lifeexp(df, year, palette='viridis', save=True)
```

**Key learning point:** Functions make it trivial to repeat analyses with different parameters. The `assert` statement catches errors early with a useful message — better than a confusing pandas error 10 lines later.

---

## Module 8 Exercise: Histogram, Boxplot, FacetGrid

### Challenge 8.1 — Distribution Comparison

**Objective:** Use a boxplot and histogram together to understand a distribution.

**The challenge:**

For the 2007 gapminder data:

1. Make a histogram of `life_exp` with 20 bins
2. Make a boxplot of `life_exp` grouped by `continent`
3. Compare the two plots: what does the histogram show that the boxplot doesn't? What does the boxplot show that the histogram doesn't?

**Solution:**

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Histogram
axes[0].hist(gap_2007['life_exp'], bins=20, color='steelblue', edgecolor='white')
axes[0].set_xlabel('Life Expectancy (years)')
axes[0].set_ylabel('Count')
axes[0].set_title('Distribution of Life Expectancy, 2007')

# Boxplot
sns.boxplot(
    data=gap_2007,
    x='continent',
    y='life_exp',
    palette='Set2',
    ax=axes[1]
)
axes[1].set_title('Life Expectancy by Continent, 2007')
axes[1].set_xlabel('Continent')
axes[1].set_ylabel('Life Expectancy (years)')

plt.tight_layout()
plt.show()

# Discussion answers:
# Histogram: shows the overall shape (bimodal? skewed?), but loses group info
# Boxplot: shows group differences, median, IQR, outliers — but loses the overall shape
```

**Key learning point:** Histograms and boxplots are complementary. Histograms show shape; boxplots show group comparisons and outliers. Use both when exploring a variable. `plt.tight_layout()` fixes overlapping labels automatically.

---

### Challenge 8.2 — FacetGrid

**Objective:** Create small multiples using FacetGrid.

**The challenge:**

1. Create a FacetGrid with one panel per continent, showing a histogram of `life_exp` for the year 2007
2. Set `col_wrap=3` so the panels wrap after 3 columns
3. Add a title to the overall figure: "Life Expectancy Distribution by Continent, 2007"
4. **Bonus:** Overlay a KDE curve on each histogram panel

**Solution:**

```python
g = sns.FacetGrid(
    gap_2007,
    col='continent',
    col_wrap=3,
    height=3.5,
    sharey=False
)

# Histogram
g.map(plt.hist, 'life_exp', bins=10, color='steelblue', edgecolor='white')

# Bonus: KDE overlay
g.map(sns.kdeplot, 'life_exp', color='red', linewidth=2)

g.set_axis_labels('Life Expectancy (years)', 'Count')
g.set_titles(col_template='{col_name}')
g.figure.suptitle('Life Expectancy Distribution by Continent, 2007', 
                   y=1.02, fontsize=14)

plt.tight_layout()
plt.show()
```

**Key learning point:** FacetGrid automates the "small multiples" pattern — the same plot for each level of a grouping variable. It handles the layout, axis labels, and titles for you. `sharey=False` lets each panel use its own y-axis scale, which is appropriate when groups have very different counts.

---

*End of exercise document.*

---

## Quick Reference: Challenge Format

Each challenge follows this template:

```
### Challenge N.M — Title

**Objective:** One sentence — what skill does this exercise practice?

**Setup:** Any code the learner needs pre-loaded.

**The challenge:** The question(s) learners answer. Written as numbered steps.

**Solution:** Complete, runnable code.

**Key learning point:** One or two sentences on the principle demonstrated.
```

---
*See `python203-curriculum-review.md` for the context of each exercise in the course structure.*
