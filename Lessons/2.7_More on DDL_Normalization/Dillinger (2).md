
  # Lesson 2.7: More on DDL, Normalization, Aggregations, Window Functions
<summary> Click for Code Sample: DELETE </summary>

```sql
-- deletes the record where the condition is met
delete from account_demo where account_id = 1;

-- deletes all the contents from the table without deleting the table
delete from account_demo;
```

- Discuss the difference between `drop` and `delete`.
  It is important to note the difference between DELETE and DROP.
  `DELETE` only removes the contents of the table, while the `DROP` command removes the table from the database. (Remember we used the statement `drop table if exists district_demo;` before we created the table in the database to make sure that it doesn't already exist in the database.)

> Ask students to delete all the contents of the table `district_demo` as well.

```sql
delete from district_demo;
```


<summary> Click for Code Sample: INSERT </summary>

> Show to the students how to upload data in bulk into a table in the database. For this, we will use the `load data local` command. It is by default turned off. We can check it with the `show variables` command. You would see that value for the variable `local_infile` would be OFF. We can then use the `set global` command to switch that variable ON by setting a value 1 to it.

```sql
show variables like 'local_infile';
set global local_infile = 1;
```

```sql
delete from district_demo;

load data local infile './district.csv' -- this file is at files_for_lesson_and_activities folder
into table district_demo
fields terminated by ',';
```

> The CSV file has been attached. Note that, since we deleted a column in the `district_demo` table, the same changes were made to the CSV file. Ask the students to do the same with the account_demo table. The CSV file is added to the folder as well. Solution:

```sql
delete from account_demo;

load data local infile './account.csv' -- this file is at files_for_lesson_and_activities folders
into table account_demo
fields terminated BY ',';
```
```sql
update district_demo
set A4 = 0, A5 = 0, A6 = 0
where A2 = 'Kladno';
```


<summary> Click for Code Sample: Simple Aggregations </summary>
 
:exclamation: Note to instructor: We will keep using the `bank` database and its `loan` table.

```sql
-- what is the total amount loaned by the bank so far
select sum(amount) from bank.loan;

-- what is the total amount that the bank has recovered/received from the customers
select sum(payments) from bank.loan;

-- what is the average loan amount taken by customers in Status A
select avg(amount) from bank.loan
where Status = 'A';
```

This is how we can use simple aggregation functions. Now, you can challenge your students changing the previous query from _"What is the average loan amount taken by customers in Status A"_ to _"What is the average loan amount taken by customers in each of the status categories? Arrange them from highest to lowest"_.

</details>

<details> 
  <summary> Click for Code Sample: Simple GROUP BY </summary>

```sql
select avg(amount) from bank.loan
group by Status;
```

```sql
select avg(amount) as Average, status from bank.loan
group by Status
order by Average asc;
```
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

 In this example, we use the `bank.order` table.

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

<summary> Click for Code Sample: GROUP BY on multiple columns </summary>

```sql
select round(avg(amount),2) - round(avg(payments),2) as "Avg Balance", status, duration
from bank.loan
group by status, duration
order by status, duration;
```



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


