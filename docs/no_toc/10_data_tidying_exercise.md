
# Data Tidying Project

\* If you would like to know the answers to the questions in this exercise, then you can take this [course in Leanpub](https://leanpub.com/universities/courses/jhu/cbds-tidying).


Often times, data scientists are handed data and asked to make sense of them. The data scientist may be asked to figure out why people are getting sick in a city or how their employer can save money. No matter the situation, these data aren't always in the most usable format. **Data wrangling** is required before any pretty visualizations can be made or interesting questions can be answered.

As such, this exercise has been generated to practice your, GitHub, terminal navigation, RStudio, and data wrangling skills.

#### GitHub Setup

To get started, you'll want to go to GitHub and start a new repository:

- Call this repository `data_tidying_project`.
- Add a short description
- Check the box to "Initialize this repository with a README.
- Click `Create Repository`

Once the repository has been created, Click on `Clone or download` and copy the "Clone with HTTPS" link provided. You'll use this to clone your repo in RStudio Cloud.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/cbds-version-control). Refer to the materials in this course if you're stuck on this part of the project.

#### RStudio Cloud Setup

- Go to the [Cloud-based Data Science Space on RStudio Cloud](https://rstudio.cloud/spaces/20345/join?access_code=n4b8J1s0XmWctSy83%2BEXbGAwj7rKcuFMI7WJEJFD){target="_blank"}
- Click on the "Projects" tab at the top of the workspace
- Make a copy of the project: `data_tidying_project`

In this project you should see a `data_tidying_project.Rmd ` file. You'll use this to get started working on your project!

*Note*: If you try to Knit this document at this time, you *will* get an error because there is code in this document that has to be edited (by you!) before it will be able to successfully knit!

To start using version control, you'll want to clone the GitHub repository you just created into this workspace. To do so, go to the Terminal and clone your project into this workspace.

A new directory with the name of your GitHub repository should now be viewable in the Files tab of RStudio Cloud. You are now set up to track your project with git.

- Why did you clone your GitHub repository into RStudio Cloud?

#### Data Science Project Setup

As discussed previously, you'll want all your data science projects you be organized from the very beginning. Let's do that now!

First, use `cd` to get yourself into the directory of your GitHub Project.  

Once in the correct directory, use `mkdir` in the terminal to create folders with the following structure:

```
\- data/
  \- raw_data/
  \- tidy_data/
\- code/
  \- raw_code/
  \- final_code/
\- figures/
  \- exploratory_figures/
  \- explanatory_figures/
\- products/
```

Now that your directories are set up you'll want to use the Terminal (or 'More' drop-down menu in the Files tab) to move (`mv`) the `data_tidying_project.Rmd ` file into `code/raw_code`. This ensures that your code file is in the correct directory.

- In what directory did you save  `data_tidying_project.Rmd `?

Once the .Rmd document is in the correct folder, you'll want to change the author of this document to your name at the top of the .Rmd document (in the YAML). Save this change before moving to the next step.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Organizing Data Science Projects](https://leanpub.com/universities/courses/jhu/cbds-organizing). Refer to the materials in this course if you're stuck on this part of the project.


#### Pushing to GitHub

You'll want to save changes to your project regularly by pushing them to GitHub. Now that you've got your file structure set up and have added an R Markdown document to your code/raw_code directory, it's a good time to stage, commit, and push these changes to GitHub. Do so now, and then take a long on GitHub to see the changes on their website!

- After this initial push, how many of the directories you created are visible on GitHub?

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/cbds-version-control). Refer to the materials in this course if you're stuck on this part of the project.

#### The Data

To get you started, let's get you acquainted with the data. The data you'll be working with are hosted at [data.world](data.world) and contain information about Sales in the US. You'll be working with two datasets.

[Data Set 1](https://data.world/retail/department-store-sales): Sales from the Retail Trade and Food Services Report from the US Census. This dataset only covers Department Stores, though the report covers a wide range of retail types. [1992-2016]

[Data Set 2](https://data.world/garyhoov/retail-sales-growth) US Retail Sales by Store Type with Growth Rate [2009-2014]

#### Getting the Data into R

To get the data read into R, we're going to use two packages: `httr` and  `readxl`. These packages will be covered in the next course. As they haven't been taught yet, we'll have you use them here, but we'll provide all the code you need in order to use them, rather than having you figure this part out on your own.

You can see the code to get these data read into RStudio in the second code chunk (`data`) provided within `data_tidying_project.Rmd`. To read the data in, first run the code in the first code chunk ( `setup`) to get the necessary packages loaded into R. Then, run the code in the second code chunk (`data`) to load the data into R. In your Environment tab, you will see that there are two new objects, `df1` and `df2` that have been created.

Once the data are read in, to take a look at these data you can use the `glimpse()` or `View()` functions in RStudio Cloud (i.e. `View(df1)` or `glimpse(df1`)

#### Saving the Raw Data

While the data are available on the Internet, what if in the future they take them down? Then, you'll have lost the data for your project or at the very least you won't have access to it. To avoid this being a problem in the future, let's save a copy of the data on RStudio Cloud now.

You'll want to save these data in the data/raw_data folder you created. We'll save these as .rds objects. This means that each has to be saved separately using the `saveRDS()` function.

Add the code to do this to the `save-data` code chunk in your `data_tidying_project.Rmd` document. Save `df1` as 'df_department.rds' and `df2` as `df_retail.rds`.

- What line of code did you use to save `df1`?

#### Wrangle the Data

We'll now work with the two datasets you read in, so that they're in a usable (long) data format and can be merged into a single data frame.

The wrangling portion of this project will use the skills you learned in [Introduction to R](https://leanpub.com/universities/courses/jhu/cbds-intro-r) and this course [Data Tidying](https://leanpub.com/universities/courses/jhu/cbds-tidying). Feel free to refer back to these materials for reference as you complete the project.

##### The retail data: `df2`

Now that you have the data in RStudio Cloud and have saved the raw data to your data/raw_data file, we're ready to start wrangling data.

To get you acquainted with the dataset, we'll first have you answer a few questions about the dataset: `df2`

- How many rows are there in `df2`
- How many columns are there in `df2`
- In 2014, what were the sales for used car dealers?


Now, the goal of wrangling these data will be to take this data frame, which is currently in the **wide** data format and wrangle it into a **long** data format.  The resulting data frame from your wrangling will have three columns: `business`, `year`, and `n`. In the `data_tidying_project.Rmd` document, you'll see the code required to accomplish this in the `wrangle-df2` code chunk.

We recommend running each line at a time. For example, start by running:

```r
## an example working with df2
## let's wrangle!
df_retail <- df2 %>%
  ## remove the r from the column names of df2
  magrittr::set_colnames(gsub("r","",df2[1,]))
```

Take a look at the output variable `df_retail` (using `glimpse()`, `View()`, or `skim()`) to see what the `magrittr::set_colnames()` code accomplishes. Once you understand this line, then move onto the next line and run:

```r
## an example working with df2
## let's wrangle!
df_retail <- df2 %>%
  ## remove the r from the column names of df2
  magrittr::set_colnames(gsub("r","",df2[1,])) %>%
  ## add a new column called "business"
  mutate(business = gsub("[?]|[.]","",`Kind of business`))
```

Again, take a look at the output variable `df_retail` to understand this additional line of code. This will help you understand what each line of code does! And, you'll understand *why* each step was taken to get the data into it's final format. Continue this process until you understand the entire chunk of code in `wrangle-df2`.

- If you were to change the line  `mutate(business = gsub("[?]|[.]","",`Kind of business`))` to `mutate(biz = gsub("[?]|[.]","",`Kind of business`))`, what would change?
- What information is in the column `n`?
- What does `filter(business == "Retail sales, total "| business=="Department stores ")` accomplish?

##### The department store data: `df1`

Having had all the code to wrangle `df2` and being able to understand what each line did, now it's your turn to wrangle a data frame! You'll take  We'll first ask a few questions to get you acquainted with `df1`.

- How many rows are there in `df1`
- How many columns are there in `df1`
- In `Jun-1992`, what were retail sales (in millions)?

Now that you have an idea of what data are in the data frame `df`, it's time to wrangle! The goal of wrangling this data set is to mirror (or mimic) what we did in the first data set. This means, that by the end of data wrangling, you should have three columns with the same names as `df_retail`: `business`, `year`, and `n`.

To accomplish this goal, you'll have to figure out what code should replace `function_name` for each line in the code chunk `wrangle-df1`. A different `function_name` will be called in each line of missing code. The arguments necessary to be included within the parentheses will also have to be added.

For example, for the first part of the code, you currently see in the

```r
df_department <- df1 %>%
  ## split Period column into one column called "month" and one called "year"
  function_name()
```

The comment ` ## split Period column into one column called "month" and one called "year"` tells you what you'll want to accomplish in the next line of code.

So, you may try adding the following:

```r
df_department <- df1 %>%
  ## split Period column into one column called "month" and one called "year"
  separate(Period, into = c('month','year'), extra = 'drop', remove = FALSE)
```

By changing `function_name()` to the appropriate function `separate()` and including the necessary code within the parentheses, we are able to accomplish exactly what the comment says we wanted to do!

Before you get started on the rest of this,  a quick note on data wrangling : As you master these skills, you *will* write code that errors and things *will* take you multiple tries before you get it right. If you're frustrated, that's ok! It's part of the process. Use the material in this course or places like [StackOverflow](https://stackoverflow.com/) to help you if you find answers if you get stuck!.

OK, now it's your job to work line by line to change `function_name` to the appropriate function in order to accomplish what the comments state you want to accomplish. Work through this code chunk line by line until you have a long data set called `df_department` with three columns:  `business`, `year`, and `n`.

- What function did you use to group the data by the `year` column?
- What function did you use to add the column name `value`?
- What function did you use to reorder the column names?

##### Pushing to GitHub

You've written a lot of code at this point, so it'd be great to add this to GitHub at this point. Use `git add`, `git commit`, and `git push` to add the file changes to your GitHub repository.


##### Merging the data

Once you have wrangled both `df1` and `df2`, you should be able to join them by binding the rows of `df_retail` and `df_department`. Change the code in the `merge-data` code chunk to generate an object called `df_total`. This should only take one function to accomplish.

##### Plotting Your Data

While data visualization is coming up in a later course, we've included a few lines of code to plot the data from the three data frames you created (`df_retail`, `df_department`, `df_total`). For this you'll use the R package `ggplot2`. Try running these lines of code in the `plot` chunk and take a look at the plots generated in the "Plots" tab of RStudio Cloud. However, don't stress if you're not exactly sure what's going on. We'll cover all of this in an upcoming course!

#### Add Markdown Text to .Rmd

Before finalizing your project you'll want to add some text outside of your code chunks to explain what you're doing in each code chunk. These explanations are incredibly helpful for someone who doesn't code.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Introduction to R](https://leanpub.com/universities/courses/jhu/cbds-intro-r). Refer to the R Markdown lesson in this course if you're stuck on this part (or the next part) of the project.

#### Knit your R Markdown Document

Last but not least, you'll want to Knit your .Rmd document into an HTML document. If you get an error, take a look at what the error says and edit your .Rmd document. Then, try to Knit again! Troubleshooting these error messages will teach you a lot about coding in R.


#### Final `push` to GitHub

Now that you've finalized your project, you'll do one final push to GitHub. add, commit, and push your work to GitHub. Navigate to your GitHub repository, and answer the final question below!

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/cbds-version-control). Refer to the materials in this course if you're stuck on this part of the project.

**Congrats on finishing your first Data Science Project**
