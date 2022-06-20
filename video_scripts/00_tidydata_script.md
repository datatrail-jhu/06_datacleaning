The idea of tidy data was formalized in 2014 in a paper written by a leader in the data science field, Hadley Wickham. The principles of tidy data, which are discussed below, provide a standard way of formatting a data set. A tidy dataset follows a number of rules relating to how rows, columns, and spreadsheets are matched up with observations, variables, and types. Tidy datasets, by design, are easier to manipulate, model, and visualize. By starting with data that are already in a tidy format or by spending the time at the beginning of a project to get data into a tidy format, the remaining steps of your data science project will be easier.

We've previously discussed what the rows and columns in a spreadsheet are. Here, we'll discuss what is meant by observations, variables, and types, all of which are used to explain the principles of tidy data. Variables in a spreadsheet are the different categories of data that will be collected. They are the different pieces of information that can be collected or measured on each person. Here, we see there are 7 different variables: ID, Last Name, First Name, Sex, City, State, and Occupation. The names for variables are put in the first row of the spreadsheet.

Observations are the measurements taken from a person for each variable are called observations. Observations for each individual are stored in a single row, with each observation being put in the appropriate column for each variable. 

Often, data are collected for the same individuals from multiple sources. For example, when you go to the doctor's office, you fill out a survey about yourself. That would count as one type of data. The measurements a doctor collects at your visit, however, would be a different type of data. 

There are four principles of tidy data. First, each variable you measure should be in one column.

Second. Each different observation of that variable should be in a different row.

Third. There should be one spreadsheet for each "type" of data.

And finally, fourth. If you have multiple spreadsheets, they should include a column in each spreadsheet (with the same column label!) that allows them to be joined or merged.

In addition to these four principles, there are a number of rules to follow when entering data into a spreadsheet, or when re-organizing untidy data that you have alredy been given for a project into a tidy format. They are rules that will help make data analysis and visualization easier down the road. They were formalized in a paper called "Data organization in spreadsheets", available at https://peerj.com/preprints/3183/. This was written by two prominent data scientists, Karl Broman and Kara Woo. In this paper, in addition to ensuring that the data are tidy, they suggest following these guidelines when entering data into spreadsheets. We'll go through each of these to make sure we're all clear on what a great tidy spreadsheet looks like.

Being consistent in data entry and throughout an analysis is key. It minimizes confusion and makes analysis simpler. For example, here we see sex is coded as "female" or "male." Those are the only two ways in which sex was entered into the data. This is an example of consistent data entry. You want to avoid sometimes coding a female's sex as "female" and then entering it as "F" in other cases. Simply, you want to pick a way to code sex and stick to it. With regard to entering a person's sex, we were talking about how to code observations for a specific variable; however, consistency matters when you're choosing how to name a variable. If you use the variable name "ID" in one spreadsheet, use the same variable name ("ID") in the next spreadsheet. Do not change it to "id" (capitalization matters!) or "identifier" or anything else in the next spreadsheet. Be consistent! Consistency matters across every step of the analysis. Name your files in a consistent format. Always code dates in a consistent format (discussed further below). Avoid extra spaces in cells. If you're careful about and consistent in data entry, it will be incredibly helpful when you get to analysis

Choosing good variable names is important. Generally, avoid spaces in variable names and file names. You'll see why this is important as we learn more about programming, but for now, know that "Doctor Visit 1" is not a good file name. "doctor_visit_v1" is much better. Stick to using underscores instead of spaces or any other symbol when possible. The same thing goes for variable names. "FirstName" is a good variable name while "First Name" with a space in the middle of it is not. Additionally, make sure that file and variable names are as short as possible while still being meaningful. "F1" is short, but it doesn't really tell you anything about what is in that file. "doctor_visit_v1" is a more meaningful file name. We know now that this spreadsheet contains information about a doctor's visit. 'v1' specifies version 1 allowing for updates to this file later which would create a new file "doctor_visit_v2." 

When entering dates, there is a global 'ISO 8601' standard. Dates should be encoded Y Y Y Y dash M M dash D D. For example if you want to specify that a measurement was taken on February 27th, 2018, you would type 2018-02-27. Y Y Y Y refers to the year, 2018. M M refers to the month of February, 02. And D D refers to the day of the month, 27. This standard is used for dates for two main reason. First, it avoids confusion when sharing data across different countries, where date conventions can differ. By all using ISO 8601 standard conventions, there is less room for error in interpretation of dates. Secondly, spreadsheet software often mishandles dates and assumes that non-date information are actually dates and vice versa. By encoding dates as Y Y Y Y dash M M dash D D, this confusion is minimized. 

Simply, fill in every cell. If the data is unknown for that cell, put 'NA.' Without information in each cell, the analyst is often left guessing. In the spreadsheets below, on the left, is the analyst to assume that the empty cells should use the date from the cell above? Or are we to assume that the date for that measurement is unknown? Fill in the date if it is known or type 'NA' if it is not. That will clear up the need for any guessing on behalf of the analyst. On the spreadsheet to the right, the first two rows have a lot of empty cells. This is problematic for the analysis. This spreadsheet does not follow the rules for tidy data. There is not a single variable per column with a single entry per row. These data would have to be reformatted before they could be used in analysis.

Sometimes people are tempted to include a number and a unit in a single cell. For weight, someone may want to put '165 l b s' in that cell. Avoid this temptation! Keep numbers and units separate. In this case, put one piece of information in the cell (the person's weight) and either put the unit in a separate column, or better yet, make the variable name weight underscore l b s. That clears everything up for the analyst and avoids a number and a unit from both being put in a single cell. As analysts, we prefer weight information to be in number form if we want to make calculations or figures. This is facilitated by the first column called Weight underscore l b s because it will be read into R as a numeric object. The second column called "Weight", however, will be read into R as a character object because of the "l b s", which makes our desired tasks more difficult.

Avoid the temptation to highlight particular cells with a color to specify something about the data. Instead, add another column to convey that information. In the example below, 1.1 looks like an incorrect value for an individual's glucose measure. Instead of highlighting the value in red, create a new variable. Here, on the right, this column has been named 'outlier.' Including 'TRUE' for this individual suggests that this individual may be an outlier to the data analyst. Doing it in this way ensures that this information will not be lost. Using font color or highlighting however can easily be lost in data processing, as you will see in future lessons. The following lessons will go into detail about which file formats are ideal for saving data, such as text files (dot t x t) and comma-delimited files (dot c s v). These file formats can easily be opened and will never require special software, ensuring that they will be usable no matter what computer or analyst is looking at the data.

The data entry guidelines discussed here and a few additional rules have been summarized below and are available online for reference at https://doi.org/10.7287/peerj.preprints.3139v5.

Most importantly, however, remember that tidy data are rectangular data. The data should be a rectangle with each variable in a separate column and each entry in a different row. All cells should contain some text, so that the spreadsheet looks like a rectangle with something in every cell.