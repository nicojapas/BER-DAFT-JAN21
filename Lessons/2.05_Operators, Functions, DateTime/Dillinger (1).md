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





<h1 class="code-line" data-line-start=0 data-line-end=1 ><a id="Lesson_25_Operators_Functions_DateTime_and_Logical_Order_of_Processing_in_SQL_0"></a>Lesson 2.5: Operators, Functions, DateTime and Logical Order of Processing in SQL</h1>
<h1 class="code-line" data-line-start=2 data-line-end=3 ><a id="Queries_205_2"></a>Queries 2.05!</h1>
<pre><code class="has-line-data" data-line-start="5" data-line-end="19" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> amount &gt; <span class="hljs-number">1000</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol = <span class="hljs-string">'SIPO'</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> order_id, account_id, bank_to, amount <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol = <span class="hljs-string">'SIPO'</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> order_id <span class="hljs-keyword">as</span> <span class="hljs-string">'OrderID'</span>, account_id <span class="hljs-keyword">as</span> <span class="hljs-string">'AccountID'</span>, bank_to <span class="hljs-keyword">as</span> <span class="hljs-string">'DestinationBank'</span>, amount  <span class="hljs-keyword">as</span> <span class="hljs-string">'Amount'</span>
<span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol = <span class="hljs-string">'SIPO'</span>;</span>

</code></pre>
<pre><code class="has-line-data" data-line-start="20" data-line-end="35" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> *, amount-payments <span class="hljs-keyword">as</span> balance
<span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> loan_id, account_id, <span class="hljs-built_in">date</span>, <span class="hljs-keyword">duration</span>, <span class="hljs-keyword">status</span>, amount-payments <span class="hljs-keyword">as</span> balance
<span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> loan_id, account_id, <span class="hljs-built_in">date</span>, <span class="hljs-keyword">duration</span>, <span class="hljs-keyword">status</span>, (amount-payments)/<span class="hljs-number">1000</span> <span class="hljs-keyword">as</span> <span class="hljs-string">'balance in Thousands'</span>
<span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-comment">-- this is the modulus operator that gives the remainder. This is a dummy example:</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">duration</span>%<span class="hljs-number">2</span>
<span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-number">10</span>%<span class="hljs-number">3</span>;</span>
</code></pre>
<blockquote>
<p class="has-line-data" data-line-start="35" data-line-end="36">These comparison operators are used with the <code>WHERE</code> clause, for filtering data:</p>
</blockquote>
<pre><code class="has-line-data" data-line-start="38" data-line-end="48" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span>;</span>
<span class="hljs-comment">-- In this case status B is for those clients where the contract has finished but the loan is not paid yet</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> <span class="hljs-keyword">in</span> (<span class="hljs-string">'B'</span>,<span class="hljs-string">'b'</span>);</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> <span class="hljs-keyword">in</span> (<span class="hljs-string">'B'</span>,<span class="hljs-string">'b'</span>) <span class="hljs-keyword">and</span> amount &gt; <span class="hljs-number">100000</span>;</span>
</code></pre>
<pre><code class="has-line-data" data-line-start="49" data-line-end="60" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>

<span class="hljs-comment">-- to get the bottom rows of a table, there is no predefined function</span>
<span class="hljs-comment">-- but you can sort the results in descending order and then use the LIMIT function</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">account</span>
<span class="hljs-keyword">order</span> <span class="hljs-keyword">by</span> account_id <span class="hljs-keyword">desc</span>
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>
<span class="hljs-comment">-- In this case, we were able to do it because the data was arranged</span>
<span class="hljs-comment">-- in ascending order of the account_id</span>
</code></pre>
<pre><code class="has-line-data" data-line-start="61" data-line-end="72" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>

<span class="hljs-comment">-- to get the bottom rows of a table, there is no predefined function</span>
<span class="hljs-comment">-- but you can sort the results in descending order and then use the LIMIT function</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">account</span>
<span class="hljs-keyword">order</span> <span class="hljs-keyword">by</span> account_id <span class="hljs-keyword">desc</span>
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>
<span class="hljs-comment">-- In this case, we were able to do it because the data was arranged</span>
<span class="hljs-comment">-- in ascending order of the account_id</span>
</code></pre>
<pre><code class="has-line-data" data-line-start="75" data-line-end="103" class="language-sql"><span class="hljs-comment">-- two conditions applied on the table</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span> <span class="hljs-keyword">and</span> amount &gt; <span class="hljs-number">100000</span>;</span>

<span class="hljs-comment">-- we can have as many conditions as we need</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span> <span class="hljs-keyword">and</span> amount &gt; <span class="hljs-number">100000</span> <span class="hljs-keyword">and</span> <span class="hljs-keyword">duration</span> &lt;= <span class="hljs-number">24</span>;</span>
<span class="hljs-comment">--</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span> <span class="hljs-keyword">or</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'D'</span>;</span>
<span class="hljs-comment">-- Status B and D are the clients that were bad for business for the bank</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.loan
<span class="hljs-keyword">where</span> (<span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span> <span class="hljs-keyword">or</span> <span class="hljs-keyword">status</span> =<span class="hljs-string">'D'</span>) <span class="hljs-keyword">and</span> amount &gt; <span class="hljs-number">200000</span>;</span>

<span class="hljs-comment">-- logical NOT operator - it negates the boolean expression that we are evaluating</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> <span class="hljs-keyword">not</span> k_symbol = <span class="hljs-string">'SIPO'</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> *
<span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> <span class="hljs-keyword">not</span> k_symbol = <span class="hljs-string">'SIPO'</span> <span class="hljs-keyword">and</span> <span class="hljs-keyword">not</span> amount &lt; <span class="hljs-number">1000</span>;</span>
</code></pre>
<pre><code class="has-line-data" data-line-start="104" data-line-end="119" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> order_id, <span class="hljs-keyword">round</span>(amount/<span class="hljs-number">1000</span>,<span class="hljs-number">2</span>)
<span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>

<span class="hljs-comment">-- checking the number of rows in the table, both methods give the same result</span>
<span class="hljs-comment">-- given that there are no nulls in the column in the second case:</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">count</span>(*) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">count</span>(order_id) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">max</span>(amount) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">min</span>(amount) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">floor</span>(<span class="hljs-keyword">avg</span>(amount)) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">ceiling</span>(<span class="hljs-keyword">avg</span>(amount)) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>
</code></pre>
<p class="has-line-data" data-line-start="121" data-line-end="122">String Functions&lt;/summary&gt;</p>
<pre><code class="has-line-data" data-line-start="124" data-line-end="138" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">length</span>(<span class="hljs-string">'himanshu'</span>);</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *, <span class="hljs-keyword">length</span>(k_symbol) <span class="hljs-keyword">as</span> <span class="hljs-string">'Symbol_length'</span> <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *, <span class="hljs-keyword">concat</span>(order_id, account_id) <span class="hljs-keyword">as</span> <span class="hljs-string">'concat'</span> <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>;</span>

<span class="hljs-comment">-- formats the number to a form with commas,</span>
<span class="hljs-comment">-- 2 is the number of decimal places, converts numeric to string as well</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> *, <span class="hljs-keyword">format</span>(amount, <span class="hljs-number">2</span>) <span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> *, <span class="hljs-keyword">lower</span>(A2), <span class="hljs-keyword">upper</span>(A3) <span class="hljs-keyword">from</span> bank.district;</span>
<span class="hljs-comment">-- It is interesting to note that select lower(A2), upper(A3), * from bank.district; doesn't work</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> A2, <span class="hljs-keyword">left</span>(A2,<span class="hljs-number">5</span>), A3, <span class="hljs-keyword">ltrim</span>(A3) <span class="hljs-keyword">from</span> bank.district;</span>
<span class="hljs-comment">-- Similar to ltrim() there is rtrim() and trim(). And similar to left() there is right()</span>
</code></pre>
<p class="has-line-data" data-line-start="138" data-line-end="140">In the first table, the column <code>date</code> is of type integer. So we will convert the column into date format.<br>
For now, this change will only be temporary as we not altering the structure of the table where the column has been defined.</p>
<pre><code class="has-line-data" data-line-start="142" data-line-end="144" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> account_id, district_id, frequency, <span class="hljs-keyword">convert</span>(<span class="hljs-built_in">date</span>,<span class="hljs-built_in">date</span>) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">account</span>;</span>
</code></pre>
<p class="has-line-data" data-line-start="145" data-line-end="147">In the function, <code>convert()</code>, the first argument is the name of the column and the second is the type to which you want to convert. Similarly, we can do it for the <code>loan</code> table:<br>
<code>select CONVERT(date,date) from bank.loan;</code>.</p>
<pre><code class="has-line-data" data-line-start="149" data-line-end="155" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> account_id, district_id, frequency, <span class="hljs-keyword">CONVERT</span>(<span class="hljs-built_in">date</span>,datetime) <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">account</span>;</span>

<span class="hljs-comment">-- next is a two step process:</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> substring_index(issued, <span class="hljs-string">' '</span>, <span class="hljs-number">1</span>) <span class="hljs-keyword">from</span> bank.card;</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">convert</span>(SUBSTRING_INDEX(issued, <span class="hljs-string">' '</span>, <span class="hljs-number">1</span>), <span class="hljs-built_in">date</span>) <span class="hljs-keyword">from</span> bank.card;</span>
</code></pre>
<p class="has-line-data" data-line-start="156" data-line-end="157">A list of formats can be found <a href="https://www.w3schools.com/sql/func_mysql_date_format.asp">here</a>.</p>
<pre><code class="has-line-data" data-line-start="159" data-line-end="165" class="language-sql"><span class="hljs-comment">-- converting the original format to the date format that we need:</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">date_format</span>(<span class="hljs-keyword">convert</span>(<span class="hljs-built_in">date</span>,<span class="hljs-built_in">date</span>), <span class="hljs-string">'%Y-%M-%D'</span>) <span class="hljs-keyword">from</span> bank.loan;</span>

<span class="hljs-comment">-- if we just want to extract some specific part of the date</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">date_format</span>(<span class="hljs-keyword">convert</span>(<span class="hljs-built_in">date</span>,<span class="hljs-built_in">date</span>), <span class="hljs-string">'%Y'</span>) <span class="hljs-keyword">from</span> bank.loan;</span>
</code></pre>
<p class="has-line-data" data-line-start="166" data-line-end="167"><strong>Logical Order Processing of Queries</strong></p>
<ol>
<li class="has-line-data" data-line-start="168" data-line-end="169">FROM</li>
<li class="has-line-data" data-line-start="169" data-line-end="170">ON</li>
<li class="has-line-data" data-line-start="170" data-line-end="171">JOIN</li>
<li class="has-line-data" data-line-start="171" data-line-end="172">WHERE</li>
<li class="has-line-data" data-line-start="172" data-line-end="173">GROUP BY</li>
<li class="has-line-data" data-line-start="173" data-line-end="174">WITH CUBE/ROLLUP</li>
<li class="has-line-data" data-line-start="174" data-line-end="175">HAVING</li>
<li class="has-line-data" data-line-start="175" data-line-end="176">SELECT</li>
<li class="has-line-data" data-line-start="176" data-line-end="177">DISTINCT</li>
<li class="has-line-data" data-line-start="177" data-line-end="178">ORDER BY</li>
<li class="has-line-data" data-line-start="178" data-line-end="179">TOP/LIMIT</li>
<li class="has-line-data" data-line-start="179" data-line-end="180">OFFSET/FETCH</li>
</ol>
<pre><code class="has-line-data" data-line-start="181" data-line-end="191" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.card
<span class="hljs-keyword">where</span> <span class="hljs-keyword">type</span> = <span class="hljs-string">'classic'</span>
<span class="hljs-keyword">order</span> <span class="hljs-keyword">by</span> card_id
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol = <span class="hljs-string">'SIPO'</span> <span class="hljs-keyword">and</span> amount &gt; <span class="hljs-number">5000</span>
<span class="hljs-keyword">order</span> <span class="hljs-keyword">by</span> order_id <span class="hljs-keyword">desc</span>
<span class="hljs-keyword">limit</span> <span class="hljs-number">10</span>;</span>
</code></pre>
<p class="has-line-data" data-line-start="194" data-line-end="195">&lt;summary&gt; Click for Code Sample: Null functions &lt;/summary&gt;</p>
<pre><code class="has-line-data" data-line-start="197" data-line-end="207" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">isnull</span>(<span class="hljs-string">'Hello'</span>);</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">isnull</span>(card_id) <span class="hljs-keyword">from</span> bank.card;</span>

<span class="hljs-comment">-- this is used to check all the elements of a column.</span>
<span class="hljs-comment">-- 0 means not null, 1 means null</span>
<span class="hljs-operator"><span class="hljs-keyword">select</span> <span class="hljs-keyword">sum</span>(<span class="hljs-keyword">isnull</span>(card_id)) <span class="hljs-keyword">from</span> bank.card;</span>

<span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol <span class="hljs-keyword">is</span> <span class="hljs-literal">null</span>;</span>
</code></pre>
<p class="has-line-data" data-line-start="208" data-line-end="209">As you might have noticed in this case, even though we see a lot of missing values in the column <code>k_symbol</code>, the above query does not filter those rows. It might be because those columns actually have value, for example, empty space. SQL considers that as a character/ value as well. So we will check for that now:</p>
<pre><code class="has-line-data" data-line-start="211" data-line-end="214" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> * <span class="hljs-keyword">from</span> bank.<span class="hljs-keyword">order</span>
<span class="hljs-keyword">where</span> k_symbol <span class="hljs-keyword">is</span> <span class="hljs-keyword">not</span> <span class="hljs-literal">null</span> <span class="hljs-keyword">and</span> k_symbol = <span class="hljs-string">' '</span>;</span>
</code></pre>
<p class="has-line-data" data-line-start="215" data-line-end="216">&lt;/details&gt;</p>
<p class="has-line-data" data-line-start="217" data-line-end="219">&lt;details&gt;<br>
&lt;summary&gt; Click for Code Sample: Case Statements &lt;/summary&gt;</p>
<p class="has-line-data" data-line-start="220" data-line-end="221">In the <code>loan</code> table, there’s column status A, B, C, and D. Using the case statement we will try to replace the values there with a brief description.</p>
<pre><code class="has-line-data" data-line-start="223" data-line-end="232" class="language-sql"><span class="hljs-operator"><span class="hljs-keyword">select</span> loan_id, account_id,
<span class="hljs-keyword">case</span>
<span class="hljs-keyword">when</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'A'</span> <span class="hljs-keyword">then</span> <span class="hljs-string">'Good - Contract Finished'</span>
<span class="hljs-keyword">when</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'B'</span> <span class="hljs-keyword">then</span> <span class="hljs-string">'Defaulter - Contract Finished'</span>
<span class="hljs-keyword">when</span> <span class="hljs-keyword">status</span> = <span class="hljs-string">'C'</span> <span class="hljs-keyword">then</span> <span class="hljs-string">'Good - Contract Running'</span>
<span class="hljs-keyword">else</span> <span class="hljs-string">'In Debt - Contract Running'</span>
<span class="hljs-keyword">end</span> <span class="hljs-keyword">as</span> <span class="hljs-string">'Status_Description'</span>
<span class="hljs-keyword">from</span> bank.loan;</span>
</code></pre>
