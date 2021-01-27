# Lesson 3.2: Joins on multiple tables

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into inner joins and outer joins to extract relevant information from more than two tables in the same database.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Use the `join` statements with more than two tables
- Use aliases with the `join` statements
- Use compound conditions with the `join` statement

---

### Lesson 1 key concepts

> :clock10: 20 min

- Use inner join to pull data from more than two tables in the same database

<details>
  <summary> Click for Code Sample: Join with Multiple Tables </summary>

- Demo how to extract all the information from three tables (`disp`, `client`, and `card`). Get all the columns from 3 tables (this is going to be useful later when we start using map, filter, etc.)

```sql
select * from bank.disp d
join bank.client c
on d.client_id = c.client_id
join bank.card ca
on d.disp_id = ca.disp_id;

select d.disp_id, d.type, d.client_id, c.birth_number, ca.type from bank.disp d
join bank.client c
on d.client_id = c.client_id
join bank.card ca
on d.disp_id = ca.disp_id
where ca.type = 'gold';
```

- One more example - demo how to extract all the information from three tables (`disp`, `client`, and `district`):

```sql
select * from bank.disp d
join bank.client c
on d.client_id = c.client_id
join bank.district da
on da.A1 = c.district_id;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.02_activities/blob/master/3.02_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/034fa79a1d597a4482275fd0e16beae6).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts (use ppt intro to sql)

> :clock10: 20 min

:exclamation: Note to the instructor: Explain to the students here which is the table on the left and which is the table on the right. The order in which the `join` statement creates the aggregate tables will help the students how to use different tables and in which order.

- Discussion on the logical order of processing of queries with simple join statement
- Discussion on the logical order of processing of queries with multiple join statements

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.02_activities/blob/master/3.02_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions </summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/9deafb8dd0a1cf97116cf7f57690ce06).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Create temporary tables
- Using compound conditions with the inner join statement

:exclamation: Note to instructor: We used a dataset where we didn't have multiple tables where more than one keys were the same in different tables. This is just a dummy example to show the students that we can use multiple conditions in the join statements.

<details>
  <summary>Click for Code Sample: Create Temporary Tables</summary>

```sql
create temporary table bank.loan_and_account
select l.loan_id, l.account_id, a.district_id, l.amount, l.payments, a.frequency
from bank.loan l
join bank.account a
on l.account_id = a.account_id;

select * from bank.loan_and_account;
```

```sql
create temporary table bank.disp_and_account
select d.disp_id, d.client_id, d.account_id, a.district_id, d.type
from disp d
join account a
on d.account_id = a.account_id;

select * from bank.disp_and_account;
```

</details>

<details>
  <summary>Click for Code Sample: Compound Conditions with Join Statements</summary>

```sql
select * from bank.loan_and_account la
join bank.disp_and_account da
on la.account_id = da.account_id
and la.district_id = da.district_id;
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.02_activities/blob/master/3.02_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/3104cc95f744b58c0a76495e42758b41).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- Use outer joins to pull data from more than two tables in the same database.

<details>
  <summary> Click for Code Sample </summary>

```sql
select a.account_id, a.district_id, a.frequency, d.A2, d.A3, l.loan_id, l.amount, l.payments
from bank.account a left join bank.district d
on a.district_id = d.A1
left join bank.loan l
on a.account_id = l.account_id
order by a.account_id;
```

</details>

:exclamation: Note to the instructor: Break down the query into two steps. First step is the join between the `account` and the `district` tables. The second step is the join between (`account` and `district`) and `loan` tables. Then explain how the left outer join is giving out those null values (not every customer in the bank has taken a loan).

- Notice the difference in the results if we remove the keyword `left` from the query above. The query with only inner joins for the same tables as above is shown below:

<details>
  <summary> Click for Code Sample </summary>

```sql
select a.account_id, a.district_id, a.frequency, d.A2, d.A3, l.loan_id, l.amount, l.payments
from bank.account a join bank.district d
on a.district_id = d.A1
join bank.loan l
on a.account_id = l.account_id
order by a.account_id;
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.02_activities/blob/master/3.02_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/facf2c56d6c65d94a7b80477606df5c7).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-join-multiple-tables](https://github.com/ironhack-labs/lab-sql-join-multiple-tables)

</details>

<details>
  <summary>Click for Solution: Lab solutions </summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/75fbaf6fec702d73d06e8dc24827953f).

</details>
