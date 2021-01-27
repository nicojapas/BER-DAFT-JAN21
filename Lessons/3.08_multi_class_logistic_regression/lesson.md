# Lesson 3.8: Multi-class classification & Classification models

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce multi-class classification problems and revisit the data analysis workflow with classification modeling using logistic regression.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Conceptualize the data analysis workflow
- Explain logistic regression
- Differentiate binary classification and multi-class classification problems
- Use logistic regression for multi class classification
- Check the accuracy of the model

---

### Lesson 1 key concepts

> :clock10: 20 min

- Introduce the multi-class classification problem
- Establish connection between SQL and Python
- Write query to pull the data from SQL into Python as a dataframe

<details>
  <summary> Click for Code Sample: Python and database connection </summary>

```python
import pymysql
from sqlalchemy import create_engine
import pandas as pd
import getpass  # To get the password without showing the input
password = getpass.getpass()
```

</details>

<details>
  <summary> Click for Code Sample: Import data into a dataframe </summary>

```python
connection_string = 'mysql+pymysql://root:' + password + '@localhost/bank'
engine = create_engine(connection_string)
query = '''select t.type, t.operation, t.amount as t_amount, t.balance, t.k_symbol, l.amount as l_amount, l.duration, l.payments, l.status
from trans t
left join loan l
on t.account_id = l.account_id;'''

data = pd.read_sql_query(query, engine)
data.head()
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.08_activities/blob/master/3.08_activity_1.md).

</details>

<details>
  <summary> Click for Solution: Activity 1 solutions </summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/253270833e1716fca5d7273469ea757d).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

Revisit Data Analysis work flow for modeling - 1

- Data acquisition (already performed)
- Exploratory data analysis
- Data Cleaning/wrangling

<details>
  <summary> Click for Code Sample </summary>

```python
data['status'].value_counts()

data.shape

data.dtypes

data.isna().sum()
data = data[data['duration'].isna() == False]

data.describe()

data['duration'] = data['duration'].astype('object') # This will be treated as categorical
data.describe()
data.isna().sum()
```

</details>

<details>
  <summary> Click for Code Sample:  Cleaning Categorical Columns</summary>

```python
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
def clean_type(x):
    if 'PRI' in x:
        return 'PRIJEM'
    else:
        return x

data['type'] = list(map(clean_type, data['type']))
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.08_activities/blob/master/3.08_activity_2.md).

</details>

<details>
  <summary> Click for Solution: Activity 2 solutions </summary>

- Link to [activity 2 solutions](https://gist.github.com/ironhack-edu/2946a99a19aa1f86c066e7dd1ffec7fc).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

Revisit Data Analysis work flow for modeling - 2

- More EDA/data cleaning
- Data pre processing

<details>
  <summary> Click for Code Sample: EDA / Data Cleaning </summary>

```python
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

```python
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

</details>

<details>
  <summary> Click for Code Sample: Data preprocessing </summary>

```python
import numpy as np
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

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.08_activities/blob/master/3.08_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/9ca2052231cc1802096e2f0c4eb7e9a9).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

Revisit Data Analysis work flow for modeling - 2

- Fitting the model
- Making predictions on the test data
- Check model accuracy

:exclamation: Note to instructor: When we work with multi class classification problem and use Logistic Regression method from `sklearn`, the argument "multi_class" can take different arguments. Discuss the one versus rest method and multinomial mehtod briefly, how they are different.

<details>
  <summary> Click for Code Sample: Train test split </summary>

```python
y = data['status']
X = np.concatenate((x, categorical), axis=1)
```

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=100)
```

</details>

<details>
  <summary> Click for Code Sample: Fitting the model </summary>

- Refer to the documentation
  [https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html]

```python
from sklearn.linear_model import LogisticRegression
classification = LogisticRegression(random_state=0, solver='lbfgs',
                  multi_class='multinomial').fit(X_train, y_train)
```

```python
predictions = classification.predict(X_test)
classification.score(X_test, y_test)
```

```python
print(y_test.value_counts())
# As you would notice here, there is a huge imbalance in the data among the different classes. We will talk more about imbalance and how to resolve it later

pd.Series(predictions).value_counts()
# This shows that the disparity in the numbers are amplified by the model
```

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_test, predictions)
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-predictions-logistic-regression](https://github.com/ironhack-labs/lab-predictions-logistic-regression)

</details>

<details>
  <summary> Click for Solution: Lab solutions </summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/c3e7fba417de11bcf152ba6329acbbb4).

</details>

---
