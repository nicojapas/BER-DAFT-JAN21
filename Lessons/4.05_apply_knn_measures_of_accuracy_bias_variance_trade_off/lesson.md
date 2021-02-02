# Lesson 4.5: Data modelling for accuracy

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to apply different models on the same data and compare the accuracies to select the best performing model. We will also talk a bit more in detail about different measures of accuracies for regression models and check how they are implemented in Python.

<!-- 🚨🚨🚨 @himanshu: do we need both unit4.csv and lesson_4.05_data.csv? it is confusing that notebook is using unit4.csv and the code example in lesson.md is using the other file.  -->

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Implement `KNN` (_k nearest neighbor_ algorithm)
- Choose the best value of `k` for the `KNN`
- Understand different measures of accuracies for regression models
- Compare linear regression and `KNN` models and select better performing model

---

### Lesson 1 key concepts

> :clock10: 20 min

- Final preparation of data for fitting the model

      - Putting numerical and categorical encoded data together
      - Train test split

- Revisit `KNN`
- Implement `KNN`

<details>
  <summary> Click for Code Sample:  </summary>

```python
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import OneHotEncoder
from sklearn.model_selection import train_test_split

data = pd.read_csv('lesson_4.05_data.csv') # this file is in files_for_lesson_and_activities folder
transformer = StandardScaler().fit(numericals)
x_standardized = transformer.transform(numericals)
```

```python
encoder = OneHotEncoder(handle_unknown='error', drop='first').fit(categoricals)
encoded = encoder.transform(categoricals).toarray()
```

```python
X = np.concatenate((x_standardized, encoded), axis=1)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=100)
```

```python
from sklearn.neighbors import KNeighborsRegressor
model = KNeighborsRegressor(n_neighbors=4)
model.fit(X_train, y_train)
```

```python
predictions = model.predict(X_test)
score = model.score(X_test, y_test)
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

1. Fit a linear regression model on the same processed data. Note we have already performed basic cleaning operations, data preprocessing - scaling and encoding, and then the train test split. Fit the model on the training data directly.
2. Find the accuracy of the model using the test data.
3. Compare the accuracies of linear regression and the `KNN` model.

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

```python
from sklearn import linear_model
from sklearn.metrics import r2_score
```

```python
lm = linear_model.LinearRegression()
model = lm.fit(X_train,y_train)
# predictions  = lm.predict(X_test)
r2_score(y_test, predictions)
```

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Choosing the best value of `k` for the `KNN`

<details>
  <summary> Click for Code Sample:  </summary>

```python
from sklearn.neighbors import KNeighborsRegressor
scores = []
for i in range(2,10):
    model = KNeighborsRegressor(n_neighbors=i)
    model.fit(X_train, y_train)
    scores.append(model.score(X_test, y_test))
```

```python
plt.figure(figsize=(10,6))
plt.plot(range(2,10),scores,color = 'blue', linestyle='dashed',
         marker='o', markerfacecolor='red', markersize=10)
plt.title('accuracy scores vs. K Value')
plt.xlabel('K')
plt.ylabel('Accuracy')
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- If you think a little bit about it, the number of neighbors might be very important for our results, but will it be the only parameter that matters? Go to the [documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html) and check the parameters and the values they can take, pick the one you think is more relevant and change its value in the model.
  **Hint**: If `K` (number of neighbors) is the most important one, maybe we could measure the way these `K` instances affect our prediction.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- The distance from the point predicted to its nearest K neighbors can be very relevant too:

```python
uniform_model = KNeighborsRegressor(n_neighbors=9)
uniform_model.fit(X_train, y_train)
uniform_model.fit(X_train, y_train)
uniform_model.score(X_test, y_test)
```

```python
distance_model = KNeighborsRegressor(n_neighbors=9, weights = "distance")
distance_model.fit(X_train, y_train)
distance_model.fit(X_train, y_train)
distance_model.score(X_test, y_test)
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts (ppt added)

> :clock10: 20 min

- [**SLIDES**](https://docs.google.com/presentation/d/1REBc9LkwY1lopuMeeG7nOgJh5yqTxWJh04rCov80qmM/edit?usp=sharing)

- Bias
- Variance
- Bias/Variance trade-off
- Over-fitting vs. under-fitting
- Discussion on Over-fitted models vs. under-fitted models

      - How they can impact the predictions

<details>
  <summary> Click for Description:  </summary>

- **Bias** - When we say that a measurement is unbiased, we mean that the average of a large set of observations will be close to the true value.

- **Variance** is a measurement of the spread between numbers in a data set.

:exclamation: Note to instructor: This is a more theoretical concept, so make sure to emphasize visuals from the slides.

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Let's visualize how `KNN` actually works. First of all install the [`mlxtend` library](http://rasbt.github.io/mlxtend/) and create a dataframe containing the two most relevant **numerical** variables and the target, in **that order**. Once you have done it sample it with `n = 100`, introduce that sample into this function with an arbitrary `k`:

```python
def knn_comparison(data, k):
    x = data.iloc[:, 0:2].values
    y = data.iloc[:, -1].astype(int).values
    knn = KNeighborsRegressor(n_neighbors=k)
    knn.fit(x, y)

    plt.figure(figsize=(16,12))
    plot_decision_regions(x, y, clf=knn)
    plt.title("Knn with K="+ str(k), fontsize = 18)
    plt.show()
```

- What can you see in the plot? Now try to create a function `plot_knn_boundaries` to loop over the previous function and iterate over the `ks = [1, 3, 5, 10, 25, 50]`. And now, _can you tell the difference between the plots_?

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

```python
def plot_knn_boundaries(data, ks = [1, 3, 5, 10, 25, 50]):
    for i in ks:
        knn_comparison(data, i)
```

- The lower the number of `k` the more over-fitted it will be. We can see that with `k = 1`, the boundaries are very clear and as we increase `k` the plots start turning very messy until the last two plots, where it is oversimplified.

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Measures of accuracies for regression models

      - `MAE` (Mean Absolute Error)
      - `MSE` (Mean Square Errors) and `RMSE` (Root Mean Square Errors)
      - `R` square
      - Adjusted `R` square

:exclamation: Note to instructor: Explain the difference between _`r` square_ and _adjusted `r` square_ - how adding more features to the model might tend to increase the `r` square even if the features are not significant while the same doesn't happen with adjusted `r` square. Adding insignificant features to the model might also lead to over-fitting (we will talk about over-fitting and under-fitting in the next session).

<details>
  <summary> Click for Discussion </summary>

- In regression problems, we measure the accuracy of an algorithm based on how far away the values that the algorithm predicts are from the true values.

![Measure of Accuracy](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.5-measures_of_accuracies.png)

</details>

<details>
  <summary> Click for Code Sample: `MAE` </summary>

The **mean absolute error** is the average (_mean_) of the absolute value of the distance between predicted and actual values. It is a measure of absolute error: because of this, we do not know if the algorithm is overestimating or underestimating when it is incorrect. Also because of the absolute value, it is not very sensitive to outliers, comparatively to other metrics.

```python
from sklearn.metrics import mean_absolute_error
score = mean_absolute_error(actual_values, predictions)
```

</details>

<details>
  <summary> Click for Code Sample: `MSE` </summary>

The **`MSE`** takes the difference between the predicted value and the actual value, squares it, then takes the average (_mean_) of those values.

```python
from sklearn.metrics import mean_squared_error
score = mean_squared_error(actual_values, predictions)
```

</details>

<details>
  <summary> Click for Code Sample: RMSE </summary>

The **`RMSE`** is the square root of the _mean square error_.

```python
import math
from sklearn.metrics import mean_squared_error
mse = mean_squared_error(actual_values, predictions)
rmse = math.sqrt(mse)
```

</details>

<details>
  <summary> Click for Code Sample: `R` square </summary>

The **`R-squared`** is the proportion of the variance in the model predictions that is predictable based on the input values. Practically, it is a measure of how likely future samples are to be predicted accurately by the model.
Adding more independent variables to a regression model tends to increase the `R-squared` value, which might give us a false idea about the accuracy of the model. This would also lead to over-fitting and can return an unwarranted high `R-squared` value.

```python
from sklearn.metrics import r2_score
score = r2_score(actual_values, predictions)
```

<details>
  <summary> Click for Discussion: `Adjusted R square` </summary>

We use the **adjusted R-squared** to compare regression models that contain different numbers of independent variables. For example, there is a model with 10 variables and the other model has 20 variables. The model with 20 variables will give a larger R square but does that really mean that this model is better than the other model. We can't say it for sure. For this, we use adjusted the `r` square.

Adjusted `r` square adjusts for the number of terms in the model. Its value increases only when the new term improves the model fit while it decreases when the term doesn't improve the model fit by a sufficient amount.

![Adjusted R square](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.5-adjusted_r_square.gif)

</details>

<details>
  <summary> Click for Code Sample: Adjusted R square </summary>

```python
score = 1 - (1-r_squared)*(len(y_test)-1)/(len(y_test)-X_test.shape[1]-1)
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- From all the regression metrics we have seen, which one do you think is the one to use in most cases?
- Calculate and plot `R2`, `MSE`, `RMSE`, and `MAE`.

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

We have seen **R2**, **MSE**, **RMSE** and **MAE**. Of course, there is not a magic solution for which you should always use it, but there are some details worth knowing:

- **R2** is scaled, which means that it is independent of the data. This one would be the one to go with if we don't know a lot about the data and general information about our model. However, it can be misleading, as it is supposed to be between **0** and **1** but sometimes **it is not** (you can read about it [here](http://www.fairlynerdy.com/what-is-r-squared/). In fact, **R2** is a **biased** estimator (more information [here](https://statisticsbyjim.com/regression/r-squared-too-high/#:~:text=Reason%201%3A%20R-squared%20is%20a%20biased%20estimate&text=In%20statistics%2C%20a%20biased%20estimator,use%20adjusted%20R2%20instead).

- **MAE** would be the _median_ of the regression metrics as what it measures is the sum of distances between predicted and real values (errors), and that won't give a special treat to really bad predictions, so if that's what we want this metric should do great.

  - _MSE_ - It is the mean of the squared distance of the errors, which will weight the bad predictions.
  - _RMSE_ - Root _MSE_, essentially it is the same but it is easier to understand within the data context

```python
from sklearn.linear_model import LinearRegression
from sklearn import metrics

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=100)

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print('Mean Absolute Error:', metrics.mean_absolute_error(y_test, y_pred))
print('Mean Squared Error:', metrics.mean_squared_error(y_test, y_pred))
print('Root Mean Squared Error:', np.sqrt(metrics.mean_squared_error(y_test, y_pred)))
print('R2:', metrics.r2_score(y_test, y_pred))
print('Adjusted R:',  1 - (1-metrics.r2_score(y_test, y_pred))*(len(y_test)-1)/(len(y_test)-X_test.shape[1]-1)

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

# Lab | Comparing regression models

1. In this final lab, we will model our data. Import sklearn `train_test_split` and separate the data.
2. Try a simple linear regression with all the data to see whether we are getting good results.
3. Great! Now define a function that takes a list of models and train (and tests) them so we can try a lot of them without repeating code.
4. Use the function to check `LinearRegressor` and `KNeighborsRegressor`.
5. You can check also the `MLPRegressor` for this task!
6. Check and discuss the results.

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
X_train, X_test, y_train, y_test = train_test_split(final_df.drop(columns = "total_claim_amount"),
                                                    final_df.total_claim_amount, test_size = 0.2)
```

```python
LR = LinearRegression()
LR.fit(X_train, y_train)
LR.score(X_test, y_test)
```

```python
def models_automation(models, X_train, y_train):
    for model in models:
        model.fit(X_train, y_train)
        print(f"{model.__class__.__name__}: Train -> {model.score(X_train, y_train)}, Test -> {model.score(X_test, y_test)}")

linear_models = [LinearRegression(), Lasso(), Ridge(), ElasticNet()]
models_automation(linear_models, X_train, y_train)
```

```python
from sklearn.neighbors import KNeighborsRegressor
knnr = [KNeighborsRegressor(5)]
models_automation([knnr], X_train, y_train)
```

```python
from sklearn.neural_network import MLPRegressor
mlpr = [MLPRegressor(max_iter = 1000)]
models_automation([mlpr], X_train, y_train)
```

</details>

---

### Additional Resources

- [Choosing the correct regression analysis method](https://statisticsbyjim.com/regression/choosing-regression-analysis/)
- [Continuous, discrete and categorical variables](https://support.minitab.com/en-us/minitab-express/1/help-and-how-to/modeling-statistics/regression/supporting-topics/basics/what-are-categorical-discrete-and-continuous-variables/)
