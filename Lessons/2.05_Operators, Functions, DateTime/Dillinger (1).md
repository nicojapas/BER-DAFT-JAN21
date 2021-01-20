# Lesson 2.5: Operators, Functions, DateTime and Logical Order of Processing in SQL

# Queries 2.05!

 ```sql
select * from bank.order
where amount > 1000;

select * from bank.order
where k_symbol = 'SIPO';

select order_id, account_id, bank_to, amount from bank.order
where k_symbol = 'SIPO';

select order_id as 'OrderID', account_id as 'AccountID', bank_to as 'DestinationBank', amount  as 'Amount'
from bank.order
where k_symbol = 'SIPO';

```
```sql
select *, amount-payments as balance
from bank.loan;

select loan_id, account_id, date, duration, status, amount-payments as balance
from bank.loan;

select loan_id, account_id, date, duration, status, (amount-payments)/1000 as 'balance in Thousands'
from bank.loan;

-- this is the modulus operator that gives the remainder. This is a dummy example:
select duration%2
from bank.loan;

select 10%3;
```
> These comparison operators are used with the `WHERE` clause, for filtering data:

```sql
select * from bank.loan
where status = 'B';
-- In this case status B is for those clients where the contract has finished but the loan is not paid yet

select * from bank.loan
where status in ('B','b');

select * from bank.loan
where status in ('B','b') and amount > 100000;
```
```sql
select * from bank.loan
limit 10;

-- to get the bottom rows of a table, there is no predefined function
-- but you can sort the results in descending order and then use the LIMIT function
select * from bank.account
order by account_id desc
limit 10;
-- In this case, we were able to do it because the data was arranged
-- in ascending order of the account_id
```
```sql
select * from bank.loan
limit 10;

-- to get the bottom rows of a table, there is no predefined function
-- but you can sort the results in descending order and then use the LIMIT function
select * from bank.account
order by account_id desc
limit 10;
-- In this case, we were able to do it because the data was arranged
-- in ascending order of the account_id
```


```sql
-- two conditions applied on the table
select *
from bank.loan
where status = 'B' and amount > 100000;

-- we can have as many conditions as we need
select *
from bank.loan
where status = 'B' and amount > 100000 and duration <= 24;
--
select *
from bank.loan
where status = 'B' or status = 'D';
-- Status B and D are the clients that were bad for business for the bank

select *
from bank.loan
where (status = 'B' or status ='D') and amount > 200000;

-- logical NOT operator - it negates the boolean expression that we are evaluating
select *
from bank.order
where not k_symbol = 'SIPO';

select *
from bank.order
where not k_symbol = 'SIPO' and not amount < 1000;
```
```sql
select order_id, round(amount/1000,2)
from bank.order;

-- checking the number of rows in the table, both methods give the same result
-- given that there are no nulls in the column in the second case:
select count(*) from bank.order;

select count(order_id) from bank.order;

select max(amount) from bank.order;
select min(amount) from bank.order;

select floor(avg(amount)) from bank.order;
select ceiling(avg(amount)) from bank.order;
```


 String Functions</summary>

```sql
select length('himanshu');
select *, length(k_symbol) as 'Symbol_length' from bank.order;
select *, concat(order_id, account_id) as 'concat' from bank.order;

-- formats the number to a form with commas,
-- 2 is the number of decimal places, converts numeric to string as well
select *, format(amount, 2) from bank.loan;

select *, lower(A2), upper(A3) from bank.district;
-- It is interesting to note that select lower(A2), upper(A3), * from bank.district; doesn't work

select A2, left(A2,5), A3, ltrim(A3) from bank.district;
-- Similar to ltrim() there is rtrim() and trim(). And similar to left() there is right()
```
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

**Logical Order Processing of Queries**

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

<details>
<summary> Click for Code Sample: Case Statements </summary>

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