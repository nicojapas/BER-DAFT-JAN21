# Lesson 2.8.: Rank & Intro to Joins

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce `RANK()` and `DENSE_RANK()` functions, and introduce the concept of **JOINS** to extract information from more than one table at the same time.

---

### Setup

To start this lesson, students should have:

- Completed lesson 2.7
- All previous Setup

---

### Learning Objectives

After this lesson, students will be able to:

- Use window functions along with `RANK()` and `DENSE_RANK()`
- Explain how simple SQL _joins_ work

---

### Lesson 1 key concepts



More on window functions

- `RANK()`
- `DENSE_RANK()`
- `ROW_NUMBER()`

<details>
<summary> Click for Code Sample </summary>

```sql
-- Goal is to rank the customers based on the amount of loan borrowed.
-- This will help us to find the nth highest amount in the table

select *, rank() over (order by amount desc) as 'Rank'
from bank.loan;
```

Note that we have not used partition by clause here. So we are not working on any windows. We are just trying to rank the data. We can achieve the same results with `row_number()` as well, as shown below:

```sql
select *, row_number() over (order by amount desc) as 'Row Number'
from bank.loan;
```

```sql
-- In this query, we are trying to rank the customers based on the amount of loan
-- they have borrowed, in each of the "k_symbol" categories

select * , rank() over (partition by k_symbol order by amount desc) as "Ranks"
from bank.order
where k_symbol <> " ";
```

As a next step if we want to create a filter on these ranks, to find out, let's say which are the top 10 customers or which is the nth highest customer, we can use this query as a subquery to find the answer to such questions. We will learn about sub queries next week.

</details>

---



---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.08_activities/blob/master/2.08_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution]().

</details>

---


---

### Lesson 2 key concepts


- Analyze the difference between `RANK()` and `DENSE_RANK()` and `ROW_NUMBER()`

  - The difference between the three is apparent when there are duplicate values in the column by which we are trying to sort the results
  - Here is a result that illustrates the difference:

![Difference between Rank, Dense Rank, and Row Number](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/2.8-difference_rank_denseRank_rowNumber.png)

<details>
  <summary> Click for Code Sample: Difference between rank(), dense_rank(), and row_number() </summary>

- Ask the students to observe the differences in the output

```sql
select *, rank() over(order by amount asc) as 'RANK'
from bank.order
where k_symbol <> ' ';

select *, dense_rank() over(order by amount asc) as 'RANK'
from bank.order
where k_symbol <> ' ';
```

- While using RANK the ties are assigned the same rank and the next ranking is skipped
- However with DENSE_RANK you get consecutive ranks without any skips

</details>

- Here is another result that illustrates the difference:

![Difference between Rank, Dense Rank, and Row Number](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/2.8-difference_rank_denseRank_rowNumber.png)

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.08_activities/blob/master/2.08_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution]().

</details>

---


---

### Lesson 3 key concepts



Introduction to SQL **joins**

- Why do we need to join two or more tables
- Different types of SQL joins
- Simple queries to create the join

<details>
<summary> Click for Code Sample: Simple Queries To Create Joins </summary>

- Introduction to SQL joins (ppt added `reference_slides/intro_sql_joins.pptx`)

      - Why do we need to join two or more tables
      - Different types of SQL joins

</details>

<details>
<summary> Click for Code Sample: Simple Queries To Create Joins </summary>

```sql
select * from bank.account a
join bank.loan l on a.account_id = l.account_id
limit 10;
```

> Note that in the query below we have used the alias for columns on which we have used `WHERE` and `ORDER BY` clause. It is not necessary in this case as there would be no conflict for MySQL because those columns duration and payments are only present in table loan. Alias is used to remove the ambiguity in case the column names are the same. It is also a good practice that one should follow.

```sql
-- Building on the same query to add some filters and order by

select * from bank.account a
join bank.loan l on a.account_id = l.account_id
where l.duration = 12
order by l.payments
limit 10;
```

-- Using an alias to select some columns

```sql
select a.account_id, a.frequency, l.loan_id, l.amount, l.duration, l.payments, l.status
from bank.account a
join bank.loan l on a.account_id = l.account_id
where l.duration = 12
order by l.payments
limit 10;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_2.08_activities/blob/master/2.08_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution]().

</details>

---


---

### Lesson 4 key concepts


More examples of simple queries to join two tables

- Left outer join
- Right outer join

<details>
  <summary> Click for Code Sample: Left Outer Join </summary>

```sql
-- Some test code

select count(distinct account_id) from bank.account;
select count(distinct account_id) from bank.loan;
```

- As you can see, there is a difference in the output from the two queries. Hence, we can say that not all the `account_id`s have information available from the `order` table. Or, you can say that not all the customers have taken a loan from the bank.

```sql
-- Left Join

select a.account_id, a.frequency, l.loan_id, l.amount, l.duration, l.payments, l.status
from bank.account a
left join bank.loan l on a.account_id = l.account_id
order by a.account_id;
```

> Note: Since this is a left join, all the rows from the table on the left are selected for sure. Hence, the result has the same number of rows as the number of rows in the `account` table. You can verify that with `select count(*) from table name` query.

</details>

<details>
<summary> Click for Code Sample: Right Outer Join </summary>

```sql
select a.account_id, a.frequency, l.loan_id, l.amount, l.duration, l.payments, l.status
from bank.account a
right join bank.loan l on a.account_id = l.account_id
order by a.account_id;
```

> Note: Again, since this is a right join, all the rows from the table on the right are selected for sure. Hence, the result has the same number of rows as the number of rows in the `loan` table. You can verify that with `select count(*) from table name` query.

</details>

### :pencil2: Practice on key concepts - Lab


<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-8](https://github.com/ironhack-labs/lab-sql-8)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---


