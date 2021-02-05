# Lesson 2.1: Intro to Databases & SQL

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to make students aware of the different data sources that data analysts come across, how to write simple queries in SQL, and extract information from SQL databases. Students will also be introduced to the case study (case study related to the _Banking_) that we will be using this and the following weeks.

---

### Setup

- Sequel Pro - For students with Mac
- MySQL Workbench - For students with Windows

### Learning Objectives

After this lesson, students will be able to:

- Identify the different data storage and retrieval tools and technologies for structured and unstructured data
- Explain the difference between database and data warehouse
- Connect to a SQL database and write simple queries in SQL to extract relevant information
- Develop business acumen for different scenarios

---

### Lesson 1 key concepts 

> [**Slides - Databases**](https://docs.google.com/presentation/d/1BIVhxnoca6v4mUf3kClUTWRPduLjID-uaVILdCzmHgU/edit?usp=sharing)

- Potential data sources available
- Tools and technologies used
- Structured vs. unstructured data

---

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.01_activities/blob/master/2.01_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution]().

</details>

---

---

### Lesson 2 key concepts (use ppt intro to SQL)


> [**Slides - Intro to SQL**](https://docs.google.com/presentation/d/1q-P3sxtKOaSWHf2V381RRY-mC-A2JOqD1KSjq8Db4I4/edit?usp=sharing)

- Introduction to relational databases
- Introduction to SQL
- Different standards used for querying (MySQL, T-SQL, PL/SQL) - a high-level overview

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.01_activities/blob/master/2.01_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution]().

</details>

---



---

### Lesson 3 key concepts

> The main objective to improve business by identifying customers that would be beneficial to the company (classification problem). Some other ad-hoc analyses that can be performed using SQL queries have been mentioned at the end of the document. More questions will be added later.

- Introduction to the case study database
- Connecting to SQL database (_workbench/Sequel Pro_)
- Structure of the database, information available in different tables
- Some business questions that we are looking to answer

### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_2.01_activities/blob/master/2.01_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions </summary>
  
- Link to [activity 3 solution]().

</details>

---

### Lesson 4 key concepts



- The basic structure of SQL queries
- Simple select queries
- Selecting columns and aliasing

```sql
use bank;
select * from trans;
select * from bank.trans;
```

It is a good practice to use the name of the database followed by the name of the table since, as an analyst, you might be working with multiple databases on a server.

```sql
-- to select some columns instead of all
select trans_id, account_id, date, type
from bank.trans;

-- to select some columns instead of all
select bank.trans.trans_id, bank.trans.account_id, bank.trans.date, bank.trans.type
from bank.trans;

-- aliasing columns
select trans_id as Transaction_ID, account_id as Account_ID, date as Date, type as Type_of_account
from bank.trans;

-- aliasing columns and table
select bt.trans_id as Transaction_ID, bt.account_id as Account_ID, bt.date as Date, bt.type as Type
from bank.trans as bt;

-- limiting the number of rows in the results
select bt.trans_id as Transaction_ID, bt.account_id as Account_ID, bt.date as Date, bt.type as Type
from bank.trans as bt
limit 10;

-- check the number of records in a table
select count(*) from bank.trans_id;
```

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_2.01_activities/blob/master/2.01_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions </summary>

- Link to [activity 4 solution]().

</details>

---


---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-intro-sql](https://github.com/ironhack-labs/lab-intro-sql)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---

### Additional Resources

- [Data warehouse schemas, Facts vs. dimensions table](http://gkmc.utah.edu/ebis_class/2003s/Oracle/DMB26/A73318/schemas.htm)
- [Data warehouses vs. Data Marts](https://www.talend.com/resources/what-is-data-mart/)
