---
title_meta  : Chapter 1
title       : Raising anchor
description : "In this first chapter you will be introduced to DataCamp's interactive interface and the Titanic data set. Once you're familiar with the Kaggle data sets, you make your first predictions using survival rate, gender data, as well as age data."

--- type:NormalExercise xp:100 skills:1 key:a1ddff3dced9bc827f83d0a600e31649e732768b
## How it works

Welcome to our tutorial on the Driven Data's Water Pumps challenge. Here you will learn how to get started with the 
competition using R. In case you're new to R, it's recommended that you first take our free [Introduction to R Tutorial](https://www.datacamp.com/courses/free-introduction-to-r). Furthermore, while not required, familiarity with machine learning techniques is a plus so you can get the maximum out of this tutorial.

In the editor on the right you should type R code to solve the exercises. When you hit the 'Submit Answer' button, every line of code is interpreted and executed by R and you get a message whether or not your code was correct. The output of your R code is shown in the console in the lower right corner. R makes use of the `#` sign to add comments; these lines are not run as R code, so they will not influence your result.

You can also execute R commands straight in the console. This is a good way to experiment with R code, as your submission is not checked for correctness.

*** =instructions
- In the editor on the right there is already some sample code. Can you see which lines are actual R code and which are comments?
- Add a line of code that calculates the sum of 8 and 15, and hit the 'Submit Answer' button.

*** =hint
Just add a line of R code that calculates the sum of 6 and 12, just like the example in the sample code!

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
test_output_contains("23", incorrect_msg = "Make sure to add a line of R code, that calculates the sum of 8 and 15. Do not start the line with a `#`, otherwise your R code is not executed!")
success_msg("Awesome! See how the console shows the result of the R code you submitted? Now that you're familiar with the interface, let's get down to R business!")
```

--- type:NormalExercise xp:100 skills:1,3 key:5ac7cf5cee32ae6b323cfb298ebfae05fc4e7bf4
## Data Mining the Water Table

Can you predict which water pumps are faulty?

Using data from Taarifa and the Tanzanian Ministry of Water, can you predict which pumps are functional, which need some repairs, and which don't work at all? This is an intermediate-level practice competition. Predict one of these three classes based on a number of variables about what kind of pump is operating, when it was installed, and how it is managed. A smart understanding of which waterpoints will fail can improve maintenance operations and ensure that clean, potable water is available to communities across Tanzania.

Let's start with loading in the training and testing set into your R environment. You will use the training set to build your model, and the test set to validate it. The URLs for the 3 `csv` files are already available as variables in the sample code. You can load this data with the `read.csv()` function: simply pass the defined URL variables. We will inspect these new variables in the following exercises.

*** =instructions
- Load the [training set values](http://s3.amazonaws.com/drivendata/data/7/public/4910797b-ee55-40a7-8668-10efd5c1b960.csv), [training set labels](http://s3.amazonaws.com/drivendata/data/7/public/0bf8bc6e-30d0-4c50-956a-603fc693d966.csv), and [test set values](http://s3.amazonaws.com/drivendata/data/7/public/702ddfc5-68cd-4d1d-a0de-f5f566f76d91.csv) and assign these to three variables: `train_values`, `train_labels` and `test_values`.

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
train_values <- read.csv(url(train_values_url))

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
train_values <- read.csv(url(train_values_url))

# Define train_labels_url
train_labels_url <- "http://s3.amazonaws.com/drivendata/data/7/public/0bf8bc6e-30d0-4c50-956a-603fc693d966.csv"

# Import train_labels
train_labels <- read.csv(url(train_labels_url))

# Define test_values_url
test_values_url <- "http://s3.amazonaws.com/drivendata/data/7/public/702ddfc5-68cd-4d1d-a0de-f5f566f76d91.csv"

# Import test_values
test_values <- read.csv(url(test_values_url))

```

*** =sct
```{r,eval=FALSE}

msg <- "Do not change the code that specifies the URLs of the 3 csvs. You can reset the sample code to its original state by pressing the reset button next to 'Submit Answer'."
lapply(c("train_values_url", "train_labels_url", "test_values_url"), test_object, undefined_msg = msg, incorrect_msg = msg)

test_correct({
  test_object("train_values")
  test_object("train_labels")
  test_object("test_values")
}, {
  test_function("read.csv", args = "file")
})

test_error()
success_msg("Well done! Now that your data is loaded in, let's see if you can understand it.")
```

--- type:MultipleChoiceExercise xp:50 skills:1,3 key:5ba0cacb49934cf4d62a10a7fb9287f6fef57a1e
## Understanding your data

Before starting with the actual analysis, it's important to understand the structure of your data. Both `test` and `train` are data frames, R's way of representing a dataset. You can easily explore a data frame using the function `str()`. `str()` gives you information such as the data types in the data frame (e.g. `int` for integer), the number of observations, and the number of variables.

The training and test set are already available in the workspace, as `train` and `test`. Apply `str()` to the training set. Which of the following statements is correct?

*** =instructions
- `train_values` has 59400 observations and 40 variables.
- `train_values` has 59400 observations and 40 variables.
- `train_values` has 59400 observations and 40 variables.
- `train_values` has 59400 observations and 40 variables.
  
*** =hint
To see the structure of the `test` variable you can make use of `str(test)`.

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
```

*** =sct
```{r,eval=FALSE}
msg1 <- "Great job! In case you want to learn more on data frames, <a href='https://www.datacamp.com/courses/free-introduction-to-r'>check out the data frames chapter in DataCamp's introduction to R course.</a>"
msg2 <- "Wrong, try again. Maybe have a look at the hint."
msg3 <- "Not so good... Maybe have a look at the hint."
msg4 <- "Incorrect. Maybe have a look at the hint."
test_mc(correct = 1, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

--- type:NormalExercise xp:100 skills:1 key:aa1b373e5055f70cb212be1eb593927ff1d48cfa
## Rose vs Jack, or Female vs Male

How many people in your training set survived the disaster with the Titanic? To see this, you can use the `table()` command in combination with the `$`-operator to select a single column of a data frame:

```
# absolute numbers
table(train$Survived) 

# percentages
prop.table(table(train$Survived))
``` 

If you run these commands in the console, you'll see that 549 individuals died (62%) and 342 survived (38%). A simple way prediction heuristic could be: "majority wins". This would mean that you will predict every unseen observation to not survive.

In general, the `table()` command can help you to explore what variables have predictive value. For example, maybe gender could play a role as well? You can explore this using the `table()` function for a two-way comparison on the number of males and females that survived, with this syntax:

```
table(train$Sex, train$Survived)
```

To get proportions, you can again wrap `prop.table()` around `table()`, but you'll have to specify whether you want row-wise or column-wise proportions. This is done by setting the second argument of `prop.table()`, called `margin`, to 1 or 2, respectively.

*** =instructions
- Calculate the survival rates in absolute numbers using `table()` on `train`.
- Calculate the survival rates as proportions by wrapping `prop.table()` around the previous `table()` call.
- Do a two-way comparison on the number of males and females that survived, in absolute numbers. Again, use the `train` data frame.
- Convert the numbers to row-wise proportions.

*** =hint
- The code for the first, second and third instruction is already given in the assignment!
- For the fourth instruction, wrap `prop.table()` around the `table()` call for the third instruction. Make sure to set the second argument of `prop.table()` correctly!

*** =pre_exercise_code
```{r,eval=FALSE}
train <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/train.csv")
test <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/test.csv")
```

*** =sample_code
```{r,eval=FALSE}
# Your train and test set are still loaded
str(train)
str(test)

# Passengers that survived vs passengers that passed away


# As proportions

  
# Males & females that survived vs males & females that passed away


# As row-wise proportions

```

*** =solution
```{r,eval=FALSE}
# Your train and test set are still loaded
str(train)
str(test)

# Passengers that survived vs passengers that passed away
table(train$Survived)

# As proportions
prop.table(table(train$Survived))
  
# Males & females that survived vs males & females that passed away
table(train$Sex, train$Survived)

# As row-wise proportions
prop.table(table(train$Sex, train$Survived), 1)
```

*** =sct
```{r,eval=FALSE}
test_error()
msg <- "Have you correctly coded the table? Do not forget to provide the table that shows the number of survivors vs passed away in percentages."
test_output_contains("table(train$Survived)", 
                     incorrect_msg = paste(msg, "shows the number of survivors vs passed away?"))
test_output_contains("prop.table(table(train$Survived))", 
                     incorrect_msg = paste(msg, "shows the number of survivors vs passed away as proportions?"))
test_output_contains("table(train$Sex, train$Survived)", 
                     incorrect_msg =  paste(msg, "shows the number of survivors vs passed away, taking into account gender."))
test_output_contains("prop.table(table(train$Sex, train$Survived),1)", 
                     incorrect_msg = paste(msg, "shows the number of survivors vs passed away in proportions, taking into account gender."))
success_msg("Well done! It looks like it makes sense to predict that all females will survive, and all men will die.")
```

--- type:NormalExercise xp:100 skills:1 key:aff8037b847c2c5c2e7f1db185813bf6c0769d3a
## Does age play a role?

Another variable that could influence survival is age; it's probable children were saved first. You can test this by creating a new column with a categorical variable `child`. `child` will take the value 1 in case age is <18, and a value of 0 in case age is >=18. 

To add this new variable you need to do two things (i) create a new column, and (ii) provide the values for each observation (i.e., row) based on the age of the passenger.

Adding a new column with R is easy and can be done via the `$`-operator. For example, 

```
train$new <- 10
``` 

would create a `new` column in the `train` data frame with the value 10 for each observation. 

To set the values based on the age of the passenger, you make use of a boolean test inside the square bracket operator. With the `[]`-operator you create a subset of rows and assign a value to a certain variable of that subset of observations. For example,

```
train$new[train$Survived == 1] <- 0
```

would give a value of 0 to the variable `new` for the subset of passengers that survived the disaster.   

*** =instructions
- Create a new column `Child` in the `train` data frame that takes the value `NA`, if the passenger's age is `NA`, `1` when the passenger is < 18 years and the value `0` when the passenger is >= 18 years.
- Do a two-way comparison on the number of children vs adults that survived, in row-wise proportions.

*** =hint
Suppose you wanted to add a new column `clothes` to the `test` set and give all males the value `"pants"` and the others `"skirt"`:
```
test$clothes <- "skirt"
test$clothes[test$Sex == 'male'] <- "pants"
```

*** =pre_exercise_code
```{r,eval=FALSE}
train <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/train.csv")
test <- read.csv(url("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/test.csv"))
```

*** =sample_code
```{r,eval=FALSE}
# Your train and test set are still loaded in
str(train)
str(test)

# Create the column child, and indicate whether child or no child


# Two-way comparison


```

*** =solution
```{r,eval=FALSE}
# Your train and test set are still loaded in
str(train)
str(test)

# Create the column child, and indicate whether child or no child
train$Child <- NA
train$Child[train$Age < 18] <- 1
train$Child[train$Age >= 18] <- 0

# Two-way comparison
prop.table(table(train$Child, train$Survived), 1)
```

*** =sct
```{r,eval=FALSE}
test_data_frame("train", columns = "Child",
                undefined_msg = "Do not remove the variable `train`, it has already been created for you.",
                undefined_cols_msg = "Make sure to specify the column `Child` inside `train`.",
                incorrect_msg = paste("It looks like you didn't correctly set all values of the `Child` column.",
                                      "You'll have to do the explained subsetting operation twice!"))

test_object("train", incorrect_msg = paste("You have correctly specified the `Child` column inside `train`,",
                                           "but there is still something wrong. Make sure not to add or edit any other columns!"))

test_output_contains("prop.table(table(train$Child, train$Survived),1)", incorrect_msg = "Do not forget to provide the table that shows the number of survivors vs passed away in row-wise proportions, taking into account age. Inside `table()`, `train$Child` comes first!. Don't forget to wrap `prop.table()` around it.")

success_msg("While less obviously than gender, age also seems to have an impact on survival.")
```

--- type:NormalExercise xp:100 skills: 1,6 key:5da127ed17a54aa18d130798b9662f7e21b14cd1
## Making your first predictions

In one of the previous exercises you discovered that in your training set, females had over a 50% chance of surviving and males had less than a 50% chance of surviving. Hence, you could use this information for your first prediction: all females in the test set survive and all males in the test set die. 

You use your test set for validating your predictions. You might have seen that, contrary to the training set, the test set has no `Survived` column. You add such a column using your predicted values. Next, when uploading your results, Kaggle will use this variable (= your predictions) to score your performance. 

*** =instructions 
- Create a variable `test_one`, identical to dataset `test`
- Add an additional column, `Survived`, that you initialize to zero.
- Use vector subsetting like in the previous exercise to set the value of `Survived` to 1 for observations whose `Sex` equals `"female"`.

*** =hint
- To create a new variable, `y`, that is a copy of `x`, you can use `y <- x`.
- To initialize a new column `a` in a data frame `df` to zero, you can use `df$a <- 0`.
- Have another look at the previous exercise if you're struggling with the third instruction.

*** =pre_exercise_code
```{r,eval=FALSE}
train <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/train.csv")
test <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/test.csv")
```

*** =sample_code
```{r,eval=FALSE}
# Your train and test set are still loaded in
str(train)
str(test)

# Create a copy of test: test_one


# Initialize a Survived column to 0


# Set Survived to 1 if Sex equals "female"

```

*** =solution
```{r,eval=FALSE}
# Your train and test set are still loaded in
str(train)
str(test)

# Create a copy of test: test_one
test_one <- test

# Initialize a Survived column to 0
test_one$Survived <- 0

# Set Survived to 1 if Sex equals "female"
test_one$Survived[test$Sex == "female"] <- 1
```

*** =sct
```{r,eval=FALSE}
msg <- "Do not remove or change the contents of `test`! You should work with a copy of `test`, namely `test_one`."
test_object("test", undefined_msg = msg, incorrect_msg = msg)

test_data_frame("test_one", "Survived",
                undefined_cols_msg = "Make sure to define the column `Survived` inside `test_one`.",
                incorrect_msg = "The column `Survived` inside `test_one` was not specified correctly. Check your code or make use of the hint.")
test_object("test_one", incorrect_msg = paste("You correctly set the `Survived` variable of the object `test_one`,",
                                              "but there is still something wrong. Make sure not to add or edit any other columns!"))

success_msg("Well done! If you want, you can already submit these first predictions to Kaggle [by uploading this csv file](http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/ch1_ex4_solution/my_solution.csv). In the next chapter you will learn how to make more advanced predictions and create your own .csv file from R.")
```
