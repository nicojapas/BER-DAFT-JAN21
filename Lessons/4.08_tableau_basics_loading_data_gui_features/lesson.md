# Lesson 4.8: Data Analysis with Tableau

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to get familiar with the interface, explore the different features available, understand some key elements while using Tableau including measures, dimensions, the canvas options. We will also start conducting some preliminary data analysis with the tool.

---

### Setup

- All previous set up
- Tableau installed

### Learning Objectives

After this lesson, students will be able to:

- Differentiate: Sheets vs. Stories vs. Dashboards
- Use the data source interface
- Manipulate fields in Tableau, use Tableau canvas and _Show Me_ tab
- Manage measures and dimensions
- Edit and add more levels of details to the visualizations
- Work with aggregated and non aggregated measures

---

### Lesson 1 key concepts

> :clock10: 20 min

- Discuss different sheets/views/features available

  - Sheets for different visualizations
  - Stories
  - Dashboards

- Data source interface

  - A table view, list view and arranging columns in an order
  - Adding filters to the data source
  - Hiding/showing (unhiding) columns
  - Understanding data types in Tableau
  - Merging files in Tableau (dummy example)

:exclamation: Note for the instructor: We will use the `files_for_lesson_and_activities/finalMergedFile.csv` file for the analysis in the following lessons. The instructor can show how to create joins in Tableau directly using the original files as a dummy example. We have already processed the data using Python, which will be used for analysis here.

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Import `files_for_lesson_and_activities/final_merged.csv` into Tableau.
- Explore the Tableau data interface.
- Filter out customers with less than 3 years in the company.
- Add a new column that flags customers older than 80 years old.
- Add a new column that flags customers with a balance higher than 6M.

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_8_2).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Create the first sheet in Tableau
- Fields as Measures and Dimensions in Tableau
- Tableau canvas (picture added)
  - Data window
  - Shelves
  - Canvas
  - Toolbar and ribbons
- Show me tab
- First visualization in Tableau (ask the students to drag and drop measures and dimensions in rows and columns and play around with the interface)

<details>
  <summary> Click for Description: Measures and dimensions </summary>

- Fields are broken up into Dimensions and Measures.

  - Dimensions (blue) are categorical fields. They are the labels in a visualization, the buckets that data falls into such as locations, product names, etc.
  - Measures (green) are quantitative fields. They are the axes in a visualization, the numbers that can be analyzed, such as price and counts of records.

- Note: If the data set contains geographic fields, such as country or city, Tableau searches an internal database and generates Latitude and Longitude fields. This enables the geographic data to be plotted on a map.

- The number of Records is a simple count of rows in the data set.

</details>

<details>
  <summary> Click for Description: Tableau Canvas </summary>

![tableau canvas](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.8-tableau_canvas.png)

Tableau canvas

- **Data window** – **purple** – drag fields from here to bring them into the view, either data fields or, if you’re on the Analytics tab, items like reference lines and box plots.
- **Shelves** – **blue** – areas where fields can be placed to control exactly how they appear in the view.
- **Canvas** – **green** – where the visualization is built. Fields can be placed directly here as well as on shelves.
- **Toolbar and ribbon** – **orange** – additional controls and menu options, including Undo and Clear Sheet.

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Explore the Tableau interface. Create some charts

      - Barplot showing the number of records by variation?
      - Add a filter on field `process_step` and filter only `start` records.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_8_2).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Managing measures and dimensions
- Plot Num Accts vs. Balance
- Aggregate measures (sum, averages, counts, etc.)
- Arranging data in an ascending or descending order
- Switching rows and columns
- Using the marks shelf to add more level of details
  - Color
  - Labels
  - Detail
  - Tooltip

<details>
  <summary> Click for Code Sample:  </summary>

![managing_measures_and_dimensions](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.8-managing_measures_and_dimensions.png)

![aggregate_Measures](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/4.8-aggregate_measures.pngs)

</details>

---

:coffee: **BREAK**

---

### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Create an scatter plot plotting **client age** vs. **balance**.
- Color marks by **gender** and make their size proportional to **number of accounts**.

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_8_2).
- Link to the tableau public page [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_8_15973499916390/scatter_plot?publish=yes).

</details>

### Lesson 4 key concepts

> :clock10: 20 min

- Adding filters
- Add notes to the plot using captions (right-click in an open area on the right)
- Add a title to the visualization and its format
- Color coding sheets
- Use the show me tab to convert visualization
- Building scatter plot (in the toolbar, go to 'Analysis' tab and uncheck aggregate measures)
- Discuss the Analytics tab in the data window

<details>
  <summary> Click for Tableau Public:  </summary>

- Link to the tableau public page [here](https://public.tableau.com/profile/himanshu.aggarwal2552#!/vizhome/lesson_4_8_15973499916390/scatter_plot?publish=yes).

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Reproduce the plot
- Annotate the customer with the highest balance
- Color differently those with a balance higher than 6M
- Use a different marker for man and women

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Lessons/lesson4_8_2).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

![Ironhack logo](https://i.imgur.com/1QgrNNw.png)

# Lab | Dashboards with Tableau

Using previously saved `unit-4-lab.tbwx`:

1. Create a sheet with a barplot of the number of customers per **Gender**.
2. Create a sheet with a barplot of the number of customers per **EmploymentStatus** and **Gender**.
3. Create a sheet with a treeplot of the number of customers per **State**.
4. Create a cross table with Marital status and Gender.
5. Create a Dashboard with all the Sheets created until now.

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- See [public tableau for solution](https://public.tableau.com/profile/alabarga#!/vizhome/Unit4-Labs/lab4_8_1).

</details>

---

### Additional Resources
