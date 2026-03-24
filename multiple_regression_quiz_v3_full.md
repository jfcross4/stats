Multiple Regression Practice Quiz — Version 2
------------------------------------------------------

# Getting the Data

Run the following:

```r
coffee = data.frame(
  productivity = c(68, 75, 72, 80, 85, 78, 70, 88, 90, 76, 82, 74),
  cups = c(1, 2, 2, 3, 4, 3, 1, 4, 5, 2, 3, 2),
  sleep = c(7, 6, 7, 5, 4, 6, 8, 5, 4, 7, 6, 7)
)

cars = data.frame(
  price = c(18000, 22000, 25000, 27000, 30000, 32000, 21000, 26000, 29000, 24000, 31000, 28000),
  mileage = c(90000, 70000, 60000, 50000, 40000, 35000, 80000, 55000, 45000, 65000, 30000, 48000),
  hybrid = c("no","yes","no","yes","yes","yes","no","yes","yes","no","yes","yes")
)

focus = data.frame(
  tasks_completed = c(6, 9, 8, 11, 10, 7, 12, 13, 9, 10, 11, 8),
  practice_hours = c(1, 3, 2, 4, 3, 1, 5, 6, 2, 4, 5, 2),
  distractions = c(5, 3, 4, 2, 3, 6, 2, 1, 4, 3, 2, 5)
)
```

# 1. Coffee and Productivity

A group of students tracked their daily habits for a week.

- `cups` = number of cups of coffee consumed  
- `sleep` = hours of sleep the night before  
- `productivity` = a score from 0–100 based on:
  - number of assignments completed  
  - self-reported focus  
  - and a (somewhat questionable) bonus for “vibes” as rated by a friend  

We want to understand how coffee relates to productivity.

a. Create a model predicting productivity from cups of coffee:

```r
m = lm(productivity ~ cups, data=coffee)
summary(m)
```

Write the equation and interpret the coefficient of `cups`.

b. Now add sleep as a predictor:

```r
m = lm(productivity ~ cups + sleep, data=coffee)
summary(m)
```

How does the coefficient of `cups` change? What might explain this?

c. Based on the two models, what can you say about sleep, coffee and productivity?

# 2. Used Cars

We want to understand what affects the price of a used car.

a. Create a model predicting price from whether the car is a hybrid:

```r
m = lm(price ~ hybrid, data=cars)
summary(m)
```

Interpret the coefficient of `hybrid`.

b. Now add mileage:

```r
m = lm(price ~ hybrid + mileage, data=cars)
summary(m)
```

How does the coefficient of `hybrid` change? Why might this happen?

c. Predict the price of a **hybrid car with 50,000 miles**.

d. Which variable appears to be the stronger predictor of price? Explain using the model output.

# 3. Focus and Practice

Students tracked their study habits over a week.

- `tasks_completed` = number of assignments completed  
- `practice_hours` = hours spent studying  
- `distractions` = number of times they checked their phone (or similar interruptions)

a. Create a model predicting tasks completed from practice hours:

```r
m = lm(tasks_completed ~ practice_hours, data=focus)
summary(m)
```

Interpret the coefficient.

b. Now add distractions:

```r
m = lm(tasks_completed ~ practice_hours + distractions, data=focus)
summary(m)
```

How does the coefficient of `practice_hours` change after adding distractions?

c. Interpret the coefficient of `distractions` in the multiple regression model.

d. Suppose a student studies for **4 hours** and has **3 distractions**. Predict how many tasks they complete.

# 4. Conceptual Twist

A student says:

> "If a variable becomes less important after adding another predictor, that means it doesn’t matter."

a. Is this statement correct? Explain.

b. Give an example from one of the models above where a variable’s importance changed after adding another predictor.

c. What does this suggest about the relationship between predictors?

# Bonus

Pick one dataset and do one of the following:

- create a third model adding an interaction (if you know how), OR  
- make a graph using **ggplot** / the **tidyverse** to visualize the relationship between the response and the predictors

For example, you might use a scatterplot with color, faceting, or a fitted line.

What new relationships do you notice?
