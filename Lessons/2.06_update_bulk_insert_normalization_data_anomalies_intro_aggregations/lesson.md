# Lesson 2.6: Normalization

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to use `INSERT`, `UPDATE`, and `DELETE` commands, and to familiarize students further with normalization. We will also talk about data anomalies in databases, then introduce aggregation functions and take a first look at using the `GROUP BY` clause.

---

### Setup

To start this lesson, students should have:

- Completed lesson 2.5
- All previous Setup

---

### Learning Objectives

After this lesson, students will be able to:

- Use `INSERT`, `UPDATE`, `DELETE` commands
- Apply Normalization methods
- Identify data anomalies
- Use `GROUP BY` and aggregations functions

---

### Lesson 1 key concepts



- DELETE vs DROP commands
- Bulk insert from a CSV file

<details>
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

```sql
delete from district_demo;
```

</details>

<details>
<summary> Click for Code Sample: INSERT </summary>


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

</details>

---



---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_2.06_activities/blob/master/2.06_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution]().

</details>

---



---

### Lesson 2 key concepts



- `UPDATE` command
- Introduction to _normalization_
- Different normalization forms
- Why normalization is important?

<details> 
  <summary> Click for Code Sample: UPDATE </summary>

```sql
update district_demo
set A4 = 0, A5 = 0, A6 = 0
where A2 = 'Kladno';
```

:exclamation: Note to instructor: Now we will go back to our original database.

</details>

#### Normalization

- Normalization is the process of efficiently organizing data in the database. It is used to **reduce redundancy** and **improve data integrity**. Redundant data is the unnecessary reoccurring data in the database (repeating groups of data) whereas data integrity ensures the quality of data in the database.

- The database designer should structure the data in such a way that eliminates unnecessary duplication(s) and provides a faster search path to all necessary information. This process of specifying and defining tables, keys, columns, and relationships to create an efficient database is called _normalization_.

- If the database is not correctly normalized, it can result in the following issues:

      - Redundant data/repetition of information
      - Inability to represent certain information
      - Difficulty to maintain information/data anomalies

- Not only does data redundancy cause the waste of computer space, but data redundancy also causes data anomalies such as insertion anomaly, deletion anomaly, and update anomaly. We will talk about it in the next session more.

#### Normalization Forms

- There are different normalization forms, which represent the degree to which a database table has been normalized. The higher the form, the better it is. Here are some normalization forms:

      - 1NF: First normal form
      - 2NF: Second normal form
      - 3NF: Third normal form
      - BCNF: Boyce-Cod normal form


<details>
TBD :
</details>

---



---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_2.06_activities/blob/master/2.06_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Class Discussion

</details>

---



---

### Lesson 3 key concepts



Data Anomalies (with an example of a poorly normalized database)

- Insert Anomaly
- Update Anomaly
- Delete Anomaly

> As we had mentioned earlier, data redundancy not only wastes storage space, but also causes data anomalies such as insertion anomaly, deletion anomaly, and update anomaly. Here we will use the example in the image and discuss the three anomalies.
> </br>

![Data Anomalies](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/2.6-data_anomalies_example.png)

- **Insert Anomaly**:

  _An insertion anomaly occurs when we want to add a new record to a relation but not all the data are available to allow us to add the new record._ Suppose that we want to add a new pharmacy with the following values of `PharmacyID`, `PharmacyName`, `Address`, and `Phone—PH3`, `PC`, `PCC`, and `P333`, respectively—to the `PrescriptionFilling` relation. Because this is a new Pharmacy and there is no prescription filled by the Pharmacy yet, there are no data of prescriptions for this pharmacy. We can give only the NULL value for the `PrescriptionID`, `MedicineID`, `MedicineName`, `Description`, `Quantity`, `LastPickupDate`, and `RefillFrequency` attributes of the `PrescriptionFilling` relation. However, `PrescriptionID` and `MedicineID` are the primary keys and can't have the NULL value. Hence, we can't add a new record for this new pharmacy to the `PrescriptionFilling` relation because we don't have a specific value for the primary key of this new record.

- **Update Anomaly**:

  _An update anomaly occurs when an update of an attribute value needs to be made at multiple places._ For example, if the name of the pharmacy, `PH1`, is changed, this change has to be made at multiple places in the first three data records.

- **Delete Anomaly**:

  _A deletion anomaly occurs when the deletion of a record leads to a loss of data._ For example, if we delete two prescriptions, `PR1` and `PR2`, filled by the pharmacy, `PH1`, by deleting the first three data records, data of the pharmacy, PH1, and two medicines, `M2` and `M3`, are removed from the relation, causing the undesired data loss.

- Revisiting **OLTP** (highly normalized) and **OLAP** (de-normalized)

:exclamation: Note to instructor: We talked about _OLTP_ and _OLAP_ processes in UNIT 2. Here, just quickly go over the difference in the process between _OLTP_ and _OLAP_. It is important to emphasize that the _OLTP_ systems are designed to ingest data from a very number of transactions while _OLAP_ tables are de-normalized and are simplified to store larger amounts of information for reporting and analysis.

<details>
<summary> Click for Code Sample </summary>

```sql
TBD code
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz


<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_2.06_activities/blob/master/2.06_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution]().

</details>

---


---

### Lesson 4 key concepts



- Aggregation functions available in SQL (`avg()`, `sum()`, `min()` , `max()`, and `count()`)
- Writing simple queries with aggregation functions

<details>
<summary> Click for Code Sample: Simple Aggregations </summary>
 
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

</details>

---

### :pencil2: Practice on key concepts - Lab

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-6](https://github.com/ironhack-labs/lab-sql-6)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---
