Multiple Regression Practice Quiz — Version 2 Solutions
-----------------------------------------------------------------

# Getting the Data

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

## a.

```r
m = lm(productivity ~ cups, data=coffee)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{productivity}} = 62.78 + 5.68(\text{cups})
\]

So for each additional cup of coffee, predicted productivity increases by about **5.68 points**.

## b.

```r
m = lm(productivity ~ cups + sleep, data=coffee)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{productivity}} = 96.07 + 0.38(\text{cups}) - 4.69(\text{sleep})
\]

The coefficient of `cups` drops a lot, from about **5.68** to about **0.38**.

That suggests the original relationship between coffee and productivity was at least partly explained by another variable, namely sleep.

## c.

The first model makes coffee look like a strong positive predictor of productivity. But after controlling for sleep, the coffee coefficient becomes very small.

This suggests coffee itself may not be doing much. Instead, students who sleep less also tend to drink more coffee, so coffee was partly acting as a stand-in for sleep patterns.

In this dataset, the original positive relationship between coffee and productivity seems to be mostly explained by the fact that coffee consumption and sleep are related.

# 2. Used Cars

## a.

```r
m = lm(price ~ hybrid, data=cars)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{price}} = 22000 + 7000(\text{hybridyes})
\]

Here `hybridyes` is 1 for hybrid cars and 0 for non-hybrids.

So the model predicts that hybrid cars cost about **$7,000 more** on average than non-hybrid cars.

## b.

```r
m = lm(price ~ hybrid + mileage, data=cars)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{price}} = 33047.17 + 2043.40(\text{hybridyes}) - 0.17(\text{mileage})
\]

After adding mileage, the coefficient of `hybrid` drops from about **7000** to about **2043**.

This happens because mileage was confounding the original relationship. Hybrid cars in this sample also tend to have lower mileage, and lower-mileage cars are more expensive. Once mileage is included, the model gives a smaller estimate of the hybrid effect.

## c.

For a hybrid car with 50,000 miles:

\[
\widehat{\text{price}} = 33047.17 + 2043.40(1) - 0.17(50000)
\]

\[
= 33047.17 + 2043.40 - 8500
\]

\[
= 26590.57
\]

Predicted price: about **$26,591**

## d.

Mileage appears to be the stronger predictor because:

- its coefficient is substantial in practical terms
- it is strongly related to price
- adding mileage changes the hybrid coefficient a lot, which suggests mileage explains an important part of the price differences

A good student answer should refer to the regression output, especially the size of the coefficients, p-values, or overall interpretation.

# 3. Focus and Practice

## a.

```r
m = lm(tasks_completed ~ practice_hours, data=focus)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{tasks completed}} = 5.43 + 1.29(\text{practice hours})
\]

So each additional hour of practice is associated with about **1.29 more tasks completed**, on average.

## b.

```r
m = lm(tasks_completed ~ practice_hours + distractions, data=focus)
summary(m)
coef(m)
```

This gives the equation:

\[
\widehat{\text{tasks completed}} = 8.17 + 0.92(\text{practice hours}) - 0.59(\text{distractions})
\]

The coefficient of `practice_hours` decreases from about **1.29** to about **0.92** after adding distractions.

This suggests that some of the original association between practice and tasks completed was tied up with the fact that students who practiced more may also have had fewer distractions.

## c.

The coefficient of `distractions` is about **-0.59**.

That means that, holding practice hours constant, each additional distraction is associated with about **0.59 fewer tasks completed**.

## d.

For a student with 4 practice hours and 3 distractions:

\[
\widehat{\text{tasks completed}} = 8.17 + 0.92(4) - 0.59(3)
\]

\[
= 8.17 + 3.68 - 1.77
\]

\[
= 10.08
\]

Predicted number of tasks completed: about **10.08**

# 4. Conceptual Twist

## a.

The statement is **not correct**.

If a variable becomes less important after adding another predictor, that does not mean it “doesn’t matter.” It may mean that the predictors are related to one another, and the new model is separating their effects.

## b.

One example is the used cars data. The coefficient of `hybrid` gets much smaller after adding `mileage`.

Another example is the coffee data, where the coefficient of `cups` becomes much smaller after adding `sleep`.

## c.

This suggests that predictors can be related to one another. When that happens, a simple regression may mix together several effects, while a multiple regression tries to isolate the effect of each predictor holding the others constant.

# Bonus

A good answer here could take several forms.

For example, with the focus data:

```r
library(tidyverse)

ggplot(focus, aes(x = practice_hours, y = tasks_completed, color = distractions)) +
  geom_point(size = 3) +
  geom_smooth(method = "lm", se = FALSE)
```

A student might notice that:
- more practice hours tends to be associated with more tasks completed
- more distractions tends to be associated with fewer tasks completed
- students with similar practice hours can still differ depending on their distraction levels

Or, for the cars data:

```r
library(tidyverse)

ggplot(cars, aes(x = mileage, y = price, color = hybrid)) +
  geom_point(size = 3) +
  geom_smooth(method = "lm", se = FALSE)
```

Things you might notice:
- price decreases as mileage increases
- hybrid cars tend to sit above non-hybrid cars
- some of the original hybrid effect may really be a mileage effect

