# Data Tidying Project

Throughout this course set, we'll have a number of projects for you to complete. These will be included as Exercises. This means they will *not* be required to pass the course and receive your certificate; however, completing them will *really* help to improve your understanding of the material covered and to ensure that you've begun to master the skills needed to be a data scientist.

In each project, we'll aim to get you started and to ask questions that will help guide you through the project. But, we'll intentionally leave pieces out where you have to figure out what needs to be done. 

This first project will require you to:

* use version control
* demonstrate the skills you've learned programming in R
* wrangle data

You can access the first project by going to the exercise accompanying this lesson.

{exercise, id: project_data_tidying}

### Data Tidying Project

Often times, data scientists are handed data and asked to make sense of them. The data scientist may be asked to figure out why people are getting sick in a city or how their employer can save money. No matter the situation, these data aren't always in the most usable format. **Data wrangling** is required before any pretty visualizations can be mad or questions can be answered. This exercise has been generated to practice your data wrangling skills.

#### GitHub Setup

To get started, you'll want to go to GitHub and start a new repository:

* Call this repository `data_tidying_project`. 
* Add a short description
* Check the box to "Initialize this repository with a README. 
* Click `Create Repository`

Once the repository has been created, Click on `Clone or download` and copy the "Clone with HTTPS" link provided. You'll use this to clone your repo in RStudio Cloud.

#### RStudio Cloud Setup

* Go to the [Data Tidying Project Workspace on RStudio Cloud](https://rstudio.cloud/spaces/955/project/28350) 
* Make a copy of this project 

In this project you should see a `data_tidying_project.Rmd ` file. You'll use this to get started working on your project!

Now that you have a copy of the project you'll use to get started you'll want to clone the GitHub repository you just created into this workspace. To do so, go to the terminal and clone your project into this workspace.

{words: 100}
? Why did you clone your GH repository into RStudio Cloud?

!  Allows project to be version controlled

A new directory with the name of your GitHub repository should now be viewable in the Files tab of RStudio Cloud. You are now set up to track your project with git.

#### Data Science Project Setup

As discussed previously, you'll want all your data science projects you be organized from the very beginning. Let's do that now!

First, use `cd` to get yourself into the directory of your GitHub Project.  

Once in the correct directory, use `mkdir` in the terminal to create folders with the following structure:

* data/
  * raw_data/
  * tidy_data/
* code/
  * raw_code/
  * final_code/
* figures/
  * exploratory_figures/
  * explanatory_figures/
* products/
  * writing/

Now that your directories are set up you'll want to use the Terminal (or 'More' drop-down menu in the Files tab) to move (`mv`) the `data_tidying_project.Rmd ` file into code/raw_code. This ensures that your code file is in the correct directory.

? In what directory did you save  `data_tidying_project.Rmd `?

! code/raw_code

Once the .Rmd document is in the correct folder, you'll want to change the author of this document to your name at the top of the .Rmd document (in the YAML). Save this change before moving to the next step.

#### Pushing to GitHub

You'll want to save changes to your project regularly by pushing them to GitHub. Now that you've got your file structure set up and have added an R Markdown document to your code/raw_code directory, it's a good time to stage, commit, and push these changes to GitHub. Do so now, and then take a long on GitHub to see the changes on their website!

? In what directory did you save  `data_tidying_project.Rmd `?

! code/raw_code

? After this initial push, how many of the directories you created are visible on GitHub?

! 1

#### The Data

To get you started, let's get you acquainted with the data. The data you'll be working with are hosted at [data.world](data.world) and contain information about Sales in the US. You'll be working with two datasets.

[Data Set 1](https://data.world/retail/department-store-sales): Sales from the Retail Trade and Food Services Report from the US Census. This dataset only covers Department Stores, though the report covers a wide range of retail types. [1992-2016]

[Data Set 2](https://data.world/garyhoov/retail-sales-growth) US Retail Sales by Store Type with Growth Rate [2009-2014]

#### Getting the Data into R 

To get the data read into R, we're going to use two packages: `httr` and  `readxl`. These packages will be covered in the next course. As they haven't been taught yet, we'll have you use them here, but we'll provide all the code you need in order to use them, rather than having you figure this part out on your own.

You can see the code to get these data read into RStudio in the second code chunk provided within `data_tidying_project.Rmd`. To read the data in, first run the code in the `setup` code chunk to get the necessary packages loaded into R. Then, run the code in the second code chunk to load the data into R. In your Environment tab, you will see that there are two new objects, `df1` and `df2` that have been created.

Use the `View()` function to view these data frames in RStudio Cloud

```r
View(df1)
View(df2)
```

#### Saving the Raw Data

While the data are available on the Internet, what if in the future they take them down? Then, you'll have lost the data for your project or at the very least you won't have access to it. To avoid this being a problem in the future, let's save a copy of the data on RStudio Cloud now. 

You'll want to save these data in 

#### Wrangling the Data

#### 

{/exercise}

