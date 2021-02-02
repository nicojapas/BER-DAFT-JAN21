# Lesson 4.6: Intro to Statistics & Distributions for random variables

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce some concepts from statistics, such as _population_, _sample_, _random samples_, _random variables_, _bias_, and _variance_. We will then develop the idea of _continuous and discrete distributions from random variables_.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Infer probability calculations and its logic
- Incorporate statistics technical vocabulary, including: _population_, _sample_, _random samples_, and _random variables (continuous and discrete)_
- Interpret continuous distributions and discrete distributions

---

### Lesson 1 key concepts

> :clock10: 20 min

- Population
- Samples and random samples
- Random variables

      - Continuous random variables
      - Discrete random variables

<details>
  <summary> Discussion: Statistics  </summary>

- A population is an aggregate/collection of creatures, things, cases, and so on. A population commonly contains too many individuals to study conveniently, so an investigation is often restricted to one or more samples drawn from it.
- The relation between the sample and the population is such that it allows inferences to be made about a population from that sample.
- A random sample means that the observations from the population are picked randomly and not without any bias.
- Differences between the population mean and the sample mean, population standard deviation, and sample standard deviation. etc.

      - The population mean and standard deviation are fixed (assumed to be fixed) and are called population parameters.
      - The sample mean, sample std. deviation varies every time we calculate them as a random sample will have different values every time. They are called sample statistics.

- A random variable, usually written `X`, is a variable whose possible values are numerical outcomes of a random phenomenon (usually the thing under observation/the thing that we are trying to measure); for eg. height people in the US, marks scored in a test, etc. This random variable can be either continuous or discrete in nature.

      - *Discrete random variable*: The set of values that his random variable can take are discrete (usually but not necessarily counts)
      - *Continuous random variable*: The set of values that his random variable can take are continuous

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

From these examples, distinguish between samples/populations and estimators/parameters:

- Election polls
- Results of general elections
- To do an inventory
- Mean salary on Manhattan based on the census
- Mean salary on Manhattan, based on a survey
- Joe's running time over 4 weeks
- Mean age at marry in France
- Industrial product evaluation (check durability of bulbs)
- Average german height

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>
    
- Solutions:
    
    - Election polls - **Sample**
    - Results of general elections - **Population**
    - To do an inventory - **Population**
    - Mean salary on Manhattan based on the census - **Parameter**
    - Mean salary on Manhattan based on a survey - **Estimator**
    - Joe's running time over 4 weeks - **Estimator**
    - Mean age at marry in France - **Population**
    - Industrial product evaluation (check durability of bulbs) - **Sample**
    - Average german height - **Parameter**
    
</details>
    
---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Introduction to **probability**

      - Experiment
      - Sample space
      - Random variables
      - Probability distributions

- Discrete distributions

      - Bernoulli's distribution
      - Binomial distribution

<details>
  <summary> Click for Description: Intro to probability </summary>

:exclamation: Note to instructor: You can use the examples of flipping a coin and rolling a dice to explain these concepts.

- **Probability theory** is concerned with determining the likelihood that a certain event will occur during a given random experiment.
- **Experiment** is any situation that involves observation or measurement. Random experiments are those which can have different outcomes regardless of the initial conditions and will be heretofore referred to simply as experiments.
- **Sample space** - The results obtained from an experiment are known as the outcomes. Sample space is the set of all possible outcomes for that experiment.
- **Events** - We can create a subset of the sample space called an event. We then enumerate all outcomes in the event. Each time the experiment is run, a given event A either occurs or does not occur. Intuitively, you should think of an event as a meaningful statement about the experiment.
- **Random Variable** is a real valued function on the sample space. It is a measurement of interest in the context of the random experiment. A random variable `X` is random in the sense that its value depends on the outcome of the experiment, which can't be predicted with certainty before the experiment is run. As mentioned before, the random variables can be either discrete or continuous.
- **Calculating probabilities** is the ratio of an event to the entire sample space.

</details>

<details>
  <summary> Click for Description: Discrete Distributions </summary>

- In the field of probability, a **distribution function** is a function that maps numerical values to probabilities.
- Discrete distributions arise from discrete random variables. Discrete random variables can take discrete values (usually but not necessarily counts).
- This means that a discrete probability distribution is characterized by having a finite or countably infinite number of outcomes in the sample space. The sum of probabilities for all outcomes in the sample space must add up to 1.

- **Bernoulli distribution** describes the outcome of a single yes or no event. An example of this distribution is a coin toss. We do not have to use a fair coin (a coin where there is a 50% chance of heads and 50% chance of tails). We can describe the probability of heads as `p` and say that the probability of tails is `1-p`.

- **Binomial distribution** - When n independent Bernoulli's experiments are conducted, it gives rise to a binomial distribution. Each of the `n` experiments is either a success or a failure with a probability of success `p`.

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- For a fixed `n = 100` and `size = 1000`, iterate over `p = [0.01, 0.1, 0.25, 0.5, 0.75, 0.9, 0.99]`. What is happening here?

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- We can take **p** as the **skewness** of our plotted distributions. The **lower** it is, the more skewed to the right it tends to be. The **higher** it is, the more skewed to the left it tends to be.

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

for i in [0.01, 0.1, 0.25, 0.5, 0.75, 0.9, 0.99]:
    plt.figure()
    x = np.random.binomial(n=100, p=i, size=1000)
    sns.distplot(x)

plt.show()
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Discrete distributions

      - Geometric distribution

- Continuous Distributions I

      - Normal distribution
      - Properties of normal distribution
      - Standard normal distribution

<details>
  <summary> Description: Geometric distribution  </summary>

- Discuss Geometric distribution, its parameters, domain (set of values of `x` on which it is defined) and properties.

</details>

<details>
  <summary> Description: Normal distribution  </summary>

- Discuss normal distribution, its parameters, domain (set of values of `x` on which it is defined) and properties.
- Standard normal distribution.

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Try to plot a Binomial distribution for each `n` in `[10, 100, 1000, 10000]` and a constant `p = 0.5`. What can you see when `n` tends to infinite? Does it look familiar to you? What happens with the mean of the created samples?

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- The bigger **n** is, the more it looks like a **Gaussian distribution**, and this is the way he discovered it, by increasing the samples of a binomial distribution. Also, the mean of the samples is closer (or it should be) to the **population mean** when **n** increases.

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

for i in [10, 100, 1000, 10000]:
    plt.figure()
    x = np.random.binomial(n=i, p=0.5, size=i)
    sns.distplot(x)

plt.show()
```

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Continuous Distributions II

      - Exponential distribution
      - Uniform distribution

<details>
  <summary> Description: Exponential distribution  </summary>

- Explain expo distribution, its parameters, domain (set of values of `x` on which it is defined) and properties.

</details>

<details>
  <summary> Description: Uniform distribution  </summary>

- Explain uniform distribution, its parameters, domain (set of values of `x` on which it is defined) and properties.

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- So far we have seen quite a lot of distributions, but is there one to rule them all? [This](http://onlinestatbook.com/stat_sim/sampling_dist/index.html) is a simulator for some distributions, try to play with it as much as you can and think about the results you are getting.
  **Hint**: Try to customize your distributions and extract some means from it

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Probably you discovered that it doesn't really matter if the initial distribution is or is not a Gaussian distribution, but if we took **n** samples from it and average them so we could create a distribution of means, it would **always** be **Gaussian**. This is well known as the [**Central Limit Theorem**](https://www.youtube.com/watch?v=b5xQmk9veZ4). You can check these additional resources: [resource 1](https://www.youtube.com/watch?v=hu3Z0JCrEMc) and [resource 2](https://www.youtube.com/watch?v=JNm3M9cqWyc).

</details>

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

# Lab | Random variable distributions

1. Get the numerical variables from our `WA_Fn-UseC_-Marketing-Customer-Value-Analysis.csv` dataset.
2. Check using a distribution plot if the variables fit the theoretical normal or exponential distribution.
3. Check if any of the transformation (log-transform, etc.) we have seen changes the result.

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

```python
from scipy import stats

## check numerical variables
continuous_df.head()
continuous_df.columns

## ['customer_lifetime_value', 'income', 'monthly_premium_auto', 'months_since_policy_inception', 'total_claim_amount']

m = continuous_df["total_claim_amount"].mean()
s = continuous_df["total_claim_amount"].std()
sns.distplot(continuous_df["total_claim_amount"])
sns.distplot(stats.norm(m,s).rvs(1000))

transformed = np.log(continuous_df["total_claim_amount"])
mt = transformed.mean()
st = transformed.std()

sns.distplot(transformed)
sns.distplot(stats.norm(mt,st).rvs(1000))

m = continuous_df["customer_lifetime_value"].mean()
s = continuous_df["customer_lifetime_value"].std()
sns.distplot(continuous_df["customer_lifetime_value"])
sns.distplot(stats.norm(m,s).rvs(1000))

sns.distplot(continuous_df["customer_lifetime_value"])
sns.distplot(m*stats.expon(1/m).rvs(1000))

transformed = np.log(continuous_df["customer_lifetime_value"])
mt = transformed.mean()
st = transformed.std()

sns.distplot(transformed)
sns.distplot(stats.norm(mt,st).rvs(1000))

m = continuous_df["monthly_premium_auto"].mean()
s = continuous_df["monthly_premium_auto"].std()
sns.distplot(continuous_df["monthly_premium_auto"])
sns.distplot(stats.norm(m,s).rvs(1000))

m = continuous_df["months_since_policy_inception"].mean()
s = continuous_df["months_since_policy_inception"].std()
sns.distplot(continuous_df["months_since_policy_inception"])
sns.distplot(stats.norm(m,s).rvs(1000))

transformed = np.log(continuous_df["months_since_policy_inception"])
mt = transformed.mean()
st = transformed.std()

sns.distplot(transformed)
sns.distplot(stats.norm(mt,st).rvs(1000))
```

</details>

---

### Additional Resources
