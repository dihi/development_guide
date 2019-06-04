
```
Author: Michael Gao
Last Updated: 06/04/2019
```
#### TODO: 
This document is a work in progress. Planned expansions include: 

    1. Timestamps/Timezones, etc.
    2. Multi-indices
    3. Keeping data when using .aggregate

# Importing 

When importing pandas, please use the following convention


```python
import pandas as pd
```

# Pandas

Pandas is the canonical library that is used to work with tabular data in python. Pandas is powerful and has many convenient functions for working with dataframes, and is a staple of data science in python. Because it is written to be flexible and handle a variety of use cases, there are many ways to accomplish the same task in Pandas. While this may seem like a good thing, it can be difficult when working in a team environment. In addition, there are ways of getting from point A to point B that are not always ideal. The purpose of this document is to provide you with a set of guidelines to follow when using the Pandas library.

Although some of these choices are to protect you from unintended behavior (for example incorrect subsetting), others are to make sure that any code you submit will adhere to a standard style that the Duke Institute for Health Innovation has chosen to adhere to. A core tenant of almost all of these guidelines is simply to err on the side of being *more* explicit about what you are trying to do rather than being concise.

## Accessing Columns

When accessing columns in Pandas, use the bracket operator `[]` rather than using dot notation `.`. This is especially important when column names cannot be coerced into the dot notation. Let's look at an example: 


```python
badly_named_df = pd.DataFrame({'column 1': [1,2,3],
                               'column_2': [2,3,4]})
```


```python
badly_named_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>column 1</th>
      <th>column_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



Here, you can call


```python
badly_named_df.column_2
```




    0    2
    1    3
    2    4
    Name: column_2, dtype: int64



but you can't use the same notation to call column 1


```python
badly_named_df.column 1
```


      File "<ipython-input-86-cc55a3ae23c4>", line 1
        badly_named_df.column 1
                              ^
    SyntaxError: invalid syntax




```python
[i for i in dir(badly_named_df) if 'column' in i] # not even an option
```




    ['_combine_match_columns',
     '_reindex_columns',
     '_sanitize_column',
     'column_2',
     'columns']



However, using the bracket notation, we never face this issue 


```python
badly_named_df['column 1']
```




    0    1
    1    2
    2    3
    Name: column 1, dtype: int64




```python
badly_named_df['column_2']
```




    0    2
    1    3
    2    4
    Name: column_2, dtype: int64



In general, we'd like to avoid having column names with spaces, but sometimes this is inevitable due to the structure of data that is being read in. Therefore, using bracket notation always will prevent any inconsistencies in your code.

## Performing `inplace` operations

There are many functions available in Pandas that allow you to perform inplace operations, meaning that the object that you are performing a function on will actually be changed even without explicit assignment. 
> **Avoid inplace = True.** 

A common example is removing rows that contains NA values:


```python
missing_df = pd.DataFrame({'column_1': [1,2,3,4,5],
                           'column_2': [2,3,4,5, None]})
missing_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>column_1</th>
      <th>column_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Correct: 
missing_df = missing_df.dropna(axis = 0)
```


```python
missing_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>column_1</th>
      <th>column_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
missing_df = pd.DataFrame({'column_1': [1,2,3,4,5],
                           'column_2': [2,3,4,5, None]})

# incorrect -- AVOID:
missing_df.dropna(axis = 0, inplace = True)
```


```python
missing_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>column_1</th>
      <th>column_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



## Subsetting

## 1. `.loc`

One of the most common operations in Pandas is subsetting a `DataFrame` object. However, there are many different ways that this operation is permitted. 
> **In general, use the `.loc` method to subset DataFrames**, especially when you plan to make modifications

There are many errors that can be prevented if you follow this general rule. Here is an example:

#### Create a DataFrame


```python
example_df = pd.DataFrame({'patient_id': [123456, 234567, 345678, 456789],
                           'age': [20, 30, 40, 50],
                           'gender': ['M', 'F', 'M', 'F']})

example_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>123456</td>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>234567</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



Now imagine we would like to subset this for all patients who are older than 35 and we want to return only the ID and the gender of the patient. The correct way to handle this is:


```python
# Correct
correct_df = example_df.loc[example_df['age'] > 35, ['patient_id', 'gender']]
correct_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Also correct:
correct_df = example_df.loc[example_df['age'] > 35, :]
correct_df.loc[:, ['patient_id', 'gender']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



### A note on `:`

The `.loc` operator lets you specify which rows you'd like to select followed by which columns. Leaving the columns blank would also have worked in the above example. i.e.


```python
example_df.loc[example_df['age'] > 35]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



However, because we want to err on the side of being explicit, please use the `:` to indicate that all columns should be returned 

The reverse direction will not even work if you do not specify which columns you would like. For example,


```python
# Not valid
example_df.loc[, 'age']
```


      File "<ipython-input-73-38e58c764b4b>", line 2
        example_df.loc[, 'age']
                       ^
    SyntaxError: invalid syntax




```python
# Valid
example_df.loc[:,['age']]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>40</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50</td>
    </tr>
  </tbody>
</table>
</div>



please make sure you return the correct type of data structure that you'd like. Note that returning a single column without the extra brackets `[]` will result in a `pd.Series` being returned, while including the brackets will return a `pd.DataFrame`


```python
type(example_df.loc[:, ['age']])
```




    pandas.core.frame.DataFrame




```python
type(example_df.loc[:, 'age'])
```




    pandas.core.series.Series



### What if I don't use `.loc`?


```python
# Incorrect
incorrect_df = example_df[example_df['age'] > 35]
incorrect_df[['patient_id', 'gender']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



Now you might say, Michael, aren't you being annoying? Those do the same thing! Maybe, but here is an example of where the last case might not be ideal. Let's say you wanted to make sure that all patients older than 35 were actually Male (there may have been a mistake in extracting the data).


```python
example_df.loc[example_df['age'] > 35, 'gender'] = 'M'
```


```python
example_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>123456</td>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>234567</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>M</td>
    </tr>
  </tbody>
</table>
</div>



No problem! What about the last method?


```python
# Reset the dataframe
example_df = pd.DataFrame({'patient_id': [123456, 234567, 345678, 456789],
                           'age': [20, 30, 40, 50],
                           'gender': ['M', 'F', 'M', 'F']})

# no .loc
example_df[example_df['age'] > 35]['gender'] = 'M'
```

    /opt/conda/lib/python3.7/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      



```python
example_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>123456</td>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>234567</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



Hm, seems like this didn't actually change what we wanted! To fully understand why this behavior occurs, feel free to read the following article:

[SettingWithCopy Warning](https://www.dataquest.io/blog/settingwithcopywarning/)

In either case, use the `.loc` operator and in many cases you will avoid any of the above issues. In our production code, we will often set the following option: `pd.set_option('mode.chained_assignment', 'raise')` which will make it so that scripts will error out if they encounter this warning.


```python
with pd.option_context('mode.chained_assignment', 'raise'):
    example_df[example_df['age'] > 35]['gender'] = 'M'
```


    ---------------------------------------------------------------------------

    SettingWithCopyError                      Traceback (most recent call last)

    <ipython-input-61-36a25caa2893> in <module>
          1 with pd.option_context('mode.chained_assignment', 'raise'):
    ----> 2     example_df[example_df['age'] > 35]['gender'] = 'M'
    

    /opt/conda/lib/python3.7/site-packages/pandas/core/frame.py in __setitem__(self, key, value)
       3368         else:
       3369             # set column
    -> 3370             self._set_item(key, value)
       3371 
       3372     def _setitem_slice(self, key, value):


    /opt/conda/lib/python3.7/site-packages/pandas/core/frame.py in _set_item(self, key, value)
       3450         # value exception to occur first
       3451         if len(self):
    -> 3452             self._check_setitem_copy()
       3453 
       3454     def insert(self, loc, column, value, allow_duplicates=False):


    /opt/conda/lib/python3.7/site-packages/pandas/core/generic.py in _check_setitem_copy(self, stacklevel, t, force)
       3282 
       3283             if value == 'raise':
    -> 3284                 raise com.SettingWithCopyError(t)
       3285             elif value == 'warn':
       3286                 warnings.warn(t, com.SettingWithCopyWarning,


    SettingWithCopyError: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy


## 2. Multiple conditions 

When subsetting on multiple conditions, it can sometimes be hard to parse exactly what is going on due to the large volume of text that it often takes to perform these multiple conditions. 
 > **Use multiple lines of code to show how you are subsetting a DataFrame multiple times**, especially with more than 2 conditions



```python
example_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>123456</td>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>234567</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>345678</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



Let's say we want to subset patients who are female and over the age of 30. We can accomplish this in two separate ways: 


```python
example_df.loc[(example_df['age'] > 30) & (example_df['gender'] == 'F'), :]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>456789</td>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



This doesn't look too bad, but as you might imagine it can get tricky as we add more conditions. For example, let's say we want 
``` 
 1. age > 30
 2. gender == "F"
 3. patient_id > 200000
 4. return age and gender columns
```


```python
example_df.loc[(example_df['age'] > 30) & (example_df['gender'] == 'F') & (example_df['patient_id'] > 200000), ['age', 'gender']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



We can make this easier to read by breaking it into multiple lines of code


```python
example_df.loc[(example_df['age'] > 30) 
               & (example_df['gender'] == 'F') 
               & (example_df['patient_id'] > 200000)
               , ['age', 'gender']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



In general, we will follow [PEP-8](https://www.python.org/dev/peps/pep-0008/#id20) guidelines for whether the operators come before or after the statements (they should come before)

It is also acceptable to simply break up the subsetting operations into multiple steps. i.e. 


```python
subset_df = example_df.loc[example_df['age'] > 30, :]
subset_df = subset_df.loc[subset_df['gender'] == 'F', :]
subset_df = subset_df.loc[subset_df['patient_id'] > 200000, :]
subset_df = subset_df.loc[:, ['age', 'gender']]

subset_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>50</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>



## Joins

There are many ways to join `DataFrames` in Pandas. Of note are `pd.concat`, `pd.append`, `pd.merge`, and `pd.join`. 
> **Unless you have a very good reason, use `pd.merge()` to join DataFrames together**

Joins are one of the most ubiquitous operations when working with tabular data. Because of some of the complexities of Pandas data structures, there are many ways of joining dataframes together. By far the most common operation is performing a SQL-like join. To do this operation, use the `pd.merge()` function. 


```python
import random
```


```python
demographics_df = pd.DataFrame({'patient_id': [1234, 2345, 3456, 4567, 5678, 6789], 
                                'age': [20, 30, 40, 50, 60, 70],
                                'sex': ['M', 'F', 'M', 'F', 'M', 'F']})

vitals_df = pd.DataFrame({'patient_id': [1234, 2345, 3456, 4567, 5678, 6789] * 2,
                          'HR': [random.randint(60, 150) for i in range(12)],
                          'SBP': [random.randint(110, 160) for i in range(12)]})
```


```python
demographics_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>




```python
vitals_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1234</td>
      <td>82</td>
      <td>148</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2345</td>
      <td>66</td>
      <td>157</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3456</td>
      <td>146</td>
      <td>144</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4567</td>
      <td>80</td>
      <td>138</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5678</td>
      <td>119</td>
      <td>147</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6789</td>
      <td>97</td>
      <td>152</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1234</td>
      <td>146</td>
      <td>131</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2345</td>
      <td>60</td>
      <td>140</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3456</td>
      <td>61</td>
      <td>127</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4567</td>
      <td>90</td>
      <td>113</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5678</td>
      <td>91</td>
      <td>125</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6789</td>
      <td>86</td>
      <td>157</td>
    </tr>
  </tbody>
</table>
</div>



Let's say we wanted to perform a left join on the `demographics_df` table and get all of the columns from the `vitals_df` table. The correct way to perform the operation is:


```python
pd.merge(demographics_df, vitals_df, how = 'left', left_on = ['patient_id'], right_on = ['patient_id'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>sex</th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>82</td>
      <td>148</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>146</td>
      <td>131</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>66</td>
      <td>157</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>60</td>
      <td>140</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>146</td>
      <td>144</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>61</td>
      <td>127</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>80</td>
      <td>138</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>90</td>
      <td>113</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>119</td>
      <td>147</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>91</td>
      <td>125</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>97</td>
      <td>152</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>86</td>
      <td>157</td>
    </tr>
  </tbody>
</table>
</div>



> The arguments `how = `, `left_on = ` and `right_on = ` must ALWAYS be present in your call. Pandas has a certain way of trying to infer these, and `how = 'inner'` is the default choice, but in order to be explicit, please make sure to include all of them whenever joining DataFrames. 


```python
# Do not use the following !!
demographics_df.merge(vitals_df, how = 'left', left_on = ['patient_id'], right_on = ['patient_id'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>sex</th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>82</td>
      <td>148</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>146</td>
      <td>131</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>66</td>
      <td>157</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>60</td>
      <td>140</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>146</td>
      <td>144</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>61</td>
      <td>127</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>80</td>
      <td>138</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>90</td>
      <td>113</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>119</td>
      <td>147</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>91</td>
      <td>125</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>97</td>
      <td>152</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>86</td>
      <td>157</td>
    </tr>
  </tbody>
</table>
</div>



> **Even though the functionality is equivalent, we prefer to use the `pd.merge` notation rather than calling a method on an existing DataFrame. This goes for any other DataFrame method that can also be expressed as a pandas function call.**

examples include `pd.melt vs DataFrame.melt` and `pd.pivot_table vs DataFrame.pivot`

## What if I want to join on the Index?

Although it is often convenient to join on index, if memory is not an issue, simply 
> **reset the index and join in using the method above**.

This keeps things consistent on our end. However, if there are other limitations, it is ok to use the `left_index` and `right_index` arguments in `pd.merge`. Do **NOT** mix the two if it can be avoided. For example, do not make calls such as:

`pd.merge(left, right, how = 'left', left_on = ['column'], right_index = True)`

This is more headache than it is worth


```python
demographics_df = demographics_df.set_index(['patient_id'])
demographics_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1234</th>
      <td>20</td>
      <td>M</td>
    </tr>
    <tr>
      <th>2345</th>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3456</th>
      <td>40</td>
      <td>M</td>
    </tr>
    <tr>
      <th>4567</th>
      <td>50</td>
      <td>F</td>
    </tr>
    <tr>
      <th>5678</th>
      <td>60</td>
      <td>M</td>
    </tr>
    <tr>
      <th>6789</th>
      <td>70</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>




```python
vitals_df = vitals_df.set_index(['patient_id'])
vitals_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1234</th>
      <td>65</td>
      <td>156</td>
    </tr>
    <tr>
      <th>2345</th>
      <td>92</td>
      <td>145</td>
    </tr>
    <tr>
      <th>3456</th>
      <td>146</td>
      <td>155</td>
    </tr>
    <tr>
      <th>4567</th>
      <td>138</td>
      <td>160</td>
    </tr>
    <tr>
      <th>5678</th>
      <td>77</td>
      <td>138</td>
    </tr>
    <tr>
      <th>6789</th>
      <td>61</td>
      <td>115</td>
    </tr>
    <tr>
      <th>1234</th>
      <td>61</td>
      <td>153</td>
    </tr>
    <tr>
      <th>2345</th>
      <td>75</td>
      <td>159</td>
    </tr>
    <tr>
      <th>3456</th>
      <td>132</td>
      <td>130</td>
    </tr>
    <tr>
      <th>4567</th>
      <td>111</td>
      <td>155</td>
    </tr>
    <tr>
      <th>5678</th>
      <td>105</td>
      <td>144</td>
    </tr>
    <tr>
      <th>6789</th>
      <td>121</td>
      <td>138</td>
    </tr>
  </tbody>
</table>
</div>




```python
using_index = pd.merge(demographics_df, vitals_df, how = 'left', left_index = True, right_index = True)
```


```python
using_index
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
    <tr>
      <th>patient_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1234</th>
      <td>20</td>
      <td>M</td>
      <td>65</td>
      <td>156</td>
    </tr>
    <tr>
      <th>1234</th>
      <td>20</td>
      <td>M</td>
      <td>61</td>
      <td>153</td>
    </tr>
    <tr>
      <th>2345</th>
      <td>30</td>
      <td>F</td>
      <td>92</td>
      <td>145</td>
    </tr>
    <tr>
      <th>2345</th>
      <td>30</td>
      <td>F</td>
      <td>75</td>
      <td>159</td>
    </tr>
    <tr>
      <th>3456</th>
      <td>40</td>
      <td>M</td>
      <td>146</td>
      <td>155</td>
    </tr>
    <tr>
      <th>3456</th>
      <td>40</td>
      <td>M</td>
      <td>132</td>
      <td>130</td>
    </tr>
    <tr>
      <th>4567</th>
      <td>50</td>
      <td>F</td>
      <td>138</td>
      <td>160</td>
    </tr>
    <tr>
      <th>4567</th>
      <td>50</td>
      <td>F</td>
      <td>111</td>
      <td>155</td>
    </tr>
    <tr>
      <th>5678</th>
      <td>60</td>
      <td>M</td>
      <td>77</td>
      <td>138</td>
    </tr>
    <tr>
      <th>5678</th>
      <td>60</td>
      <td>M</td>
      <td>105</td>
      <td>144</td>
    </tr>
    <tr>
      <th>6789</th>
      <td>70</td>
      <td>F</td>
      <td>61</td>
      <td>115</td>
    </tr>
    <tr>
      <th>6789</th>
      <td>70</td>
      <td>F</td>
      <td>121</td>
      <td>138</td>
    </tr>
  </tbody>
</table>
</div>




```python
# or just reset the index
demographics_df = demographics_df.reset_index()
vitals_df = vitals_df.reset_index()
```


```python
pd.merge(demographics_df, vitals_df, how = 'left', left_on = ['patient_id'], right_on = ['patient_id'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>patient_id</th>
      <th>age</th>
      <th>sex</th>
      <th>HR</th>
      <th>SBP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>65</td>
      <td>156</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1234</td>
      <td>20</td>
      <td>M</td>
      <td>61</td>
      <td>153</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>92</td>
      <td>145</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2345</td>
      <td>30</td>
      <td>F</td>
      <td>75</td>
      <td>159</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>146</td>
      <td>155</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3456</td>
      <td>40</td>
      <td>M</td>
      <td>132</td>
      <td>130</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>138</td>
      <td>160</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4567</td>
      <td>50</td>
      <td>F</td>
      <td>111</td>
      <td>155</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>77</td>
      <td>138</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5678</td>
      <td>60</td>
      <td>M</td>
      <td>105</td>
      <td>144</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>61</td>
      <td>115</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6789</td>
      <td>70</td>
      <td>F</td>
      <td>121</td>
      <td>138</td>
    </tr>
  </tbody>
</table>
</div>



# Resources 

- [Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/)
- [Pandas Jupyter Notebook Tutorial](https://github.com/dsahduke/Numpy-Pandas)
