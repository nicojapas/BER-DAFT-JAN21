# Lesson 4.9: Advanced Tableau

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce Tableau features such as _creating bins_, _creating calculated fields_, and use some quick table calculations and other user-defined table calculations. We will also talk about topics for projects students will be working the next week.

---

### Setup

- All previous set up

### Learning Objectives

After this lesson, students will be able to:

- Create bins from numerical columns/measures
- Create simple calculated fields using functions
- Use the `IF-ELSE` conditions to create user-defined buckets/bins
- Select your preferred project brief for a mid-bootcamp project

:exclamation: Note to instructor: After you explain the project and its objectives, please make sure that the students pick a project (out of the two choices available) by the end of the day. If they have time, they can also start working on the project. Also let them know that for this project, we have chosen the topics for them but for the final project, the students can choose a topic of their own choice. It would be important to emphasize that they should choose a project where they can showcase their skills well, learned during the bootcamp.

---

### Lesson 1 key concepts

> :clock10: 20 min

- Using more than one measure/dimension in the rows/columns
- Rearranging rows and columns in the data
- Working with the axes
- Using simple quick table calculation

<details>
  <summary> Click for Code Sample:  </summary>

- In this exercise, we are trying to compare Control and Test Variations along with the number of clients who were present in the different steps in the complete process.
- Please refer to the tableau public link [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_9_1/number_participants_by_variation-Step3).
- Instructions to obtain the plots are mentioned in the individual sheets
- Tableau files are also attached to the folder.

</details>

<details>
  <summary> Quick Table Calculation </summary>

![quick_table_calculation](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.9-quick_table_calculation.png).

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

Encourage the students to play around with the features to learn more. Some of the things that the students can do could be:

- Reproduce the plot.
- Switch rows and columns and check if the plot looks better visually.
- Add another measure along with the Count of clients.
- Switch Test to the left and Control to the right.
- Color code the data based on the process step.
- For the tip detail add some other piece of information. <!-- 🚨🚨🚨 @himanshu: what does this mean? -->

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_9_1a).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Table calculations

<details>
  <summary> Click for Code Sample:  </summary>

- In this exercise, we are trying to find the percent of dropouts in each step of the process/ percent of people who continued to the next stage. And we are trying to compare between Control and Test variations.
- Please refer to the tableau public link [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_9_2_table_calculations/percentage_drops3).
- Instructions to obtain the plots are mentioned in the individual sheets
- Tableau files are also attached to the folder.

:exclamation: Note for instructor: Encourage the students to play around with the features to learn more. The instructor should suggest some ideas for the students to try.

</details>

<details>
  <summary> Click for Instructions: Activity 2 </summary>

![table_calculations](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.9-table_calculations.png)

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Create a plot showing the number of confirmations per week.
- Plot a different line for each variation.
- Use Table calculations to show a running total of confirmations.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_9_2).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Create bins using the "create bin" option.
- Use the `IF-ELSE` conditional to create bins.

<details>
  <summary> Click for Description: Exercise 1  </summary>

- In the first exercise, we will create bins from the measure Client Age. We will see the distribution of how many people participated in each age group.
- Please refer to the tableau public link [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_9_3_calculated_fields_with_bins/createbinsbuckets).
- Tableau files are also attached to the folder.

:exclamation: Note for instructor: Encourage the students to play around with the features to learn more.

![creating_bins_age_column](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.9-creating_bins_age_column.png)

</details>

<details>
  <summary> Click for Description: Exercise 2  </summary>

- In the second exercise, we will use the `IF-ELSE` condition to create bins, and compare the average balances of each age group created.
- Please refer to the tableau public link [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_9_3_calculated_fields_with_bins/createbinsbuckets).
- Tableau files are also attached to the folder.

![age_buckets](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.9-age_buckets.png)

:exclamation: Note for instructors: Encourage the students to play around with the features to learn more. The instructor should suggest some ideas for the students to try there.

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Create bins for the **Balance** measurement.
- Compare the distribution of balance for male and female customers (you may need to filter out some records).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_9_3).

</details>

---

### Lesson 4: Mid-Bootcamp Project Intro

> :clock10: 20 min

- Introduce Mid-Bootcamp Project and Assessment

  - Intro & Discuss each project brief and its objectives
  - Discuss what is expected of the students: Deliverables and timeline
  - Tell them about the parameters on which the students will be graded: Evaluation Criteria and feedback structures

  - **Deliverable**

    - The students will be required to upload the project on git hub.
    - The repo should be properly organized with the proper use of folders and naming conventions for the folders
    - Make sure that the students update the Readme.md document in the repo describing the project, its purpose, and also briefly describing the methodology used to solve the problems

- Discuss the strategy that they can use to plan for the targets/goals for each day of the week
  - Introduce agile: values, kanban, and rituals.
  - Introduce some tools they can use to make their work more effective.
  - Introduce (lightly) the idea of learnings + next steps, retrospectives, MVPs, and scoping.

> Optional [Slides](https://docs.google.com/presentation/d/1zWa1rrhr8CxyLynC4nEBPnTBaxflUMo5YUzG1KtICf0/edit?usp=sharing) on agile and PM

:exclamation: Note for instructor: The students will use the next half of the day to learn more about the projects. By the end of the day, the students should tell the instructor or the TA about the project they have picked and the day by day plan for the next week.

<details>
  <summary> Project 1 Details:  </summary>

> Note: Please check the `Unit 05` folder for the details. Students should let the instructor/TA know which project they will choose by the end of the day.

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

# Lab | Advanced Tableau visualization

Using previously saved `unit-4-lab.tbwx`:

1. Create a sheet with a Choropleth map of the number of customers per **State**.
2. Make a regression plot of **Customer Lifetime Value** and **Income**.
3. Create a boxplot relating **Total Claim Amount** and **Vehicle Size**.
4. Create a Dashboard with these new charts.
5. Create a story to summarize your analysis.

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Labs/lab4_9_1).

</details>

---

### Additional Resources
