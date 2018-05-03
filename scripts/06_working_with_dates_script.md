In lessons an earlier course, you were introduced to different types of objects in R, such as characters, numeric, and logicals. Then, in earlier lessons in this course, we covered how to work with strings and factors in detail. The remaining type of variable we haven't yet covered is how to work with dates and time in R. 

As with strings and factors, there is a tidyverse package to help you work with dates more easily. The lubridate package will make working with dates and times easier. Before working through this lesson, you'll want to be sure that lubridate has been installed and loaded in:
  
When working with dates and times in R, you can consider either dates, times, or date-times. Date-times refer to dates plus times, specifying an exact moment in time. It's always best to work with the simplest possible object for your needs. So, if you don't need to refer to date-times specifically, it's best to work with dates. To get objects into dates and date-times that can be more easily worked with in R, you'll want to get comfortable with a number of functions from the lubridate package. Here, we'll discuss how to create date and date-time objects from strings and individual parts. Date information is often provided as a string. The functions within the lubridate package can effectively handle this information. To use them to generate date objects, you can call a function using y, m, and d in the order in which the year, month , and date appear in your data. The code below produces identical output for the date September 29th, 1988, despite the three distinct input formats. This uniform output makes working with dates much easer in R. However, this has only covered working with date objects. To work with date-time objects, you have to further include hour , minute, and second into the function, specified by h, m, and s, respectively. For example, in the code at the bottom of the slide, you can see that the output contains time information in addition to the date information generated in the functions at the top of the slide.

If you have a data set where month, date, year, and/or time information are included in separate columns, the functions within lubridate can take this separate information and create a date or date-time object. To work through examples using the functions make underscore date and make underscore time date, we'll use a dataset called n y c flights 13. As this dataset is not included with the R by default, you'll have to install and load it in directly. Loading this package makes a data frame called flights, which includes "on-time data for all flights that departed NYC in 2013," available. We will work with this dataset to demonstrate how to create a date and date-time object from a dataset where the information is spread across multiple columns.

First, to create a new column, as we've done throughout the lessons in this course, we will use mutate. To create a date object, we'll use the function make underscore date. We just then need to supply the names of the columns containing the year, month, and day information to this function. 

A similar procedure is used to create a date-time object; however, this requires the function make undersocre date time and requires columns with information about time be specified. Here, hour and minute are included to the function's input.

The reason we've dedicated an entire lesson to working with dates and have shown you how to create date and date-time objects in this lesson is because you often want to plot data over time or calculate how long something has taken. Being able to accomplish these tasks is an important job for a data scientist. So, now that you know how to create date and date-time objects, we'll work through a few examples of how to work with these objects. Often you're most interested in grouping your data by year, or just looking at monthly or weekly trends. To accomplish this, you have to be able to extract just a component of your date object. You can do this with the functions: year, month, mday,wday, hour, minute and second. Each will extract the specified piece of information from the date or date-time object. 

In addition to being able to look at trends by month or year, which requires being able to extract that component from a date or date-time object, it's also important to be able to operate over dates.  If I give you a date of birth and ask you how old that person is today, you'll want to be able to calculate that. This is possible when working with date objects. By subtracting this birthdate from today's date, you'll learn now many days old this person is. By specifying this object using as dot duration, you'll be able to extract how old this person is in years. Using addition, subtraction, multiplication, and division is possible with date objects, and accurately takes into account things like leap years and different number of days each month. This capability and the additional functions that exist within lubridate can be enormously helpful when working with dates and date-time objects.

While covering the basics, this lesson has not covered how to work with times, much detail about how to operate on date or date-time objects, nor how to deal with timezones in date-time objects. To gain a deeper understanding, check out Chapter 16 the r for data science book by Hadley Wickham, available at http://r4ds.had.co.nz/.