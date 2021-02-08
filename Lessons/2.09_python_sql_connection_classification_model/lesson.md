# Lesson 2.9: Python/SQL Connection & Classification Models

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to learn how to connect Python IDE with SQL database, run queries in Python environment to extract information as dataframes, and work on it using Python libraries. The students will get a quick introduction to another supervised machine learning models class, the classification models, and what kind of problem do we solve with them and how we can implement them using Python.

- [**Slides:**](https://docs.google.com/presentation/d/1J3QaeBBb0gUZPi8scQ1rGRH-4bAMCu6L0aRLfahSjUk/edit?usp=sharing)

---

### Setup

To start this lesson, students should have:

- Completed lesson 2.8
- All previous Setup

---

### Learning Objectives

After this lesson, students will be able to:

- Connect SQL with Python IDE
- Run queries in Python environment and pull data in dataframes
- Interpret Supervised Machine Learning Classification Problems
- Apply the Logistic Regression model

---

### Lesson 1 key concepts



Establish a connection between SQL and Python with `SQLAlchemy` and `PyMySql`

- Using engine object with pandas to run queries
- Using an executable class to run queries

<details>
<summary> Click for Code Sample </summary>

```shell
# installing SQLAlchemy and PyMySql (in case it is not installed)

$ pip install sqlalchemy
$ pip install PyMySQL
```

1. **Using Engine Object with pandas**

```python
import pymysql
from sqlalchemy import create_engine
import pandas as pd
import getpass  # To get the password without showing the input
password = getpass.getpass()
```

> Note that when you use _SQLAlchemy_ and establish the connection, you do not even need to be logged in Sequel Pro or MySQL Workbench.

- This is the general syntax **`'dialect+driver://username:password@host:port/database'`** to create the connection string:

```python
connection_string = 'mysql+pymysql://root:' + password + '@localhost/bank'
engine = create_engine(connection_string)
data = pd.read_sql_query('SELECT * FROM loan', engine)
data.head()
```

2. **Using engine object with executable class**

```python
result = engine.execute('SELECT * FROM loan')
for row in result:
    print(row)

rows = [row for row in result]
pd.DataFrame(rows)
```

```python
# Running other queries in SQL

engine.execute("DROP DATABASE IF EXISTS BootCamps")
engine.execute("CREATE DATABASE IF NOT EXISTS BootCamps")
engine.execute("USE BootCamps")
```

```python
query = 'select order_id as "OrderID", account_id as "AccountID", bank_to as "DestinationBank", amount  as "Amount" \
from bank.order \
where k_symbol = "SIPO" \
limit 100'
data = pd.read_sql_query(query, engine)
data.head()
```

</details>

---



---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.09_activities/blob/master/2.09_activity_1.md).

</details>

<details>
  <summary> Click for Solution: Activity 1 solutions </summary>

- Link to [activity 1 solution]().

</details>

---



---

### Lesson 2 key concepts




- More examples of running queries in Python IDE
- Revisit Supervised Machine Learning Models
- Classification Model - Logistic Regression
- Some assumptions of the logistic regression model

<details>
  <summary>Assumptions of logistic regression model </summary>
  
- No free lunch theorem - No model comes without certain assumptions. If the assumptions of the model are not met, it will have adverse effects on the mode. Understanding those assumptions helps us with data cleaning and pre-processing methods.

- Some assumptions that are not required in the logistic regression model (but are important for linear regression):

      - It does not require a linear relationship between the dependent and independent variables.
      - The error terms (residuals) do not need to be normally distributed.
      - It makes no assumption about the distribution of independent variables.
      - Homoscedasticity / Homogeneity of variance is not required (Optional - If the instructor wants to talk more about it).

<details>
  <summary> Homoscedasticity / Homogeneity of variance </summary>

For linear regression, Homoscedasticity is talked about in terms of residuals (errors). If the variance of the residuals is the same around the regression line, then the points are homoscedastic and the assumption is followed. If the variance of the residuals is varying, then the points are heteroscedastic. The residuals can also be checked with the independent variables and should display the same behavior. The figure below shows the concept.

![Homoscedasticity](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/2.9-homoscedasticity.png)

Implications if the assumption is not met are:

1. With linear regression, the _OLS_ method is used to minimize the residuals/errors. By default, it gives equal weight to all the observations. But if heteroscedasticity is present, the cases with larger disturbance/variance have more pull on the regression line.
2. Heteroscedasticity also means that the residuals are biased which is a problem for other statistical tests (ANOVA, tests for significance) and calculating confidence intervals.

Dealing with heteroscedasticity

1. Instead of _OLS_, weighted least squares can be used.
2. Transformation of the dependent variable using one of the variances stabilizing transformations can be done (square root transformation, logarithmic transformation)

</details>

- Some of the other assumptions of the logistic regression model are mentioned here:

      - Dependent variable structure - For binary logistic regression, the dependent variable should be binary.
      - Observation independence - The observations should be independent of each other ie they should not come from repeated measurements.
      - Absence of multicollinearity - There should be little or no multi-collinearity between the independent variables.
      - The linearity of independent variables and log odds - The independent variables should be linearly dependent on **log odds**.
      - Large sample size - You need a minimum of 10 cases with the least frequent outcome for each independent variable in your model.

</details>

<details>
<summary> Click for Code Sample </summary>

```python
query = '''select * from trans t
left join loan l
on t.account_id = l.account_id
where l.status in ('A', 'B');'''

data = pd.read_sql_query(query, engine)
data.head()
```

</details>

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.09_activities/blob/master/2.09_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution]().

</details>

---


---

### Lesson 3 key concepts



- Introduce the classification problem at hand
- Import data into Python from SQL
- Perform data cleaning and data processing

> Now we want to develop a classification model to predict the status of the customer based on the information available. We will use the data from the previous query to develop the model.

<details>
  <summary> Click for Code Sample </summary>

```python
# Extracting the data (the same as the previous query)

query = '''select t.type, t.operation, t.amount as t_amount, t.balance, t.k_symbol, l.amount as l_amount, l.duration, l.payments, l.status
from trans t
left join loan l
on t.account_id = l.account_id
where l.status in ('A', 'B');'''

data = pd.read_sql_query(query, engine)
data.head()
```


```python
# Extracting the data (the previous query modified)

query = '''select t.type, t.operation, t.amount as t_amount, t.balance, t.k_symbol, l.amount as l_amount, l.duration, l.payments, l.status
from trans t
left join loan l
on t.account_id = l.account_id
where l.status in ('A', 'B');'''
data = pd.read_sql_query(query, engine)
data.head()
```

</details>

<details>
  <summary>Click for Code Sample: Data Cleaning -  Categorical columns</summary>

```python
data.shape
data.dtypes

data['duration'] = data['duration'].astype('object') # This will be treated as categorical
data.describe()
data.isna().sum()

## checking all the categorical columns
data['type'].value_counts()

# since we have a lot values for operation which are of type vyber,
# we are not removing that data from type column
data['operation'].value_counts()
def cleanOperation(x):
    x = x.lower()
    if 'vyber' in x:
        return "vyber"
    elif 'prevod' in x:
        return "prevod"
    elif 'vklad' in x:
        return 'vklad'
    else:
        return 'unknown'

data['operation'] = list(map(cleanOperation, data['operation']))
```

```python
data['k_symbol'].value_counts()
data['k_symbol'].value_counts().index
def cleankSymbol(x):
    if x in ['', ' ']:
        return 'unknown'
    else:
        return x

data['k_symbol'] = list(map(cleankSymbol, data['k_symbol']))
data = data[~data['k_symbol'].isin(['POJISTINE', 'SANKC. UROK', 'UVER'])]
```

```python
data['duration'].value_counts().index
def cleanDuration(x):
    if x in [48, 60]:
        return 'other'
    else:
        return str(x)
data['duration'] = list(map(cleanDuration, data['duration']))
data.head()
```

</details>

<details>
  <summary> Click for Code Sample: Data Cleaning -  Numerical columns</summary>

```python
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

```python
# Checking for multicollinearity

corr_matrix=data.corr(method='pearson')  # default
fig, ax = plt.subplots(figsize=(10, 8))
ax = sns.heatmap(corr_matrix, annot=True)
plt.show()
```


```python
sns.distplot(data['t_amount'])
plt.show()

sns.distplot(data['l_amount'])
plt.show()

sns.distplot(data['balance'])
plt.show()

sns.distplot(data['payments'])
plt.show()
```

```python
from sklearn.preprocessing import Normalizer
# from sklearn.preprocessing import StandardScaler

X = data.select_dtypes(include = np.number)

# Normalizing data
transformer = Normalizer().fit(X)
x_normalized = transformer.transform(X)
x = pd.DataFrame(x_normalized)
```

```python
cat = data.select_dtypes(include = np.object)
cat = cat.drop(['status'], axis=1)
categorical = pd.get_dummies(cat, columns=['type', 'operation', 'k_symbol', 'duration'])
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_2.09_activities/blob/master/2.09_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution]().

</details>

---
---

### Lesson 4 key concepts


- Train test split
- Apply the logistic regression model
- Check the results - confusion matrix

<details>
<summary> Click for Code Sample </summary>

```python
y = data['status']
X = np.concatenate((x, categorical), axis=1)
```

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=100)
```

```python
from sklearn.linear_model import LogisticRegression
classification = LogisticRegression(random_state=0, solver='lbfgs',
                  multi_class='ovr').fit(X_train, y_train)
```

```python
classification.score(X_test, y_test)
predictions = classification.predict(X_test)
classification.score(X_test, y_test)
```

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_test, predictions)
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_2.09_activities/blob/master/2.09_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution]().

</details>

---

---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-9](https://github.com/ironhack-labs/lab-sql-9)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---
---

- [SQLAlchemy - cheat sheet](https://www.pythonsheets.com/notes/python-sqlalchemy.html)
- [PyMySQL - cheat sheet](http://www.nonbleedingedge.com/cheatsheets/pymysql.html)
