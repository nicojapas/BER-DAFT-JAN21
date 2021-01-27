# Lesson 3.7: Correlated subqueries & Rolling Calculations

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into correlated subqueries and write more complex queries with `join` statements and subqueries. We will also talk more about `lag_functions`, `self joins` and how it can be used to perform rolling calculations. We will also revisit logistic regression and go in details on how the algorithm works to make those classifications.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Write more complex correlated subqueries
- Use `self joins` to perform rolling calculations - some examples:

  - Month on month percentage change for monthly active users (MAU)
  - Number of retained users per month
  - Number of users that did not come back this month (churned users)

- Differentiate _binary classification_ and _multi-class classification_ problems

- Interpret how the logistic regression algorithm uses loss function, optimization method, sigmoid function to predict classes

---

### Lesson 1 key concepts

> :clock10: 20 min

More examples on correlated subqueries.

- Write a query to find the month on month monthly active users (MAU)
- Use **lag()** function to get the active users in the previous month

<details>
  <summary> Click for Code Sample: Month on Month - Monthly Active Users</summary>

```sql
create or replace view user_activity as
select account_id, convert(date, date) as Activity_date,
date_format(convert(date,date), '%M') as Activity_Month,
date_format(convert(date,date), '%Y') as Activity_year
from bank.trans;
```

```sql
create or replace view Monthly_active_users as
select count(distinct account_id) as Active_users, Activity_year, Activity_Month
from user_activity
group by Activity_year, Activity_Month
order by Activity_year, Activity_Month;
```

</details>

<details>
  <summary> Click for Code Sample: active users in the previous month </summary>

```sql
with cte_activity as (
  select Active_users, lag(Active_users,1) over (partition by Activity_year) as last_month, Activity_year, Activity_month
  from Monthly_active_users
)
select * from cte_activity
where last_month is not null;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.07_activities/blob/master/3.07_activity_1.md).

</details>

<details>
  <summary> Click for Solution: Activity 1 solutions </summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/8c84a8e079d5cd1de1dff602232812f3).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> [**Slides**](https://docs.google.com/presentation/d/1q-P3sxtKOaSWHf2V381RRY-mC-A2JOqD1KSjq8Db4I4/edit?usp=sharing)

:exclamation: Note for instructor: Use the PPT from previous lesson 2.1.2 to refresh te concepts for the class.

> :clock10: 20 min

Use `self joins` to perform rolling calculations - some examples

- Number of retained users per month

<details>
  <summary>Click for Code Sample: Number of retained users per month</summary>

```sql
with distinct_users as (
  select distinct account_id , Activity_month, Activity_year
  from user_activity
)
select count(distinct d1.account_id) as Retained_customers, d1.Activity_month, d1.Activity_year
from distinct_users d1
join distinct_users d2
on d1.account_id = d2.account_id and d1.activity_month = d2.activity_month + 1
group by d1.Activity_month, d1.Activity_year
order by d1.Activity_year, d1.Activity_month;
```

We will create a view of the previous query. We will then use the results from the view to find the percentage change in the number of retained customers month over month.

```sql
create or replace view retained_customers_view as
with distinct_users as (
  select distinct account_id , Activity_month, Activity_year
  from user_activity
)
select count(distinct d1.account_id) as Retained_customers, d1.Activity_month, d1.Activity_year
from distinct_users d1
join distinct_users d2 on d1.account_id = d2.account_id
and d1.activity_month = d2.activity_month + 1
group by d1.Activity_month, d1.Activity_year
order by d1.Activity_year, d1.Activity_month;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.07_activities/blob/master/3.07_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/204cf9749c56dfb794b96b738ba85667).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

Examples on `self joins` to perform calculations such as monthly customer retention/customer churn.

- Percentage change in the number of retained customers.

<details>
  <summary>Click for Code Sample</summary>

- Step 1

```sql
select * from retained_customers;
```

- Step 2

```sql
select Retained_customers, lag(Retained_customers, 1) over(partition by Activity_year), Activity_month, Activity_year
from retained_customers;
```

- Step 3

```sql
select (Retained_customers-lag(Retained_customers, 1) over(partition by Activity_year)) as Diff, Activity_month, Activity_year
from retained_customers;
```

</details>

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.07_activities/blob/master/3.07_activity_3.md).

</details>

<details>
  <summary> Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/4b88859b09cb93e5154c424e04eabf60).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min
> [**Slides**](https://docs.google.com/presentation/d/1JPVYRMUrPvKzi-rXVY_QWm3jU8GLmFhYT3KQKTIth4E/edit?usp=sharing)

- Binary vs. multi-class classification problems
- How logistic regression predicts classes

      - Sigmoid function
      - Loss function
      - Optimization method

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.07_activities/blob/master/3.07_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/2e89abf07d676473abfa945ef7f957ca).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-rolling-calculations](https://github.com/ironhack-labs/lab-sql-rolling-calculations)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/683e96867ca6b6f01ccff6fd5311d8cc).

</details>
