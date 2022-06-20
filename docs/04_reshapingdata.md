# Reshaping Data

### Data Formats

Tidy data generally exist in two forms: wide data and long data. Both types of data are used and needed in data analysis, and fortunately, there are tools that can take you from wide-to-long and from long-to-wide. This makes it easy to work with any tidy data set. We'll discuss the basics of what wide and long data are and how to go back and forth between the two in R. Getting data into the right format will be crucial later when summarizing data and visualizing it.

#### Wide Data

Wide data has a column for each variable and a row for each observation. Data are often entered and stored in this manner. This is because wide data are often easy to understand at a glance. For example, this is a wide data set:


![Wide dataset](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g2bfdb07292_0_151)

This is a dataset we've looked at in a previous lesson. As discussed previously, it's a rectangular and tidy dataset. Now, we can also state that it is a wide dataset. Here you can clearly see what measurements were taken for each individual and can get a sense of how many individuals are contained in the dataset.

Specifically, each individual is in a different row with each variable in a different column. At a glance we can quickly see that we have information about four different people and that each person was measured in four different ways.

#### Long Data

Long data, on the other hand, has one column indicating the type of variable contained in that row and then a separate row for the value for that variable. Each row contains a single observation for a single variable.  Below is an example of a long data set:


![Long dataset](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_0)

This long dataset includes the exact same information as the previous wide dataset; it is just stored differently. It's harder to see visually how many different measurements were taken and on how many different people, but the same information is there.

While long data formats are less readable than wide data at a glance, they are a lot easier to work with during analysis. Most of the tools we'll be working with use long data. Thus, to go from how data are often stored (wide) to working with the data during analysis (long), we'll need to understand what tools are needed to do this and how to work with them.

### R Packages

Converting your data from wide-to-long or from long-to-wide data formats is referred to as **reshaping** your data. There are two primary packages in R that will help you reshape your data: [tidyr](https://tidyr.tidyverse.org/) and [reshape2](https://stat.ethz.ch/pipermail/r-packages/2010/001169.html). We'll walk through the important functions of these two packages and work through a few examples using the functions in each. However, as with most helpful packages in R, there is more functionality than what is discussed here, so feel free to explore the additional resources at the bottom to learn even more.


![Reshaping data](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_13)

For these examples, we'll work with the `airquality` dataset available in R. The data in this dataset includes "Daily air quality measurements in New York, May to September 1973." This is a wide dataset because each day is in a separate row and there are multiple columns with each including information about a different variable (ozone, solar.r, wind, temp, month, and day).

We can see the first few lines of this dataset using the following code:

```r
head(airquality)
```


![Air quality dataset](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_4)

Again, wide data are easy to decipher at a glance. We can see that we have six different variables for each day, with each one of these variables (measurements) being stored in a separate column.

#### tidyr

Within tidyr, there are two functions to help you reshape your data.

* `gather()`: go from wide data to long data
* `spread()`: go from long data to wide data

To get started, you'll need to be sure that the `tidyr` package is installed and loaded into your RStudio session.

```r
install.packages("tidyr")
library(tidyr)
```

##### gather()

As data are often stored in wide formats, you'll likely use `gather()` a lot more frequently than you'll use `spread()`. This will allow you to get the data into a long format that will be easy to use for analysis.

In `tidyr`, `gather()` will take the `airquality` dataset from wide to long, putting each column name into the first column and each corresponding value into the second column. Here, the first column will be called `key`. The second column will still be `value`.

```r
## use gather() to reshape from wide to long
gathered <- gather(airquality)

## take a look at first few rows of long data
head(gathered)
```


![gather dataset](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_205)

However, it's very easy to change the names of these columns within `gather()`. To do so you define what the key and value columns names should be within `gather()`:

```r
## to rename the column names that gather provides,
## change key and value to what you want those column names to be
gathered <- gather(airquality, key="variable", value="value")

## take a look at first few rows of long data
head(gathered)
```


![gather column names changed](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_213)

However, you're likely not interested in your day and month variable being separated out into their own variables within the `key` column. In fact, knowing the day and month associated with a particular data point helps identify that particular data point. To account for this, you can exclude `day` and `month` from the variables being included in the `key` column by specifying all the variables that you *do* want included in the `key` column.  Here, that means specifying `ozone`, `solar.r`, `wind`, and `temp`. This will keep `day` and `month` in their own columns, allowing each row to be identified by the specific day and month being discussed.

```r
## in gather(), after key and value, you can specify which variables
## you want included in the long format
## it will leave the other variables as is
gathered <- gather(airquality, key="variable", value="value", ozone, solar.r, wind, temp)

## take a look at first few rows of long data
head(gathered)
```


![gather specifying which variables to include in long format](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_222)

Now, when you look at the top of this object, you'll see that `month` and `day` remain in the data frame and that variable combines information from the other columns in airquality (`ozone`, `solar.r`, `wind`, `temp`). This is still a long format dataset; however, it has used `month` and `day` as IDs when reshaping the data frame.

##### spread()

To return your long data back to its original form, you can use `spread()`. Here you specify two columns: the column that contains the names of what your wide data columns should be (`key=variable`) and the column that contains the values that should go in these columns (`value=value`). The data frame resulting from `spread()` will have the original information back in the wide format (again, the columns will be in a different order). But, we'll discuss how to rearrange data in the next lesson!

```r
## use gather() to reshape from wide to long
spread_data <- spread(gathered, key=variable, value=value)

## take a look at the spread data
head(spread_data)

## compare that back to the original
head(airquality)
```


![spread data](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_236)

#### reshape2

As with many things in R, there is more than one way to solve a problem. While `tidyr` provides a more general solution for reshaping data, `reshape2` was specifically designed for reshaping data. The details aren't particularly important yet, but later on as you carry out your own analyses it will be good to know about both packages. To get started using `reshape2`, you'll have to first install the library and load it into your R session:

```r
## install the package
install.packages('reshape2')

## load the package into R Session
library(reshape2)
```

There are two main functions within the `reshape2` package:

* `melt()`: go from wide data to long data
* `dcast()`: go from long data to wide data

##### melt()

The `melt()` function will allow you to get the data into a long format that will be easy to use for analysis. When you melt a dataset with the default options, `melt()` will take every column, put the column name into a `variable` column, and then put the values of those variables into a `value` column. For the `airquality` data set, below we first assign the melted data frame to the object `melted`. Then we take a look at the top (`head()`) and bottom(`tail()`) of this melted data frame (`melted`).

```r
## puts each column name into the 'variable' column
## puts corresponding variable's value in 'value' column
melted <- melt(airquality)

## let's take a look at the top of the melted data frame
head(melted)

## and at the bottom of that melted data frame
tail(melted)
```

When you run this code you see that each column from the original data frame (`ozone`, `solar.r`, `wind`, `temp`,`month`, and `day`) are now repeated in the variables column, and each days' value for that variable is now in the `value` column. This is now a long format dataset!


![melted data](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_43)

Now, to use month and day as identifiers as we did with `tidyr` above, the approach is slightly different. With `gather()`, you specified the column names that you wanted to gather and omitted the column names that you wanted to retain as identifiers. With `melt()` you will do the opposite. You will specify `day` and `month` as identifiers for the dataset and omit the remaining variables. You'll want to use the following syntax:

```r
## melt the data frame
## specify each row using month and day
melted <- melt(airquality, id.vars = c("month","day"))

## look at the first few rows of the melted data frame
head(melted)
```


![melted data frame using IDs](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_53)

Despite the slight change in how the code was specified, the result here using `melt()` is the same as what was achieved above using `gather()`.

##### cast

You'll likely have to go from long-to-short format less frequently; however, it's good to know there are two approaches to accomplishing this within `reshape2` whenever it is necessary.

* `acast()`: taking a long frame and returning a matrix/array/vector
* `dcast()`: taking a long frame and returning a data frame

To return our melted data back into its original wide form, we'll use `dcast()`. The syntax here separates what should be used as an identifier for each row in the resulting wide format (`month + day` below) and which column includes the values that should be the column headers (`variable` below). These two pieces of information are separated by a tilde (`~`).

```r
## to get our data back to its original form
## specify which columns should be combined to use as identifiers
## and which column should be used to specify the columns
original <- dcast(melted, month + day ~ variable)

head(original)

head(airquality)
```


![dcast to obtain original data](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/export/png?id=14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI&pageid=g38bb68a532_0_148)

As you can see, aside from the column order changing, the information in `original` is the same as what was in the data frame we started with (`airquality`).

While reshaping data may not be the most exciting topic, having this skill will be indispensable as you start working with data. It's best to get these skills down early!

### Additional Resources

* [tidyr](https://tidyr.tidyverse.org/), part of the [tidyverse](https://www.tidyverse.org/) and developed by [Hadley Wickham](http://hadley.nz/) and [Lionel Henry](https://github.com/lionel-)
* [reshape2](https://stat.ethz.ch/pipermail/r-packages/2010/001169.html), developed by [Hadley Wickham](http://hadley.nz/)
* [tidyr tutorial](https://blog.rstudio.com/2014/07/22/introducing-tidyr/) by [Hadley Wickham](http://hadley.nz/)
* [reshape2 tutorial](http://seananderson.ca/2013/10/19/reshape/) by [Sean C. Anderson](http://seananderson.ca/)
* [tidyr vs reshape2](https://www.r-bloggers.com/2016/06/how-to-reshape-data-in-r-tidyr-vs-reshape2/) by [Alberto Giudici](https://www.erim.eur.nl/people/alberto-giudici/)

### Slides and Video

![Reshaping Data](https://youtu.be/4Y-AdchVDXo)

* [Slides](https://docs.google.com/presentation/d/14msuN3MbQE6BSIaNu2ipv1-5ypgvWlxsGwn3jmpFyAI/edit?usp=sharing)
