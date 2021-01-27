# Lesson 3.9: Measures of accuracy & Data Imbalance

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into the concepts of logistic regression including accuracy measures of the model and model assumptions. We will also introduce data imbalance in logistic regression problems and the methods to resolve the problem.

---

### Setup

- All previous set up
- Use the Jupyter notebook from the last lesson

### Learning Objectives

After this lesson, students will be able to:

- Measure of accuracy for classification problems
- Recognize the assumptions of logistic regression model
- Test the assumptions of the model
- Identify data imbalance and how to resolve the issue

---

### Lesson 1 key concepts (word doc added)

:exclamation: Note for instructor: Use this [document](files_for_lesson_and_activities/accuracy_measures_classification_models.md).

> :clock10: 20 min

Checking model accuracy

- Confusion matrix
- Precision, Recall
- `ROC AUC` curve

:exclamation: Note to instructor: `ROC AUC` curves are defined for binary classification problems. So use the reference of binary classification here.

<details>
  <summary> Click for Code Sample: Confusion Matrix </summary>

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_test, predictions)
```

</details>

<details>
  <summary> Click for Code Sample: ROC AUC </summary>

```python
from sklearn import metrics
import matplotlib.pyplot as plt

y_pred_proba = classification.predict_proba(X_test)[::,1]
fpr, tpr, _ = metrics.roc_curve(y_test,  y_pred_proba)
auc = metrics.roc_auc_score(y_test, y_pred_proba)
plt.plot(fpr,tpr)
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.09_activities/blob/master/3.09_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/c6498af075f924e258c857cb478a8a26).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Revisiting assumptions of linear regression
- Some assumptions in linear regression that are not required for logistic regression
- Assumptions of logistic regression model

:Exclamation: Note to instructor: Students and instructor can use this [additional resource](https://www.lexjansen.com/wuss/2018/130_Final_Paper_PDF.pdf).

<details>
  <summary> Difference in assumptions between linear regression and logistic regression</summary>

- Logistic regression does not need a linear relationship between the dependent and independent variables.
- The independent variables do not need to be multivariate normal – although multivariate normality yields a more stable solution.
- The error terms (the residuals) do not need to be multivariate normally distributed.
- Homoscedasticity is not needed. Logistic regression does not need variances to be heteroscedastic for each level of the independent variable.

</details>

:exclamation: Note to instructor: [Additional resource for the instructor][https://www.statisticssolutions.com/wp-content/uploads/wp-post-to-pdf-enhanced-cache/1/assumptions-of-logistic-regression.pdf]

<details>
  <summary>Assumptions of logistic regression</summary>

- Binary logistic regression requires the dependent variable to be binary.
- Since logistic regression assumes that `P(Y=1)` is the probability of the event occurring, it is necessary that the dependent variable is coded accordingly. That is, for a binary regression, the factor level 1 of the dependent variable should represent the desired outcome.
- The error terms need to be independent.
- The model should have little or no multicollinearity. That is that the independent variables should be independent from each other.
- Logistic regression assumes linearity of independent variables and log odd.
- It requires quite large sample sizes. Some statisticians recommend at least 30 cases for each parameter to be estimated.

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.09_activities/blob/master/3.09_activity_2.md).

</details>

<details>
  <summary> Click for Solution: Activity 2 solutions </summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/03ac3336dc53d3126c405d45c32def36).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

Testing the assumptions of the logistic regression

- Multicollinearity (already discussed with Linear Regression)
- Linear relationship between independent variables and log odds

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.09_activities/blob/master/3.09_activity_3.md).

</details>

<details>
  <summary> Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/f265b29d88cb90648fdae63a73a9f821).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Introduce data imbalance
- Methods to resolve data imbalance

      - Upsampling methods
      - Downsampling methods

:exclamation: Note to instructor: A Jupyter has been added with the code samples. In the class just explain the concept of imbalance and how it can impact the model predictions. Also explain the intuition/idea behind the _upsampling_ and _downsampling_ methods. Students can look at the code later as a DIY lesson.

<!-- (Note for Alberto : We can also include it in the activity 🚨🚨🚨 @alberto**) -->

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.09_activities/blob/master/3.09_activity_4.md).

</details>

<details>
  <summary> Click for Solution: Activity 4 solutions </summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/99019a80c8e3c896e31d1efa2f8cbfe1).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-imbalanced-data](https://github.com/ironhack-labs/lab-imbalanced-data)

</details>

<details>
  <summary> Click for Solution: Lab solutions </summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/63802156ad119876dd68fa5d8a8fdfa6).

</details>
