# Module 5.5: Data Cleaning

*Instructor Note: This is a new, crucial module that bridges the gap between data manipulation (Pandas) and data visualisation (Matplotlib/Seaborn). It should be taught as live coding in a Jupyter Notebook. We'll use a deliberately "dirty" version of the gapminder data to demonstrate common problems.*

---

## Welcome to the Real World: Dirty Data

So far today, we've been working with a beautiful, pristine dataset. There are no missing values, every number is actually a number, and all the text is perfectly formatted.

If you have ever collected data in the real world, you know this is a lie.

Real data is messy. Instruments fail and leave blank cells. Humans type "N/A" instead of leaving it blank. People spell "Australia", "australia", and " Australia " differently. 

Data cleaning is often 80% of a researcher's data analysis workload. If you don't clean your data, your visualisations will fail or, worse, they will be wrong without you realizing it.

Let's load a new dataset: `gapminder_dirty.csv`. I have deliberately sabotaged this file to reflect the kind of errors you see every day.

```python
import pandas as pd

# Load the dirty data
dirty = pd.read_csv('gapminder_dirty.csv')

# Let's look at the first few rows
dirty.head()
```

*(Instructor Note: point out issues visible in head: maybe an 'NaN' in life_exp, or an extra space in the continent name).*

We can't spot all the errors just by looking at the first 5 rows. We need a diagnostic toolkit.

## Step 1: Diagnosing the Problems

Before we change anything, we need to know what's broken. There are three questions we always ask a new dataset:

1. **Are there missing values?**
2. **Are the data types correct?**
3. **Are there duplicate rows?**

### Finding Missing Values

In Pandas, a missing value is usually represented as `NaN` (Not a Number). 

We can ask Pandas which cells are missing using `.isnull()`.

```python
# This returns True if missing, False if not
dirty.isnull().head()
```

A wall of Trues and Falses isn't very helpful. But we can chain `.sum()` onto the end of it. In Python, `True` counts as 1 and `False` counts as 0. So adding them up tells us exactly how many missing values are in each column!

```python
# Count missing values per column
dirty.isnull().sum()
```

Ah! We can see that `life_exp` is missing 14 values, and `gdpPercap` is missing 6 values. We will need to deal with those.

### Checking Data Types

Next, we check the data types using `.dtypes`. 

```python
# Check data types
dirty.dtypes
```

Look closely at the output:
- `country` is `object` (string) - Correct.
- `continent` is `object` - Correct.
- `year` is `float64` - Wait, years should be whole numbers (`int`), not decimals!
- `life_exp` is `float64` - Correct.
- `pop` is `object` - **Danger!** Population should be a number. If it's an `object`, it means there's text hidden in that column somewhere, and we won't be able to do math on it.

### Checking for Duplicates

Sometimes data gets entered twice.

```python
# How many duplicate rows are there?
dirty.duplicated().sum()
```

If this returns a number greater than 0, we have duplicate rows. 

Now we have our to-do list:
1. Fix missing values in `life_exp` and `gdpPercap`
2. Fix the data type of `year` and `pop`
3. Remove duplicates

Let's fix them one by one. **Crucial rule:** always work on a copy of your data when cleaning, or re-assign it to a new variable. We'll build a new DataFrame called `clean`.

```python
# Start our cleaning pipeline
clean = dirty.copy()
```

---

## Step 2: Handling Missing Values (NaNs)

There are two main ways to handle missing data: **drop it** or **fill it**. 

### Dropping Missing Data

If we are missing the target variable we want to study (like `life_exp`), we often have no choice but to throw that row away. We can't plot a life expectancy if we don't know what it is.

We use `.dropna()`. We use `subset` to tell Pandas *which* column to check for missing values.

```python
# Drop rows where life_exp is missing
clean = clean.dropna(subset=['life_exp'])

# Verify it worked
clean.isnull().sum()
```

The 14 missing life expectancies are gone.

### Filling Missing Data

For `gdpPercap`, let's try the other approach. Maybe we don't want to lose those rows. We could fill the missing values with a reasonable guess — like the median GDP for that whole dataset.

We use `.fillna()`.

```python
# Calculate the median GDP
median_gdp = clean['gdpPercap'].median()
print(f"Filling missing GDPs with: {median_gdp}")

# Fill the missing values
clean['gdpPercap'] = clean['gdpPercap'].fillna(median_gdp)

# Verify
clean.isnull().sum()
```

Great! No more missing values anywhere.

---

## Step 3: Fixing Data Types

### Converting Floats to Integers

The `year` column loaded as `float64` (e.g., 2007.0) because `NaN` values force a column to become a float. Now that we've removed the NaNs, we can convert it back to a clean integer using `.astype()`.

```python
# Convert year to integer
clean['year'] = clean['year'].astype(int)

# Check the first few rows
clean[['country', 'year']].head()
```

### Forcing Text to Numbers

The `pop` (population) column is an `object` (string). Why? Let's look at the unique values.

```python
# (Instructor: maybe find a row where someone typed "10 million" instead of 10000000)
# Instead of hunting, we can force Pandas to convert it to numeric.

# errors='coerce' means "if you find text you can't turn into a number, turn it into a NaN"
clean['pop'] = pd.to_numeric(clean['pop'], errors='coerce')

# Now we have new NaNs where the text used to be! Let's drop them.
clean = clean.dropna(subset=['pop'])
```

---

## Step 4: Cleaning Up Text Categories

Let's look at the `continent` column.

```python
# What are the unique continents?
clean['continent'].unique()
```

You might see output like: `['Asia', 'Europe', 'africa', 'Africa', 'Americas ', 'Americas']`

This is a classic problem. Capitalisation is inconsistent, and someone accidentally hit the spacebar after "Americas ". To Python, "Africa" and "africa" are two completely different continents. If we group by continent now, we'll get 8 continents instead of 5.

We can fix all text in a column using `.str` methods.

```python
# .str.strip() removes invisible spaces at the start/end
# .str.title() capitalises the first letter of each word

clean['continent'] = clean['continent'].str.strip().str.title()

# Let's check again
clean['continent'].unique()
```

Perfect. Just 5 continents.

---

## Step 5: Removing Duplicates and Renaming Columns

Finally, let's get rid of those duplicate rows we found earlier.

```python
# Drop exact duplicate rows
clean = clean.drop_duplicates()
```

And as a finishing touch, maybe we want our column names to be more readable for plotting later. Let's rename `life_exp` to `life_expectancy`.

```python
# Rename columns using a dictionary: {'old_name': 'new_name'}
clean = clean.rename(columns={'life_exp': 'life_expectancy', 'gdpPercap': 'gdp_per_capita'})

# Let's admire our beautiful, clean dataset!
clean.info()
```

Look at that `info()` output. No missing values. Years are integers. Population is numeric. Continents are clean. 

Now, and **only now**, are we ready to start visualising this data. If we had tried to make a scatterplot with the dirty data, Seaborn would have crashed when it tried to calculate the size of bubbles using the text-corrupted population column.

---

### 🛑 Formative Exercise (10 minutes)

**The Challenge:**

I have provided another file: `survey_dirty.csv`.

1. Load the data into a variable called `survey`.
2. Run a diagnostic: How many missing values are there? What are the data types?
3. Drop any rows missing a `height_cm` measurement.
4. Fill any missing `weight_kg` measurements with the **mean** weight.
5. The `gender` column has inconsistent capitalisation and hidden spaces. Clean it up so there are only clean categories.
6. Run `.info()` to prove your dataset is clean.

*Instructor Note: Put up the red sticky note if you get stuck. Put up the green sticky note when you're done.*

```python
# Solution for Instructor:
survey = pd.read_csv('survey_dirty.csv')
print(survey.isnull().sum())
print(survey.dtypes)

survey = survey.dropna(subset=['height_cm'])

mean_weight = survey['weight_kg'].mean()
survey['weight_kg'] = survey['weight_kg'].fillna(mean_weight)

survey['gender'] = survey['gender'].str.strip().str.lower()

survey.info()
```

---
*End of Module 5.5. Transition to Module 6: Visualisation.*