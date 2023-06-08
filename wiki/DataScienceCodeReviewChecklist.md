# Data Science Code Review Checklist (Under Construction!)
 
When you write scripts that will be used as part of a data science workflow, such as querying data, cleaning data, or running models/evaluation, you will need to request a code review. This code review process is intended to serve two main purposes. The first is to ensure that any code that you write that will eventually be utilized by others (whether that means other team members or in production) has been checked for errors and potential improvements. The second is to serve as an educational opportunity - there are often ways to optimize your code that are much easier to learn if they come from an experienced individual rather than figuring it out yourself the hard way.

Code reviews take time and require the reviewer to read your code (obviously). Therefore, it is extremely important that you make sure that your code adheres to as many conventions described in this repository as possible so that the review process can be expedited. This document will serve as a checklist to make sure that your code is ready for code review.


## 1. Make sure that your code will run from beginning to end from the command-line.

If a reviewer sets the working directory to where your code is and runs `python <yourscript>.py`, the script should be able to run from beginning to end.

## 2. Do not put unnecessary code in your script.

Oftentimes, you will try and perform exploratory work or print statements in the script to help yourself debug this script. Either set a flag such as `DEBUG = False` as the top of the script or add a command line argument to make sure that this does not run each time the script is executed. 

In addition, do not import libraries that you do not use.

## 3. Adhere to the Style guide in this repository

When writing python code, use the style guide in this repository (Style.md). This will make it much easier to read your code. When in doubt, use [PEP-8](https://www.python.org/dev/peps/pep-0008/) guidelines. One way to test to see if your code is `PEP-8` compliant is to install pylint and run `pylint <your_script>.py`

## 4. If you have installed additional libraries, let the reviewer know which version you have installed

For example, if you download `catboost`, you can find the version by running

```
import catboost

print(catboost.__version__)
```

## 5. Common pitfalls to avoid as you prepare for your code review

* Data availability - do not use data to make predictions that is not available to the model at the time of prediction
    * Diagnosis codes during the course of an encounter are not available during the encounter
    * Many analyte results are not available at the time of collection. Only use analyte results that are available at the time of prediction.
* Training / test sets - apply the same transformations to both sets
    * Standardize transformations on the training set and then apply that exame same set of transformations to the test set
* Test set selection - test model on a distinct, more recent temporal set of data
    * Can pick either a discrete amount of time (e.g., most recent 6 months, most recent 12 months) or a discrete propotion of data (e.g., most recent 10% of data, most recent 20% of data)
