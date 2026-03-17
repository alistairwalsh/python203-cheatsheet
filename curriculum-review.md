# Curriculum Review: PYTHON203 — Data Manipulation and Visualisation in Python
**Prepared for:** Intersect Australia  
**Prepared by:** Alistair Walsh, Intersect Trainer  
**Date:** March 2026  
**Version:** 1.0

---

## Executive Summary

PYTHON203 (Data Manipulation and Visualisation in Python) is a two-day workshop aimed at researchers who want to go beyond spreadsheets and use Python for data analysis. The course covers Pandas and Matplotlib/Seaborn, with the Software Carpentry (SWC) gapminder dataset providing a running example.

The course has real strengths: the visualisation modules (6–8) contain genuinely excellent live-coding content, and the choice of gapminder data is pedagogically sound. However, a structural review reveals four significant gaps that reduce learning outcomes and instructor confidence:

1. **Four modules (1, 3, 4, 5) contain no live content** — they are collections of external links. Learners who need those topics (indexing, data types, merging) are directed away from the classroom.
2. **There are no formative exercises.** The SWC model that PYTHON203 is built on depends on a teach → exercise → discuss rhythm. Without exercises, learners are passive observers.
3. **There is no data cleaning module.** Cleaning is the first thing researchers do with real data and is arguably more important to teach than advanced visualisation.
4. **The motivating narrative is absent.** Day 1 begins with setup logistics and links; there is no moment that answers the question *"why am I here and what will I be able to do?"*

This document provides:
- A principled critique against SWC pedagogy
- A module-by-module analysis with specific, actionable recommendations
- A proposed revised course structure
- Suggested two-day timeline
- Supporting materials (see companion files)

The recommended approach is a **targeted revision** rather than a full rewrite. The visualisation modules are strong and should be preserved. The Pandas modules need live content written to fill the link gaps. A new data-cleaning module is proposed for insertion between Pandas and Visualisation.

---

## 1. Alignment with Software Carpentry Principles

PYTHON203 is positioned as a follow-on to Software Carpentry's Python Novice Inflammation workshop. It inherits SWC's dataset choices and general framing. It should therefore be evaluated against SWC's pedagogical principles.

### 1.1 Formative Assessment

**SWC principle:** Learners should receive a challenge exercise every 10–20 minutes. Exercises serve multiple purposes: they reveal misconceptions before they compound, they give learners a moment of active practice, and they give instructors real-time feedback about pace.

**Current state:** PYTHON203 has zero formative exercises. The visualisation modules in particular run for extended periods of live coding with no pause for learner practice. Module 6 presents 12 incremental versions of the same scatterplot in sequence — a learner who loses the thread at version 4 has no recovery point.

**Recommendation:** Add a 5–10 minute challenge exercise at the end of every substantive module. See companion file `python203-exercises.md` for complete exercise set.

### 1.2 Live Coding

**SWC principle:** Instructors type code in front of learners. Learners follow along. The pace should be set by the slowest typist in the room, not the fastest explainer. Slides replace live coding only for conceptual overviews (1–2 minutes maximum).

**Current state:** The visualisation modules (6–8) are excellent examples of live coding — incremental, well-commented, building toward a meaningful output. Modules 1, 3, 4, 5 are not live coding; they are reading lists.

**Recommendation:** Modules 1, 3, 4, 5 must be replaced with live notebook content. The reading lists can remain as optional "going further" links at the end of each module.

### 1.3 Motivation and Authentic Context

**SWC principle:** Every lesson should answer *"why does this matter to me?"* within the first five minutes. Novice learners need to see the end goal before they begin building toward it.

**Current state:** Module 0 is logistics. Module 1 is links. The first moment of genuine engagement is Module 2, which begins loading a CSV. There is no opening that shows learners what they will be able to do by the end of the course, no connection to their own research workflows, and no explanation of why Python is a better choice than the tools they already know.

**Recommendation:** Add a 10–15 minute motivating introduction at the start of Day 1. See companion file `python203-motivating-intro.md` for a complete script.

### 1.4 Narrative Thread

**SWC principle:** A single dataset running throughout a course creates coherence. Learners spend cognitive energy on the dataset problem, not on constantly re-orienting to new data.

**Current state:** PYTHON203 has a partial narrative thread. Modules 2–5 use gapminder-style data (life expectancy, GDP per capita, population by country and year). Modules 6–8 also use gapminder data, specifically a 2016 snapshot. However:
- Module 1 links to SWC's inflammation data (a completely different dataset)
- There is no explicit framing of the research question ("what is the relationship between wealth and health across countries and time?")
- The transition from data manipulation to visualisation is not narrated — learners do not see why the cleaning/manipulation steps they did in Pandas are prerequisites for the plots they make in Seaborn

**Recommendation:** Establish the research question explicitly in the motivating introduction. Make each module's contribution to that question explicit. *"Now that we can load and clean the data, we're going to ask: can we see the relationship between income and life expectancy? That's going to require the visualisation tools in the next section."*

### 1.5 Sticky Notes / Help Signals

**SWC principle:** Physical or digital help signals (sticky notes, Zoom reactions) allow instructors to pace themselves against learner progress without interrupting the flow.

**Current state:** Not mentioned in course materials. Should be established in Module 0.

**Recommendation:** Add sticky note protocol to Module 0 logistics. For online delivery, specify the Zoom reaction equivalent.

---

## 2. Module-by-Module Analysis

### Module 0: Getting Started
**Current content:** Logistics — software installation, dataset download, Jupyter setup.  
**Assessment:** Adequate for its purpose.  
**Issues:**
- Does not establish help signal protocol
- Does not preview what learners will achieve
- Missing: a 2-minute "by end of day 2, you'll be able to make this" moment with a polished example plot

**Recommendations:**
1. Add sticky note / help signal protocol
2. Add 2-minute preview: show the finished gapminder bubble chart and name the tools that will produce it
3. Confirm all imports work at the end of Module 0 (a simple `import pandas as pd; print(pd.__version__)` cell that all learners run together)

---

### Module 1: Short Python Recap
**Current content:** Links to SWC Python Novice and Data Carpentry Ecology lessons.  
**Assessment:** Not fit for purpose as written. External links are not a module.  
**Issues:**
1. Learners cannot be expected to self-direct through SWC materials in real time
2. The SWC materials cover far more than is needed (loops, conditionals, file I/O) — and use a different dataset
3. There is no live coding moment to warm up the room
4. The gap between "what SWC taught" and "what PYTHON203 needs" is never bridged

**Recommendations:**
1. Replace with a 20–30 minute live coding module covering *only* what is needed for Pandas: lists, dictionaries, functions, and an informal conceptual introduction to DataFrames as "a dictionary of lists"
2. The SWC links can remain as "if you need a deeper refresher" resources
3. Include one challenge exercise: "Write a function that takes a list of numbers and returns the mean"

See companion file `python203-module1-notebook.md` for complete replacement content.

---

### Module 2: Pandas Part I — Loading and Exploring Data
**Current content:** Live notebook — reading CSVs, `df.head()`, `df.info()`, `df.describe()`, column selection, basic filtering.  
**Assessment:** Good. This is the strongest Pandas module. The incremental approach works well.  
**Issues:**
1. No formative exercise
2. `df.info()` output is shown but not fully explained (dtype column, non-null counts)
3. No connection forward to cleaning (the `info()` output often reveals the problems we'll fix later)

**Recommendations:**
1. Add formative exercise: "How many rows have GDP data for Asia in 1997?"
2. When showing `df.info()`, pause to ask: "What would it mean if a column showed fewer non-null values than the total rows?"
3. Add one sentence of forward linkage: "You might notice some columns have unexpected types — we'll come back to that in the data cleaning module."

---

### Module 3: Pandas Part II — Indexing, Slicing, Subsetting
**Current content:** Links to SWC Data Carpentry Pandas episode.  
**Assessment:** Not fit for purpose. Indexing and subsetting are not optional topics — they are used throughout Modules 6–8.  
**Issues:**
1. `.loc` vs `.iloc` is not covered live; learners who haven't done SWC will be lost
2. Boolean indexing (the `df[df['col'] > value]` pattern) is used extensively in visualisation without being explicitly taught
3. MultiIndex and hierarchical indexing (briefly relevant for groupby results) is not covered anywhere

**Recommendations:**
1. Write a 25–30 minute live module covering:
   - `.loc` for label-based selection
   - `.iloc` for integer-based selection
   - Boolean indexing with one and two conditions (`&`, `|`)
   - The `.query()` method as a readable alternative
2. Include formative exercise: filter the gapminder data to show only European countries with life expectancy above 75 in the year 2007
3. Link to SWC materials as "going further"

---

### Module 4: Pandas Part III — Data Types and Formats
**Current content:** Links to SWC Data Carpentry Pandas episodes on data types.  
**Assessment:** Not fit for purpose. Data types are foundational to understanding why operations fail.  
**Issues:**
1. Type errors are the most common source of frustration in pandas (`object` vs `int64`, strings that look like numbers)
2. Datetime handling — critically important for longitudinal research data — is not covered anywhere
3. Categorical data type — relevant for ordered variables like education level — is not mentioned

**Recommendations:**
1. Merge this module into the new **Data Cleaning** module (see Section 3)
2. Cover dtype inspection and conversion in the context of real cleaning tasks
3. Include datetime parsing as a 5-minute live demo: `pd.to_datetime()`
4. Link to SWC materials as "going further"

---

### Module 5: Pandas Part IV — Combining DataFrames
**Current content:** Links to SWC Data Carpentry Pandas episodes on merging.  
**Assessment:** Not fit for purpose. Merging is used in the gapminder exercises implicitly.  
**Issues:**
1. `pd.merge()` vs `pd.concat()` distinction is never taught
2. The difference between inner, left, right, and outer joins is not covered
3. Learners who have multiple CSV files from their own experiments (extremely common in research) have no framework for combining them

**Recommendations:**
1. Write a 20–25 minute live module covering:
   - `pd.concat()` for stacking DataFrames with the same columns (e.g., "I have a file per year")
   - `pd.merge()` for joining on a key column (e.g., "I have metadata in a separate file")
   - Demonstration of inner vs left join with a simple example
2. Use gapminder data: show merging a country metadata file (region, income group) onto the main gapminder dataset
3. Include formative exercise: concatenate two gapminder year subsets and verify the result

---

### Module 6: Visualisation Part I — Scatterplot
**Current content:** Step-by-step construction of a gapminder scatterplot through 12 incremental versions, using Seaborn and Matplotlib. Good narrative and authentic dataset.  
**Assessment:** Strong content, structural problem. The 12 versions are not adequately motivated.  
**Issues:**
1. The opening does not frame the visual question: *"We want to see whether richer countries have longer life expectancy"*
2. Each styling iteration (version 1 → 12) is presented as "here's the next thing to add" without explaining *why* this particular change improves the communication
3. No formative exercise
4. The `hue` parameter is added in version 6 but the reason (to reveal a confounding variable — continent) is not made explicit

**Recommendations:**
1. Begin the module with the finished plot and ask: "What story is this plot telling? What would the same data look like without the colour coding?"
2. Group the 12 versions into three conceptual phases:
   - **Phase 1 (versions 1–3):** Getting the data right — axes, scale, data subset
   - **Phase 2 (versions 4–7):** Revealing structure — colour, size, the continent story
   - **Phase 3 (versions 8–12):** Publication quality — labels, fonts, saving to file
3. At each phase transition, pause and ask: "What question does the current plot answer? What question can't it answer yet?"
4. Add formative exercise: "Reproduce this plot but colour by income group instead of continent. What do you notice?"

---

### Module 7: Visualisation Part II — Barplot
**Current content:** Basic barplot → grouped barplot → barplot with custom function → defensive programming with assertions.  
**Assessment:** Good. The defensive programming section is genuinely excellent and differentiates this course.  
**Issues:**
1. The motivation for moving from scatterplot to barplot is not stated ("when do I use a bar chart vs a scatter plot?")
2. The custom function pattern is introduced without explaining *why* we would write a function for a plot (reproducibility, parameterisation)
3. No formative exercise

**Recommendations:**
1. Open with a 2-minute framing: "A scatterplot shows continuous relationships. A barplot shows comparisons across categories. Our question now is: how does average life expectancy compare across continents?"
2. Before the custom function, motivate it: "We've just made this plot for 2007. What if we wanted the same plot for 1997? And 1987? We'd copy-paste the code three times — but what if we later want to change the colour? Functions solve this."
3. Add formative exercise: "Modify the function to accept a `palette` parameter. Call it with three different palettes."

---

### Module 8: Visualisation Part III — Advanced Plots
**Current content:** Histogram, KDE/density plot, boxplot, FacetGrid, heatmap, line plot, jointplot.  
**Assessment:** Good breadth. Some coherence issues.  
**Issues:**
1. Seven plot types in one module is too many without a decision framework ("when would I use each of these?")
2. FacetGrid is introduced after the individual plots — this is the right order — but the connection back to the barplot function is not made ("FacetGrid is Seaborn's built-in version of what we built manually")
3. No formative exercise
4. The heatmap uses a correlation matrix — this is a slight conceptual leap that should be flagged

**Recommendations:**
1. Open with a one-slide decision framework (can be verbal): "Histogram → single variable distribution; Boxplot → comparing distributions across groups; Line plot → change over time; FacetGrid → small multiples; Heatmap → relationships between many variables at once"
2. After FacetGrid: "Notice that this does automatically what we built manually in Module 7. Real libraries are often implementations of patterns you've already learned."
3. Add formative exercise: "Make a FacetGrid of histograms of life expectancy, one panel per continent."
4. Consider separating the heatmap/jointplot into a "bonus" section for fast finishers

---

### Module 9: Wrap Up
**Current content:** Minimal. Appears to be a placeholder.  
**Assessment:** Not fit for purpose.  
**Issues:**
1. No consolidation of what was learned
2. No connection to learners' own work
3. No "where to go from here" with specific, actionable next steps
4. No feedback mechanism

**Recommendations:**
1. Replace with a 20–25 minute structured closing:
   - **5 min:** Recap the narrative arc — "We started with a CSV, cleaned it, explored it, then made publication-quality figures"
   - **5 min:** "Applying this to your own data" — a brief live demo of loading an arbitrary CSV (prepared in advance), running `df.info()` and `df.describe()`, and making a quick plot
   - **5 min:** Specific "next steps" resources: Pandas documentation, Seaborn gallery, matplotlib tutorials, SWC Python lessons for deeper coverage
   - **5 min:** Course feedback form (if available)
   - **5 min:** Open Q&A / "help me apply this to my data" time

---

## 3. Proposed New Module: Data Cleaning

One of the most significant gaps in PYTHON203 is the absence of any data cleaning content. In every researcher's workflow, data cleaning precedes analysis. This is not a marginal topic — it is the majority of the work.

**Proposed module: "Pandas Part V — Cleaning Real Data"**

This module would sit between the current Module 5 (Combining DataFrames) and Module 6 (Visualisation Part I). Duration: approximately 40–45 minutes.

**Learning objectives:**
By the end of this module, learners will be able to:
1. Identify missing values, wrong types, and inconsistent formatting in a DataFrame
2. Use `df.isnull()`, `df.dropna()`, and `df.fillna()` appropriately
3. Convert column types with `df.astype()` and `pd.to_numeric()`
4. Rename columns for clarity and consistency
5. Find and remove duplicate rows
6. Apply these techniques to a realistic "dirty" version of the gapminder dataset

See companion file `python203-data-cleaning.md` for complete module content.

---

## 4. Proposed Revised Course Structure

### Overview

| # | Module | Type | Duration | Day |
|---|--------|------|----------|-----|
| 0 | Getting Started + Motivating Intro | Setup + Demo | 30 min | Day 1 |
| 1 | Python Recap (live) | Live coding + exercise | 35 min | Day 1 |
| 2 | Pandas I: Loading and Exploring | Live coding + exercise | 40 min | Day 1 |
| — | *Morning break* | — | 15 min | Day 1 |
| 3 | Pandas II: Indexing and Subsetting | Live coding + exercise | 35 min | Day 1 |
| 4 | Pandas III: Groupby and Aggregation | Live coding + exercise | 35 min | Day 1 |
| — | *Lunch* | — | 60 min | Day 1 |
| 5 | Pandas IV: Combining DataFrames | Live coding + exercise | 30 min | Day 1 |
| NEW | Pandas V: Cleaning Real Data | Live coding + exercise | 45 min | Day 1 |
| — | *Afternoon break* | — | 15 min | Day 1 |
| 6 | Vis I: Scatterplot | Live coding + exercise | 50 min | Day 1 |
| — | *End of Day 1 recap* | — | 10 min | Day 1 |
| 7 | Vis II: Barplot + Functions | Live coding + exercise | 50 min | Day 2 |
| — | *Morning break* | — | 15 min | Day 2 |
| 8a | Vis III: Histogram, Boxplot, Line | Live coding + exercise | 50 min | Day 2 |
| 8b | Vis IV: FacetGrid, Heatmap, Jointplot | Live coding (bonus) | 30 min | Day 2 |
| — | *Lunch* | — | 60 min | Day 2 |
| — | *Bring your own data* session | Hands-on | 45 min | Day 2 |
| 9 | Wrap Up + Next Steps | Recap + Q&A | 25 min | Day 2 |

**Total instructional time:** ~7 hours across 2 days (excluding breaks)

### Notes on the revised structure

**"Bring your own data" session (Day 2 afternoon):** This is the highest-value session for learner retention. Learners are asked in the pre-course survey to bring a CSV from their own work. The instructor circulates and helps learners apply the morning's techniques to their own dataset. This has a powerful motivational effect and produces immediate, transferable skill.

**Module 4 renamed:** The current "Data types and formats" module is subsumed into the new Data Cleaning module. In its place, a dedicated **Groupby and Aggregation** module is recommended. `df.groupby().mean()` and similar operations are the most common analytical step after loading and cleaning, and they are not adequately covered in the current course.

**Module 8 split:** The current Module 8 covers seven plot types. Splitting into 8a (core types: histogram, boxplot, line) and 8b (advanced: FacetGrid, heatmap, jointplot) allows 8b to be treated as bonus/optional material for fast groups or Day 2 afternoon overflow.

---

## 5. Detailed Two-Day Timeline

### Day 1

| Time | Activity | Notes |
|------|----------|-------|
| 09:00 | Welcome and logistics (Module 0) | Sticky notes, help protocol, confirm software works |
| 09:15 | Motivating introduction | Show finished plot, frame research question, why Python |
| 09:30 | Module 1: Python Recap | Lists, dicts, functions, DataFrame concept |
| 10:05 | Module 1 exercise + discuss | "Write a mean function" |
| 10:15 | Module 2: Pandas I | Load CSV, head/info/describe, column selection |
| 10:55 | Module 2 exercise + discuss | Filter rows, count, inspect |
| 11:00 | **Morning break** | |
| 11:15 | Module 3: Pandas II — Indexing | .loc, .iloc, boolean indexing, .query() |
| 11:50 | Module 3 exercise + discuss | Filter Europe, life expectancy > 75, 2007 |
| 12:00 | Module 4: Pandas III — Groupby | groupby, mean, count, agg, sort_values |
| 12:35 | Module 4 exercise + discuss | Mean GDP per capita by continent |
| 12:45 | **Lunch** | |
| 13:30 | Module 5: Pandas IV — Combining | concat, merge, join types |
| 14:00 | Module 5 exercise + discuss | Concatenate two year subsets |
| 14:10 | Module NEW: Data Cleaning | isnull, dropna, fillna, astype, rename, duplicates |
| 14:55 | Data cleaning exercise + discuss | Clean the dirty gapminder file |
| 15:00 | **Afternoon break** | |
| 15:15 | Module 6: Vis I — Scatterplot | 3-phase approach, colour, scale, labels |
| 16:05 | Module 6 exercise + discuss | Colour by income group |
| 16:20 | End of Day 1 recap | What we did, what's tomorrow |
| 16:30 | **End of Day 1** | |

### Day 2

| Time | Activity | Notes |
|------|----------|-------|
| 09:00 | Day 1 recap, questions | 10 min max |
| 09:10 | Module 7: Vis II — Barplot + Functions | Bar, grouped bar, custom function, assertions |
| 10:00 | Module 7 exercise + discuss | Parameterise the function |
| 10:10 | **Morning break** | |
| 10:25 | Module 8a: Vis III — Core plots | Histogram, boxplot, line plot |
| 11:15 | Module 8a exercise + discuss | FacetGrid of histograms |
| 11:30 | Module 8b: Advanced plots (optional) | FacetGrid, heatmap, jointplot |
| 12:00 | **Lunch** | |
| 13:00 | Bring Your Own Data session | Circulate, help learners apply to their own CSVs |
| 13:45 | Share-out: 2–3 learners show what they made | Optional but powerful |
| 14:00 | Module 9: Wrap Up | Recap, next steps, resources, feedback |
| 14:25 | Open Q&A | |
| 14:30 | **End of Day 2** | |

---

## 6. Priority Recommendations

If a full revision is not feasible before the next delivery, the following changes can be made in priority order:

### Immediate (before next delivery, ~2–3 hours work)
1. **Add the motivating introduction** (script provided in `python203-motivating-intro.md`)
2. **Add formative exercises to Modules 6–8** (exercises provided in `python203-exercises.md`)
3. **Add the sticky note protocol to Module 0**

### Short-term (before next quarter, ~1 day work)
4. **Write live content for Module 1** (content provided in `python203-module1-notebook.md`)
5. **Write live content for Module 3** (indexing and subsetting — most critical Pandas gap)
6. **Add the Data Cleaning module** (content provided in `python203-data-cleaning.md`)

### Medium-term (course refresh, ~2–3 days work)
7. **Write live content for Modules 4 and 5**
8. **Revise Module 6 to use the 3-phase structure**
9. **Add "Bring Your Own Data" session to Day 2**
10. **Revise Module 9 with structured wrap-up content**

---

## 7. Supporting Materials

The following companion files accompany this review:

| File | Contents |
|------|----------|
| `python203-exercises.md` | Complete formative exercise set for all modules |
| `python203-motivating-intro.md` | Script for Day 1 opening (10–15 min) |
| `python203-module1-notebook.md` | Replacement Module 1 as a Jupyter notebook narrative |
| `python203-data-cleaning.md` | New Data Cleaning module as a Jupyter notebook narrative |

---

## Appendix A: Recommended Resources for Further Development

- **The Carpentries Instructor Training:** https://carpentries.github.io/instructor-training/
- **SWC Python Novice Gapminder lesson:** https://swcarpentry.github.io/python-novice-gapminder/
- **Pandas documentation (user guide):** https://pandas.pydata.org/docs/user_guide/
- **Seaborn tutorial:** https://seaborn.pydata.org/tutorial.html
- **Teaching Tech Together (Greg Wilson):** https://teachtogether.tech/ — the theoretical foundation for SWC pedagogy

---

*End of review document.*
