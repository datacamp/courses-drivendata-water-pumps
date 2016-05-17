---
title_meta  : Chapter 2
title       : Predict and Measure
description : "In this chapter we will use a common machine learning technique to make and evaluate predictions."

--- type:NormalExercise xp:100 skills:1 key:040bca4c1f
## First Prediction

Let's start making a few predictions using a common machine learning technique called a Random Forest. Although it is a fairly complex technique, it is often a good place to start since it can handle a large number of features. It is fast, and can help quickly estimate which variables are important. 

To create a Random Forest analysis in R, you make use of the `randomForest()` function in the aptly named [`randomForest`](http://www.rdocumentation.org/packages/randomForest) package. 

- First, you provide the `formula`. There is no argument `class` here to inform the function that you're dealing with predicting a categorical variable, so you need to turn `status_group` into a factor with three levels: `as.factor(status_group) ~ ... `
- The `data` argument takes the `train` data frame.
- When the `importance` argument is set to `TRUE`, you can inspect variable importance.
- The `ntree` argument specifies the number of trees to grow. Limit these when having only limited computational power at your disposal. 

To end, since Random Forest uses randomization, you set a seed using `set.seed(42)` to assure reproducibility of your results. Once the model is constructed, you can use the prediction function `predict()`.


Here, you will use the following formula as an input to `randomForest`:

```
as.factor(status_group) ~ longitude + latitude + extraction_type_group +                                     quality_group + quantity + waterpoint_type +                                       construction_year
```
As you can see, these are variables that you have examined in the previous exercises. Now it is time to see how well they work as predictor variables.


*** =instructions
- Perform a Random Forest and name the model `model_forest`. Use the variables provided in the sample code. Train the model on the data set `train`.
- Set the number of trees (`ntree`) to grow to 10, make sure you can inspect variable `importance` (set to TRUE), and set `nodesize` to 2.
- Make a prediction (`pred_forest_train`) on the test set using the `predict()` function with inputs of `model_forest` and `train`
- Inspect the first few rows of `pred_forest_train` using `head()`

*** =hint
- Remember to set the `data` argument to `train`, the `importance` to `TRUE`, the `ntree` to `10` and the `nodesize` argument to 2.
- Make sure to call `head()` to observe the resulting output from your predictions.

*** =pre_exercise_code
```{r eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
```

*** =sample_code
```{r eval=FALSE}
# Load the randomForest library
library(randomForest)

# Set seed and create a random forest classifier
set.seed(42)
model_forest <- randomForest(as.factor(status_group) ~ longitude + latitude + extraction_type_group + quality_group + quantity + waterpoint_type + construction_year, data = ___, importance = ___, ntree = ___, nodesize = ___)

# Use random forest to predict the values in train
pred_forest_train <- predict(___, ___)

# Observe the first few rows of your predictions


```

*** =solution
```{r eval=FALSE}
# Load the randomForest library
library(randomForest)

# Set seed and create a random forest classifier
set.seed(42)
model_forest <- randomForest(as.factor(status_group) ~ longitude + latitude + extraction_type_group + quality_group + quantity + waterpoint_type + construction_year,
                             data = train, importance = TRUE, ntree = 10, nodesize = 2)

# Use random forest to predict the values in train
pred_forest_train <- predict(model_forest, train)

# Observe the first few rows of your predictions
head(pred_forest_train)
```

*** =sct
```{r eval=FALSE}
test_function("randomForest", args = "x", 
              incorrect_msg = "Make sure not to change the variables in the `formula` provided in the sample code!")

test_function("randomForest", args = c("data", "importance", "ntree", "nodesize"),
              incorrect_msg = "Remember to set the `data` argument to `train`, the `importance` argument to `TRUE`, the `ntree` argument to `10` and the `nodesize` argument to 2.")

test_data_frame("model_forest", columns = "terms",
                incorrect_msg = "`my_forest` is not correct. Maybe check the hint on how to call the `randomForest()` function.")

test_function("predict", args = "object", eval = FALSE,
              incorrect_msg = "When calling `predict()`, you need to provide two arguments here: the random forest object and the train data set.") 

test_object("pred_forest_train", 
            incorrect_msg = paste("Looks like `pred_forest_train` is calculated incorrectly. Use `model_forest` and `train` as inputs in `predict()`."))

test_output_contains("head(pred_forest_train)", incorrect_msg = "Don't forget to observe the first few rows of your prediction using `head()`.")

success_msg("Nice! Let's look at the results a little more closely in the next few exercises.")
            
```

--- type:MultipleChoiceExercise xp:50 skills:1,3  key:79f9fe0183
## Evaluating the Random Forest

You can still access your first random forest with `model_forest` and predictions as `pred_forest_train`. You can use the library `caret` to view the confusion matrix for the model. This will tell you the model's accuracy on the training set as well as other performance measures like sensitivity and specificity. You can see this by running the following code:

```
library(caret)
confusionMatrix(pred_forest_train, train$status_group)
```
Based on the output, what was the positive predictive value for the `non functional` class?

*** =instructions
- 0.87
- 0.92
- 0.81
- 0.85
  
*** =hint
Look at the 'Statistics by Class' area of the of the output. Then find the column for the `non functional` class. 

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ch2ex3.Rdata"))
library(randomForest)
```

*** =sct
```{r,eval=FALSE}
msg1 <- "Wrong, try again. Maybe have a look at the hint."
msg2 <- "Very good! That is a pretty high predictive value. The overall accuracy of your model on the training set was 0.84. This random forest would have performed at a 0.79 on the test set if you submitted it to DrivenData."
msg3 <- "Incorrect. Maybe have a look at the hint."
msg4 <- "Not quite... Maybe have a look at the hint."
test_mc(correct =2, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

--- type:MultipleChoiceExercise xp:50 skills:1,3  key:5e7cac5612
## Variable Importance

Now it is time to take a look at how important the inputs were to your predictive model. Here, you can use:

```
importance(model_forest)

varImpPlot(model_forest)
```

to assess the predictive utility of the given variables in the model. According to the output, what variable is LEAST important based on the mean decrease in accuracy?

Note: The more the accuracy of the random forest decreases due to the exclusion (or permutation) of a single variable, the more important that variable is deemed. Variables with a large mean decrease in accuracy are more important for classification of the data. 

*** =instructions
- quantity
- construction_year
- latitude
- quality_group
  
*** =hint
Look at the 'Statistics by Class' area of the of the output. Then find the column for the `non functional` class. 

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ch2ex3.Rdata"))
library(randomForest)
```

*** =sct
```{r,eval=FALSE}
msg1 <- "Wrong, try again. Maybe have a look at the hint."
msg2 <- "Not quite... Maybe have a look at the hint."
msg3 <- "Incorrect. Maybe have a look at the hint."
msg4 <- "Awesome! It looks like the `quality_group` variable had the least predictive power in this model. The `quanity` variable seemed to be the most useful variable in this model."
test_mc(correct =4, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

--- type:NormalExercise xp:100 skills:1 key:8ba5efa2f7
## Adding Features

There are a lot of features in this data set, and many of them will not be useful inputs to commonly for machine learning techniques without some adjustments. That is why this data set is all about feature selection and feature engineering. You have already made a pretty solid prediction only using a handful of variables. Now it is time to go through an example of some feature engineering that can help boost your prediction accuracy. 

We can examine the variable `installer` by using `summary(train$installer)`. We can see that there are a lot of terms that are likely the same installer, but have differenct names. For example, there are many instances that refer to 'Government': 'Gover', 'GOVER', 'Government', 'Govt' etc. All of these will be considered different factors unless you find a way to aggregate them. One quick, and simplistic, way to do this would be to take the first 3 letters of each factor and make them lower case. Then, we can aggregate the terms that are most frequent and only use those as predictors, putting all other variables into an 'other' category.

When you create new features like this, you have to remember to make the same one on the `test` set. That way, you can make a prediction on the `test` set using the same model.


*** =instructions
- Check out the code that defines the new variable `install_3`
- Do a two-way comparison on the number of functioning water pumps with the `install_3` variable, in absolute numbers. Again, use the `train` data frame.
- Convert the numbers to row-wise proportions.
- Inspect the code that creates `install_3` on the `test` set for later testing.

*** =hint
- Use `table(train$install_3, train$status_group)` and `prop_table()`.

*** =pre_exercise_code
```{r,eval=FALSE}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ex3.Rdata"))
train <- merge(train_labels, train_values)
```

*** =sample_code
```{r,eval=FALSE}
# Observe the installer variable
summary(train$installer)

# Make installer lowercase, take first 3 letters as a sub string
train$install_3 <- substr(tolower(train$installer),1,3)
train$install_3[train$install_3 %in% c(" ", "", "0", "_", "-")] <- "other"

# Take the top 15 substrings from above by occurance frequency
install_top_15 <- names(summary(as.factor(train$install_3)))[1:15]
train$install_3[!(train$install_3 %in% install_top_15)] <- "other"
train$install_3 <- as.factor(train$install_3)

# Table of the install_3 variable vs the status of the pumps
table(___, ___)

# As row-wise proportions, install_3 vs status_group
prop.table(table(___, ___), margin = 1)

# Create install_3 for the test set using same top 15 from above
test$install_3 <- substr(tolower(test$installer),1,3)
test$install_3[test$install_3 %in% c(" ", "", "0", "_", "-")] <- "other"
test$install_3[!(test$install_3 %in% install_top_15)] <- "other"
test$install_3 <- as.factor(test$install_3)
```

*** =solution
```{r,eval=FALSE}
# Observe the installer variable
summary(train$installer)

# Make installer lowercase, take first 3 letters as a sub string
train$install_3 <- substr(tolower(train$installer),1,3)
train$install_3[train$install_3 %in% c(" ", "", "0", "_", "-")] <- "other"

# Take the top 15 substrings from above by occurance frequency
install_top_15 <- names(summary(as.factor(train$install_3)))[1:15]
train$install_3[!(train$install_3 %in% install_top_15)] <- "other"
train$install_3 <- as.factor(train$install_3)

# Table of the install_3 variable vs the status of the pumps
table(train$install_3, train$status_group)

# As row-wise proportions, install_3 vs status_group
prop.table(table(train$install_3, train$status_group), margin = 1)

# Create install_3 for the test set using same top 15 from above
test$install_3 <- substr(tolower(test$installer),1,3)
test$install_3[test$install_3 %in% c(" ", "", "0", "_", "-")] <- "other"
test$install_3[!(test$install_3 %in% install_top_15)] <- "other"
test$install_3 <- as.factor(test$install_3)

```

*** =sct
```{r,eval=FALSE}
test_data_frame("train", incorrect_msg = "You do not need to add any code to define `install_3`. Please use the code provided.")

msg <- "Have you correctly coded the table? Do not forget to provide the table that "
test_output_contains("table(train$install_3, train$status_group)", 
                     incorrect_msg =  paste(msg, "shows the install_3 variable vs the number of water pumps at each functional status."))
test_output_contains("prop.table(table(train$install_3, train$status_group), margin = 1)", 
                     incorrect_msg = paste(msg, "shows the proportion of the install_3 variable vs the number of water pumps at each functional status?"))
test_error()
success_msg("Great work! It looks like there are a few installer groups that show a high proportion of non functional wells. In the next exercise you will see how the new variable performs and prepare your data for submission.")
```

--- type:NormalExercise xp:100 skills:1 key:bb76b8a1a4
## Predict, Submit and Next Steps

Now that you have created the new variable `install_3`, it is time to make a new prediction. Then you can observe the importance and statistics to see how it performs.

Then, you can use the model to create predictions based on the test set. These results can be stored in a data frame called `submission` with a column for the well `id` and the predicted `status_group`. This data frame can then be written to a csv with `write.csv()` and submitted to DrivenData  [here](https://www.drivendata.org/competitions/7/submissions/). Ex: `write.csv(submission, file = filepath)`

This has only been an introduction to the water pumps challenge; there are still many ways to improve and refine your predictions. Here are a few suggestions for some next steps:

- More feature selection: Look through the features in `train` and continue to add features that may have predictive power.
- More feature engineering: Look to features that can be engineered to create more useful predictors. There are many location and character variables that could be refined to improve your predictions.
- Improve the random forest: One way to improve the accuracy is to increase the number of trees produced by the random forest and experiment with increasing and decreasing the minimum node size.
- Look to new machine learning techniques: There are many other machine learning techniques that could be utilzed to improve these models. KNN, SVM and logistic regression models and ensembles could give you the edge to climb the leaderboard.

*** =instructions
- Add the `install_3` variable to the random forest model
- Look at the importance of the features in `model_forest`
- Make predictions on the test set using `model_forest`

*** =hint
Remember to fill in all of the blanks. Add `install_3` to the random forest formula, look at the importance of the model and use `predict()` on the test set and the model.

*** =pre_exercise_code
```{r,eval=FALSE}
library(caret)
library(randomForest)
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/driven_data_ch2ex5.Rdata"))

```

*** =sample_code
```{r,eval=FALSE}
# randomForest and caret packages are pre-loaded
set.seed(42)
model_forest <- randomForest(as.factor(status_group) ~ longitude + latitude + extraction_type_group + quantity + waterpoint_type + construction_year + ___,
                             data = train, importance = TRUE,
                             ntree = 10, nodesize = 2)
                             
# Predict using the training values
pred_forest_train <- predict(model_forest, train)
importance(___)
confusionMatrix(pred_forest_train, train$status_group)

# Predict using the test values
pred_forest_test <- predict(___, ___)

# Create submission data frame
submission <- data.frame(test$id)
submission$status_group <- pred_forest_test
names(submission)[1] <- "id"

```

*** =solution
```{r,eval=FALSE}
# randomForest and caret packages are pre-loaded
set.seed(42)
model_forest <- randomForest(as.factor(status_group) ~ longitude + latitude + extraction_type_group + quantity + waterpoint_type + construction_year + install_3,
                             data = train, importance = TRUE,
                             ntree = 10, nodesize = 2)
                             
# Predict using the training values
pred_forest_train <- predict(model_forest, train)
importance(model_forest)
confusionMatrix(pred_forest_train, train$status_group)

# Predict using the test values
pred_forest_test <- predict(model_forest, test)

# Create submission data frame
submission <- data.frame(test$id)
submission$status_group <- pred_forest_test
names(submission)[1] <- "id"

```

*** =sct
```{r,eval=FALSE}
test_function("randomForest", args = "x", 
              incorrect_msg = "Make sure to add the new variable `install_3` to the random forest formula.")

test_function("randomForest", args = c("data", "importance", "ntree", "nodesize"),
              incorrect_msg = "Remember to keep the `data` argument set to `train`, the `importance` argument to `TRUE`, the `ntree` argument to `10` and the `nodesize` argument to 2.")

test_data_frame("model_forest", columns = "terms",
                incorrect_msg = "`my_forest` is not correct. Make sure to only add `install_3` to the formula. There is no need to change the other inputs to `randomForest()`")

test_function("predict", args = "object", eval = FALSE, index = 2,
              incorrect_msg = "When calling `predict()`, you need to provide two arguments here: the random forest object and the test data set.") 

success_msg("Awesome! Now you can submit the results of the random forest by downloading [this csv](https://s3.amazonaws.com/assets.datacamp.com/production/course_1032/datasets/Submission_Water_Pumps.csv) and visiting the [submission page](https://www.drivendata.org/competitions/7/submissions/) for DriveData. You will see the model has an accuracy of 0.80 on the test set. Keep working and climb that leaderboard!")
```
