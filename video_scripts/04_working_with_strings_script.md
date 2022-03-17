Strings were introduced in an earlier lesson; however, to review briefly here: 

a string is a sequence of characters, letters, numbers or symbols.

string r is a package within the tidyverse specifically designed to work well with strings. All functions within this package start with str underscore. There are many helpful functions within the string r package. We'll only review the basics here, but if you're looking to accomplish something with a string and aren't sure how to approach it, the string r package is a good first place to look. You'll want to be sure you've installed string r using install dot packages and loaded it into R using hte library function before starting this lesson. The best way to work through this lesson is to copy and paste every line of code into your R Studio window and see if the output makes sense to you. Working with strings and regular expressions is best learned by practice. 

We'll begin by discussing the available functions within string r. As we'll only cover a few of the functions within string r in this lesson, it's important to remember that if you start typing str underscore within R Studio, a list of the many options will show up.

When working with strings, some of the most frequent tasks you'll need to complete are to  determine the length of a string, to combine strings together, and to subset strings. We'll begin with an object comprised of three strings. We can determine the length of each string in the vector using this code. Here we see that the first string has a length of 26. If you were to go back and count the characters in the first string, you would see that this 26 includes each letter, space, and period in that string. The length of a string does not just could the letters in its length. The length includes every character. The second and third strings each have length 12. 

If you were interested in combining strings, you'd want to use str underscore c. We the first bit of code here, the output from this doesn't look quite right. You may want a space between these two words when you combine the two strings. That can be controlled with the sep argument within str underscore c.

Often, it's important to get part of a string out. To do this, you'll want to subset the string using the str underscore sub function. For example, if you wanted only the first three characters in the string below, you would specify that within str underscore sub.

You can also use negative numbers to count from the end of the string. For example, here we see code that returns the last three positions in the string.

Finally, if you wanted to sort a string alphabetically, str underscore sort can help you accomplish that.

So far we have discussed the basics of working with strings within string r. However, working with strings becomes infinitely easier with an understanding of regular expressions. Regular expressions, abbreviated to regular expressions, are used to describe patterns within strings. They can take a little while to get the hang of but become very helpful once you do. With regular expressions, instead of specifying that you want to extract the first three letters of a string (as we did earlier), you could more generally specify that you wanted to extract all strings that start with a specific letter or that contain a specific word somewhere in the string using regular expressions. We'll explore the basics of regular expressions here. The use them in string r, the general format is open parentheses string comma pattern equals sign regexp close parentheses, which you'll see used in practice throughout this lesson. We'll cover a number of helpful string r functions in the rest of the lesson. To begin, if interested in finding a pattern at the beginning or end of a string, you can specify that using a regexp. For example, if you wanted to only look at names that started with the letter M, you would specify that using a regexp. The pattern you would include would be caret M to identify all strings that start with the letter M. To specify those strings that end with a capital M, you would specify the pattern dollar sign M. We'll walk through these examples now.

To get comfortable with using regular expressions with strings, str underscore view can be very helpful. The output from str underscore view highlights what portion of your string match the pattern specified in your regular expression with a gray box. For example, to we'll start using anchors and str underscore view. In this first example we see in the Viewer Panel that str_view has identified the names that start with the letter M.

However, if you try to match strings that end with the letter "M", no match is found.

To identify names by that end with the letter "a", you would use this syntax.

Note, however, that regular expressions are case sensitive. To match patterns, you have to consider that "A" and "a" are different characters.  

To count the number of matches within your strings, you would use str underscore count. Here, using the names vector we've been using, we see that str underscore count produces a 1 for those names that start with M and a 0 otherwise.

However, if we instead wanted a count of the numbers of lowercase "m"s, we could still use str underscore count function to accomplish that. Notice here we've removed the specification to just look at the beginning of the string. Here, we're looking for lowercase m's anywhere in the string and counting them:
 
Instead of returning a count, at times you're just interested in knowing which strings match the pattern you're searching for. In these cases you'll want to use str underscore detect. This function simply returns a TRUE if the string matches the pattern specified and FALSE otherwise.

To return the actual string that matches the specified pattern, rather than a TRUE or FALSE, you'll look to str underscore subset. This function pulls out those strings that match the specified pattern. For example, to obtain the subset of names whose values start with the capital letter "M", you would use the following:

To extract only the portions of the string that match the specified pattern, you would use str underscore extract. This function returns the pattern specified for strings where it is found and N A otherwise. For example, by searching for names that start with M, below, we see that the second and fourth strings in our vector return the pattern specified and that the first and third strings in the vector return NA, as they do not start with a capital "M".

The final basic function from string r that we'll discuss is str underscore replace. This function identifies a regex and replaces each occurrence with whatever replacement the user specifies. For example, below we search for strings that start with the capital letter "M" and replace each of them with a question mark. All strings that do not match the regex are returned unchanged.

Above we discuss two common patterns searched for using regular expressions: starts with, which is specified by the caret symbol, and ends with, which is specified by a dollar sign. However, there are a number of additional common ways to match patterns. They are summarized here, and we'll discuss each one in slightly more detail now.

To search for a set of characters, you place these characters within brackets. Here, we use this syntax identify anywhere in the strings where you have a lowercase vowel. Note, that we're now using str underscore view underscore all to identify all occurrences of these characters, rather than str underscore view, which only identifies the first occurrence in each string.

To search for anything other than a specified set of characters, you will add a caret  before the vowels within the brackets. This regular expressions specifies that you are searching for any character that is not a lowercase vowel within your strings.

To search for digits (numeric variable between 0 and 9) in a string you use backslash d; however, backslashes are protected characters in R. This means that you have to escape this character first with an additional backslash , to let R know that you want to search for the digit regular expression.

Identifying whitespace in R identifies any spaces, tabs or newlines. Note that again we have to escape the backlash s with a backslash for R to recognize the regular expression

To identify any character except for a newline you'll use a period. Notice in our addresses example that there are no newlines, so this pattern will match with the entire string. 

Searches for regular expressions allow you to specify how many times a pattern should be found within the string. We'll discuss a few approaches to addressing repetition within regular expressions here.

Using the definitions on the last slide, we can see that the following code will identify patterns within the addresses vector where n shows up one more more times in a string. 

While the difference is slight in the output here, we're identifying portions of the string where n shows up exactly once. So, instead of the 'nn' in Pennsylvania matching together, the code here splits these up, due to the fact that we're specifying the pattern match 'n' exactly one time

If you only wanted to match strings where n showed up twice in a row, you could specify that in this way.

This could similarly be achieved by specifying to search for the pattern 'nn' one or more times using a plus sign.

You can also specify a range of the number of times to search for a pattern within your string. Below, we see that if we specify n be searched for at least two and at most 3 times, the pattern matches within our string. However, if we increase that to between three and four times, no pattern matching occurs, as there are never three or four n's in a row in our strings.

This lesson set out to introduce you to how to work with strings within R Studio, using the string r package and to introduce you to regular expressions. We've covered a number of functions and concepts in this lesson. Feel free to review the material and practice as much as you need before completing this quiz!