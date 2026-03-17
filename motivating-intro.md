# PYTHON203 — Motivating Introduction
**Day 1 Opening Script — 10–15 minutes**  
**Purpose:** Establish the "why", show the end goal, frame the two-day narrative

---

## Instructor Notes

**Before you start:**
- Have the final gapminder scatter plot displayed on screen as people walk in (or have it ready to show immediately)
- Have the course dataset downloaded and placed in the working directory
- Have a text editor or Jupyter notebook open but blank — you're not live coding yet

**Tone:** Conversational, not lecturing. Pause for real responses when you ask questions. This is a dialogue, not a monologue.

**Timing:** This runs about 12–15 minutes. Don't rush it. The time investment here pays off for two days.

---

## The Script

---

*[Show the finished gapminder scatter plot — the one with log-scale x-axis, coloured continents, population-sized bubbles, clean labels.]*

So — before we do anything else, I want to show you what we're going to build together by end of today.

This is a plot I made with about 25 lines of Python. It's showing data from 142 countries in 2007. Along the bottom, GDP per capita — how wealthy a country is on average. On the side, life expectancy in years. Each bubble is a country. The size of the bubble is the population. The colour tells you which continent it's on.

Take a moment to look at it.

*[pause 10–15 seconds]*

What's the story here? What does this plot tell you?

*[Take 2–3 responses. Guide toward: richer countries tend to have longer life expectancy, but there's a ceiling effect; Africa is clustered in the bottom-left; some Asian countries have high life expectancy despite modest GDP.]*

Exactly. And notice what you just did — you looked at 142 numbers and immediately spotted patterns. That's what visualisation does. That's why we're here.

---

Now, hands up: how many of you have done this kind of analysis in Excel?

*[Most hands go up]*

And how many of you have wanted to do something that Excel just couldn't do — or it could technically do it but it took two hours of clicking and dragging and then someone emailed you a new data file and you had to start over?

*[Laughter, knowing nods]*

Yeah. That's the thing about Excel. It's brilliant for looking at data. It's painful for *processing* data. And it has a secret problem that nobody talks about, which is that it doesn't record what you did. I can give you this plot, but I can't give you the Excel steps that produced it in a way you could repeat or verify.

With Python, I can give you the script. Twenty-five lines. You run it, you get the exact same plot. Your collaborator runs it, they get the same plot. A reviewer asks "where did this come from?" — you send them the script.

That's reproducibility. That's what makes Python worth learning.

---

Here's the other thing I want to say about this plot: the data behind it is not exotic.

It's a CSV file. That's it.

*[Open the file in a text editor or show it briefly]*

Country, continent, year, life expectancy, population, GDP per capita. Columns and rows. Every one of you has a file like this. Maybe it's survey responses. Maybe it's experimental measurements. Maybe it's clinical records, or metadata from samples, or census data, or grant expenditure. Different numbers, same shape.

And that's the skill we're building over these two days — the ability to take a CSV file and turn it into something that tells a story.

---

Let me tell you what the two days look like, because I think it helps to know where you're going.

Today, we're going to spend most of the morning on the engine: Pandas. Pandas is the Python library that handles tabular data — the kind of data you're used to in Excel. We're going to load data, explore it, filter it, summarise it, merge files together, and — here's the bit nobody else teaches but everybody needs — we're going to clean it. Because real data is never clean. We'll talk about what to do when values are missing, when a number column has somehow loaded as text, when someone typed "australia" in one row and "Australia" in another.

This afternoon, we start making pictures. We're going to build that plot I just showed you, step by step. Not by copying and pasting a finished script — by typing it together and understanding every line.

Tomorrow, we'll expand to more plot types: bar charts, histograms, box plots, grids of small multiples. And in the afternoon, you'll have time to apply what you've learned to your own data. I'd encourage you to have a CSV from your own work open and ready for that session.

---

A quick note on how this course works.

We're going to type code together. I'll type, you'll type. If you're faster, wait for the rest of the room. If you fall behind, use the red sticky note — that's our signal for "I need a moment." If something goes wrong and you get an error, that's not a problem — errors are how you learn. Put up the red sticky and we'll sort it out.

About every 20–30 minutes, we'll stop and you'll work through a short challenge. These are not tests. Nobody is marking you. They are practice opportunities, and they are the most important part of the course. Research consistently shows that people who attempt exercises, even if they don't finish them, learn more than people who just watch. So please engage with them, even if your answer is wrong.

If you have a question, ask it. If you think I've made a mistake, tell me — I probably have, and it's more useful to catch it than to let it go.

---

Right. So let's start.

We're going to use this dataset for the whole two days. It's from Gapminder — an organisation founded by the late Hans Rosling, who was, genuinely, one of the great data communicators of the twentieth century. If you've never seen his TED talk, watch it tonight. It's 20 minutes and it will change how you think about data.

The dataset covers 142 countries across twelve time points from 1952 to 2007. Life expectancy, population, and GDP per capita. Simple data, profound story.

Our question — the thread that runs through both days — is this: *What is the relationship between wealth and health, and how has it changed over 50 years?*

By the time we're done, we'll have answered that question in several different ways, with several different types of plot. And you'll have the tools to ask the same kind of question about your own data.

Let's open Jupyter and get started.

---

## Optional Extension (if time permits, ~3 min)

*[Only use this if the room is engaged and you have time before the scheduled start of Module 1.]*

Actually — before we open Jupyter — one more thing.

I want to show you what this data looked like 50 years earlier.

*[Filter the plot to 1952 or show a second saved plot]*

Same axes. 1952. What do you notice?

*[pause]*

The whole world was poorer. Life expectancy was shorter everywhere. And the gap — the spread from bottom-left to top-right — was actually smaller. Not because things were more equal, but because nobody was in the top-right corner yet.

Now 2007.

*[Switch back]*

In 55 years, most of the world moved to the right and upward. That's one of the most remarkable things that has ever happened in human history, hiding in a CSV file.

That's what data analysis lets you see.

Okay. Now let's open Jupyter.

---

## Post-Script Notes for Instructors

**Common questions at this stage:**

*"Do I need to know Python already?"*  
We'll do a quick Python recap right after this. We're covering only what you need for today — lists, dictionaries, functions. If you did the pre-course SWC lesson, great. If not, don't worry.

*"Can I use this on my own data?"*  
Yes, and we'll do exactly that on Day 2 afternoon. Bring a CSV.

*"What version of Python?"*  
Python 3.x. Specifically the version in your Anaconda/conda environment. Don't worry about the exact version.

*"Will you share the slides / notebook?"*  
All notebooks and exercises are available in the shared folder / GitHub repo (link in chat).

---

*End of motivating introduction script.*
