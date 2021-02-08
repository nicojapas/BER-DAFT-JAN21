# Lesson 6.4: Advanced SQL | Pivots - Intro to Stored Procedures

### Lesson Duration: 3 hours

> Purpose: In the lessons, we will get back to SQL and learn some advanced SQL techniques, including creating pivots and stored routines (procedures and functions). We will start with a review of SQL and then continue with building on the concepts and techniques we learned in units 2 and 3.

### Case Scenario and Objective

We will be using the same dataset we used for Unit 2 and Unit 3. The business case scenario is the same. You are working as an analyst for a bank. The bank offers services to private clients. The services include account management, offering loans, etc. The bank stores information about clients, including accounts held, transactions over the last few months, loans granted, cards issued, etc. The bank managers hope to improve their understanding of customers and seek specific actions to improve services.

---

### Learning Objectives

After this lesson, students will be able to:

- List & explain the behaviour of the most important of SQL queries
- Use Pivots to make data more accesible 
- Explain what a stored procedure does

---

### Lesson 1 key concepts

> :clock10: 20 min

- SQL Review

  - SLQ joins to extract information from multiple tables
  - Aggregation and subqueries

<details>
<summary> Click for description: Join operations </summary>

- We are interested in finding information about clients such as their account numbers, their demographics (where they are from), and the loan details but only of clients that are marked as status "B". Customers with status "B" are the customers for whom the contract is finished but they have not repaid the loan.

```sql
select a.account_id, a.district_id, a.frequency, d.A2 as District, d.A3 as Region, l.loan_id, l.amount, l.payments, l.status
from bank.account a join bank.district d
on a.district_id = d.A1
join bank.loan l
on a.account_id = l.account_id
where l.status = "B"
order by a.account_id;
```

<!-- 🚨🚨🚨 Guillem: this code crashed in my machine @Himanshu -->
<!-- 🚨🚨🚨 @Guillem: could we get the notebook you used for the teachback and link it here? -->

Now we will use the result and try to answer some important business questions: what is the average amount of the loan and the average payment made, for the customers who defaulted?

</details>

<details>
<summary> Click for description: Aggregation and subqueries </summary>

- We are interested in finding about only those default customers whose amount borrowed was more than the average amount borrowed by all the customers who defaulted. <!-- 🚨🚨🚨 @himanshu: it is pretty confusing with all the "defaulted" mixed in the sentence? Is there any way to simplify this? -->

```sql
select * from (
  select a.account_id, a.district_id, a.frequency, d.A2 as District,
    d.A3 as Region, l.loan_id, l.amount, l.payments, l.status
  from bank.account a join bank.district d
  on a.district_id = d.A1
  join bank.loan l
  on a.account_id = l.account_id
where l.status = "B"
order by a.account_id)sub1
where sub1.amount > (
  select round(avg(amount),2)
  from bank.loan
  where status = "B")
order by amount desc;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

Using the same dataset, answer the following questions:

- How many accounts do we have?
- And how many of them are defaulted?
- What is the percentage of defaulted people in the dataset?
- What can we extract from here? <!-- 🚨🚨🚨 @himanshu: does this mean - what can we conclude based on the previous findings? -->

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

```sql
with total as (
  select count(account_id) as c
  from account
),
total_default as (
  select count(a.account_id) as c
	from account a
  join district d
	on a.district_id = d.A1
		join loan l
		on a.account_id = l.account_id
		where l.status = "B"
)
select total.c as total,
  total_default.c as defaulted,
  total_default.c / total.c * 100 as percentage
from total, total_default;
```

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- SQL Review

  - Common table expressions
  - Correlated subqueries

<details>
<summary> Click for description: Common table expressions </summary>

Now we want to find out when was the last transaction made, by the customers identified in the previous step.

```sql
with cte_1 as (
  select * from (
    select a.account_id, a.district_id, a.frequency, d.A2 as District,
      d.A3 as Region, l.loan_id, l.amount, l.payments, l.status
    from bank.account a
    join bank.district d
    on a.district_id = d.A1
      join bank.loan l
      on a.account_id = l.account_id
      where l.status = "B"
      order by a.account_id ) sub1
    where sub1.amount > (
      select round(avg(amount),2)
      from bank.loan
      where status = "B")
    order by amount desc)
select cte_1.account_id, cte_1.amount, max(date(t.date)) as Last_transaction
from cte_1
join bank.trans t on cte_1.account_id = t.account_id
group by cte_1.account_id, cte_1.amount
order by cte_1.amount desc;
```

</details>

<details>
<summary> Click for Description: Correlated subqueries </summary>

- For this problem, we will take a look at the `loan` table. We are interested in finding information on those customers who made a payment higher than the average payment from customers with the same loan duration and who are status "A". We can use this info for promotional marketing as these would be important customers.

```sql
select *
from bank.loan l1
where l1.payments > (select avg(payments) from bank.loan l2 where l1.duration = l2.duration)
and l1.status = "A"
order by account_id;
```

- Now, to fetch more information, you can use the results of this query and join it with some other table as well, as shown in the previous queries.

:exclamation: Note for instructor: You can mention some of the other techniques we talked about earlier, for eg., datetime functions, string functions, case statements, window functions, temporary tables, etc, if there is enough time. They key idea here is that the students should get back in the mental framework of working on queries. Therefore, we just have these few examples as an quick review.

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

Find the `account_id`, `amount` and `date` of the first transaction of the defaulted people if its amount is at least twice the average of non-default people transactions.

Hint: Use the query used in class.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

```sql
with cte_1 as (
  select * from (
    select a.account_id, a.district_id, a.frequency, d.A2 as District,
      d.A3 as Region, l.loan_id, l.amount, l.payments, l.status
    from bank.account a
    join bank.district d
    on a.district_id = d.A1
      join bank.loan l
      on a.account_id = l.account_id
      where l.status = "B"
      order by a.account_id) sub1
    where sub1.amount > (select round(avg(amount),2) * 2 from bank.loan where status = "A")
    order by amount desc)
select cte_1.account_id, cte_1.amount, min(date(t.date)) as First_transaction
from cte_1
join bank.trans t on cte_1.account_id = t.account_id
group by cte_1.account_id, cte_1.amount
order by cte_1.amount desc;
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Pivoting data

<details>
<summary> Click for description </summary>

Pivoting can be used to make the output more presentable and easier to look at and make decision.

- In the next query, we are trying to look at the total transaction amount from all the customers in the first quarter of the year 1993. We can achieve this by using a simple group by condition as shown below:

```sql
select account_id, round(sum(amount),2) as Amount, date_format(date, "%M") as Month
from trans
where date_format(date, "%Y") = 1993 and date_format(date, "%m") <= 3
group by account_id, Month;
```

- If you look at the output, it is not very readable as we want to see the trend in the transaction for each user. We can improve the readability of this output by pivoting the data.

</details>

<details>
  <summary>Click for Code Sample: Pivot Implementation using Case Statement</summary>

```sql
select account_id,
max ( case when Month = 'January' Then Amount end) as January,
max ( case when Month = 'February' Then Amount end) as February,
max ( case when Month = 'March' Then Amount end) as March
from (
  select account_id, round(sum(amount),2) as Amount, date_format(date, "%M") as Month
  from trans
  where date_format(date, "%Y") = 1993 and date_format(date, "%m") <= 3
  group by account_id, Month) sub1
group by account_id
order by account_id;
```

- Here is another implementation of pivot using the pivot operator (not available in mysql but available in other sql standards).

```sql
select account_id, 'January', 'February', 'March'
from (
  select account_id, amount, date_format(date, "%M") as Month from trans
  where date_format(date, "%Y") = 1993 and date_format(date, "%m") <= 3
) as sub1
pivot (
  sum(amount) for Month in ('January', 'February', 'March')
) as sub2;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

Create a pivot table showing the average amount of transactions made by frequency for each district.

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

```sql
select district,
avg( case when frequency = 'POPLATEK MESICNE' Then avg_amount end) as 'POPLATEK MESICNE',
avg( case when frequency = 'POPLATEK PO OBRATU' Then avg_amount end) as 'POPLATEK PO OBRATU',
avg( case when frequency = 'POPLATEK TYDNE' Then avg_amount end) as 'POPLATEK TYDNE'
from (
  select district.A2 AS district, account.frequency, round(avg(trans.amount),2) as avg_amount
  from trans
  join account
  on trans.account_id = account.account_id
  join district
  on district.A1 = account.district_id
  group by district.A2, account.frequency) sub1
group by district
order by district;
```

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 mins

- Stored routines (procedures and functions)

  - What are stored routines
  - How are they useful
  - Example of a simple stored routine

<details>
  <summary> Click for Description: What are stored routines </summary>

- A stored routine is a set of SQL statements that can be stored in the server.
- A stored routine is either a procedure or a function. Stored routines are created with `CREATE PROCEDURE` and `CREATE FUNCTION` statements.
- Stored routines may call other stored routines.

</details>

<details>
  <summary> Click for Description: Why are they useful </summary>

- _Reusability_: They can be executed by multiple users on the server without the need of rewriting the code again.
- _Security_: They can ensure that each operation is properly logged. For eg., in banks the users would not have access to the database tables directly but can only execute specific stored routines.
- _Performance_: They are more efficient as the user does not need to rewrite the code. Also when they are executed for the first time, a query execution plan is created by SQL which is then implemented faster every time the routine is run again.

</details>

<details>
  <summary> Click for Code: Creating simple stored procedure </summary>

- We want to create a stored procedure that returns the number of rows in a table.

:exclamation: Note for instructor: This is just a simple introduction, explain the syntax. Also, let the students know that we will talk about this in detail in the later session.

```sql
delimiter //
create procedure number_of_rows_proc (out param1 int)
begin
select COUNT(*) into param1 from bank.account;
end;
//
delimiter ;

call number_of_rows_proc(@x);
select @x;
```

</details>

<details>
  <summary> Click for Code: Creating simple stored function </summary>

- We want to create a stored function that outputs a simple string.

:exclamation: Note for instructor: This is just a simple introduction, explain the syntax. Also, let the students know that we will talk about this in detail in the later session.

```sql
create function print_function (x char(20))
returns char(50) deterministic
return concat('Iron', x, '!');

select print_function('hack');
```

- A procedure or function is considered "deterministic" if it always produces the same result for the same input parameters, and "not deterministic" otherwise. If not specified, the default is "not deterministic".

</details>

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<!-- 🚨🚨🚨 @Guillem @Himanshu: do we have a lab here? -->

<details>
  <summary> Click for Instructions: Lab </summary>

```python
TBD
```

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

```python
TBD
```

</details>

---

:sandwich: **LUNCH BREAK**

---
