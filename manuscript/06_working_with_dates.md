# Working with: dates

In lessons an earlier course, you were introduced to different types of objects in R, such as characters, numeric, and logicals. Then, in earlier lessons in this course, we covered how to work with strings and factors in detail. The remaining type of variable we haven't yet covered is how to work with dates and time in R. 

As with strings and factors, there is a tidyverse package to help you work with dates more easily. The `lubridate` package will make working with dates and times easier. Before working through this lesson, you'll want to be sure that `lubridate` has been installed and loaded in:

```r
install.packages('lubridate')
library(lubridate)
```

### Dates and time basics

When working with dates and times in R, you can consider either **dates**, **times**, or **date-times**. Date-times refer to dates plus times, specifying an exact moment in time. It's always best to work with the simplest possible object for your needs. So, if you don't need to refer to date-times specifically, it's best to work with dates. 

### Creating dates and date-time objects

To get objects into dates and date-times that can be more easily worked with in R, you'll want to get comfortable with a number of functions from the lubridate package. Below we'll discuss how to create date and date-time objects from (1) strings and (2) individual parts.

#### From strings

Date information is often provided as a string. The functions within the `lubridate` package can effectively handle this information. To use them to generate date objects, you can call a function using `y`, `m`, and `d` in the order in which the year (`y`), month (`m`), and date (`d`) appear in your data. The code below produces identical output for the date September 29th, 1988, despite the three distinct input formats. This uniform output makes working with dates much easer in R.

```
ymd("1988-09-29")

mdy("September 29th, 1988")

dmy("29-Sep-1988")
```

![creating date and date-time objects](images/06_working_with_dates/06_datacleaning_working_with_dates-2.png)

However, this has only covered working with date objects. To work with date-time objects, you have to further include hour (`h`), minute(`m`), and second (`s`) into the function. For example, in the code below, you can see that the output contains time information in addition to the date information generated in the functions above:

```r
ymd_hms("1988-09-29 20:11:59")
```

#### From individual parts

If you have a data set where month, date, year, and/or time information are included in separate columns, the functions within `lubridate` can take this separate information and create a date or date-time object. To work through examples using the functions `make_date()` and `make_timedate()`, we'll use a dataset called `nycflights13`. As this dataset is *not* included with the R by default, you'll have to install and load it in directly:

```r
install.packages('nycflights13')
library(nycflights13)
```

Loading this package makes a data frame called `flights`, which includes "on-time data for all flights that departed NYC in 2013," available. We will work with this dataset to demonstrate how to create a date and date-time object from a dataset where the information is spread across multiple columns.

First, to create a new column, as we've done throughout the lessons in this course, we will use `mutate()`. To create a `date` object, we'll use the function `make_date()`. We just then need to supply the names of the columns containing the year, month, and day information to this function. 

```r
## make_date() creates a date object 
## from information in separate columns
flights %>% 
  select(year, month, day) %>% 
  mutate(departure = make_date(year, month, day))
```

![mutate and make_date() create a new column -- departure -- with a date object](images/06_working_with_dates/06_datacleaning_working_with_dates-4.png)

A similar procedure is used to create a date-time object; however, this requires the function `make_datetime()` and requires columns with information about time be specified. Below, `hour` and `minute` are included to the function's input.

```r
## make_datetime() creates a date-time object 
## from information in separate columns
flights %>% 
  select(year, month, day, hour, minute) %>% 
  mutate(departure = make_datetime(year, month, day, hour, minute))
```
![mutate and make_datetime() create a new column -- departure -- with a date-time object](images/06_working_with_dates/06_datacleaning_working_with_dates-5.png)


### Working with dates

The reason we've dedicated an entire lesson to working with dates and have shown you how to create date and date-time objects in this lesson is because you often want to plot data over time or calculate how long something has taken. Being able to accomplish these tasks is an important job for a data scientist. So, now that you know how to create date and date-time objects, we'll work through a few examples of how to work with these objects 

#### Getting components of dates

Often you're most interested in grouping your data by year, or just looking at monthly or weekly trends. To accomplish this, you have to be able to extract just a component of your date object. You can do this with the functions: `year()`, `month()`, `mday()`,`wday()`, `hour()`, `minute()` and `second()`. Each will extract the specified piece of information from the date or date-time object. 

```r
mydate <- ymd("1988-09-29")

## extract year information
year(mydate)

## extract day of the month
mday(mydate)

## extract weekday information
wday(mydate)

## label with actual day of the week
wday(mydate, label = TRUE)
```

![lubridate has specific functions to extract components from date and date-time objects](images/06_working_with_dates/06_datacleaning_working_with_dates-6.png)

### Time spans

In addition to being able to look at trends by month or year, which requires being able to extract that component from a date or date-time object, it's also important to be able to operate over dates.  If I give you a date of birth and ask you how old that person is today, you'll want to be able to calculate that. This is possible when working with date objects. By subtracting this birthdate from today's date, you'll learn now many days old this person is. By specifying this object using `as.duration()`, you'll be able to extract how old this person is in years.

```r
## how old is someone born on Sept 29, 1988
mydate <- ymd("1988-09-29")

## subtract birthday from todays date
age <- today() - mydate
age

## a duration object can get this information in years
as.duration(age)
```

![dates and date-times can be operated upon](images/06_working_with_dates/06_datacleaning_working_with_dates-7.png)

Using addition, subtraction, multiplication, and division is possible with date objects, and accurately takes into account things like leap years and different number of days each month. This capability and the additional functions that exist within lubridate can be enormously helpful when working with dates and date-time objects.

### What's not covered in this lesson

This lesson has not covered how to work with times, much detail about how to operate on date or date-time objects, nor how to deal with timezones in date-time objects. To learn more about these subjects, explore the additional resources below.


### Additional Resources

* [r4ds : Chapter 16 - Dates and times](http://r4ds.had.co.nz/dates-and-times.html) by [Hadley Wickham](http://hadley.nz/)
* [lubridate](http://lubridate.tidyverse.org/), part of the [tidyverse](https://www.tidyverse.org/)
* [hms](https://github.com/tidyverse/hms) package, for working with time objects. This package is also part of the [tidyverse](https://www.tidyverse.org/)

### Slides and Video

![Working with dates](https://www.youtube.com/watch?v=hGx-1Ib6Lic)

* [Slides](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/edit?usp=sharing)


{quiz, id: quiz_06_working_with_dates}

### Working with: dates quiz

? To obtain the day of the week of your birthday, which function would you use?

A) wday()
b) d()
c) date()
d) day()

? What information does `year("1985-05-04") - year("1988-09-29")` give you?

a) the number of months difference between the two dates 
B) the number of years difference between the two dates
c) the number of days difference between the two dates
d) the number of minutes difference between the two dates

? What day of the week was December 28th, 1992?

A) Mon
b) Tues
c) Wed
d) Thurs
e) Fri
f) Sat
g) Sun

{/quiz}

