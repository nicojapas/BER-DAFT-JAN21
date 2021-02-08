# Lesson 2.3: DateTime strings, null functions & logical order of processing queries

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to teach students how to use other advanced functions such as _DateTime_ functions and _null_ functions for data processing and filtering. Also, acknowledging and understanding the logical order of processing queries will help the students to write efficient queries and debug errors.

---

### Setup

To start this lesson, students should have:

- Completed lesson 2.2
- All previous Setup

---

### Learning Objectives

After this lesson, students will be able to:

- Manipulate DateTime data to suit your projects requirements
- Apply multiple conditions using the `WHERE` clause and `case` statement

---

### Lesson 1 key concepts



Using DateTime functions

- Converting other data types to date, DateTime
- Changing formats of date columns using `date_format()`

<details>
<summary> Click for Code Sample </summary>

:exclamation: Note for instructor: Keep working on the `bank` database.

In the first table, the column `date` is of type integer. So we will convert the column into date format.
For now, this change will only be temporary as we not altering the structure of the table where the column has been defined.

```sql
select account_id, district_id, frequency, convert(date,date) from bank.account;
```

In the function, `convert()`, the first argument is the name of the column and the second is the type to which you want to convert. Similarly, we can do it for the `loan` table:
`select CONVERT(date,date) from bank.loan;`.

```sql
select account_id, district_id, frequency, CONVERT(date,datetime) from bank.account;

-- next is a two step process:
select substring_index(issued, ' ', 1) from bank.card;
select convert(SUBSTRING_INDEX(issued, ' ', 1), date) from bank.card;
```

A list of formats can be found [here](https://www.w3schools.com/sql/func_mysql_date_format.asp).

```sql
-- converting the original format to the date format that we need:
select date_format(convert(date,date), '%Y-%M-%D') from bank.loan;

-- if we just want to extract some specific part of the date
select date_format(convert(date,date), '%Y') from bank.loan;
```

</details>

---

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.03_activities/blob/master/2.03_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution]().

</details>

---


---

### Lesson 2 key concepts



- Logical order of processing SQL queries

. The order is shown below:

1.  FROM
2.  ON
3.  JOIN
4.  WHERE
5.  GROUP BY
6.  WITH CUBE/ROLLUP
7.  HAVING
8.  SELECT
9.  DISTINCT
10. ORDER BY
11. TOP/LIMIT
12. OFFSET/FETCH

> Here is a [link](https://www.itprotoday.com/sql-server/logical-query-processing-what-it-and-what-it-means-you) to the logical order of processing SQL queries.

<details>
<summary> Click for Code Sample: Logical order of processing </summary>

```sql
select * from bank.card
where type = 'classic'
order by card_id
limit 10;

select * from bank.order
where k_symbol = 'SIPO' and amount > 5000
order by order_id desc
limit 10;
```


</details>

---



---

### Lesson 3 key concepts



Null functions

- What a `null` value means in SQL - Three valued logic in SQL
- Checking for the `null` values


<details>
<summary> Click for Code Sample: Null functions </summary>

```sql
select isnull('Hello');
select isnull(card_id) from bank.card;

-- this is used to check all the elements of a column.
-- 0 means not null, 1 means null
select sum(isnull(card_id)) from bank.card;

select * from bank.order
where k_symbol is null;
```

As you might have noticed in this case, even though we see a lot of missing values in the column `k_symbol`, the above query does not filter those rows. It might be because those columns actually have value, for example, empty space. SQL considers that as a character/ value as well. So we will check for that now:

```sql
select * from bank.order
where k_symbol is not null and k_symbol = ' ';
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz


<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.03_activities/blob/master/2.03_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution]().

</details>

---



---

### Lesson 4 key concepts



Case statements in SQL

- Why do we need case statements

      - Using case statements

- Data types in SQL (will be added to the next session)

<details>
<summary> Click for Code Sample </summary>

In the `loan` table, there's column status A, B, C, and D. Using the case statement we will try to replace the values there with a brief description.

```sql
select loan_id, account_id,
case
when status = 'A' then 'Good - Contract Finished'
when status = 'B' then 'Defaulter - Contract Finished'
when status = 'C' then 'Good - Contract Running'
else 'In Debt - Contract Running'
end as 'Status_Description'
from bank.loan;
```

</details>

---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-3](https://github.com/ironhack-labs/lab-sql-3)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---

