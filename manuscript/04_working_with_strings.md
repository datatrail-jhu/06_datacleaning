# Working with: strings

### Strings review

Strings were introduced in an earlier lesson; however, to review briefly here: 

> A string is a sequence of characters, letters, numbers or symbols.

So within R, you could create a string using this syntax. Note that the string begins and ends with quotation marks:

```r
stringA <- "This sentence is a string."
```

Multiple strings can be stored within vectors. So, if you have multiple vectors that you want to store in a single object, you could do so by using c() around the strings you want to store and commas to separate each individual string:

```r
objectA <- c( "This sentence is a string.", "Short String", "Third string" )
```

### stringr

[stringr](http://stringr.tidyverse.org/) is a package within the [tidyverse](https://www.tidyverse.org/) specifically designed to work well with strings. All functions within this package start with `str_`, as you'll see below. There are *many* helpful functions within the stringr package. We'll only review the basics here, but if you're looking to accomplish something with a string and aren't sure how to approach it, the stringr package is a good first place to look. To install and load the stringr package, you'll use the following:

```r
# If not already installed, you'll need to install the package
install.packages("stringr")

# load package into R
library(stringr)
```

The best way to work through this lesson is to copy and paste every line of code into your RStudio window and see if the output makes sense to you. Working with strings and regular expressions is best learned by practice. 

#### Available functions

As we'll only cover a few of the functions within stringr in this lesson, it's important to remember that if you start typing "str_" within RStudio, a list of the many options will show up.

![str_ image](images/04_working_with_strings/04_datacleaning_working_with_strings-3.png)

### String basics

When working with strings, some of the most frequent tasks you'll need to complete are to:
* determine the length of a string
* combine strings together
* subset strings


#### String length

Returning to our object with three strings from earlier in the lesson, we can determine the length of each string in the vector.  

```r
objectA <- c( "This sentence is a string.", "Short String", "Third string" )

str_length(objectA)
```

![str_length output](images/04_working_with_strings/04_datacleaning_working_with_strings-4.png)

Here we see that the first string has a length of 26. If you were to go back and count the characters in the first string, you would see that this 26 includes each letter, space, and period in that string. The length of a string does not just could the letters in its length. The length includes every character. The second and third strings each have length 12. 

#### Combining strings: `str_c()`

If you were interested in combining strings, you'd want to use `str_c`.

```r
str_c( "Good", "Morning")
```

![str_c](images/04_working_with_strings/04_datacleaning_working_with_strings-5.png)

However, the output from this doesn't look quite right. You may want a space between these two words when you combine the two strings. That can be controlled with the `sep` argument.

```r
str_c( "Good", "Morning", sep=" ")
```

#### Subsetting strings: `str_sub()`

Often, it's important to get on part of a string out. To do this, you'll want to subset the string using the `str_sub()` function. For example, if you wanted only the first three characters in the string below, you would specify that within `str_sub()`.

```r
object <- c( "Good", "Morning")

str_sub(object, 1, 3)
```

![str_sub output](images/04_working_with_strings/04_datacleaning_working_with_strings-6.png)

You can also use negative numbers to count from the end of the string. For example, below we see code that returns the last three positions in the string.

```r
object <- c( "Good", "Morning")

str_sub(object, -3, -1)
```
![str_sub output counting from end of string](images/04_working_with_strings/04_datacleaning_working_with_strings-7.png)

#### String sorting: `str_sort()`

Finally, if you wanted to sort a string alphabetically, `str_sort()` can help you accomplish that.

```r
names <- c("Keisha", "Mohammed", "Jane")

str_sort(names)
```

![str_sort() output sorts strings](images/04_working_with_strings/04_datacleaning_working_with_strings-8.png)


### Regular expressions

Above we discuss the basics of working with strings within `stringr`. However, working with strings becomes infinitely easier with an understanding of regular expressions. Regular expressions (regexps) are used to **describe patterns within strings**. They can take a little while to get the hang of but become very helpful once you do. With regexps, instead of specifying that you want to extract the first three letters of a string (as we did above), you could more generally specify that you wanted to extract all strings that start with a specific letter or that contain a specific word somewhere in the string using regexps. We'll explore the basics of regexps here.

The use them in `stringr`, the general format is `function(string , pattern = regexp)`, which you'll see used in practice below. The set of functions from `stringr` we'll cover are listed below 

We'll cover a number of helpful `stringr` functions:

* **str_view()** - View the first occurrence in a string that matches the regex
* **str_view_all()** - View all occurrences in a string that match the regex
* **str_count()** - count the number of times a regex matches within a string
* **str_detect()** - determine if regex is found within string
* **str_subset()** - return subset of strings that match the regex
* **str_extract()** - return portion of each string that matches the regex
* **str_replace()** - replace portion of string that matches the regex with something else

#### Anchors

If interested in finding a pattern at the beginning (`^`) or end (`$`) of a string, you can specify that using a regexp. For example, if you wanted to only look at names that started with the letter "M", you would specify that using a regexp. The pattern you would include would be `"^M"` to identify all strings that start with the letter M. To specify those strings that end with a capital M, you would specify the pattern `"$M"`.


#### Show matches: str_view()

To get comfortable with using regexps with strings, `str_view()` can be very helpful. The output from `str_view()` highlights what portion of your string match the pattern specified in your regexp with a gray box. For example, to we'll start using anchors and `str_view()` below:

```r
names <- c("Keisha", "Mohammed", "Jane", "Mathieu")

## identify strings that start with "M"
str_view(names, "^M")
```

![`str_view()` identifies names that start with M](images/04_working_with_strings/04_datacleaning_working_with_strings-10.png)

In this first example we see in the Viewer Panel that str_view has identified the names that start with the letter M.

However, if you try to match strings that end with the letter "M", no match is found.

```r
## identify strings that end with "M"
str_view(names, "M$")
```

![`str_view()` does not identify any names that end with M](images/04_working_with_strings/04_datacleaning_working_with_strings-11.png)

To identify names by that end with the letter "a", you would use the following.

```r
## identify strings that end with "a"
str_view(names, "a$")
```

![`str_view()` identifies names that end with a](images/04_working_with_strings/04_datacleaning_working_with_strings-12.png)

Note, however, that regexps are case sensitive. To match patterns, you have to consider that "A" and "a" are different characters.  

```r
## identify strings that end with "A"
str_view(names, "A$")
```

![`str_view()` does not identify any names that end with A](images/04_working_with_strings/04_datacleaning_working_with_strings-13.png)

#### Count matches: `str_count()`

To count the number of matches within your strings, you would use `str_count()`. Below, using the names vector we've been using, we see that `str_count()` produces a 1 for those names that start with "M" and a 0 otherwise.

```r
## identify strings that start with "M"
## return count of the number of times string matches pattern
str_count(names, "^M")
```

![str_count() strings that start with "M"](images/04_working_with_strings/04_datacleaning_working_with_strings-14.png)

However, if we instead wanted a count of the numbers of lowercase "m"s, we could still use `str_count()` to accomplish that. Notice below we've removed the specification to just look at the beginning of the string. Here, we're looking for lowercase m's anywhere in the string and counting them:

```r
## identify strings that have a lowercase "m"
## return count of the number of times string matches pattern
str_count(names, "m")
```

![str_count() strings that have an m in them](images/04_working_with_strings/04_datacleaning_working_with_strings-15.png)

#### Detect matches: `str_detect()`

Instead of returning a count, at times you're just interested in knowing which strings match the pattern you're searching for. In these cases you'll want to use `str_detect()`. This function simply returns a TRUE if the string matches the pattern specified and FALSE otherwise.

```r
## identify strings that start with "M"
## return TRUE if they do; FALSE otherwise
str_detect(names, "^M")
```

![str_detect() returns TRUE for strings that match the specified pattern; FALSE otherwise](images/04_working_with_strings/04_datacleaning_working_with_strings-16.png)


#### Subset matches: `str_subset()`

To return the actual string that matches the specified pattern, rather than a TRUE/FALSE, you'll look to `str_subset()`. This function pulls out those strings that match the specified pattern. For example, to obtain the subset of names whose values start with the capital letter "M", you would use the following:

```r
## identify strings that start with "M"
## return whole string
str_subset(names, "^M")
```

![str_subset() returns the strings that match the pattern specified](images/04_working_with_strings/04_datacleaning_working_with_strings-17.png)

#### Extract matches: `str_extract()`

To extract only the portions of the string that match the specified pattern, you would use `str_extract()`. This function returns the pattern specified for strings where it is found and NA otherwise. For example, by searching for names that start with M, below, we see that the second and fourth strings in our vector return the pattern specified ("M") and that the first and third strings in the vector return NA, as they do not start with a capital "M".

```r
## return "M" from strings with "M" in it
## otherwise, return NA
str_extract(names, "^M")
```

![str_extract() returns the portions of the strings that match the pattern specified](images/04_working_with_strings/04_datacleaning_working_with_strings-18.png)

#### Replace matches: `str_replace()`

The final basic function from `stringr` that we'll discuss is `str_replace()`. This function identifies a regex and replaces each occurrence with whatever replacement the user specifies. For example, below we search for strings that start with the capital letter "M" and replace each of them with a question mark. All strings that do *not* match the regex are returned unchanged.

```r
## replace capital M with a question mark
str_replace(names, "^M", "?")
```

![str_replace() replaces regex with specified characters](images/04_working_with_strings/04_datacleaning_working_with_strings-19.png)

#### Common regular expressions

Above we discuss two common patterns searched for using regular expressions: starts with (`^`) and ends with (`$`). However, there are a number of additional common ways to match patterns. They are listed here, and we'll discuss each one in slightly more detail below.



##### Searching for characters

To search for a set of characters, you place these characters within brackets. Below, this will identify anywhere in the strings where you have a lowercase vowel. Note, that we're now using `str_view_all()` to identify all occurrences of these characters, rather than `str_view()`, which only identifies the first occurrence in each string.

```r
## identify all lowercase vowels
str_view_all(names, "[aeiou]")
```

![brackets specify which characters to search for](images/04_working_with_strings/04_datacleaning_working_with_strings-21.png)


##### Searching for anything *other* than a set of characters

By adding a caret (`^`) before the vowels within the brackets, this regular expressions specifies that you are searching for any character that is not a lowercase vowel within your strings.

```r
## identify anything that's NOT a lowercase vowel
str_view_all(names, "[^aeiou]")
```

![brackets with a caret first specify which characters NOT to search for](images/04_working_with_strings/04_datacleaning_working_with_strings-22.png)


##### Search for digits

To search for digits (numeric variable between 0 and 9) in a string you use "\d"; however, backslashes are protected characters in R. This means that you have to escape this character first with an additional backslash (`\`), to let R know that you want to search for the regular expression "\d".

```r
addresses <- c("1234 Main Street", "1600 Pennsylvania Ave", "Brick Building")

## identify anything that's a digit
str_view_all(addresses, "\\d")
```

![\d searches for digits](images/04_working_with_strings/04_datacleaning_working_with_strings-23.png)

##### Search for whitespace

Identifying whitespace in R identifies any spaces, tabs or newlines. Note that again we have to escape the "\s" with a backslash for R to recognize the regular expression.

```r
## identify any whitespace
str_view_all(addresses, "\\s")
```

![\s searches for whitespace](images/04_working_with_strings/04_datacleaning_working_with_strings-24.png)


##### Identify any character (except newline)

To identify any character except for a newline you'll use `"."`. Notice in our addresses example that there are no newlines, so this pattern will match with the entire string. 

```r
## identify any character
str_view_all(addresses, ".")
```

![. searches for any character](images/04_working_with_strings/04_datacleaning_working_with_strings-25.png)


#### Repetition within regular expressions

Searches for regular expressions allow you to specify how many times a pattern should be found within the string. To do so, you use the following:

* ? : 0 or 1
* + : 1 or more
* \\* : 0 or more

* {n} : exactly n times
* {n,} : n or more times
* {n,m} : between n and m times

##### Examples of repetition within regular expressions

Using the definitions above, we can see that the following code will identify patterns within the addresses vector where n shows up one more more times in a string. 

```r
## identify any time n shows up one or more times
str_view_all(addresses, "n+")
```

![+ specifies to match the pattern one or more times](images/04_working_with_strings/04_datacleaning_working_with_strings-27.png)


While the difference is slight in the output here, we're identifying portions of the string where n shows up exactly once. So, instead of the 'nn' in Pennsylvania matching together, the code here splits these up, due to the fact that we're specifying the pattern match 'n' exactly one time:

```r
## identify any time n shows up
str_view_all(addresses, "n{1}")
```

![{#} looks to match the pattern exactly the number of times within the curly braces](images/04_working_with_strings/04_datacleaning_working_with_strings-28.png)

If you only wanted to match strings where n showed up twice in a row, you could specify that in this way:

```r
## identify any time n shows up exactly two times in a row
str_view_all(addresses, "n{2}")
```

![{2} specifies that the pattern must be found exactly twice](images/04_working_with_strings/04_datacleaning_working_with_strings-29.png)

This could similarly be achieved by specifying to search for the pattern 'nn' one or more times (`+`):

```r
## identify any time 'nn' shows up one or more times 
str_view_all(addresses, "nn+")
```
![nn+ searches for double n one or more times in a string](images/04_working_with_strings/04_datacleaning_working_with_strings-30.png)

You can also specify a range of the number of times to search for a pattern within your string. Below, we see that if we specify n be searched for at least two and at most 3 times, the pattern matches within our string. However, if we increase that to between three and four times, no pattern matching occurs, as there are never three or four n's in a row in our strings.

```r
## identify any time n shows up two or three times 
str_view_all(addresses, "n{2,3}")

## identify any time n shows up three or four times 
str_view_all(addresses, "n{3,4}")
```

![{n,m} looks to pattern match between n and m times](images/04_working_with_strings/04_datacleaning_working_with_strings-31.png)

### Conclusion

This lesson set out to introduce you to how to work with strings within RStudio, using the stringr package and to introduce you to regular expressions. We've covered a number of functions and concepts in this lesson. Feel free to review the material and practice as much as you need before completing this quiz!

### Additional Resources

* [r4ds : Chapter 14 - Strings](http://r4ds.had.co.nz/strings.html) by [Hadley Wickham](http://hadley.nz/)
* [stringr documentation](http://stringr.tidyverse.org/articles/regular-expressions.html), part of the [tidyverse](https://www.tidyverse.org/)


### Slides and Video

![Working with strings](https://www.youtube.com/watch?v=r6SVAIajWjk)

* [Slides](https://docs.google.com/presentation/d/1-6k6usRR06dp-YK-M3HUOUgQZKXXqKnAD5RC4TISGhw/edit?usp=sharing)


{quiz, id: quiz_04_working_with_strings}

### Working with: strings quiz

The best way to learn how to work with strings is to actually practice working with them. Use the following object to answer the questions on this quiz:

```r
colors <- c('red','orange','yellow','green','blue','violet','#C8C8C8','#000000')
```
Note: be sure you scroll to copy the entire vector. It ends with a closed parenthesis.

{choose-answers: 4, points: 2}
?1 To return the value 'TRUE' for each string that contains numbers in the object `colors`, which would you use?

C) str_detect(colors, '\\d')
m) str_subset(colors, '[0-9]')
o) str_extract(colors, '\\d')
o) str_extract(colors, '[0-9]')
o) str_view(colors, '\\d')
o) str_view_all(colors, '\\d')

{choose-answers: 4, points: 2}
?1 To return the values of each string that contains numbers in the object `colors`, which would you use?

C) str_subset(colors, '[0-9]')
m) str_detect(colors, '\\d')
o) str_extract(colors, '\\d')
o) str_extract(colors, '[0-9]')
o) str_view(colors, '\\d')
o) str_view_all(colors, '\\d')

{choose-answers: 4, points: 2}
?2 To print the values of the `colors` object that start with a lowercase letter, which would you use?

C) str_subset(colors, '^[a-z]')
m) str_count(colors, '[a-z]')
o) str_view(colors, '[a-z]')
o) str_replace(colors, '[a-z]', '?')
o) str_extract(colors, '\\d')
o) str_extract(colors, '[0-9]')
o) str_view(colors, '\\d')
o) str_view_all(colors, '\\d')

{choose-answers: 4, points: 2}
?2 To print the number of lowercase letters in each string within `colors`, which would you use?

C) str_count(colors, '[a-z]')
m) str_subset(colors, '^[a-z]')
o) str_view(colors, '[a-z]')
o) str_replace(colors, '[a-z]', '?')
o) str_extract(colors, '\\d')
o) str_extract(colors, '[0-9]')
o) str_view(colors, '\\d')
o) str_view_all(colors, '\\d')

{choose-answers: 4, points: 2}
?3 What does `str_view_all(colors, "\\d+[A-Z]+")` match?

C) at least one digit followed by at least one capital letter
C) one or more digits followed by one or more capital letters
o) any uppercase letter
o) one digit followed by one capital letter
o) any digit
o) any uppercase letter followed by a digit
o) a string consisting of only uppercase letters

{choose-answers: 4, points: 2}
?4 What does `str_count(colors, '[a-zA-Z]')` accomplish?

C) counts the number of letters in each string
C) prints the number of letters in each string
o) counts the number of characters in each string
o) counts the number of lowercase letters in each string
o) counts the number of capital letters in each string
o) counts the number of digits in each string

{choose-answers: 4, points: 2}
?5 If you were to identify which strings start with `#`, what would you use?

C) str_view_all(colors, '^#')
m) str_view_all(colors, 'w$')
o) str_view_all(colors, '#$')
o) str_view_all(colors, '$#')
o) str_view_all(colors, '#^')
o) str_view_all(colors, '!#')
o) str_view_all(colors, '#!')

{choose-answers: 4, points: 2}
?5 If you were to identify which strings ends with `w`, what would you use?

C) str_view_all(colors, 'w$')
m) str_view_all(colors, '^#')
o) str_view_all(colors, '#$')
o) str_view_all(colors, '$#')
o) str_view_all(colors, '#^')
o) str_view_all(colors, '!#')
o) str_view_all(colors, '#!')

{points: 2}
?6 Using, str_view_all(), how would you only highlight strings that start with a vowel?

! str_view_all(colors, '^[aeiou]')

{points:3}
?7 Go to the [Chromebook Data Science Space on RStudio Cloud](https://rstudio.cloud/spaces/3919/join?access_code=RUUQ%2BeEgKea0oMF7EJy4UePldyBBMu7d0amv2KFC) and click on your copy of the 'swirl' project (If you haven't made a copy yet, do so now.) Then, type `swirl()` to get started. Tell Swirl your first name when it asks what to call you. Then, type the number that corresponds to the course `CBDS Data Tidying`. Type the number that corresponds to the lesson `L06 Working With Strings Q01 Swirl`. Do this swirl module! Once complete, paste the code at the end of the lesson here.

! /.*[rsz6||l06u||eC3R||fnP4||5KIw||JBNP||Cgzr||sM2P||ftet||2dMS].*/i


{points:3}
?8 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L06 Working With Strings Q02 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[1Rzf||QFqK||tW1C||ISNC||KYiI||J3j0||bpYg||8kZQ||mun1||QvfN].*/i

{points:3}
?9 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L06 Working With Strings Q03 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[nb4G||OrwM||xpZo||gJUC||SHAA||Bmw3||eT4R||kCzI||4CC7||Uhsq].*/i

{points:3}
?10 Within the same course on swirl: `CBDS Data Tidying`, navigate to the lesson `L06 Working With Strings Q04 Swirl`. Do this swirl module! Once complete, paste the code provided at the end of the swirl module here.

! /.*[mjKw||YSyq||pKwL||dSzP||CXwj||lppJ||26dw||g9tL||bte7||jvkU].*/i


{/quiz}

{exercise, id: exercise_04_working_with_strings}

### DataCamp:  Working with Strings

{case-sensitive: false}
? To get more practice working with strings, go to the course [String Manipulation in R with stringr](https://www.datacamp.com/courses/string-manipulation-in-r-with-stringr) on DataCamp. Login and complete the first two chapters this course:

- String basics
- Introduction to stringr

Once you've completed the two chapters in this course, type "completed" in the box below.

*Note*: This requires a DataCamp login, which costs money. As an exercise, this is not required to pass the course but it *will* help you get a better grasp on the concepts covered in these lessons.

*Reminder*: Videos in DataCamp are essential to understanding the material. It's best not to skip over these videos!

! completed

{/exercise}

