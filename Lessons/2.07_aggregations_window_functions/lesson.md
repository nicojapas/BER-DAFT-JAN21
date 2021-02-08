# Lesson 2.7: Aggregations & Window functions

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce _summary_ queries, and learn how to _aggregate_ results based on a _single column or multiple columns_. Also, we will talk about _window functions_ we use for executing calculations.

---

### Setup

To start this lesson, students should have:

- Completed lesson 2.6
- All previous Setup

---

### Learning Objectives

After this lesson, students will be able to:

- Apply `GROUP BY` clause on a single column
- Apply `GROUP BY` clause on multiple columns
- Use window functions to perform simple calculations

---

### Lesson 1 key concepts



- Queries using aggregate functions
- Using aggregate functions with `GROUP BY` clause (`GROUP BY` on a single column)

<details>
<summary> Click for Code Sample </summary>

Revisiting the last query we did using GROUP BY. In this query we will try to find out the average balances for the different statuses of people who have taken loans:

```sql
-- step1:
select round(avg(amount),2) as "Avg Amount", round(avg(payments),2) as "Avg Payment", status
from bank.loan
group by status
order by status;

-- step 2:
select round(avg(amount),2) - round(avg(payments),2) as "Avg Balance", status
from bank.loan
group by status
order by status;
```

```sql
-- Find the average amount of transactions for each different kind of k_symbol

select round(avg(amount),2) as Average, k_symbol from bank.order
group by k_symbol
order by Average asc;
```

As you can see, whichever `k_symbol` was empty, those values were also aggregated. We can remove those values by using a simple filter on it.

```sql
select round(avg(amount),2) as Average, k_symbol from bank.order
where k_symbol<> ' '
group by k_symbol
order by Average asc;
```

```sql
-- the same query with NOT operator

select round(avg(amount),2) as Average, k_symbol from bank.order
where not k_symbol = ' '
group by k_symbol
order by Average asc;
```

</details>

---



---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.07_activities/blob/master/2.07_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution]().

</details>

---



---

### Lesson 2 key concepts



- Using aggregate functions with `GROUP BY` clause (`GROUP BY` more than one column)
- More examples on `GROUP BY` clause

<details>
<summary> Click for Code Sample </summary>

:exclamation: Note to instructor: We will keep building on the example we used before. Now, we want to deep dive based on months as well:

```sql
select round(avg(amount),2) - round(avg(payments),2) as "Avg Balance", status, duration
from bank.loan
group by status, duration
order by status, duration;
```

Emphasize on the order used in the `ORDER BY` clause. Run the next query and explain the students the difference between the two outputs:

```sql
select round(avg(amount),2) - round(avg(payments),2) as "Avg Balance", status, duration
from bank.loan
group by status, duration
order by duration, status;
```

You can add more layers with the `GROUP BY` clause to further group the data based on multiple columns. For this example, we will take a look at the transaction table in the database. We want to analyze the average balance based on the `type`, `operation` and `k_symbol` fields:

```sql
-- Query without the "order by" clause
select type, operation, k_symbol, round(avg(balance),2)
from bank.trans
group by type, operation, k_symbol;
```

```sql
-- Query with the "order by" clause
select type, operation, k_symbol, round(avg(balance),2)
from bank.trans
group by type, operation, k_symbol
order by type, operation, k_symbol;
```

</details>

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.07_activities/blob/master/2.07_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution]().

</details>

---



---

### Lesson 3 key concepts



- Using `GROUP BY` and `HAVING` clauses together
- Difference between `WHERE` and `HAVING` clauses
- Using multiple conditions in `HAVING` clause

Discussion: `WHERE` vs. `HAVING` clause

> **WHERE** clause is used to apply the condition to the rows before the aggregation while **HAVING** clause is used to apply the condition after the data has been aggregated. We will try to explain the difference through the next example.

<details>
  <summary> Click for Code Sample: "group by" with "having" clause </summary>

```sql
select type, operation, k_symbol, round(avg(balance),2) as Average
from bank.trans
where k_symbol <> '' and k_symbol <> ' ' and  operation <> ''
group by type, operation, k_symbol
having Average > 30000
order by type, operation, k_symbol;
```

- As you can see the filter using the `having` clause was applied to the aggregated column. A regular filter can be used, just like the `where` clause but that is not an efficient way of using the `having` clause. It is shown in the example below:

```sql
-- Not the most efficient way of using the HAVING clause

select type, operation, k_symbol, round(avg(balance),2) as Average
from bank.trans
where k_symbol <> '' and k_symbol <> ' '
group by type, operation, k_symbol
having operation <> ''
order by type, operation, k_symbol;
```

</details>

<details>
<summary> Click for Code Sample: Group By with HAVING clause </summary>

```sql
-- Using the same query as before

select round(avg(amount),2) - round(avg(payments),2) as Avg_Balance, status, duration
from bank.loan
group by status, duration
having Avg_Balance > 100000
order by duration, status;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz


<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_2.07_activities/blob/master/2.07_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution]().

</details>

---


---

### Lesson 4 key concepts


- Window functions `OVER()` and `PARTITION BY()`
- Using aggregation functions with window functions
- How are they different than `GROUP BY` functions

> Window functions also operate on a subset but they do not reduce the target to a single value. They operate on a window which is specified by `PARTITION BY()`. As you will see in the query, we are trying to compare individual balances with the average balance for that particular duration of the loan. This would be very difficult and complicated to get using a `GROUP BY` clause.

Here are some of the available window functions. We will talk about `RANK()` and `DENSE_RANK()` in the next session.

![List of window functions](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/2.7-list_of_window_functions.png)

<details>
  <summary> Click for Code Sample </summary>

```sql
select loan_id, account_id, amount, payments, duration, amount-payments as "Balance",
avg(amount-payments) over (partition by duration) as Avg_Balance
from bank.loan
where amount > 100000
order by duration, balance desc;
```

```sql
-- You can also have ORDER BY clause followed by partition by as shown below:
select loan_id, account_id, amount, payments, duration, amount-payments as "Balance",
avg(amount-payments) over (partition by duration order by duration asc, amounts desc) as Avg_Balance
from bank.loan
where amount > 100000;
```

</details>

> You can use other aggregations functions as required. It is important to note that even though the column "Avg_Balance" is aggregated, you can't apply the `HAVING` clause to this column unlike with a `GROUP BY` clause. You can use the results of this query as a subquery and then use these results as a table itself. We will talk more about subqueries in the next week.

---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-7](https://github.com/ironhack-labs/lab-sql-7)

</details>

<details>
  <summary>Click for Solution: Lab solutions </summary>

- Link to the [lab solution]().

</details>

---



---
