# Tidying Data

So far we've discussed what tidy and untidy data are. We've (hopefully) convinced you that tidy data are the right type of data to work with. And, more than that, hopefully we've explained that data are not always the tidiest when they come to you at the start of a project. An incredibly important skill of a data scientist is to be able to take data from an untidy format and get it into a tidy format. We've started to discuss how to do this in the last lesson where we learned to reshape data. In this lesson, we'll discuss a number of other ways in which data can be tidied and the necessary tools to **tidy data**. 

These skills are often referred to as **data wrangling**. They are skills that allow you to wrangle data from the format they're currently in into the format you actually want them in. 

As this is an incredibly important topic, this will be a long lesson covering a number of packages and topics. Take your time working through it and be sure to understand all of the examples!

![data wrangling example](images/03_tidyingdata/03_datacleaning_tidyingdata-1.png)

### dplyr

Within R, there is a package specifically designed for helping you wrangle data. This package is called [`dplyr`](https://dplyr.tidyverse.org/) and will allow you to easily accomplish many of the data wrangling tasks necessary. In this lesson, we will cover a number of functions that will help you wrangle data using `dplyr`:

* `%>%` - pipe operator for chaining a sequence of operations
* `glimpse()` - get an overview of what's included in dataset
* `filter()` - filter rows
* `select()` - select, rename, and reorder columns
* `arrange()` - reorder rows
* `mutate()` - create a new column
* `group_by()` - group variables 
* `summarize()` - summarize information within a dataset
* `left_join()` - combining data across data frame

If you have not already, you'll want to be sure this package is installed and loaded:

```r
install.packages('dplyr')
library(dplyr)
```

### tidyr

We will also return to the `tidyr` package. The same package that we used to reshape our data will be helpful when tidying data. The main functions we'll cover from `tidyr` are:

* `unite()` - combine contents of two or more columns into a single column
* `separate()` - separate contents of a column into two or more columns

If you have not already, you'll want to be sure this package is installed and loaded:

```r
install.packages('tidyr')
library(tidyr)
```

### janitor

The third package we'll include here is the `janitor` package. This package provides tools for cleaning messy data. The main functions we'll cover from janitor are:

* `clean_names()` - clean names of a data frame
* `tabyl()` - get a helpful summary of a variable

If you have not already, you'll want to be sure this package is installed and loaded:

```r
install.packages('janitor')
library(janitor)
```

### skimr

The final package we'll discuss here is the `skimr` package. This package provides a quick way to summarize a data frame. We'll discuss its most useful function here:

* `skim()` - summarize a data frame

If you have not already, you'll want to be sure this package is installed and loaded:

```r
install.packages('skimr')
library(skimr)
```

### The Pipe Operator

Before we get into the important functions within `dplyr`, it will be very useful to discuss what is known as the **pipe operator**. The pipe operator looks like this in R: `%>%`. Whenever you see the pipe `%>%`, think of the word "then", so if you saw the sentence "I went to the the store and %>% I went back to my house," you would read this as I went to the store and *then* I went back to my house. The pipe tells you to do one think and *then* do another.

Generally, the pipe operator allows you to string a number of different functions together in a particular order. If you wanted to take data frame A and carry out function B on it in R, you could depict this with an arrow pointing from A to B:

A --> B

Here you are saying, "Take A and *then* feed it into function B."

In R syntax, from what you've seen so far, what is depicted by the arrow above would be carried out by calling the function B on the data frame object A:

```r
B(A)
```

Alternatively, you could use the pipe operator (`%>%`):

```r
A %>% B
```

However, often you are not performing just one action on a data frame, but rather you are looking to carry out multiple functions. We can again depict this with an arrow diagram.

A --> B --> C --> D

Here you are saying that you want to take data frame A and carry out function B, *then* you want to take the output from that and *then* carry out function C. Subsequently you want to take the output of that and *then* carry out function D. In R syntax, we would first apply function B to data frame A, then apply function C to this output, then apply function D to this output. This results in the following syntax that is hard to read because multiple calls to functions are nested within each other:

```r
D(C(B(A)))
```

Alternatively, you could use the pipe operator. Each time you want take the output of one function and carry out something new on that output, you will use the pipe operator:

```r
A %>% B %>% C %>% D
```

Below we'll use this pipe operator a lot. Essentially, it takes output from the left hand side and feeds it into a function on the right hand side. You'll get a better understanding of how it works as you run the code below. But, when in doub remember that the pipe operator should be read as *then*.

### Filtering Data

When working with a large dataset, you're often interesting in only working with a portion of the data at any one time. For example, if you had data on people from ages 0 to 100 years old, but you wanted to ask a question that only pertained to children, you would likely want to only work with data from those individuals who were less than 18 years old. To do this, you would want to **filter** your dataset to only include data from these select individuals. Filtering can be done by row or by column. We'll discuss the syntax in R for doing both. Please note that the examples in this lesson and the organization for this lesson were adapted from [Suzan Baert's](https://suzan.rbind.io/) wonderful `dplyr` tutorials. Links to the all four tutorials can be found in the "Additional Resources" section at the bottom of this lesson.

For the examples below, we'll be using a dataset from the `ggplot2` package called `msleep`. (You'll learn more about this package in a later course on data visualization.) This dataset includes sleep times and weights from a number of different mammals. It has 83 rows, with each row including information about a different type of animal, and 11 variables. As each row is a different animal and each column includes information about that animal, this is a **wide** dataset.

To get an idea of what variables are included in this data frame, you can use `glimpse()`. This function summarizes how many rows there are (`Observations`) and how many columns there are (`Variables`). Additionally, it gives you a glimpse into the type of data contained in each column. Specifically, in this data set, we know that the first column is `name` and that it contains a character vector (`chr`) and that the first three entires are "Cheetah", "Owl monkey", and "Mountain beaver." It works similarly to the `summary()` function covered in an earlier course.

```r
## install packages if you haven't already
install.packages('dplyr')

## load package
library(dplyr)

## take a look at the data
glimpse(msleep)
```

![Glimpse of msleep dataset](images/03_tidyingdata/03_datacleaning_tidyingdata-12.png)

#### Filtering Rows

If you were only interested in learning more about the sleep times of "Primates," we could filter this dataset to include only data about those mammals that are also Primates. As we can see from `glimpse()`, this information is contained within the `order` variable. So to do this within R, we use the following syntax:

```r
msleep %>%
  filter(order == "Primates")
```

Note that we are using the equality `==` comparison operator that you learned about in the previous course. Also note that we have used the pipe operator to feed the `msleep` data frame into the `filter()` function. This is short hand for:

```r
filter(msleep, order == "Primates")
```

![Filtered to only include Primates](images/03_tidyingdata/03_datacleaning_tidyingdata-13.png)

Here, we have a smaller dataset of only 12 mammals (as opposed to the original 83) and we can see that the `order` variable column only includes "Primates." 

But, what if we were only interested in Primates who sleep more than 10 hours total per night? This information is in the `sleep_total` column. Fortunately, `filter()` also works on numeric variables. To accomplish this, you would use the following syntax, separating the multiple filters you want to apply with a comma:

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10)
```

Note that we have used the "greater than" comparison operator with `sleep_total`.

Now, we have a dataset focused in on only 5 mammals, all of which are primates who sleep for more than 10 hours a night total.

![Numerically filtered dataset](images/03_tidyingdata/03_datacleaning_tidyingdata-14.png)

We can obtain the same result with the AND `&` logical operator instead of separating filtering conditions with a comma:

```r
msleep %>%
  filter(order == "Primates" & sleep_total > 10)
```

Note that the number of columns hasn't changed. All 11 variables are still shown in columns because the function `filter()` filters on rows, not columns.

#### Selecting Columns

While `filter()` operates on rows, it *is* possible to filter your dataset to only include the columns you're interested in. To select columns so that your dataset only includes variables you're interested in, you will use `select()`. 

Let's start with the code we just wrote to only include primates who sleep a lot. What if we only want to include the first column (the name of the mammal) and the sleep information (included in the columns `sleep_total`, `sleep_rem`, and `sleep_cycle`)? We would do this by starting with the code we just used, adding another pipe, and using the function `select()`. Within `select`, we specify which columns we want in our output.

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_total, sleep_rem, sleep_cycle)
```

![Data with selected columns](images/03_tidyingdata/03_datacleaning_tidyingdata-15.png)

Now, using `select()` we see that we still have the five rows we filtered to before, but we only have the four columns specified using `select()`. Here you can hopefully see the power of the pipe operator to chain together several commands in a row. Without the pipe operator, the full command would look like this:

```r
select(filter(msleep, order == "Primates", sleep_total > 10), name, sleep_total, sleep_rem, sleep_cycle)
```

#### Renaming Columns

`select()` can also be used to rename columns. To do so, you use the syntax: `new_column_name = old_column_name` within `select`. For example, to select the same columns and rename them `total`, `rem` and `cycle`, you would use the following syntax:

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, total=sleep_total, rem=sleep_rem, cycle=sleep_cycle)
```
![Data with renamed columns names](images/03_tidyingdata/03_datacleaning_tidyingdata-16.png)

### Reordering

In addition to filtering rows and columns, often, you'll want the data arranged in a particular order. It may order the columns in a logical way, or it could be to sort the data so that the data are sorted by value, with those having the smallest value in the first row and the largest value in the last row. All of this can be achieved with a few simple functions.

#### Reordering Columns

The `select()` function is powerful. Not only will it filter and rename columns, but it can also be used to reorder your columns. Using our example from above, if you wanted `sleep_rem` to be the first sleep column and `sleep_total` to be the last column, all you have to do is reorder them within `select()`. The output from `select()` would then be reordered to match the order specified within `select()`.

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total)
```

Here we see that sleep_rem `name` is displayed first followed by `sleep_rem`, `sleep_cycle`, and `sleep_total`, just as it was specified within `select()`. 

![Data with reordered columns names](images/03_tidyingdata/03_datacleaning_tidyingdata-18.png)

#### Reordering Rows

Rows can also be reordered. To reorder a variable in ascending order (from smallest to largest), you'll want to use `arrange()`. Continuing on from our example above, to now sort our rows by the amount of total sleep each mammal gets, we would use the following syntax:

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total) %>%
  arrange(sleep_total)
```

![Data arranged by total sleep in ascending order](images/03_tidyingdata/03_datacleaning_tidyingdata-19.png)

While `arrange` sorts variables in ascending order, it's also possible to sort in descending (largest to smallest) order. To do this you just use `desc()` with the following syntax:

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total) %>%
  arrange(desc(sleep_total))
```

By putting `sleep_total` within `desc()`, `arrange()` will now sort your data from the primates with the longest total sleep to the shortest.

![Data arranged by total sleep in descending order](images/03_tidyingdata/03_datacleaning_tidyingdata-20.png)

`arrange()` can also be used to order non-numeric variables. For example, `arrange()` will sort character vectors alphabetically. 

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total) %>%
  arrange(name)
```

![Data arranged alphabetically by name](images/03_tidyingdata/03_datacleaning_tidyingdata-21.png)

If you would like to reorder rows based on information in multiple columns, you can specify them separated by commas. This is useful if you have repeated labels in one column and want to sort within a category based on information in another column. In the example here, if there were repeated primates, this would sort the repeats based on their total sleep.

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total) %>%
  arrange(name, sleep_total)
```

### Creating new columns

You will often find when working with data that you need an additional column. For example, if you had two datasets you wanted to combine, you may want to make a new column in each dataset called `dataset`. In one dataset you may put `datasetA` in each row. In the second dataset, you could put `datasetB`. This way, once you combined the data, you would be able to keep track of which dataset each row came from originally. More often, however, you'll likely want to create a new column that calculates a new variable based on information in a column you already have. For example, in our mammal sleep dataset, `sleep_total` is in hours. What if you wanted to have that information in minutes? You could create a new column with this very information! The function `mutate()` was *made* for **all** of these new-column-creating situations. This function has a lot of capabilities. We'll cover the basics here. 

Returning to our `msleep` dataset, after filtering and re-ordering, we can create a new column with `mutate()`. Within `mutate()`, we will calculate the number of minutes each mammal sleeps by multiplying the number of hours each animal sleeps by 60 minutes.

```r
msleep %>%
  filter(order == "Primates", sleep_total > 10) %>%
  select(name, sleep_rem, sleep_cycle, sleep_total) %>%
  arrange(name) %>%
  mutate(sleep_total_min = sleep_total * 60)
```

![Mutate to add new column to data](images/03_tidyingdata/03_datacleaning_tidyingdata-23.png)

### Separating Columns 

Sometimes multiple pieces of information are merged within a single column even though it would be more useful during analysis to have those pieces of information in separate columns. To demonstrate, we'll now move from the `msleep` dataset to talking about another [dataset](https://raw.githubusercontent.com/suzanbaert/RTutorials/master/Rmd_originals/conservation_explanation.csv) that includes information about conservation abbreviations in a single column.

To read this file into R, we'll use the `httr` package, which will be discussed in detail in a future lesson. For now, however, know that we're using this to read in a file from the Internet using the code below.

```r
## if not already installed, you'll have to run the following line of code
## install.packages('httr')
## install.packages('readr')

## load the libraries
library(httr)
library(readr)

## download file 
GET("https://raw.githubusercontent.com/suzanbaert/RTutorials/master/Rmd_originals/conservation_explanation.csv", write_disk(tf <- tempfile(fileext = ".csv")))
conservation <- read_csv(tf)

## take a look at this file
head(conservation)
```

![Conservation data set](images/03_tidyingdata/03_datacleaning_tidyingdata-25.png)

In this dataset, we see that there is a single column that includes *both* the abbreviation for the conservation term as well as what that abbreviation means. Recall that this violates one of the tidy data principles covered in the first lesson: Put just one thing in a cell. To work with these data, you could imagine that you may want these two pieces of information (the abbreviation and the description) in two different columns. To accomplish this in R, you'll want to use `separate()` from `tidyr`.

The `separate()` function requires the name of the existing column that you want to separate (`conservation abbreviation`), the desired column names of the resulting separated columns (`into = c("abbreviation", "description")`), and the characters that currently separate the pieces of information (`sep = " = "`). We have to put `conservation abbreviation` in back ticks in the code below because the column name contains a space. Without the back ticks, R would think that `conservation` and `abbreviation` were two separate things. This is another violation of tidy data! Variable names should have underscores, not spaces!

```r
conservation %>%
  separate(`conservation abbreviation`, 
           into = c("abbreviation", "description"), sep = " = ")
```

The output of this code shows that we now have two separate columns with the information in the original column separated out into `abbreviation` and `description`.

![Output of separate()](images/03_tidyingdata/03_datacleaning_tidyingdata-26.png)

### Merging Columns 

The opposite of `separate()` is `unite()`. So, if you have information in two or more different columns but wish it were in one single column, you'll want to use `unite()`. Using the code forming the two separate columns above, we can then add on an extra line of `unite()` code to re-join these separate columns, returning what we started with.

```r
conservation %>%
  separate(`conservation abbreviation`, 
           into = c("abbreviation", "description"), sep = " = ") %>%
  unite(united_col, abbreviation, description, sep = " = ")
```

![Output of unite()](images/03_tidyingdata/03_datacleaning_tidyingdata-27.png)

### Cleaning up column names

While maybe not quite as important as some of the other functions mentioned in this lesson, a function that will likely prove very helpful as you start analyzing lots of different datasets is `clean_names()` from the `janitor` package. This function takes the existing column names of your dataset, converts them all to lowercase letters and numbers, and separates all words using the underscore character. For example, there is a space in the column name for conservation. `clean_names()` will convert `conservation abbreviation` to `conservation_abbreviation`. These cleaned up column names are a lot easier to work with when you have large datasets.

```r
conservation %>%
  clean_names()
```

![clean_names() output](images/03_tidyingdata/03_datacleaning_tidyingdata-28.png)

### Combining data across data frames

There is often information stored in two separate data frames that you'll want in a single data frame. There are *many* different ways to join separate data frames. They are discussed in more in [this tutorial](http://stat545.com/bit001_dplyr-cheatsheet.html) from [Jenny Bryan](https://www.stat.ubc.ca/~jenny/). Here, we'll demonstrate how the `left_join()` function works, as this is used frequently. 

Let's try to combine the information from the two different datasets we've used in this lesson. We have `msleep` and `conservation`. `msleep` contains a column called `conservation`. This column includes lowercase abbreviations that overlap with the uppercase abbreviations in the `abbreviation` column in the `conservation` dataset.

To handle the fact that in one dataset the abbreviations are lowercase and the other they are uppercase, we'll use `mutate()` to take all the lowercase abbreviations to uppercase abbreviations using the function `toupper()`. 
 
We'll then use `left_join()` which takes all of the rows in the first dataset mentioned (`msleep`, below) and incorporates information from the second dataset mentioned (`conserve`, below), when information in the second dataset is available. The `by = ` argument states what columns to join by in the first ("conservation") and second ("abbreviation") datasets. This join adds the `description` column from the `conserve` dataset onto the original dataset (`msleep`). Note that if there is no information in the second dataset that matches with the information in the first dataset, `left_join()` will add NA. Specifically, for rows where conservation is "DOMESTICATED" below, the `description` column will have NA because "DOMESTICATED"" is not an abbreviation in the `conserve` dataset.

```r
## take conservation dataset and separate information
## into two columns
## call that new object `conserve`
conserve <- conservation %>%
  separate(`conservation abbreviation`, 
           into = c("abbreviation", "description"), sep = " = ")


## now lets join the two datasets together
msleep %>%
  mutate(conservation = toupper(conservation)) %>%
  left_join(conserve, by = c("conservation" = "abbreviation"))
```

![Data resulting from left_join](images/03_tidyingdata/03_datacleaning_tidyingdata-29.png)

It's important to note that there are many other ways to join data, which are covered in more detail on this [dplyr join cheatsheet](http://stat545.com/bit001_dplyr-cheatsheet.html) from Jenny Bryan. For now, it's important to know that joining datasets is done easily in R using tools in `dplyr`. As you join data frames in your own work, it's a good idea to refer back to this cheatsheet for assistance.

### Grouping Data

Often, data scientists will want to summarize information in their dataset. You may want to know how many people are in a dataset. However, more often, you'll want to know how many people there are within a group in your dataset. For example, you may want to know how many males and how many females there are. To do this, grouping your data is necessary. Rather than looking at the total number of individuals, to accomplish this, you first have to **group the data** by the gender of the individuals. Then, you count within those groups. Grouping by variables within `dplyr` is straightforward.

#### group_by()

There is an incredibly helpful function within `dplyr` called `group_by()`. `group_by()` groups a dataset by one or more variables. On its own, it does not appear to change the dataset very much. The difference between the two outputs below is subtle:

```r
msleep

msleep %>%
  group_by(order)
```

![group_by() output](images/03_tidyingdata/03_datacleaning_tidyingdata-31.png)

In fact, the only aspect of the output that is different is that the number of different orders is now printed on your screen. However, in the next section, you'll see that the output from any further functions you carry out at this point will differ between the two datasets.

### Summarizing Data

Throughout data cleaning and analysis it will be important to summarize information in your dataset. This may be for a formal report or for checking the results of a data tidying operation.

#### summarize()

Continuing on from the previous examples, if you wanted to figure out how many samples are present in your dataset, you could use the `summarize()` function. 

```r
msleep %>%
  select(order) %>%
  summarize(N=n())
```

This provides a summary of the data with the new column name we specified above (`N`) and the number of samples in the dataset. Note that we could also obtain the same information by directly obtaining the number of rows in the data frame with `nrow(msleep)`.

![Summarize with n()](images/03_tidyingdata/03_datacleaning_tidyingdata-32.png)

However, if you wanted to count how many of each different `order` of mammal you had. You would first `group_by(order)` and then use `summarize()`. This will summarize within group.

```r
msleep %>%
  group_by(order) %>% 
  select(order) %>%
  summarize(N=n())
```

The output from this, like above, includes the column name we specified in summarize (`N`). However, it includes the number of samples in the group_by variable we specified (`order`).

![group_by() and summarize with n()](images/03_tidyingdata/03_datacleaning_tidyingdata-33.png)

There are other ways in which the data can be summarized using `summarize()`. In addition to using n() to count the number of samples within a group, you can also summarize using other helpful functions within R, such as `mean()`, `median()`, `min()`, and `max()`. 

For example, if we wanted to calculate the average (mean) total sleep each order of mammal got, we could use the following syntax:

```r
msleep %>%
  group_by(order) %>% 
  select(order, sleep_total) %>%
  summarize(N=n(), mean_sleep=mean(sleep_total))
```

![summarize using mean()](images/03_tidyingdata/03_datacleaning_tidyingdata-34.png)

#### tabyl()

In addition to using `summarize()` from `dplyr`, the `tabyl()` function from the `janitor` package can be incredibly helpful for summarizing categorical variables quickly and discerning the output at a glance. Again returning to our `msleep` dataset, if we wanted to get a summary of how many samples are in each order category and what percent of the data fall into each category we could call tabyl on that variable. For example, if we use the following syntax, we easily get a quick snapshot of this variable. 

```r
msleep %>%
  tabyl(order)
```

![summarize using tabyl() from janitor](images/03_tidyingdata/03_datacleaning_tidyingdata-35.png)

Note, that `tabyl` assumes categorical variables. If you want to summarize numeric variables `summary()` works well. For example, this code will summarize the values in `msleep$awake` for you. 

```r
summary(msleep$awake)
```

![summarize numeric variables](images/03_tidyingdata/03_datacleaning_tidyingdata-36.png)

#### skim()

When you would rather get a snapshot of the entire dataset, rather than just one variable, the `skim()` function from the `skimr` package can be very helpful. The output from `skim()` breaks the data up by variable type. For example, the `msleep` data set is broken up into `character` and `numeric` variable types. The data are then summarized in a meaningful way for each. This function provides a lot of information about the entire data set. So, when you want a summarize a dataset and quickly get a sense of your data, `skim()` is a great option!

```r
skim(msleep)
```

![summarize entire dataset using skim() from skimr](images/03_tidyingdata/03_datacleaning_tidyingdata-37.png)


### Conclusion

We have gone through a number of ways to work with data in this lesson. Mastering the skills in this lesson will provide you with a number of critical data science skills. Thus, running the examples in this lesson and practicing on your own with other data sets will be essential to succeeding as a data scientist.

### Additional Resources

* [dplyr](https://dplyr.tidyverse.org/), part of the [tidyverse](https://www.tidyverse.org/)
* [janitor](https://cran.r-project.org/web/packages/janitor/index.html), from [Sam Firke](http://samfirke.com/)
* dplyr tutorials by [Suzan Baert](https://suzan.rbind.io/) [Part 1](https://suzan.rbind.io/2018/01/dplyr-tutorial-1/) [Part 2](https://suzan.rbind.io/2018/02/dplyr-tutorial-2/) [Part 3](https://suzan.rbind.io/2018/02/dplyr-tutorial-3/) [Part 4](https://suzan.rbind.io/2018/04/dplyr-tutorial-4/) 
* [janitor tutorial](https://medium.com/@verajosemanuel/janitor-a-good-r-package-for-data-cleaning-f3c733632ad9) by
* [dplyr join cheatsheet](http://stat545.com/bit001_dplyr-cheatsheet.html) by [Jenny Bryan](https://www.stat.ubc.ca/~jenny/)

**Note**: a lot of the examples in this lesson were modified from the dplyr tutorials by [Suzan Baert](https://suzan.rbind.io/). To get an even **deeper** understanding of how to tidy data using `dplyr`, take a look at all of her dplyr tutorials. 

### Slides and Video

![Tidying Data](https://www.youtube.com/watch?v=tEbDnvlfu0k)

* [Slides](https://docs.google.com/presentation/d/1Z1pukaF-HrZHEwSfr3SV8N2Slo2rMEJxpgl1qJv-QL4/edit?usp=sharing)


{quiz, id: quiz_03_tidyingdata}

### Tidying Data quiz

{choose-answers: 4}
?1 To create a new column which function would you use?

C) mutate()
m) arrange()
o) clean_names()
o) glimpse()
o) new_column()
o) new()

{choose-answers: 4}
?1 To order the rows in a dataframe based on a variable's value, which function would you use?

C) arrange()
m) mutate()
o) clean_names()
o) glimpse()
o) new_column()
o) new()

{choose-answers: 4}
?2 Which of these is not a capability of select()?

C) choose what rows to include in output
C) filter rows based on a condition
C) read in a CSV file
o) choose what columns to include in output
o) rename columns
o) reorder columns

{choose-answers: 4}
?3 To summarize an entire dataset, what *function* would you use?

C) skim()
m) skimr
o) summary()
o) tabyl()
o) summarize()
o) janitor
o) dplyr 

{choose-answers: 4}
?3 To summarize an entire dataset, you could use the `skim()` function, what package is that in?

C) skimr
m) skim()
o) summary()
o) tabyl()
o) summarize()
o) janitor
o) dplyr

{choose-answers: 4, points: 2}
?4 If you were to select only the cars in the `mtcars` data set with horsepower > 100 using the code `newdata <- mtcars %>% filter(hp>100)`, how many cars would be left in your data frame?

C) 23
m) 32
m) 9
o) 0
o) 30
o) 15

{choose-answers: 4, points: 2}
?4 If you were to select only the cars in the `mtcars` data set with horsepower > 100 using the code `newdata <- mtcars %>% filter(hp<100)`, how many cars would be left in your data frame?

C) 9
m) 23
o) 32
o) 0
o) 30
o) 15

{choose-answers: 4, points: 2}
?5 How many different types of iris `Species` are included in the `iris` dataset?

C) 3 
m) 2
o) 100
o) 150
o) 1
o) 5
o) 7
o) 20

{choose-answers: 4, points: 2}
?5 What's the smallest `Sepal.Width` in the `iris` dataset?

C) 2
m) 3
o) 100
o) 150
o) 1
o) 5
o) 7
o) 20

{choose-answers: 4, points: 2}
?6 If you had six different variables in the columns of a dataset (`df`) and wanted to select three of them (height, weight, bmi) and display them in alphabetical order, which of these would achieve that?

C) df %>% select(bmi, height, weight)
m) df %>% select(height, weight, bmi)
m) df %>% arrange(height, weight, bmi)
o) df %>% arrange(bmi, height, weight)
o) df %>% filter(bmi, height, weight)
o) df %>% filter(height, weight, bmi)
o) df %>% mutate(bmi, height, weight)
o) df %>% mutate(height, weight, bmi)


{choose-answers: 4, points: 2}
?6 If you had a dataset (`df`) and wanted to order your dataframe by height, then by weight, and then by bmi, which of these would you use?

C) df %>% arrange(height, weight, bmi)
m) df %>% select(bmi, height, weight)
o) df %>% arrange(bmi, height, weight)
o) df %>% select(height, weight, bmi)
o) df %>% filter(bmi, height, weight)
o) df %>% filter(height, weight, bmi)
o) df %>% mutate(bmi, height, weight)
o) df %>% mutate(height, weight, bmi)

{points:3}
?7 Go to the [Chromebook Data Science Space on RStudio Cloud](https://rstudio.cloud/spaces/3919/join?access_code=RUUQ%2BeEgKea0oMF7EJy4UePldyBBMu7d0amv2KFC){target="_blank"} and click on your copy of the 'swirl' project (If you haven't made a copy yet, do so now.) Then, type `swirl()` to get started. Tell Swirl your first name when it asks what to call you. Then, type the number that corresponds to the course `CBDS Data Tidying`. Type the number that corresponds to the lesson `L05 Tidying Data Q01 Swirl`. Do this swirl module! Once complete, paste the code at the end of the lesson here.

! /.*[PNtY||zcwt||rRYH||5ASJ||h37d||ytrE||IUfi||yCgl||SFhU||SVIw].*/i


{points:3}
?8 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q02 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[Mo5t||i1JE||8LsR||vLX3||w69f||0FTs||H6lV||hdFq||VuSA||yM3v].*/i

{points:3}
?9 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q03 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[rHQl||0Z5K||UBOe||41nS||WB94||U4vN||Tukh||PvCX||0Bbb||MtAB].*/i

{points:3}
?10 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q04 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[NuVE||7McM||hwxV||1Ezv||v0U0||c8Mu||KyMD||LKdb||fVen||uGNF].*/i

{points:3}
?11 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q05 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[wQm9||k0Ch||GOeX||Deip||F4qR||uQxa||T73b||8a3O||YX4d||NhvW].*/i

{points:3}
?12 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q06 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[Ijam||uwou||se9I||xtkQ||zUP2||KI93||vkER||pPuI||MHAY||yPeC].*/i

{points:3}
?13 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q07 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[c3LP||2mAv||ehD7||hfyY||ddm4||842l||4SCN||IC9p||lr0w||8PTy].*/i

{points:3}
?14 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q08 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[YJrr||xn6F||9Bpo||DRdg||HZLd||ei67||rbHO||VxD2||lqjr||cMRv].*/i

{points:3}
?15 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q09 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[rGzl||0hoi||i1xp||DhKi||VYmP||gN7N||4t6g||4T9O||z5NB||L1wF].*/i

{points:3}
?16 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q10 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[Zfue||0nKO||ThFp||BSMV||Aqjj||RTBg||ut5K||7Rda||XVvD||MLjG].*/i

{points:3}
?17 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q11 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[s1nk||81qt||bkQB||PewF||XNKs||1yba||1ulO||xaN1||yShU||2nio].*/i

{points:3}
?18 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L05 Tidying Data Q12 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[maPZ||0QE3||gmUp||6PbN||DgtM||yuDr||wiYW||KpHj||OPeU||6l0g].*/i

{/quiz}

{exercise, id: exercise_03_tidyingdata_dplyr}

### DataCamp: Data Manipulation in R with dplyr

{case-sensitive: false}
? To get more practice with tidying data, go to the course [Data Manipulation in R with dplyr](https://www.datacamp.com/courses/dplyr-data-manipulation-r-tutorial) on DataCamp. Login and complete this course. Once you've completed this course, type "completed" in the box below.

*Note*: This requires a DataCamp login, which costs money. As an exercise, this is not required to pass the course but it *will* help you get a better grasp on the concepts covered in these lessons.

*Reminder*: Videos in DataCamp are essential to understanding the material. It's best not to skip over these videos!

! completed

{/exercise}

{exercise, id: exercise_03_tidyingdata_tidyr}

### DataCamp:  Cleaning Data in R

{case-sensitive: false}
? To get more practice with tidying data, go to the course [Cleaning Data in R](https://www.datacamp.com/courses/cleaning-data-in-r) on DataCamp. Login and complete the first two chapters this course:

- Introduction and exploring raw data
- Tidying data

Once you've completed the two chapters in this course, type "completed" in the box below.

*Note*: This requires a DataCamp login, which costs money. As an exercise, this is not required to pass the course but it *will* help you get a better grasp on the concepts covered in these lessons.

*Reminder*: Videos in DataCamp are essential to understanding the material. It's best not to skip over these videos!

! completed

{/exercise}

