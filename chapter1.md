---
title_meta  : Chapter 1
title       : Intro to DrivenData Water Pumps
description : "In this first chapter, you will be introduced to DataCamp's interactive interface and the DrivenData Water Pumps data set. You will then evaluate the structure of the data by visualizing some the data's important features."

--- type:NormalExercise xp:100 skills:1
## How it works

Welcome to our tutorial on the Driven Data's Water Pumps challenge. Here you will learn how to get started with the 
competition using R. In case you're new to R, it's recommended that you first take our free [Introduction to R Tutorial](https://www.datacamp.com/courses/free-introduction-to-r). Furthermore, while not required, familiarity with machine learning techniques is a plus so you can get the maximum out of this tutorial.

In the editor on the right, you should type R code to solve the exercises. When you hit the 'Submit Answer' button, every line of code is interpreted and executed by R and you get a message whether or not your code was correct. The output of your R code is shown in the console in the lower right corner. R makes use of the `#` sign to add comments; these lines are not run as R code, so they will not influence your result.

You can also execute R commands straight in the console. This is a good way to experiment with R code, as your submission is not checked for correctness.

*** =instructions
- In the editor on the right, there is already some sample code. Can you see which lines are actual R code and which are comments?
- Add a line of code that calculates the sum of 8 and 15, and hit the 'Submit Answer' button.

*** =hint
Just add a line of R code that calculates the sum of 8 and 15, just like the example in the sample code!

*** =pre_exercise_code
```{r eval=FALSE}
# no pec
```

*** =sample_code
```{r eval=FALSE}
# Calculate 3 * 4
3 * 4

# Calculate 8 + 15


```

*** =solution
```{r eval=FALSE}
# Calculate 3 + 4
3 * 4

# Calculate 8 + 15
8 + 15
```

*** =sct
```{r eval=FALSE}
test_output_contains("12", incorrect_msg = "Do not remove the line of R code that calculates the product of 3 and 4. Instead, just add another line that calculates the sum of 8 and 15.")
test_output_contains("23", incorrect_msg = "Make sure to add a line of R code, that calculates the sum of 8 and 15. Do not start the line with a `#`, otherwise, your R code will not be executed!")
success_msg("Awesome! See how the console shows the result of the R code you submitted? Now that you're familiar with the interface, let's get down to R business!")
```

--- type:NormalExercise xp:100 skills:1,3 
## Data Mining the Water Table

Using data from Taarifa and the Tanzanian Ministry of Water, can you predict which pumps are functional, which need some repairs, and which don't work at all? This is an intermediate-level practice competition by [DrivenData](https://www.drivendata.org/). Predict one of these three classes based on a number of variables about what kind of pump is operating, when it was installed, and how it is managed. A smart understanding of which water points will fail can improve maintenance operations and ensure that clean, potable water is available to communities across Tanzania.

Let's start with loading in the training and testing set into your R environment. You will use the training set to build your model, and the test set to validate it. The URLs for the 3 `csv` files are already available as variables in the sample code. You can load this data with the `read.csv()` function: simply pass the defined URL variables. We will inspect these new variables in the following exercises. Here is a quick explanation of the 3 data frames that you will import:

- `train_values` corresponds to the independent variables for the training set.
- `train_labels` contains    the dependent variable (`status_group`) for each of the rows in `train_values`
- `test_values` is the independent variables that need predictions

*** =instructions
- Load the [training set values](http://s3.amazonaws.com/drivendata/data/7/public/4910797b-ee55-40a7-8668-10efd5c1b960.csv), [training set labels](http://s3.amazonaws.com/drivendata/data/7/public/0bf8bc6e-30d0-4c50-956a-603fc693d966.csv), and [test set values](http://s3.amazonaws.com/drivendata/data/7/public/702ddfc5-68cd-4d1d-a0de-f5f566f76d91.csv) and assign these to three variables: `train_values`, `train_labels` and `test_values`.
- The code for `train_values` is already provided and can help you import the other two csv files.

*** =hint
- An example of proper usage of `read.csv` is provided for you in the sample code!
- Use similar code to assign `train_set_values` and `test_set_values`

*** =pre_exercise_code
```{r,eval=FALSE}
# no pec

```

*** =sample_code
```{r,eval=FALSE}
# Define train_values_url
train_values_url <- "http://s3.amazonaws.com/drivendata/data/7/public/4910797b-ee55-40a7-8668-10efd5c1b960.csv"

# Import train_values
train_values <- read.csv(train_values_url)

# Define train_labels_url
train_labels_url <- "http://s3.amazonaws.com/drivendata/data/7/public/0bf8bc6e-30d0-4c50-956a-603fc693d966.csv"

# Import train_labels
train_labels <- 

# Define test_values_url
test_values_url <- "http://s3.amazonaws.com/drivendata/data/7/public/702ddfc5-68cd-4d1d-a0de-f5f566f76d91.csv"

# Import test_values
test_values <- 

```

*** =solution
```{r,eval=FALSE}
# Define train_values_url
train_values_url <- "http://s3.amazonaws.com/drivendata/data/7/public/4910797b-ee55-40a7-8668-10efd5c1b960.csv"

# Import train_values
train_values <- read.csv(train_values_url)

# Define train_labels_url
train_labels_url <- "http://s3.amazonaws.com/drivendata/data/7/public/0bf8bc6e-30d0-4c50-956a-603fc693d966.csv"

# Import train_labels
train_labels <- read.csv(train_labels_url)

# Define test_values_url
test_values_url <- "http://s3.amazonaws.com/drivendata/data/7/public/702ddfc5-68cd-4d1d-a0de-f5f566f76d91.csv"

# Import test_values
test_values <- read.csv(test_values_url)

```

*** =sct
```{r,eval=FALSE}

msg <- "Do not change the code that specifies the URLs of the 3 csvs. You can reset the sample code to its original state by pressing the reset button next to 'Submit Answer'."
lapply(c("train_values_url", "train_labels_url", "test_values_url"), test_object, undefined_msg = msg, incorrect_msg = msg)

test_correct({
  test_object("train_values")
}, {
  test_function("read.csv", args = "file", index = 1, incorrect_msg = "Use `read.csv(train_values_url)` to create `train_values`.")
})

test_correct({
  test_object("train_labels")
}, {
  test_function("read.csv", args = "file", index = 2, incorrect_msg = "Use `read.csv(train_labels_url)` to create `train_labels`.")
})

test_correct({
  test_object("test_values")
}, {
  test_function("read.csv", args = "file", index = 3, incorrect_msg = "Use `read.csv(test_values_url)` to create `test_values`.")
})

test_error()
success_msg("Well done! Now that your data is loaded in, you can start exploring it!")
```

--- type:MultipleChoiceExercise xp:50 skills:1,3 
## Understanding your data

Before starting with the actual analysis, it's important to understand the structure of your data. The variables loaded in the previous exercise, `train_labels`, `train_values`, and `test_values`, are data frames, R's way of representing a dataset. You can easily explore a data frame using the function `str()`. `str()` gives you information such as the data types in the data frame (e.g. `int` for integer), the number of observations, and the number of variables. It is a great way to get a feel for the contents of the data frame.

The 3 data frames are already loaded into your workspace. Apply `str()` to each variable to see its dimensions and basic composition. Which of the following statements is correct?

*** =instructions
- `train_labels` has 14850 observations and 40 variables.
- `train_labels` has 59400 observations and 2 variables.
- `train_values` has 14850 observations and 2 variables.
- `train_values` has 59400 observations and 40 variables.
  
*** =hint
To see the structure of the `test_values` variable you can make use of `str(test_values)`.

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
```

*** =sct
```{r,eval=FALSE}
msg1 <- "Wrong, try again. Maybe have a look at the hint."
msg2 <- "Not so good... Maybe have a look at the hint."
msg3 <- "Incorrect. Maybe have a look at the hint."
msg4 <- "Great job! In case you want to learn more on data frames, <a href='https://www.datacamp.com/courses/free-introduction-to-r'>check out the data frames chapter in DataCamp's introduction to R course.</a>"
test_mc(correct =4, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

--- type:NormalExercise xp:100 skills:1
## Water table()

As you can see from the last exercise, these are large datasets with a bunch of variables. To simplify things, it is common to merge the independent values and the dependent labels into one data frame. This can be acheived using the `merge()` command on `train_values` and `train_labels`. Going forward, this will make it easier when modeling and manipulating the data frame. 

The next steps will be to begin to examine the features of the data set. The objective of this challenge is to predict whether or not a water pump is functional for the test set. We are given the status of the pumps for the newly merged `train` data frame as the column `status_group`. 

So, how many water pumps in the `train` data frame are functional? One way to see this is to use the `table()` command. It can also be used with the `$`-operator to select a single column of data:

```
# absolute numbers
table(train$status_group) 

# proportions
prop.table(table(train$status_group))
``` 

If you run those commands in the console, you will see that 38.4% of water pumps in the `train` data frame are non-functional. It would also be helpful to see a two-way table comparing a variable with `status_group` to see if it may have predictive value. For example:

```
# absolute numbers
table(train$payment, train$status_group)

# proportions
prop.table(table(train$payment, train$status_group), margin = 1)

```

The second argument in `prop.table()` is called `margin` and can be set to either 1 or 2, determining whether the proportions are calculated row-wise or column-wise.


*** =instructions
- Calculate the pump status in absolute numbers using `table()` on `train`.
- Calculate the pump status as proportions by wrapping `prop.table()` around the previous `table()` call.
- Do a two-way comparison on the number of functioning water pumps with the `quantity` variable, in absolute numbers. Again, use the `train` data frame.
- Convert the numbers to row-wise proportions.

*** =hint
- The code for the first, second, and third instruction is already given in the assignment!
- For the fourth instruction, wrap `prop.table()` around the `table()` call for the third instruction. Make sure to set the second argument of `prop.table()` correctly!

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
```

*** =sample_code
```{r,eval=FALSE}
# Merge data frames to create the data frame train
train <- merge(train_labels, train_values)

# Look at the number of pumps in each functional status group
table(___)

# As proportions

  
# Table of the quantity variable vs the status of the pumps


# As row-wise proportions, quantity vs status_group
prop.table(table(___, ___), margin = 1)
```

*** =solution
```{r,eval=FALSE}
# Merge data frames to create the data frame train
train <- merge(train_labels, train_values)

# Look at the number of pumps in each functional status group
table(train$status_group)

# As proportions
prop.table(table(train$status_group))
  
# Table of the quantity variable vs the status of the pumps
table(train$quantity, train$status_group)

# As row-wise proportions, quantity vs status_group
prop.table(table(train$quantity, train$status_group), margin = 1)


```

*** =sct
```{r,eval=FALSE}
msg <- "Have you correctly coded the table? Do not forget to provide the table that "
test_output_contains("table(train$status_group)", 
                     incorrect_msg = paste(msg, "shows the number of water pumps at each functional status."))
test_output_contains("prop.table(table(train$status_group))", 
                     incorrect_msg = paste(msg, "shows the proportion of water pumps at each functional status."))
test_output_contains("table(train$quantity, train$status_group)", 
                     incorrect_msg =  paste(msg, "shows the number of water pumps at each functional status vs the quantity variable."))
test_output_contains("prop.table(table(train$quantity, train$status_group), margin = 1)", 
                     incorrect_msg = paste(msg, "shows the proportion of water pumps at each functional status by the quantity variable."))
test_error()
success_msg("Well done! It looks like if the quantity variable is 'dry', it is likely that the pump is not functional. We will continue to explore some more variables next.")
```

--- type:NormalExercise xp:100 skills:1
## Explore and Visualize

Another great way to explore your data is to create a few visualizations. This can help you better understand the structure and potential limitations of particular variables. 

Check out the structure of the variables in `train` with `str()`. You will see the majority of them are categorical. If there aren't too many categories in a variable, a bar chart can be a great way to visualize and digest your data. 

In the sample code to the right, you have been provided with a command that will produce a plot that shows a breakdown of the `quantity` variable broken up by `status_group`. Here are a few variables that you can view with a similar command:

- `quality_group` - The quality of the water
- `extraction_type_class` - The kind of extraction the water point uses
- `payment` - What the water costs
- `source_type` - The source of the water
- `waterpoint_type` - The kind of water point

You can see descriptions of all of the variables on the competition page [here](https://www.drivendata.org/competitions/7/page/25/). 


*** =instructions
- Use the package `ggplot2` to create a bar chart for the variable `quantity` using the aesthetic `fill` to partition by `status_group`
- Then make a similar plot for `quality_group` 
- And again for `waterpoint_type`

*** =hint
Use the same code that is provided for the `quantity` plot. Simply change the first argument in the command.

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
```

*** =sample_code
```{r,eval=FALSE}
# Load the ggplot package and examine train
library(ggplot2)
str(train)

# Create bar plot for quantity
qplot(quantity, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top")

# Create bar plot for quality_group
qplot(___, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top")

# Create bar plot for waterpoint_type
qplot(___, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top") + 
  theme(axis.text.x=element_text(angle = -20, hjust = 0))

```

*** =solution
```{r,eval=FALSE}
# Load the ggplot package and examine train
library(ggplot2)
str(train)

# Create bar plot for quantity
qplot(quantity, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top")

# Create bar plot for quality_group
qplot(quality_group, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top")

# Create bar plot for waterpoint_type
qplot(waterpoint_type, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top") +
  theme(axis.text.x=element_text(angle = -20, hjust = 0))

```

*** =sct
```{r,eval=FALSE}
msg <- "There is no need to change the commands in the sample code."
test_function_v2("qplot", "x", eval = FALSE, index = 1, 
                 incorrect_msg = msg)
test_function_v2("qplot", "x", eval = FALSE, index = 2, 
                 incorrect_msg = paste(msg, " Simply change the `x` value in `qplot()` to `quality_group`"))
test_function_v2("qplot", "x", eval = FALSE, index = 3, 
                 incorrect_msg = paste(msg, " Simply change the `x` value in `qplot()` to `waterpoint_type`"))
test_error()
success_msg("Awesome! Now let's look at a few more visualizations.")
```
--- type:MultipleChoiceExercise xp:50 skills:1,3 
## Which Well Quantities are Funtional?

Take another look at the first plot from the last exercise. Judging from the plot, which quantity level is most likely to be non-functional?

You could also run `prop.table(table())` to see the proportion of wells that fall into each set of categories.

*** =instructions
- Dry
- Enough
- Sufficient
- Seasonal
  
*** =hint
Which column has the largest proportion of non-functional wells? Which one would make the most intuitive sense?

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
library(ggplot2)
qplot(quantity, data=train, geom="bar", fill=status_group) + 
  theme(legend.position = "top")

```

*** =sct
```{r,eval=FALSE}
msg1 <- "Very good! Many times, the variables that have the most predictive power are also the ones that are most intuitive."
msg2 <- "Not so good... Maybe have a look at the hint."
msg3 <- "Incorrect. Maybe have a look at the hint."
msg4 <- "Try again!"
test_mc(correct =1, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

--- type:NormalExercise xp:100 skills: 1,6
## Continuous Variable Viz

You just made some great plots that compared some categorical variables based on the well status. Now you can look a some ordinal or continuous variables using `ggplot2` and `geom_histogram`. 

It could be useful to observe the distribution of a few of these variables. Here is a list of a few variables in `train` that you could view in this way:

- `amount_tsh` - Total static head (amount water available to water point)
- `gps_height` - Altitude of the well
- `population` - Population around the well
- `construction_year` - Year the water point was constructed

Feel free to use the script in the sample code to create histograms for any of the above variables to observe their distributions.

If you want to learn more about the mechanics of `ggplot`, [this course](https://www.datacamp.com/courses/data-visualization-with-ggplot2-1) is a great place to start.

*** =instructions 
- Create a set of histogram of `construction_year` for wells that are 'functional', 'functional needs repair', and 'non functional'. 
- Then create the same plots but only for wells that have a construction year that is not missing (which in this case is coded as `0`s). Simply fill in the two missing variables.

*** =hint
- Fill in the `x` variable in the aesthetic to complete the first plot
- Fill in the `facet_grid` theme with the same variable as the first plot

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
```

*** =sample_code
```{r,eval=FALSE}
library(ggplot2)

# Create a histogram for `construction_year` grouped by `status_group`
ggplot(train, aes(x = ___)) + 
  geom_histogram(bins = 20) + 
  facet_grid( ~ status_group)

# Now subsetting when construction_year is larger than 0
ggplot(subset(train, construction_year > 0), aes(x = ___)) +
  geom_histogram(bins = 20) + 
  facet_grid( ~ ___)

```

*** =solution
```{r,eval=FALSE}
library(ggplot2)

# Create a histogram for `construction_year` grouped by `status_group`
ggplot(train, aes(x = construction_year)) + 
  geom_histogram(bins = 20) + 
  facet_grid( ~ status_group)

# Now subsetting when construction_year is larger than 0
ggplot(subset(train, construction_year > 0), aes(x = construction_year)) +
  geom_histogram(bins = 20) + 
  facet_grid( ~ status_group)

```

*** =sct
```{r,eval=FALSE}
test_ggplot(1, aes_fail_msg = "Don't forget to fill in the `x` value in the aesthetic with `construction_year` for the first plot!")
test_ggplot(2, aes_fail_msg = "Don't forget to fill in the `x` value in the aesthetic with `construction_year` for the first plot!", facet_fail_msg = "Look at the `facet_grid()` layer on the first plot and fill in the same variable for the second plot.")
test_error()
success_msg("Great work! As you can see, the first plot showed us that there were a lot of missing values coded as 0's. After subsetting them out, we could see that there may some differences between the distribution of functional wells and non-functional wells.")
```

--- type:NormalExercise xp:100 skills: 1,6
## Mapping Well Locations

Two other variables that would be worth checking out would be `longitude` and `latitude`. It would make sense that the location of the wells could be connected to the probability that they are functioning. We could look at a histogram of the two variables, but we could be missing some major features of the data. 

First, you can start off creating a scatter plot for `longitude` and `latitude` to see where the wells are located throughout the country. Then see if there is any visible clustering around certain areas or landmarks by making the color of the points correspond to the values in `status_group`. 

Next, you can use the googleVis package to create a map of Tanzania and overlay the locations of the wells. This will give a more visually appealing representation of the water point locations within the country. There are so many data points within `train` that the plot will take a long time to generate. To make things simpler, you can simply plot the first 1000 points in `train`.


*** =instructions 
- Create a scatter plot with `latitude` as the x-axis and `longitude` as the y-axis. You can subset out data points that are missing (and coded as 0's here). Also, add `status_group` as the color variable in the plot.
- Use `googleVis` to plot the data on top of a map of Tanzania. To do this, code has been provided that creates a variable `longlat` that conforms to the inputs for `gvis GeoChart`. Plot the resulting map produced by `gvisGeoChart`

*** =hint
- Fill in the blanks for ggplot, the rest of the code is ready to produce the scatter plot!
- Inspect the two plots produced


*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
train$Size <- 1
library(googleVis)
```

*** =sample_code
```{r,eval=FALSE}
library(ggplot2)
library(googleVis)

# Create scatter plot: latitude vs longitude with color as status_group
ggplot(subset(train, latitude < 0 & longitude > 0), aes(x=___, y=___, color=___)) + 
  geom_point(shape=1) + 
  theme(legend.position = "top")

# Create a column 'latlong' to input into gvisGeoChart
train$latlong <- paste(round(train$latitude,2), round(train$longitude, 2), sep=":")

# Use gvisGeoChart to create an interactive map with well locations
wells_map <- gvisGeoChart(train[1:1000,], locationvar = "latlong", 
                          colorvar = "status_group", sizevar = "Size", 
                          options = list(region = "TZ"))

# Plot wells_map
wells_map

```

*** =solution
```{r,eval=FALSE}
library(ggplot2)
library(googleVis)

# Create scatter plot: latitude vs longitude with color as status_group
ggplot(subset(train, latitude < 0 & longitude > 0), aes(x=latitude, y=longitude, color=status_group)) + 
  geom_point(shape=1) + 
  theme(legend.position = "top")

# Create a column 'latlong' to input into gvisGeoChart
train$latlong <- paste(round(train$latitude,2), round(train$longitude, 2), sep=":")

# Use gvisGeoChart to create an interactive map with the first 1000 well locations
wells_map <- gvisGeoChart(train[1:1000,], locationvar = "latlong", 
                          colorvar = "status_group", sizevar = "Size", 
                          options = list(region = "TZ"))
# Plot wells_map
wells_map

```

*** =sct
```{r,eval=FALSE}
test_ggplot(1, aes_fail_msg = "Don't forget to fill in the `x` value in the aesthetic with `latitude`, `y` with `longitude` and, `color` with `status_group` for the first plot!")

test_function("gvisGeoChart", c("data", "locationvar", "colorvar", "sizevar"), eval = F, 
              incorrect_msg = "You do not need to change the inputs for `gvisGeoChart`, simply use the code provided.")

test_error()

success_msg("Awesome job! Go to the next chapter to make some predictions!")
```
