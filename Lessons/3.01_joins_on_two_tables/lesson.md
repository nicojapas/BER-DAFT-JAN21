# Lesson 3.1: Keys & Joins

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into the concept of primary keys and foreign keys in database tables. We will use them to create joins between two or more tables and use inner joins and extract relevant information from the database. Students will also learn about different forms of relationships between different tables in a database.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Use inner joins and outer joins (left outer and right outer) on two tables to extract information
- Interpret relationships between tables in a database
- Explain the importance of primary keys and foreign keys
- List the properties of primary keys and foreign keys

---

### Lesson 1 key concepts

> :clock10: 20 min

- Simple query using inner join to extract information from two tables
- Simple query using left outer join to extract information from two tables
- Simple query using right outer join to extract information from two tables

---

<details>
  <summary> Click for Code Sample: Inner Join </summary>

- Write a query to extract information from the `loan` and the `account` tables to get information for all accounts.

```sql
select * from bank.loan l
join bank.account a
on l.account_id = a.account_id;

select l.loan_id, l.account_id, l.amount, l.payments, a.district_id, a.frequency, a.date
from bank.loan l
join bank.account a
on l.account_id = a.account_id;

select * from bank.account a
join bank.loan l
on a.account_id = l.account_id;
```

:exclamation: Note to instructor: You will notice the first and the last query have the same output. Explain to the students why the results are the same because of the way that inner join works.

</details>

<details>
  <summary> Click for Code Sample: Left Outer Join </summary>

```sql
select * from bank.loan l
left join bank.account a
on l.account_id = a.account_id;

select * from bank.account a
left join bank.loan l
on a.account_id = l.account_id;
```

:exclamation: Note to instructor: You will notice there is a big difference in the output of these queries. Explain to the students why the results are different because of the way that inner join works.

</details>

<details>
  <summary>Click for Code Sample: Right Outer Join </summary>

- In the next two queries we have just changed the keyword from 'left' to 'right'.

```sql
select * from bank.account a
right join bank.loan l
on a.account_id = l.account_id;

select * from bank.loan l
right join bank.account a
on l.account_id = a.account_id;
```

:exclamation: Note to instructor: You will notice there is a big difference in the output of these queries. Explain to the students why the results are different because of the way that inner join works.

</details>

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.01_activities/blob/master/3.01_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/9e1a1d78d131c6743809812b958a6b3c).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts (use ppt intro to sql)

> :clock10: 20 min

- Revisiting ER Diagrams
- Degrees of Relationships in an ER Model

      - One to One
      - One to Many
      - Many to Many

> :exclamation: Note: A relationship represents an association between entities. The number of entity types involved in a relationship is called the _"degree"_ of a relationship. Given entity `A`, entity `B`, and their relationship `R`, the maximum cardinality of relationship (`R`) describes the maximum number of entity `B` instances that can be associated with any instance of entity `A` and vice-versa. There are only two values for the maximum cardinality: one or many (more than one).

> The minimum cardinality of relationship (`R`) between entities `A` and `B` describes the minimum number of entity `B` instances that can be associated with any instance of entity `A` and vice versa. There are only two values for the minimum cardinality: mandatory or optional. The _mandatory_ value means that the minimum number of instances is at least 1. The _optional_ value means that the minimum number of instances is 0.

![Degrees of Relationship](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/3.1-vert_degrees_of_relationships.png)

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.01_activities/blob/master/3.01_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/32413c0a66a23dd9ebb4fd4d5bec583d).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Significance of primary key and its properties
- Significance of foreign keys and its properties
- Why are they important

:exclamation: Notes: **Primary keys** are used to uniquely identify every record in the table. By definition, they are _UNIQUE_ and _NOT NULL_. They are used to establish relationships between tables, enforcing the data integrity constraints on the database as well. Primary keys also help with indexing which in turn helps to speed up queries, searches and sorting through the data. Primary keys result in _CLUSTERED_ unique indexes by default. We will talk about clustered and non-clustered indexes later.
Here are some of the properties of primary keys:

- There can only be one primary key for a table.
- The primary key consists of one or more columns.
- The primary key enforces the entity integrity of the table.
- The primary key uniquely identifies a row.

:exclamation: Notes: **Foreign keys** are the columns of a table that points to the primary key of another table so as to provides a link between data in two tables. The table containing the foreign key can be referred to as the child table, and the table containing the candidate key can be referred to as the referenced or parent table

Here are some of the properties of foreign keys:

- A relational FK references a relational PK
- A FK's referencing column types agree with corresponding referenced column types
- It should protect the referential integrity of the data ie a FK's referencing column values must appear as corresponding referenced column values

Why are they important? Given the snapshot below, observe and explain what might happen if we do not have references of keys from other tables as foreign keys in the table shown below? Here is the image below.

![Why are PF and FK important](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/3.1-foreign_key_referential_integrity_broken.png)

If we do not have the foreign keys then we will have to include all the data from the other tables into the same table. This would create a lot of data redundancy. Such issues are taken care of by an important concept called Normalization. We will introduce this concept in greater detail later this week. It is important to emphasize to the students, that designing a database is the task of data engineers but to write an efficient join queries, it is good to understand these these concepts.

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.01_activities/blob/master/3.01_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/ce22f0481ed2ab6a2917d48edc3c7af0).

</details>

---

### Lesson 4 key concepts

> :clock10: 20 min

- More examples on inner join to work with two tables
- More examples on left outer join to work with two tables

<details>
  <summary> Click for Code Sample: Inner Joins </summary>

- Write a query to extract information from the `client` and the `district` tables to get information for all the clients of the city and region they are from.

```sql
-- Step 1
select * from bank.client c join district d
on c.district_id = d.A1;

-- Step 2
select c.client_id, c.birth_number, c.district_id, d.A2, d.A3
from bank.client c join bank.district d
on c.district_id = d.A1;
```

</details>

<details>
  <summary> Click for Code Sample: Outer Joins </summary>

Write queries to extract information about the accounts:

a) returning `account_id`, `operation`, `frequency`, sum of amount, sum of balance,
b) where the balance is over 1000,
c) `operation` type is `VKLAD` and
d) that have an aggregated amount of over 500,000.

```sql
-- step 1
select * from bank.trans t
left join bank.account a
on t.account_id = a.account_id;

-- step 2
select * from bank.trans t left join bank.account a on t.account_id = a.account_id
where t.operation = 'VKLAD' and balance > 1000;

-- step 3
select t.account_id, t.operation, a.frequency, sum(amount) as TotalAmount, sum(balance) as TotalBalance
from bank.trans t left join bank.account a on t.account_id = a.account_id
where t.operation = 'VKLAD' and balance > 1000
group by t.account_id, t.operation, a.frequency;

-- step 4
select t.account_id, t.operation, a.frequency, sum(amount) as TotalAmount, sum(balance) as TotalBalance
from bank.trans t left join bank.account a on t.account_id = a.account_id
where t.operation = 'VKLAD' and balance > 1000
group by t.account_id, t.operation, a.frequency
having TotalAmount > 500000
order by TotalAmount desc
limit 10;
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>
  
- Link to [activity 4](https://github.com/ironhack-edu/data_3.01_activities/blob/master/3.01_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/307fa612d1ffc6cddc223337628138e5).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-join](https://github.com/ironhack-labs/lab-sql-join)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/68cf9e79c27cd1300200c2ab2c1f675c).

</details>

---

### Additional Resources

- [Relationships between tables](https://www.linkedin.com/pulse/importance-foreign-key-constraint-tim-miles/)
