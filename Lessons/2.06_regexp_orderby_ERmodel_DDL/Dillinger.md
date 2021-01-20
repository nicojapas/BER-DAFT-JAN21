# LESSON QUERIES


# Queries 2.06!

 
```sql
select A3 from bank.district;
select distinct A3 from bank.district;
```

```sql
select * from bank.order
where k_symbol in ('leasing', 'pojistine');
```


```sql
select * from bank.account
where district_id in (1,2,3,4,5);
```

```sql
-- We are trying to get the same result using the between operator.
-- Note that 1 and 5 are included in the range of values compared/evaluated

select * from bank.account
where district_id between 1 and 5;

select * from bank.loan
where amount - payments between 1000 and 10000;
```


```sql
select * from bank.district
where A3 like 'north%';

select * from bank.district
where a3 like 'north_M%';
-- This would return all the results for
-- 'north  Moravia', 'northMoravia', northMiami'
```

```sql
select * from bank.district
where a3 regexp 'north';

-- Now we will take a look at another table
-- to see the difference between LIKE and REGEXP
select * from bank.order
where k_symbol regexp 's';

select * from bank.order
where k_symbol regexp '^s';

select * from bank.order
where k_symbol regexp 'o$';

-- We can include multiple conditions at the same time
select distinct k_symbol from bank.order
where k_symbol regexp 'ip|is';
```
```sql
select distinct a2 from bank.district
order by a2;

select distinct a2 from bank.district
order by a2 asc;

select * from bank.district
order by a3;

select * from bank.district
order by a3 desc;
```


```sql
select * from bank.order
order by account_id, bank_to;

select * from bank.order
order by account_id, bank_to, k_symbol;
```


- In the table definition of `account_demo`, the column date was defined as _integer_ type. We will modify the column to _date_ type.

```sql
alter table account_demo
modify date date;
select * from account_demo;
```

> Drop a column

```sql
alter table district_demo
drop column A15;
select * from district_demo;
```

> Rename table name

```sql
alter table account_demo
rename to accountDemo;
```

> Rename column name in a table

```sql
alter table district_demo
rename column A1 to dist_id;
```

> Add a new column

```sql
alter table accountDemo
add column balance int(11) after date;
```




