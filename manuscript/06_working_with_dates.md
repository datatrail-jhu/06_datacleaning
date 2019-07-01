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

{format: png}
![creating date and date-time objects](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/export/png?id=1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c&pageid=g39252087a2_0_22)

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

{format: png}
![mutate and make_date() create a new column -- departure -- with a date object](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/export/png?id=1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c&pageid=g39252087a2_0_0)

A similar procedure is used to create a date-time object; however, this requires the function `make_datetime()` and requires columns with information about time be specified. Below, `hour` and `minute` are included to the function's input.

```r
## make_datetime() creates a date-time object 
## from information in separate columns
flights %>% 
  select(year, month, day, hour, minute) %>% 
  mutate(departure = make_datetime(year, month, day, hour, minute))
```
{format: png}
![mutate and make_datetime() create a new column -- departure -- with a date-time object](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/export/png?id=1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c&pageid=g39252087a2_0_4)


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

{format: png}
![lubridate has specific functions to extract components from date and date-time objects](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/export/png?id=1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c&pageid=g39252087a2_0_27)

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

{format: png}
![dates and date-times can be operated upon](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/export/png?id=1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c&pageid=g39252087a2_0_37)

Using addition, subtraction, multiplication, and division is possible with date objects, and accurately takes into account things like leap years and different number of days each month. This capability and the additional functions that exist within lubridate can be enormously helpful when working with dates and date-time objects.

### What's not covered in this lesson

This lesson has not covered how to work with times, much detail about how to operate on date or date-time objects, nor how to deal with timezones in date-time objects. To learn more about these subjects, feel free to explore the additional resources below, or hold off for a later course in a different course set!


### Additional Resources

* [r4ds : Chapter 16 - Dates and times](http://r4ds.had.co.nz/dates-and-times.html) by [Hadley Wickham](http://hadley.nz/)
* [lubridate](http://lubridate.tidyverse.org/), part of the [tidyverse](https://www.tidyverse.org/)
* [hms](https://github.com/tidyverse/hms) package, for working with time objects. This package is also part of the [tidyverse](https://www.tidyverse.org/)

### Slides and Video

![Working with dates](https://www.youtube.com/watch?v=_eutzRAVabQ)

* [Slides](https://docs.google.com/presentation/d/1iLU-H6-GZw7EhL2syFZnjn_tC_cI6s9PmKRxcSOjg1c/edit?usp=sharing)


{quiz, id: quiz_06_working_with_dates}

### Working with: dates quiz

{choose-answers: 4}
?1 To obtain the day of the week of your birthday, which function would you use?

C) wday()
m) mday()
o) d()
o) date()
o) day()
o) Day()
o) weekday()
o) birthday()
o) bday()

{choose-answers: 4}
?1 To obtain the date of the month of your birthday, which function could you use?

C) mday()
m) wday()
o) d()
o) date()
o) day()
o) Day()
o) weekday()
o) birthday()
o) bday()

{choose-answers: 4}
?2 What information does `year("1985-05-04") - year("1988-09-29")` give you?

C) the number of years difference between the two dates
o) the number of days difference between the two dates
o) the number of minutes difference between the two dates
o) the number of seconds difference between the two dates
o) the number of months difference between the two dates 
o) the years 1985 and 1988 added together 


{choose-answers: 4, points: 2}
?3 What code would you use to tell you what day of the week December 28th, 1992 was?

C) wday(ymd("1992-12-28"))
C) wday(ydm("1992-28-12"))
m) wday(ymd("1992-28-12"))
o) wday(ydm("1992-12-28"))
o) mday(ymd("1992-12-28"))
o) mday(ydm("1992-28-12"))
o) mday(ymd("1992-28-12"))
o) mday(ydm("1992-12-28"))

{choose-answers: 4, points: 2}
?3 What code would you use to tell you what day of the week May 4th, 1985 was?

C) wday(ymd("1985-05-04"))
C) wday(ydm("1985-04-05"))
m) wday(ymd("1985-04-05"))
o) wday(ydm("1985-05-04"))
o) mday(ymd("1985-05-04"))
o) mday(ydm("1985-04-05"))
o) mday(ymd("1985-04-05"))
o) mday(ydm("1985-05-04"))

{choose-answers: 4}
?4 What day of the week was December 28th, 1992?

C) Mon
m) Sat
o) Tues
o) Wed
o) Thurs
o) Fri
o) Sun

{choose-answers: 4}
?4 What day of the week was May 4th, 1985?

C) Sat
m) Mon
o) Tues
o) Wed
o) Thurs
o) Fri
o) Sun

{points:3}
?5 Go to the [Cloud-based Data Science Space on RStudio Cloud](https://rstudio.cloud/spaces/20345/join?access_code=n4b8J1s0XmWctSy83%2BEXbGAwj7rKcuFMI7WJEJFD) and click on your copy of the 'swirl' project (If you haven't made a copy yet, do so now.) Then, type `swirl()` to get started. Tell Swirl your first name when it asks what to call you. Then, type the number that corresponds to the course `CBDS Data Tidying`. Type the number that corresponds to the lesson `L08 Working With Dates Q01 Swirl`. Do this swirl module! Once complete, paste the code at the end of the lesson here.

! /.+(LB51|VieR|uI3f|HhIT|NHRJ|ClbO|jCCy|VYKM|0IMK|AAwD).+/i


{points:3}
?6 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L08 Working With Dates Q02 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.+(HFgx|8POP|GAXk|GY2t|mhna|7kqz|NR1J|TFpX|K6kS|8qsA).+/i

{/quiz}
