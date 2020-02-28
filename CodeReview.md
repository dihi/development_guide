# Code Review Guidelines

Things to think through in preparation for the code review:

* Data availability - not using data from the future to make prediction
    * No diagnosis codes during the course of the encounter
    * No results that are not available at the time of prediction used in a model
* Training / test sets - same transformations applied to both sets
    * Standardize training set and then apply that to the test set
* Test set selection - don’t want to pick test set over the same time frame
    * Want a temporal validation on temporal held out set
