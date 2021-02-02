# Lesson 4.7: Intro to BI & Tableau

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce business intelligence and business intelligence tool _Tableau_ to the students. The accent is also on emphasizing that business knowledge is critical for business analysts/data analysts. We will also talk about `KPI`s and metrics. Also, we will explain the business case study (`A/B Testing`) for Tableau.

---

### Setup

- All previous installation
- Tableau installation (book at least 10-15 min of the class to make sure everyone has it)

### Learning Objectives

After this lesson, students will be able to:

- Explain what is business intelligence/business analysis
- Define and design some `KPI`s and metrics for business success tracking
- Interpret the use and benefits of `A/B Testing` to make more informed product design decisions
- Use Tableau as a BI Tool to get more insights as a team. That includes:

      - Connect Tableau with different data sources
      - Go about Tableau interface with confidence
      - Explore the different features available in Tableau and choose them according to the project needs

---

### Lesson 1 key concepts

> :clock10: 20 min

- Introduction to Business Intelligence
- Available Business Intelligence tools
- `KPI`s and metrics

  <details>
    <summary> Description: Intro to BI</summary>

**Business Intelligence** is an application of data analysis that aids companies in making data-driven decisions. Using Business Intelligence technologies, we can provide insights into the performance of a business in the past and the present as well as make predictions or recommendations for the future. A key difference between Business Intelligence and data analysis is those insights are meant to be actionable and geared towards helping a business make decisions.

  </details>

  <details>
    <summary> Description: BI Tools</summary>

- Business Intelligence tools are designed to make sense of the huge quantities of data that organizations accumulate over time. The BI tools analyze this information and present it as actionable information that can guide decision making.

- Business Intelligence software makes up a large heterogeneous category of software. Not all tools in the category can be meaningfully compared to each other. There are several types of BI tools of which the most substantive are the _Full-Stack Business Intelligence Tools_ and _Data Visualization Tools_.

- Some examples:

      - Tableau
      - Power BI
      - SAP Business Intelligence
      - Domo
      - IBM cognos analytics
      - Qliksense

  </details>

  <details>
    <summary> Description: `KPI`s and Metrics </summary>

- A **metric** is a quantifiable measure that provides us with information about how well a company is doing at achieving its business objectives. Since companies can only improve the things they measure, it is a crucial part of an analyst's job to use the most appropriate metrics. Many companies like to use the term `KPI` (Key Performance Indicator) interchangeably with metrics. An example of a metric is the amount of money spent per sale or the percent of customers that buy a product out of all customers visiting the site.

  </details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

  <details>
    <summary> Click for Instructions: Activity 1 </summary>

Think of possible `KPI`s in the context of Ironhack:

- KPIs for the organization?
- KPIs for your class?
- KPIs for you as an individual student?

  </details>

  <details>
    <summary>Click for Solution: Activity 1 solutions</summary>

_`KPI`s for the organization:_

- Number/percentage of students asking for information
- Number/percentage of students enrolled
- Number/percentage of students hired in the 6 months after graduation
- ...

_`KPI`s for your class:_

- Number of delayed labs
- Kahoot points
- Number of katas done

_`KPI`s for you as an individual student:_

- Time to complete lab
- Kahoot points
- Number of katas done
- ...

  </details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Introduction to Tableau
- Introduction to a business problem

  <details>
    <summary> Description: Intro to Tableau  </summary>

- **Tableau** is one of the most popular and intuitive applications for business intelligence and visual data exploration today. The application makes it easy to connect to an existing data source, create interactive visualizations, and perform basic analysis to discover insights. In this lesson, we will learn about the different features of Tableau and how it can be a valuable addition to your analytics tool kit. We will also cover how to load data into Tableau - both from file and hosted data sources.

  </details>

  <details>
    <summary> Description: Tableau Features </summary>

Tableau has several features that make it a great tool for data analysis. In this section, we will highlight some of the most important ones:

- _Drag & Drop Interface_: One of the things that makes Tableau so popular is its ease of use. It allows you to intuitively drag and drop fields where you want them and move things around to get exactly the visualization you want.
- _Variety of Visualizations_: Tableau has several types of visualizations you can use - from bar charts, line charts, and scatter plots to area charts, bubble charts, treemaps. Speaking of maps, it also allows you to visualize your data geographically.
- _Dashboards and Stories_: You can combine several visualizations into dashboards and stories that focus on communicating interesting insights with the stakeholders that need the information.
- _Data Exploration_: Perhaps one of Tableau's most useful features is how easy it is to explore data with it. You can start with an aggregated view of the data, filter to zoom in on something that looks interesting, add dimensions to get more well-rounded perspectives, and look at the underlying records to get to the bottom of what you are trying to investigate.

  </details>

  <details>
    <summary> Description: Intro to Business Problem  </summary>

- [**SLIDES**](https://docs.google.com/presentation/d/1bhXVQKmpkPgVUzIVOD8d8Ud7lG1O4wejByoRyTyjKJM/edit?usp=sharing)

  </details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

  <details>
    <summary> Click for Instructions: Activity 2 </summary>

- Think and draw how you would visualize the KPIs you identified for yourself. <!-- 🚨🚨🚨@himanshu: KPIs for what? -->

  </details>

  <details>
    <summary>Click for Solution: Activity 2 solutions</summary>

- Share your dashboard with the group.

  </details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Introduction to `A/B Testing`
- Data preparation for importing into Tableau

> [**Slides**](https://docs.google.com/presentation/d/1bhXVQKmpkPgVUzIVOD8d8Ud7lG1O4wejByoRyTyjKJM/edit?usp=sharing)

  <details>
    <summary> Click for Description: Introduction to `A/B Testing`  </summary>

:exclamation: Note to instructor: The instructor should give a few examples to explain what is `A/B testing`. Which are some common contexts of use? What have you used `A/B Testing` for in the past? Go-to example is a conversion funnel.

  </details>

  <details>
    <summary> Click for Code Sample: Data preparation for importing into tableau </summary>

```python
import pandas as pd
import numpy as np
pd.set_option('display.max_columns', None)

data1 = pd.read_csv('./ab_testing_case_study_docs/df_final_demo.txt')
print(data1.shape)

data1['client_id'].nunique()  # To check if all the IDs in the data are unique
data1.isna().sum()
data1[data1['clnt_age'].isna()]
data1 = data1.dropna()
data1.to_csv('df_final_demo.csv') # Exporting data to a csv file
```

```python
data2a = pd.read_csv('./ab_testing_case_study_docs/df_final_web_data_pt_1.txt')
data2b = pd.read_csv('./ab_testing_case_study_docs/df_final_web_data_pt_2.txt')

data2a.head()
data2b.head()

data2a.isna().sum()
data2b.isna().sum()

data2a.to_csv('df_final_web_data_pt_1.csv')
data2b.to_csv('df_final_web_data_pt_2.csv')

data2 = pd.concat([data2a, data2b], axis=0)
data2.to_csv('df_final_web_data.csv')
```

  </details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

  <details>
    <summary> Click for Instructions: Activity 3 </summary>
  
  - Think of metrics for our `A/B experiment`.
  - Think about how to present the results of the experiment visually.

  </details>

  <details>
    <summary>Click for Solution: Activity 3 solutions</summary>

- Total number of customers that confirm the transaction
- Percentage of customers per process step in both variation groups

      - Line plot
      - Bar plot

  </details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Data preparation for Tableau
- Importing data into Tableau

  <details>
    <summary> Click for Code Sample: Data Preparation </summary>

- Merging the two files

```python
data1.head()
data2.head()

data = data2.merge(data1, left_on='client_id', right_on='client_id')
print(data.shape)
data.head()
```

:exclamation: Note to instructor: The size of the original `data2` merged file was ~700k but this merged file has ~450k. This means that there are a lot of clients in the `data1` file that are not present here.

```python
data3 = pd.read_csv('./ab_testing_case_study_docs/df_final_experiment_clients.txt')
data3.head()

data3.isna().sum()
data3 = data3.dropna()

# Checking for unique clients
print(data3.shape)
data3['client_id'].nunique()
```

```python
data = data.merge(data3, left_on='client_id', right_on='client_id')
data3.to_csv('df_final_experiment_clients.csv')

# As you can see, again there is information size drop from ~450k to ~320k.
# You can find out who were the clients whose
# information was available on data3 but not in data2.

data['date_time'] = pd.to_datetime(data['date_time'])
data.to_csv('finalMergedFile.csv')
data.shape

df = data.copy()  # we will use df to analyse data with python later
```

  </details>

:exclamation: Note to instructor: We will use the final merged file with Tableau. The instructor can also talk about how to merge different files using Tableau directly but there is definitely more flexibility when we use a programming language such as Python.

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

  <details>
    <summary> Click for Instructions: Activity 4 </summary>

- Load `finalMergedFile.csv` into Tableau. Explore the column types. Are they the same as those in your Pandas dataframe?

   </details>

   <details>
     <summary>Click for Solution: Activity 4 solutions</summary>

      <!-- 🚨🚨🚨 @himanshu: Do we have a solution here? -->

   </details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

  <details>
    <summary> Click for Instructions: Lab </summary>

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

# Lab | Getting started with Tableau

1. Load `labs/data/WA_Fn-UseC_-Marketing-Customer-Value-Analysis.csv` into a notebook.
2. Create a barplot of the number of customers per **Gender**.
3. Create a barplot of the number of customers per **EmploymentStatus** and **Gender**.
4. Create a barplot of the number of customers per **Gender**.
5. Identify **Measurements** and **Dimensions**. Are they the same as the ones in your Pandas dataframe? Modify accordingly.
6. Save as `unit-4-lab.tbwx`

  </details>

  <details>
    <summary>Click for Solution: Lab solutions</summary>

- See [public tableau with the solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Labs/lab4_9_4).

  </details>

---

### Additional Resources
