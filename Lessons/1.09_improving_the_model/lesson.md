# Lesson 1.9: Improving the model

### Lesson Duration: 3 hours

> Purpose: The purpose of this DIY (Do It Yourself) lesson is to go through the complete process of building the model and using different techniques to improve the accuracy of the model. As George Box famously said, "All models are wrong but some are useful".
> Students will also understand what the git is and how to use GitHub. As a final learning for today, students will realise the importance of delivery and presentation of the analysis/findings from the data analysis journey.

 The `sqlalchemy` is the library/database toolkit for Python for database connections and `PyMySQL` is the driver. We have different drivers based on the type of database we are trying to connect to.

### Learning Objectives

After this lesson, students will be able to:

- Apply a linear regression model from the beginning
- Improve the accuracy of the model
- Collaborate using Git and GitHub
- Deliver impactful presentations

- Install sequelPro/MySQL Workbench (if not installed)
- Load SQL database
- Install `sqlalchemy` and `PyMySQL`

---

### Lesson 1 key concepts


- Revisiting the model
- List down the followed steps
- Spot the areas in the model where changes could be made
- Apply the changes
- Fit the model and check the accuracy
- Compare the changes in the different models



Some changes that could be suggested include different data processing techniques such are:

    - Choosing a different way to fill null values
    - Working with a categorical variable to reduce the number of categories
    - Different data transformation
    - A different method of remove outliers
    - Choosing different scaling method
---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-customer-analysis-round-7](https://github.com/ironhack-labs/lab-customer-analysis-round-7)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---

- Quick recap to version control systems: Why git and Github?
- Managing a repository on GitHub (quick recap - already discussed on day 1):
  - Create a repo
  - Make changes and commit to a repo
- Forking and cloning a repo
  - Fork vs. clone
  - Create a pull request

</details>

---
---

#### :pencil2: Check for Understanding - Class activity/quick quiz


<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Fist to five

How everyone is doing with git and GitHub?

</details>

---



- Working with branches
- Resolving merge conflicts
- Adding large files on GitHub
- Initializing directories on personal computer as GitHub repos

<details>
<summary> Click for Code Sample </summary>

```shell
$ git branch 	                        # shows the current branches in the repo
$ git branch -a 	                    # shows all branches (even the ones you haven't worked on)
$ git checkout -b <NameOfNewBranch>	  # creates new branch
$ git checkout <BranchName> 	        # switches to the branch we want to work on
$ git pull 	                          # pulls the latest changes to the branch we are working on (git pull = git fetch + git merge)
$ git fetch 	                        # gets all branches from the repository
```

# Merging Branches

Case 1: Merges changes in branch to the master file

```shell
$ git checkout master
$ git merge <Name of Branch to be merged to Master>
```

Case 2: Merges changes in master file to the branch

```shell
$ git checkout <branch name>
$ git merge master
```

</details>

---


---

### Lesson 3 key concepts



- Presentations/reporting the results of your analysis

  - Create visually appealing presentations
  - Understanding your audience/key stakeholders
  - Demonstrate business acumen/strong business knowledge
  - Creating a story to answer business questions
  - Deliver actionable insights and business insights
  - "Everything should be made as simple as possible, but not simpler"

---

#### :pencil2: Check for Understanding - Class activity/quick quiz



<details>
  <summary> Click for Instructions: Activity 2 </summary>

**Daisy chain**

Check the full analysis process, step by step.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

1. Problem (case study)
2. Getting Data
3. Cleaning/Wrangling/EDA
4. Processing Data
5. Modeling
6. Model Validation
7. Reporting

</details>

---



---

### :pencil2: Practice on key concepts - Lab



<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-customer-analysis-final-round](https://github.com/ironhack-labs/lab-customer-analysis-final-round)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution]().

</details>

---

### Additional Resources

- [Best practices for PowerPoint presentations](https://alum.mit.edu/best-practices-powerpoint-presentations)
- [More tips on PowerPoint formatting](https://www.workfront.com/blog/10-tips-for-designing-presentations-that-dont-suck-part-1)
