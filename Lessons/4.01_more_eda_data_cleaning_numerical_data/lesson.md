# Lesson 4.1: Linear Regression - Case Study Intro - & Exploratory Data Analysis

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to revisit the _linear regression_ case study we looked at in the first unit but with a larger dataset. We will dive deeper into the _exploratory data analysis_ process for numerical variables, to identify the data cleaning and some data processing operations we would perform.

---

> **Case Study: [Healthcare for all](./Unit4_case_study.docx)**

<!-- 🚨🚨🚨 @sandrabosk: turn into .md -->

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Translate the business problem into Data Analysis tasks (using linear regression)
- Conduct Exploratory Data Analysis (_EDA_) to understand the nature of available data
- Identify data cleaning and data processing operations on **numerical** variables

---

### Lesson 1 key concepts

> :clock10: 20 min

- Introduce the business problem again (this time the data has more features and rows)
- Checking numerical and categorical variables and correcting data types
- Conduct exploratory data analysis ( `EDA` ) - 1

      - Analyze numerical variables
      - Identify the changes to be made

<details>
  <summary> Click for Code Sample: Importing data  </summary>

```python
#importing libraries
import pandas as pd
import numpy as np
import datetime
import warnings

warnings.filterwarnings('ignore')
warnings.filterwarnings("ignore", message="numpy.ufunc size changed")

import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

pd.set_option('display.max_columns', None)
```

```python
#importing data
data = pd.read_csv('./unit4.csv') # this file is in files_for_lesson_and_activities folder
data.head()
data.shape
```

```python
# Checking for null values
nulls = pd.DataFrame(data.isna().sum()/len(data))
nulls= nulls.reset_index()
nulls.columns = ['column_name', 'Percentage Null Values']
nulls.sort_values(by='Percentage Null Values', ascending = False)
```

```python
# Check the numerical variables
numericals = data.select_dtypes(np.number)
numericals.head()
```

- The `INCOME` might be an important factor in predicting the gift value, so even though it has a lot of null values, we will not drop the column.
- In this exercise, we will try a more precise method to replace the null values, instead of simply replacing them by a constant value, mean or median.
- We will use a similar method for the column `TIMELAG` .
- Explain to the students how you are interpreting the different plots to make decisions in the next steps.

```python
# working with INCOME column EDA + Data cleaning
data['INCOME'].hist()
```

- Looking at the histogram, we can see we need to replace the missing values first.
- In the previous lessons, we talked about replacing null values with mean and median and some other constant value. In the later lessons, we will look at methods different than filling with constants (mean and median).

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary>Click for Activity 1</summary>

- How many ways exist to cope with the null values? When should you use each one?
- How we could find out gender value when this field is null?
- Homeownership has two values [ `H` = Homeowner, `U` = Unknown]. Fill the null values accordingly.

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- **Drop**: Data can't be obtained, not a lot of entries (rows) or a lot of `NaN` s in the columns (columns)

```python
gender_is_null = data.GENDER.isnull()
data.drop(gender_is_null.index)
```

- **Replace**: We have some other information that tells us we can do this, even if it is not the missing information. For example, if the data follow an approximately normal distribution, we might want to substitute the missing values with the mean. You always need to have something that "tells you" that you can replace the data.

```python
data.HOMEOWNR.fillna('U')
```

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

Cleaning numerical variables

- Imputing null values
- Fixed values technique vs. interpolation technique

<details>
  <summary> Click for Code Sample: Interpolation Method </summary>

```python
# How interpolation works
data['INCOME'][0:40].plot()  # To check how interpolation would fill the missing values
new_income_data = data['INCOME'][0:40].interpolate(method='linear')

# new_income_data = data['INCOME'][0:40].interpolate(method='akima')  # Other methods that can be used
# new_income_data = data['INCOME'][0:40].interpolate(method='polynomial', order=3)  # Other methods that can be used
new_income_data.plot()
plt.show()
```

```python
# Test what does the distribution look like after we have used interpolation method
points = data['INCOME'].interpolate(method='akima')
sns.distplot(points[1:])   # We are using the index __1:__ as first value was NaN
```

- It is important to compare the results with other methods and then choose the best one.

```python
# Testing interpolation method with mean and median methods
points2 = data['INCOME'].fillna(np.mean(data['INCOME']))
sns.distplot(points2)

# Note that unlike "np.mean()" , "np.median()" doesn't work if there are any null values in the column
median = np.median(data['INCOME'].fillna(0))

points3 = data['INCOME'].fillna(median)
sns.distplot(points3)
```

```python
# Finally choosing mean method
data['INCOME'] = data['INCOME'].fillna(np.mean(data['INCOME']))
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

Interpolation isn't a bad method to replace `NaN` s but you might find some other solutions that are more efficient and effective. Can you think of at least one? Implement your proposal.
**Hint**: Analytically, when you have `NaN` s, you work with them as if they were a test set.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- It is a regression task that can be solved with linear regression, for example.

```python
from sklearn.linear_model import LinearRegression

X = data[~data.INCOME.isnull() ][['HV1', 'IC1']]
y = data[~data.INCOME.isnull()]['INCOME']

X_nulls = data[data.INCOME.isnull() ][['HV1', 'IC1']]

model = LinearRegression().fit(X,y)
income_pred = model.predict(X_nulls)

data[data.INCOME.isnull() ]['INCOME'] = np.round(income_pred)
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Cleaning numerical variables

      - Using data transformations

<details>
  <summary> Click for Code Sample: Working with INCOME column EDA + Data cleaning </summary>

```python
# Working with INCOME column EDA + Data cleaning
data['TIMELAG'].hist(bins=20)
plt.show()

sns.boxplot(y=data['TIMELAG'])
plt.show()
```

- Data in the Timelag column is highly skewed (positive skewness).
- Removing outliers straight away might not be the best idea as it would remove a lot of data points from the data.
- We will try some transformations.

```python
def log_transfom_clean_(x):
    if np.isfinite(x) and x!=0:
        return np.log(x)
    else:
        return np.NAN # We are returning NaNs so that we can replace them later

def sqrt_transfom_clean_(x):
    if np.isfinite(x) and x>=0:
        return np.sqrt(x)
    else:
        return np.NAN # We are returning NaNs so that we can replace them later
```

```python
# Using the functions to check the distribution of transformed data
pd.Series(map(log_transfom_clean_, data['TIMELAG'])).hist()
plt.show()

pd.Series(map(sqrt_transfom_clean_, data['TIMELAG'])).hist()
plt.show()
```

```python
# Use log transformation to replace the values of the column now
data['TIMELAG'] = list(map(log_transfom_clean_, data['TIMELAG']))
data['TIMELAG'] = data['TIMELAG'].fillna(np.mean(data['TIMELAG']))
sns.distplot(data['TIMELAG'])
plt.show()
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

Logarithmic transformation is one of the most common transformations when it comes to visualization. Can you guess why?

</details>

<details>
  <summary> Click for Solution: Activity 3 solutions </summary>

A logarithmic scale is common to visualize exponential data as they are the inverse function of each other, so the result would be a linear visualization. This is needed because we visualize exponential functions properly otherwise. As an example, you can see some corona virus visualizations, like [this one](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.1-COVID-Logarithmicvslinear.png). Check the log transform with the `IC` n columns.

```python
sns.distplot(data['IC1'])
sns.distplot(np.log(data['IC1']))

sns.distplot(data['IC2'])
sns.distplot(np.log(data['IC2']))

sns.distplot(data['IC3'])
sns.distplot(np.log(data['IC3']))

sns.distplot(data['IC4'])
sns.distplot(np.log(data['IC4']))
```

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

Cleaning numerical variables

- Filtering/subsetting data
- Using filter function
- Removing outliers

<details>
  <summary> Click for Code Sample  </summary>

- Even after using the transformation, there is still some skewness in the column `TIMELAG` . We will remove the outliers only from the right side of the distribution plot.

```python
# Checking how many values will be removed if the outliers are removed
iqr = np.percentile(data['TIMELAG'],75) - np.percentile(data['TIMELAG'],25)
upper_limit = np.percentile(data['TIMELAG'],75) + 1.5*iqr
print(upper_limit)

new_df = data[data['TIMELAG'] > upper_limit]
len(new_df)  # THis checks the number of points that will be removed
```

- Explain how the `filter` function works (syntax is very similar to the `map` functions).

:exclamations: Note to instructor: Feel free to use some of your examples if there is enough time or students are not able to understand it so they need a bit reinforcement.

```python
# Using filters
points = list(filter(lambda x: x < upper_limit, data['TIMELAG']))
len(points)

## some other simple applications of filter

lst = [0,1,2,3,4,5,6,7,8,9,10]
list(filter(lambda x: x % 2 == 0, lst))
```

```python
# Removing outliers
data = data[data['TIMELAG'] < upper_limit]
sns.distplot(data['TIMELAG'])
plt.show()
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- What is the difference between **map**, **filter**, and **reduce**?
- Use **map**, **filter** and **reduce** to get the sum of the square root of the odd numbers between 0 and 100

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- They are completely different:

      - **Map** applies a function to all the items in a given list.
      - **Filter** creates a list of elements from another for which a function returns true.
      - **Reduce** performs some computation on a list and returns the of that computation applied all over the list.

```python
from functools import reduce

lst = list(range(100))
reduce(lambda a,b: a+b, map( np.sqrt, filter(lambda x: x % 2 == 1, lst)), 0)
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-cleaning-numerical-data](https://github.com/ironhack-labs/lab-cleaning-numerical-data)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/2de338838ac15098565d6143f30e9732).

---

### Additional Resources
