---
tags: [DIHI]
title: Getting started with Data Science at DIHI
created: '2020-05-20T03:13:57.093Z'
modified: '2020-06-02T20:55:05.448Z'
---

# Getting started with Data Science at DIHI

This note aims to provide a high-level overview of the activities that data science @ DIHI entails and provides a roadmap and lists of resources to help you get started.

## Project Lifecycle

Before we go into individual resources and concepts that are important to learn, it can be helpful to understand the full life cycle of each of our data-science related projects. Within each step of this life cycle, different skills and values are emphasized. 

## Ideation, Exploratory Analysis, Cohort Creation

In broad strokes, the first part of data science projects at DIHI involves coming up with the right hypotheses and exploring the actual data. Oftentimes, this will involve creating a cohort of patients, encounters, or other entity that can be studied further. For example, imagine that our goal is try to create a model to predict inpatient mortality. Outside of the data science realm, it is important to try and understand *why* such a model might be useful, and in fact whether or not such a solution is even needed. Once we have defined an initial model or task to start with, we can begin by 

 1) Extracting Data
 2) Performing Exploratory Data Analysis
 3) Creating a cohort

The skills involved in this step *roughly ordered in terms of how critical they are*

### Python

Python is a general-purpose programming language that DIHI has settled on for the majority of our work. You may have used other programming languages in the past for statistical analysis, such as SAS or R. In the past, we supported the use of both R and python, but this lead to a much higher overhead and difficulty in productionalizing code (read: caused me too many headaches to code review)

#### Resources for Learning Python

 * DataCamp modules
 * Very quick overview: https://github.com/dsahduke/Jupyter_notebooks_python/blob/master/Python%20Programming.ipynb
 * Book Reference: [Learning Python the Hard Way](https://files.meetup.com/18552511/Learn%20Python%20The%20Hard%20Way%203rd%20Edition%20V413HAV.pdf)

#### Goals

There is so much to learn with any programming language, and although python is easy to learn, it is inevitably difficult to master. The level of proficiency that we'll hope to get to will depend on your programming background, but I think a good goal to strive for would be:

 * Variables, comments, overall syntax
 * Working with strings, lists, and dictionaries and other python libaries
 * Basic Control flow (`if`, `elif`, `else`, `while`, `for`, etc)
 * Writing custom functions

### Pandas (Python Library)

Pandas is the primary way with which we interface with datasets. It is a library for python that allows us to work with structured datasets from files on your computer to databases. It supports an incredible amount of functionality, from slicing data, reshaping, subsetting, transforming, etc. Because of this, the learning curve can get steep, although we will try and enforce best practices to make sure you don't have to get into all of the craziness that is pandas.

#### Resources:

 * Datacamp Modules
 * [Pandas Overview](https://github.com/dsahduke/Numpy-Pandas/blob/master/Introduction%20to%20Pandas.ipynb) 
 * [DIHI Pandas Style Guide](https://github.com/dihi/development_guide/blob/master/Pandas.ipynb)
 * [Pandas Documentation](https://pandas.pydata.org/docs/)

#### Goals:
 * Learn the difference between a `Series` and a `DataFrame`
 * Be able to subset and filter data using `.loc`
 * Learn to implement joins with `pd.merge`
 * Learn simple `split-apply-combine` examples with `.groupby` and `.apply`
  

### SQL (Structured Query Language)

Much of the data that we work with is stored in something called relational databases (or more formally, Relational Database Management Systems - RDBMS). In order to query the data to answer questions you may have, it is important to learn the fundamental language for communicating with these databases. Although SQL queries can get extremely complex, learning the basics will pay large dividends

#### Resources for Learning SQL

 * DataCamp modules
 * Quick overview from my class: https://github.com/dsahduke/sql_and_rdbms/blob/master/sql%2Brmdbs.ipynb
 * [A fun interactive book for learning the basics](https://selectstarsql.com/)

#### Goals
 * Understanding of `SELECT`, `FROM`, `JOIN`, `WHERE`
 * Be able to write basic queries to extract and filter data from multiple tables

### Plotting Libraries (Seaborn, Matplotlib)

Plotting is a useful way to visualize data and to communicate results. Python has a rich ecosystem of plotting libraries, all with varying degrees of complexity and flexibility. For example, `Pandas` has some plotting capability that uses `matplotlib` under the hood. You can plot a histogram by calling `df['column'].hist()`. However, the amount of customization is limited and the plots are often not very aesthetic. The `Seaborn` library, on the other hand, aims to be an easy to use and also nice-looking alternative, which I would recommend you learn. 

#### Resources for plotting in Python

 * [Seaborn documentation](https://seaborn.pydata.org/)
 * [Pyplot documentation](https://matplotlib.org/tutorials/introductory/pyplot.html)
 * DataCamp

#### Goals
 * Given a dataframe, be able to create simple box plots, histograms, etc. from the data
 * Learn how to facet (or create multiple plots split by a particular variable value)
 * Learn basic customization (title, axes, legend, etc.)

## Shaping Data and Machine Learning

Once we have performed the main exploratory analyses and found a cohort of interest, we usually proceed to defining and outcome, shaping data, and then fitting machine learning models. Entire graduate programs can be structured around machine learning, so we'll cover mostly the basics and you may work on a variety of models if your project calls for it. The main resource for machine learning that I would recommend is the following text:

> https://faculty.marshall.usc.edu/gareth-james/ISL/ISLR%20Seventh%20Printing.pdf

In particular, you'll want to gain at least passing familiarity with the following concepts:

 * Regression vs. Classification
 * Assessing Model Accuracy (we'll go much more in depth than the book does on this topic)
 * Model selection and Regularization
 * Classification via Logistic Regression
 * Tree-based methods

If you have any questions regarding machine learning, please feel free to reach out directly and I'd be happy to explain a particular concept to you.

We may also use neural networks for some of our models, but they are outside the scope of what you'll need to know. Once we get to the point where neural networks will be necessary, I will provide the necessary resources if you're interested.

The main software component of machine learning will be the python package `scikit-learn`. Although the documentation exists, I would recommend that you learn by example since the application programmer interface can be a bit difficult to understand at first glance. 

In terms of shaping data and generating outcomes, most of that will be covered by fundamental `Pandas` topics that you will have needed to pick up for the initial exploratory work. Shaping data also includes feature engineering, which will involve you using some of your clinical knowledge and is best explained in the context of a project, so we will pass on that for now as well.

## Productionalization

One of the most important steps in the process of data science at DIHI is getting things ready for production. You may have completed all of the steps above, but a model is only truly useful once the results are in the hands of the end users. Although most of this work will be done by senior staff at DIHI, it is important that we adhere to some of the principles of production-level code. In particular, we care that 

 * All code adheres to internal style guides
 * Majority of functions are unit-tested
 * Repositories are created with feature-branch gitflow

Although this may seem completely unrelated to all of the prior data science work, it is perhaps the most critical piece to ensure that the products that we create build trust and are ultimately effective. An important step to helping this process will be to adhere to the following style guide (for Pandas):

 * github.com/dihi/development_guide/blob/master/Pandas.ipynb

In addition, making sure the core tenets of [pep-8](https://www.python.org/dev/peps/pep-0008/) are not violated will be helpful as well. This may seem nitpicky and analogous to teachers making sure that your essays have only one space after periods or exactly 12-point font with 1-inch margins, but it is imperative to make sure that our team can maintain and feel confident about software that will ultimately impact the lives of patients at Duke. 

## Versioning and Questions

The current version of this guide is: `0.1`.
For questions, please reach out to michael.gao@duke.edu







