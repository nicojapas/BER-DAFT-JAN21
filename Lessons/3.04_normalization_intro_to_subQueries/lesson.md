# Lesson 3.4: Normalization & Intro to subqueries

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to dive deeper into the concept of normalization - functional dependencies, different forms of normalization, and to understand that normalization of a database is an important database design principle. We will also introduce simple subqueries.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Use normalization for better database design
- Analyze functional dependencies in tables
- Apply different methods for normalization
- Write simple subqueries

:exclamation: Note to instructor: Let the students know that this is going to be a very brief introduction to subqueries. We will talk more in detail about that topic in the next sessions.

:exclamation: Note to instructor: Since most of the topics in this half of the day are more theoretical (lectures/discussions), during the activities we would ask the students to write queries to extract information based on what they have learned in the previous lessons.

### Lesson 1 key concepts

> :clock10: 20 min

:exclamation: Note to instructor: We have already discussed the concept of normalization in week 2, so this could be more like a discussion/recap where you can include students.

:exclamation: Note to instructor: We will focus more on the concept of functional dependencies and how that is used to normalize the database. Also emphasize that we are not going to get into too many details about normalization as it is usually the work of data engineers and database designers/architects, but this is only to provide some foundational knowledge on the subject.

- Revisit normalization, importance of normalization in database design
- How not so well normalized database can impact the performance
- Introduce functional dependencies in a database

If the database is not correctly normalized, it can result in the following issues:

- Redundant data/repetition of information
- Inability to represent certain information
- Difficulty to maintain information/data anomalies

> Not only does data redundancy cause the waste of computer space, but it also causes data anomalies such as insert, deletion and update anomaly. We will talk about it in the next session more.

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_3.04_activities/blob/master/3.04_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/73d77cdfb16ed629826f24e26a14d199).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

:exclamation: Note for instructor: Use this [document](files_for_lesson_and_activities/normalization_and_functional_dependencies.md). Students have access to the same file in the cloned repository with activities.

> :clock10: 20 min

- Functional dependencies

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_3.04_activities/blob/master/3.04_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/5beddaa4def3c9ea3fb67ffd3cdb3d83).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

Process of normalization. Different forms of normalization:

- `1NF`: First normal form
- `2NF`: Second normal form
- `3NF`: Third normal form
- `BCNF`: Boyce-Codd normal form

When you normalize a database, you start from the general and work towards the specific, applying certain tests along the way. It means decomposing (dividing/breaking down) a "big" un-normalized table (file) into several smaller tables by:

    - Eliminating insertion, update and delete anomalies
    - Establishing functional dependencies
    - Removing transitive dependencies
    - Reducing non-key data redundancy

A relation is in `1NF` if each cell of the relation contains only one value ie each table contains all atomic data items, and also no repeating groups, and a designated primary key (no duplicated rows).

[!1NF](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/3.4-1NF.png)

:exclamation: Note to instructor: If the instructor considers that it is important to explain different normalization forms, they can use the provided resources. But, the instructor can also ask the interested students to refer the resources themselves after giving a brief intro about functional dependencies and come back with questions if they have any. Please refer to the `files_for_lesson_and_activities/normalization_and_functional_dependencies.md` file).

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_3.04_activities/blob/master/3.04_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions </summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/69c0c45190fb47d7faef593353a138a8).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

:exclamation: Note to instructor: In this section, we will just introduce the concept of subqueries and show an example of _self-contained subquery_. Correlated subqueries will be covered in the later sessions.

Introduction to subqueries

- Self-contained subqueries
- Correlated subqueries
- Using the results of another query as a table in a database

Using subqueries is a convenient capability in the language when you want one query to operate on the result of another, and if you prefer not to use intermediate objects like variables for this purpose. Primarily there are two kinds of subqueries:

- `self-contained` - queries that are independent of the outer query.
- `correlated` - queries that have references (correlations) to columns of tables from the outer query.
- [OPTIONAL] `scalar` - queries that return a single value and that are allowed where a single-valued expression is expected.

One of the advantages of self-contained subqueries compared to correlated ones is the ease of troubleshooting. You can always copy the inner query to a separate window and troubleshoot it independently. When you are done troubleshooting and fixing what you need, you can paste the subquery back in the host query. Also, important to is that `self-contained` (`uncorrelated`) subqueries _run only once_ for the entire query.

<details>
  <summary> Click for Code Sample </summary>

Lets use the `loan` table from the `bank` database. We want to identify the customers who have borrowed amount which are more than the average amount of all customers. This would not be possible to achieve through simple queries that have used before (without using variables which we will take a look at, later in the course). For this we will use a subquery.

```sql
-- step 1: calculate the average
select avg(amount) from bank.loan;

-- step 2 --> pseudo code the main goal of this step ....
select * from bank.loan
where amount > "AVERAGE";

-- step 3 ... create the query
select * from bank.loan
where amount > (
  select avg(amount)
  from bank.loan
);
```

```sql
-- step 4 - Prettify the result. Let's find top 10 such customers
select * from bank.loan
where amount > (select avg(amount)
from bank.loan)
order by amount desc
limit 10;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_3.04_activities/blob/master/3.04_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/1d57f70d75d7877f1a3a7097d465881b).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-database-normalization](https://github.com/ironhack-labs/lab-database-normalization)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/e71d52a89d768e99984cb07f4a41374d).

</details>

---

### Additional Resources

- [Normalization](https://condor.depaul.edu/gandrus/240IT/accesspages/normalization3.htm)
