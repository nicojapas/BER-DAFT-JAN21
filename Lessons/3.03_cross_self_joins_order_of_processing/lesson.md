# Lesson 3.3: Cross self-joins

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce `self joins` and `cross joins` with two tables in the same database. We will also revisit logical order of processing for queries with multiple join statements.

:exclamation: Note to instructor: You can mention that `join` statements can also be used to pull information from tables that are not within the same database. At the end of lesson 4, if the instructor feels that there's enough time and that the class is ready to learn about it, we have added a few examples to show how it can be implemented.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Work with `self join`
- Work with `cross join`
- Interpret the logical order of processing queries with multiple `join` statements

---

### Lesson 1 key concepts

> :clock10: 20 min

- Understanding `self join`
- Implementing `self join`

:exclamation: Note to instructor: If this example finishes off early, you can start with the next example as that might take a little more time than this first example.

<details>
  <summary> Click for description: Self Joins </summary>

- As the name suggests, a `self join` is a join on the same table; ie. it allows you to join a table to itself. It is useful for querying hierarchical data or comparing rows within the same table.

</details>

<details>
  <summary> Click for Code Sample: Example 1 </summary>

- Here in this example we are trying to find the customers that are from the same district.

```sql
select * from bank.account a1
join bank.account a2
on a1.account_id <> a2.account_id
and a1.district_id = a2.district_id
order by a1.district_id;
```

:exclamation: Note: In the query before, focus on the `<>` operator that we used. This is also an example of _compound_ conditions in the join statement.

```sql
select a1.account_id, a2.account_id, a1.district_id
from bank.account a1
join bank.account a2
on a1.account_id <> a2.account_id
and a1.district_id = a2.district_id
order by a1.district_id, a1.account_id;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.03_activities/blob/master/3.03_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/a277c41e12d6ebc17ef7953132bffa54).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts (use ppt intro to sql)

> :clock10: 20 min

- More examples on `self join`
- Discuss other use cases for `self join`

<details>
  <summary> Click for Code Sample: Example2 </summary>

- Here in this example we are trying to find the customers that are both the `OWNER` and `DISPONENT` (look at the table `disp`)

```sql
select * from bank.disp d1
join bank.disp d2
on d1.account_id = d2.account_id
and d1.type <> d2.type;
```

```sql
select d1.account_id, d1.type as Type1, d2.type as Type2
from bank.disp d1
join bank.disp d2
on d1.account_id = d2.account_id
and d1.type <> d2.type;
```

:exclamation: Note to instructor: As you will see, there are repeated values for each of the account `id`s. Lets try to solve this problem now.

</details>

<details>
  <summary>Click for Code Sample: Example 2 continued</summary>

- Method 1

```sql
select d1.account_id, d1.type as Type1, d2.type as Type2
from bank.disp d1
join bank.disp d2
on d1.account_id = d2.account_id and d1.type <> d2.type
where d1.type = 'DISPONENT';
```

- Method 2

```sql
drop temporary table if exists combo;

create temporary table combo
select d1.account_id, d1.type as Type1, d2.type as Type2, row_number() over(order by account_id) as RowNumber
from bank.disp d1
join bank.disp d2
on d1.account_id = d2.account_id and d1.type <> d2.type;

select * from combo
where RowNumber % 2 = 1;
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

<!-- 🚨🚨🚨 @alberto: do we have any activity here? -->

```sql
TBD
```

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

```sql
TBD
```

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Understanding `cross join`
- Implementing `cross join`

<details>
  <summary> Click for description: Cross Joins </summary>

A `cross join` is used when you wish to create a combination of every row from two tables. The main idea of the `cross join` is that it returns the Cartesian product of the joined tables. Each row from one table is connected to every other row in the other table.

</details>

<details>
  <summary> Click for Code Sample </summary>

- General syntax

```sql
select * from table1
cross join table2;
```

- Lets say we want to find all the combinations of different card types and ownership of account. We have not talked about sub queries yet. We will cover sub queries in greater detail later.

```sql
select * from (
  select distinct type from bank.card
) sub1
cross join (
  select distinct type from bank.disp
) sub2;
```

</details>

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.03_activities/blob/master/3.03_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/2bbf4477bf48cec4addb65ae97c200ff).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- More examples on `cross join`
- Discuss the use cases for `cross join`
- [optional] Join statements on tables in different databases

<details>
  <summary> Click for description: Self Joins </summary>

- The `CROSS JOIN` is used to generate a paired combination of each row of the first table with each row of the second table. This join type is also known as `cartesian` join.

</details>

<details>
  <summary> Click for Code Sample </summary>

- Note: this is just a dummy example.

```sql
create temporary table bank.distinct_cards;
select distinct type from bank.card;

create temporary table bank.distinct_frequency;
select distinct frequency from bank.account;

select * from distinct_cards
cross join distinct_frequency;
```

</details>

<details>
  <summary> Some more details on cross coins </summary>

The `CROSS JOIN` have a high potential to consume more resources and they can cause performance issues as they are computationally very expensive. This is because it produces the number of rows that are returned is the product of number of rows in table 1 times the number of rows in the other table.

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.03_activities/blob/master/3.03_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/e0512728c9341c7bef9ae8528337a5fc).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-self-cross-join](https://github.com/ironhack-labs/lab-sql-self-cross-join)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/e831878790121466a78998a96cda29fb).

</details>

---

### Additional Resources

- [A few good examples on `self` joins](https://www.sqlservertutorial.net/sql-server-basics/sql-server-self-join/)
