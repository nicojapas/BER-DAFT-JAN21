# Lesson 4.2: Exploratory Data Analysis & Regular Expressions

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into the exploratory data analysis process for categorical variables, identify the data cleaning and data processing operations we would perform. We will also introduce some more advanced data cleaning operations including regular expressions.

### Setup

- All previous set up
- Use the same Jupyter notebook as used in 4.1

### Learning Objectives

After this lesson, students will be able to:

- Conduct exploratory data analysis to understand the nature of available data
- Identify data cleaning and data processing operations on **categorical** variables
- Bucket data, for example to convert a numerical column into a categorical one
- Explain how regular expressions work and what to use them for

---

### Lesson 1 key concepts

> :clock10: 20 min

Conduct exploratory data analysis (`EDA`) - 1

- Analyze categorical variables
- Identify the changes to be made

<details>
  <summary> Click for Code Sample:  </summary>

Note this is a way to check the categorical variables (`dtypes` as object). If we want to perform any filtering operation, we will take the complete dataset and not just the categorical columns.

```python
categoricals = data.select_dtypes(np.object)
categoricals.head()
```

```python
# Deleting columns with over 80% empty values

data['PVASTATE'].value_counts()
data['RECP3'].value_counts()
data['VETERANS'].value_counts()
data = data.drop(columns=['PVASTATE', 'RECP3', 'VETERANS'], axis=1)
```

```python
data['HOMEOWNR'].value_counts()
```

As you can see, there is a lot of `null` values in the column but it is still not as many that the column might be removed. And if we filter out those values we will lose a lot of data. Another way of replacing those empty values is by replacing them with the maximum represented category but this introduces a bias.
Another advanced methods include using machine learning to predict those values. Here in this case we will delete this column instead of inducing a bias.

```python
data = data.drop(columns=['HOMEOWNR'], axis=1)
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

For the column `DOMAIN`, discuss which option is better to clean the rows where the values are empty.

- Option 1: Filtering the rows with the empty values.
- Option 2: Replacing the empty values with some other category, the most frequently represented value in that column.

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

```python
data = data[data['DOMAIN'] != " "]

# Note after you filter, it is a good practice to reset the index
data = data.reset_index(drop=True)
```

```python
data["DOMAIN"].value_counts()
```

If you pay some attention to the values, you will find that `C4` is missing. Therefore, we could replace the missing values for it.

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

Cleaning categorical variables

- Using the `map` function to work on categories of a column
- Comparing categorical features with `Y` using box plot
- Impute missing values

<details>
  <summary> Click for Code Sample:  </summary>

```python
# Cleaning column GENDER

data['GENDER'].value_counts()
def clean_gender_col(x):
    if x in ['',' ' ,'U', 'C', 'J', 'A']:
        return 'other'
    else:
        return x

data['GENDER'] = list(map(clean_gender_col, data['GENDER']))
```

```python
# Visually analyzing categorical data with Target variable
sns.boxplot(x="GENDER", y="AVGGIFT", data=data)
plt.show()

sns.barplot(x="GENDER", y="AVGGIFT", data=data)
plt.show()
```

- Looking at the box plot, we can see that there is not a lot of variation in Y with different categories, we can delete the column gender

```python
data = data.drop(columns=['GENDER'], axis=1)
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- There is a more efficient way to use `map` over pandas dataframes, and it is called [apply](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html). Use it instead of the `map` for applying the previous function to the same data. Do the same using the equivalent lambda function.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

```python
data['GENDER'] = data['GENDER'].apply(clean_gender_col)

data['GENDER'] = data['GENDER'].apply(lambda x: 'other' if x in ['',' ' ,'U', 'C', 'J', 'A'] else x)
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Dealing with a large number of categories in a column
- Grouping data/bucketing data

<details>
<summary> Click for Code Sample: Dealing with a large number of categories </summary>

This code gives us the names of the states that should be put into category `Other`:

```python
vals = pd.DataFrame(data['STATE'].value_counts())
vals = vals.reset_index()
vals.columns = ['state', 'counts']
group_states_df = vals[vals['counts']<2500]
group_states = list(group_states_df['state'])
group_states
```

```python
def clean_state(x):
    if x in group_states:
        return 'other'
    else:
        return x

data['STATE'] = list(map(clean_state, data['STATE']))
```

</details>

<details>
<summary> Click for Code Sample: Grouping data/bucketing data  </summary>

```python
# Creating buckets/groups of data

ic2_labels = ['Low', 'Moderate', 'High', 'Very High']
data['ic2_'] = pd.cut(data['ic2'],4, labels=ic2_labels)
data # or: data['ic2_']
```

```python
# There is also pd.qcut which is based on quantiles.

pd.cut(data['ic2'],4)     # to check the bins
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

Use the column `MDMAUD` to reduce the number of categories to two (`XXXX` and other).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>
    
```python
data["MDMAUD"].apply(lambda x: "other" if x != "XXXX" else x)
```
</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

Introduction to regular expressions

<details>
  <summary> Click for Code Sample: Simple Regex  </summary>

```python
import re

text = "That person wears marvelous trousers."

pattern = '[A-z]'
pattern = 'That'
pattern = '[That]'
pattern = '[atsdhksdgs]'
re.findall(pattern, text)

text = "This is an A and B conversation, so C your way out of it."
pattern = '[^A-z]'
re.findall(pattern, text)
```

</details>

<details>
  <summary> Click for Code Sample: Quantifiers </summary>

- `*`: Matches previous character 0 or more times
- `+`: Matches previous character 1 or more times
- `?`: Matches previous character 0 or 1 times (optional)
- `{}`: Matches previous characters however many times specified within:
- `{n}`: Exactly n times
- `{n,}`: At least n times
- `{n,m}`: Between n and m times

```python
text = "The complicit caat interacted with the other cats exactly as we expected."
pattern = "c*t"
print(re.findall(pattern, text))

text = "The complicit caat interacted with the other cats exactly as we expected."

pattern = 'c*a*t'
print(re.findall(pattern, text))

text = "The complicit caaaat ct interacted with the other cats exactly as we expected."
pattern = "a+"
print(re.findall(pattern, text))
# Returns matches where the previous character appears 1 or more times

text = "Is the correct spelling color or colour?"
pattern = "colou?r"
print(re.findall(pattern, text))

text = "We can match the following: aaaawwww, aww, awww, awwww, awwwww"
pattern = "aw{3}"
print(re.findall(pattern, text))

pattern = "aw{1,}"
print(re.findall(pattern, text))text = "Let's see how we can match the following: aaw, aaww, aawww, awwww, awwwww"

pattern = "a{2,}w{2,}"
print(re.findall(pattern, text))
```

</details>
   
#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

Create a function to automate the process of reducing the number of values of a categorical column.

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

```python
def reduce_categorical(x, to_keep = [], to_replace = [], processed = "Other"):
    if x in to_keep or (to_keep == [] and x not in to_replace):
        return x
    else:
        return processed
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

# Lab | Cleaning categorical data

1. In this lab we will explore numerical data, save the continuous and discrete variables into `continuous_df` and `discrete_df` variables. We will be using `WA_Fn-UseC*-Marketing-Customer-Value-Analysis.csv` dataset.
2. Plot a correlation matrix, what can you see?
3. Create a function to plot every discrete variables. Do the same with continuous variables (be careful, you may change the plot type to another one better suited for continuous data).
4. What can you see in the plots?
5. Look for outliers in the continuous variables we have found. Hint: There was a good plot to do that.
6. Have you found outliers? If you have, what should we do with them?
7. Check nan values per column.
8. Define a function that differentiate between continuous and discrete variables. Hint: Number of unique values might be useful. Store continuous data into a `continuous` variable and do the same for `discrete` and categorical.
9. for the categorical data, check if there is some kind of text in a variable so we would need to clean it. Hint: Use the same method you used in step 7. Depending on the implementation, decide what to do with the variables you get.
10. Get categorical features.
11. What should we do with the customer id column?

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

```python
continuous_df = numericals.drop(columns = discrete)
discrete_df = numericals[discrete]
```

```python
mask = np.zeros_like(numericals.corr())
mask[np.triu_indices_from(mask)] = True
with sns.axes_style("white"):
    f, ax = plt.subplots(figsize=(16, 10))
    ax = sns.heatmap(numericals.corr(), mask=mask,
                     square=True, linewidths=1, cmap="coolwarm",
                     vmax = 0.8, vmin = -0.8)

# We can see income and months since
# policy inception are somewhat related
```

```python
def df_bar(df):
    """
    Docs
    """
    sns.set(rc={'figure.figsize':(16,8)})

    for i, col in enumerate(df):
        plt.figure(i)
        sns.barplot(x = df[col].value_counts().index, y = df[col].value_counts())

    plt.show()
```

```python
def df_hist(df):
    """
    docs
    """
    sns.set(rc={'figure.figsize':(16,8)})
    for i, col in enumerate(df):
        plt.figure(i)
        sns.distplot(df[col], color = list(BASE_COLORS.keys())[i])
        plt.show()

# Only total claim amount (target), income and customer
# lifetime look like continuous variables. better
#check more in depth what's happening there:
# Distribution looks fine, probably no NaNs were replaced.
```

```python
box_colors = ["blue", "yellow", "red"]

f, ax = plt.subplots(1, 3, figsize=(16,8))

for i, col in enumerate(continuous[:3]):
    sns.boxplot(data = continuous_df[col], ax = ax[i], color = box_colors[i])
    ax[i].set_title(col, fontsize = 14)

plt.show()
# Serious outliers in customer lifetime value and monthly_premium_auto. Drop them.
```

---

### Additional Resources
