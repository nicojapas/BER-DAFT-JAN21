# Lesson 3.5: Subqueries & Nested subqueries

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper in subqueries - how to use the self-contained subqueries with the `WHERE` clause with the different operators, including the `IN` operator and comparison operator. We will also talk in more detail about other clauses that can be used with nesting, including `HAVING`, `SELECT` and `FROM`.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Write self-contained subqueries using the `WHERE` clause along with the `IN` and comparison operators.
- Use subqueries with other clauses including `HAVING`, `SELECT` and `FROM`.
- Interpret the logical order of processing for subqueries.

---

### Lesson 1 key concepts

> :clock10: 20 min

- Subqueries with the `WHERE` clause
- Using comparison operators: Comparison with a single value

<details>
  <summary> Click for Code Sample: Using comparison operators: Comparison with a single value</summary>

This is a simple example where we are trying to show how subqueries are used. The same could also be achieved by using `HAVING` clause and no subquery.

```sql
select * from (
  select account_id, bank_to, account_to, sum(amount) as Total
  from bank.order
  group by account_id, bank_to, account_to
) sub1
where total > 10000;
```

- Sample A: The result from this query will be used again in later session to build further in the other topic we will cover.
- In this query we are trying to find those banks from the `trans` table where the average amount of transactions is over 5500.
- If we try to find this result directly, it would not be possible as we need only the names of the banks and not the averages in this case.

```sql
select bank from (
  select bank, avg(amount) as Average
  from bank.trans
  where bank <> ''
  group by bank
  having Average > 5500) sub1;
```

- Sample B : The result from this query will be used again in later session to build further in the other topic we will cover.
- In this query we are trying to find the `k_symbols` based on the average amount from the table `order`. The average amount should be more than 3000.

```sql
select k_symbol from (
  select avg(amount) as Average, k_symbol
  from bank.order
  where k_symbol <> ' '
  group by k_symbol
  having Average > 3000
  order by Average desc
) sub1;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.05_activities/blob/master/3.05_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solutions](https://gist.github.com/ironhack-edu/413b12dbd30f1cd002f56aa1a66d4398).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts (use ppt intro to sql)

> :clock10: 20 min

- More examples on sub queries - Using the `IN` operator: Comparison with a list of values.

<details>
  <summary> Click for Code Sample A :Using IN operator: Comparison with a list of values</summary>

- In this query we will use the results from Sample A. In that query we found the banks from the `trans` table where the average amount of transactions is over 5500. Now we will use those results to filter the results from the `order` table where `bank_to` is in the list of banks found previously.

```sql
select * from bank.order
where bank_to in (
  select bank from (
    select bank, avg(amount) as Average
    from bank.trans
    where bank <> ''
    group by bank
    having Average > 5500
    ) sub1
)
and k_symbol <> ' ';
```

</details>

<details>
  <summary> Click for Code Sample B :Using the `IN` operator: Comparison with a list of values </summary>

- In this query we will use the results from Sample B. In that query we found the `k_symbols` based on the average amount from the table `order`. The average amount was more than 3000. Now we will use the results from the query above to only see the transactions from the `trans` table where the `k_symbol` value is the result from the above query.

```sql
select * from bank.trans
where k_symbol in (
  select k_symbol as symbol from (
    select avg(amount) as Average, k_symbol
    from bank.order
    where k_symbol <> ' '
    group by k_symbol
    having Average > 3000
    order by Average desc
  ) sub1
);
```

</details>

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.05_activities/blob/master/3.05_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions </summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/fb21d8939e02d58d718c6fb65e7d5f36).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Some properties/important points on subqueries
- Logical order of processing of subqueries

:exclamation: Note to instructor: Use the previous queries as reference to explain these concepts.

<details>
  <summary> Properties/important points on sub queries</summary>

1. A subquery is a `select` statement that is included with another query.
2. Enclose the subquery in parenthesis.
3. A subquery can return a single value, a list of values or a complete table.
4. A subquery can't include an `ORDER BY` clause.
5. There can be many levels of nesting in the subquery.
6. When you use a subquery, its results can't be included in the `SELECT` statement of the main query.

</details>

<details>
  <summary> Logical order of processing</summary>

1.  `FROM`
2.  `ON`
3.  `JOIN`
4.  `WHERE`
5.  `GROUP BY`
6.  `HAVING`
7.  `SELECT`
8.  `DISTINCT`
9.  `ORDER BY`
10. `LIMIT`

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.05_activities/blob/master/3.05_activity_3.md).

</details>

<details>
  <summary> Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/050ec5c4188738489a58bac7411a5652).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Introduction to nested sub queries with more than one level of nesting.

<details>
  <summary> Click for Lesson Description </summary>

Here we are again using Sample A to further filter the results based on aggregation on the amount column as can be seen in the query below:

```sql
select k_symbol from (
  select avg(amount) as mean, k_symbol
  from bank.order
  where bank_to in (
    select bank
    from (
      select bank, avg(amount) as Average
      from bank.trans
      where bank <> ''
      group by bank
      having Average > 5500
    ) sub1
  )
  and k_symbol <> ' '
  group by k_symbol
  having mean > 2000
) sub;
```

Here we are again using Sample B to further filter the results based on aggregation on the balance column as can be seen in the query below:

- Step 1

```sql
select avg(balance) as Avg_balance, operation
from bank.trans
where k_symbol in (
  select k_symbol as symbol
  from (
    select avg(amount) as Average, k_symbol
    from bank.order
    where k_symbol <> ' '
    group by k_symbol
    having Average > 3000
    order by Average desc
  ) sub1
)
group by operation;
```

- Step2: Now we want only the name of the operation that has the higher balance.

```sql
select operation from (
  select avg(balance) as Avg_balance, operation
  from bank.trans
  where k_symbol in (
    select k_symbol as symbol
    from (
      select avg(amount) as Average, k_symbol
      from bank.order
      where k_symbol <> ' '
      group by k_symbol
      having Average > 3000
      order by Average desc
    ) sub1
  )
  group by operation
) sub2
order by Avg_balance
limit 1;
```

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-subqueries](https://github.com/ironhack-labs/lab-sql-subqueries)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/94a6f31128e5adaf8694df2d5566acc2).

</details>
