# Lesson 3.6: Correlated subqueries

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce CTEs (common table expressions) and how they can be used to simplify nested queries, along with views and how they can be used to store frequently required results. We will also introduce correlated subqueries.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Create and use the CTEs (Common Table Expressions) in SQL.
- Create and use views in SQL.
- Understand and write simple correlated subqueries.

---

### Lesson 1 key concepts

> :clock10: 20 min

- What are common table expressions (CTE)
- Writing the CTEs

<details>
  <summary>Click for Description: CTEs </summary>

A **common table expression** is a named object that stores temporarily results of a query and it exists only within the execution scope of a single SQL statement. Here are some of the reasons for using the CTEs:

- Improves readability and performance of the query
- Helps in simplifying the queries
- Recursive CTEs can be used for hierarchical data (this is not in scope of this class though, but good to mention)

</details>

<details>
  <summary> Click for Code Sample </summary>

- A very simple example to show the general syntax
- The query after the `AS` keyword can be any query (from a simple to a very complex)

```sql
with cte_loan as (
  select * from bank.loan
)
select * from cte_loan
where status = 'B';
```

In this query, we want to find the total amount and total balance of each customer in the transactions table and then pull more information on those customers by using a join between the CTE and the account table:

```sql
with cte_transactions as (
  select account_id, sum(amount), sum(balance)
  from bank.trans
  group by account_id
)
select * from cte_transactions ct
join account a
on ct.account_id = a.account_id;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.06_activities/blob/master/3.06_activity_1.md).

</details>

<details>
  <summary> Click for Solution: Activity 1 solutions </summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/dcaa7ef25326ff996db1367058e3c9ba).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- What are views and their importance
- Working with views in SQL

  - Create a view

<details>
  <summary>Click for Code Sample: Views</summary>

<details>
  <summary> Click for Description: views and their importance </summary>

- Views are like virtual tables in the database that can be used for querying just like a regular table but they do not store any information permanently in them, like a table does; ie. a table occupies actual memory in the database but views don't.
- Views can be built with queries on a single or multiple tables.
- Reasons why views are a very useful tool:

      - *Security*: Different users can be given access to different sets of views and not the complete database for eg. different departments would need different access levels based on the sensitivity of the information (hospital database where information about patients health is not available to administration or finance)
      - *Query simplicity*: It can help write neater and simplified query by not using many levels of nesting.

</details>

- In this query, we are creating a view to find the current customers that might be risky in the future. For this we found the average balance for the current customers in category `C` and checked which are the customers that have balances more than the average balance for that status category:

```sql
create view running_contract_ok_balances as
with cte_running_contract_OK_balances  as (
  select *, amount-payments as Balance
  from bank.loan
  where status = 'C'
  order by Balance
)
select * from cte_running_contract_OK_balances
where Balance > (
  select avg(Balance)
  from cte_running_contract_OK_balances
)
order by Balance desc
limit 20;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.06_activities/blob/master/3.06_activity_2.md).

</details>

<details>
  <summary> Click for Solution: Activity 2 solutions </summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/f7c5f0998b05eaaac89d1d2d7b280d63).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

Working with views in SQL

- Use the `WITH CHECK OPTION` clause
- Drop an existing view

<details>
  <summary> Click for Description </summary>

Views also allow database administrators to restrict data retrieval. Through MySQL views, it is also possible to update data in the underlying table. MySQL views, when used with the `WITH CHECK OPTION` allow for a safeguard mechanism against the data inserted into the underlying table.
Some of the points to keep in mind before updating data in a view are:

- The view should not have any aggregate clauses or function like `SUM`, `COUNT`, etc.
- The view should not have any right or left outer joins.
- The view should not have `UNION`, `GROUP BY`, `HAVING`, `DISTINCT` clauses.

The `WITH CHECK OPTION` prevents a view from updating or inserting rows that are not visible through it. In other words, whenever you update or insert a row of the base tables through a view, MySQL ensures that the insert or update operation is conformed with the definition of the view.

</details>

<details>
  <summary> Click for Code Sample: `WITH CHECK OPTION` </summary>

```sql
drop view if exists customer_status_D;

create view customer_status_D as
select * from bank.loan
where status = 'D'
with check option;
```

Or you can also use :

```sql
create or replace view customer_status_D as
select * from bank.loan
where status = 'D'
with check option;
```

Now if we try to insert new values in the table through the view, it doesn't work as the check is not met for status `D`:

```sql
select * from customer_status_D;

insert into customer_status_D values (0000, 00000, 987398, 00000, 60, 00000, 'C');
```

But, in this case we have removed the `WITH CHECK OPTION` and now, if we try to insert new values in the table through the view, it works even if the status `D` condition is not met:

```sql
create or replace view customer_status_D as
select * from bank.loan
where status = 'D';

select * from customer_status_D;

insert into customer_status_D values (0000, 00000, 987398, 00000, 60, 00000, 'C');

select * from  bank.loan
order by loan_id;
```

</details>

<details>
  <summary> Click for Code Sample: DROP EXISTING VIEW </summary>

```sql
drop view if exists customer_status_D;
```

</details>

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.06_activities/blob/master/3.06_activity_3.md).

</details>

<details>
  <summary> Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/fb93b5d1b635631c2a80d495620ee5c8).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Introduction to correlated subqueries
- Writing simple correlated subqueries

Correlated subqueries have references known as _correlations_ to columns from tables in the outer query. They tend to be trickier to troubleshoot when problems occur because you can't run them independently. If you copy the inner query and paste it in a new window (to make it runnable), you have to substitute the correlations with constants representing sample values from your data. But then when you're done troubleshooting and fixing what you need, you have to replace the constants back with the correlations. This makes troubleshooting correlated subqueries more complex and more prone to errors.

Unlike self-contained subqueries that are executed only once during the execution of the query, correlated subqueries are executed once for each row thats processed by the main query. The picture below shows how they work:

![Correlated Subqueries](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/3.6-correlated_subqueries.png)

<details>
  <summary> Click for Code Sample</summary>

Here we will try to build on the same example that we looked at for self-contained subquery. We extracted the results only for those customers whose loan amount was greater than the average. Here is the self-contained subquery:

```sql
select * from bank.loan
where amount > (
  select avg(amount)
  from bank.loan
)
order by amount desc
limit 10;
```

Now we want to find those customers whose loan amounts are greater than the average but only within the same status group; ie. we want to find those averages by each group and simultaneously compare the loan amount of that customer with its status group's average.

```sql
select * from bank.loan l1
where amount > (
  select avg(amount)
  from bank.loan l2
  where l1.status = l2.status
)
order by amount desc;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.06_activities/blob/master/3.06_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/4a3cadbe07596f407fac3a3a6d8de9fe).

</details>

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-advanced-queries](https://github.com/ironhack-labs/lab-sql-advanced-queries)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/beb761fe0dadf271e2d2ddb4d0b9f9fc).

</details>
