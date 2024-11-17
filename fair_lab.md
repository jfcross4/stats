Ray Fair: Predicting the Results of Presidential Elections
-----------------------------------------

# Chapter 1: It's the Economy, Stupid

* First Read Chapter 1, "It's the Economy, Stupid" and then run the following code and answer the chapter one questions in a Google Doc.

## Getting the Data

```r
library(tidyverse)

fair = read.table("https://fairmodel.econ.yale.edu/vote2012/pres.txt",
                  header=TRUE)

View(fair)
```

Explanation of Variables
========================================================

**VP:**: Democratic share of the two-party presidential vote

**I:** 1 if there is a Democratic presidential incumbent at the time of the
election and −1 if there is a Republican presidential incumbent

**DPER:** 1 if a Democratic presidential incumbent is running again, −1
if a Republican presidential incumbent is running again, and 0
otherwise.

**DUR:** 0 if either party has been in the White House for one term, 1 [−1] if
the Democratic [Republican] party has been in the White House for
two consecutive terms, 1.25 [−1.25] if the Democratic [Republican]
party has been in the White House for three consecutive terms, 1.50
[−1.50] if the Democratic [Republican] party

**WAR:** 1 for the elections of 1918, 1920, 1942, 1944, 1946, and 1948, and
0 otherwise.

**G:** growth rate of real per capita GDP in the ﬁrst three quarters of the
on-term election year (annual rate).

**P:** absolute value of the growth rate of the GDP deﬂator in the ﬁrst 15
quarters of the administration (annual rate) except for 1920, 1944,
and 1948, where the values are zero.

**Z:** number of quarters in the ﬁrst 15 quarters of the administration in
which the growth rate of real per capita GDP is greater than 3.2
percent at an annual rate except for 1920, 1944, and 1948, where
the values are zero.

___
**Question 1:** Compare this data to the data table on page 12 of your reading.  What differences do you see?

___

## Calculating Incumbent Vote Share

```r
fair = 
fair %>%
  mutate(incumbent_vote_share = 
           ifelse(I==1, VP, 100-VP))
```

## Calculating "IDUR" (Incumbent Party Duration) and "IPER" (incumbent running again)

```r
fair = 
fair %>%
  mutate(IDUR = I*DUR, IPER = I*DPER)
```


## Reproducing Figure 1-1

```r
fair %>% ggplot(aes(G, incumbent_vote_share, label=YEAR))+
  geom_label()
```
adding a best fit line

```r
fair %>% ggplot(aes(G, incumbent_vote_share, label=YEAR))+
  geom_label()+
  geom_smooth(method="lm", se=FALSE)
```

___
**Question 2:** In which election(s) did the incumbent candidate most outperform what we might expect based on GDP growth?

___

___
**Question 3:** In which election(s) did the incumbent candidate most underperform what we might expect based on GDP growth?

___

# Chapter 2: The Tools in Seven Easy Lessons

* First read *Monday*, *Tuesday*, and *Wednesday* then run the following code and answer the following questions.

## Getting an Equation for the Best Fit Line

```r
best_fit_line = 
  lm(incumbent_vote_share ~ G, data=fair)

best_fit_line
```
## Making Predictions using the Best Fit Line

```r
fair$predicted_incumbent_vote_share =
predict(best_fit_line)
```

## Finding the Errors

```r
fair = 
fair %>% 
mutate(error = 
        incumbent_vote_share - 
        predicted_incumbent_vote_share)
        
View(fair)
```
## Finding the Sum of the Squared Errors

```r
fair %>%
  summarize(SSE = sum(error^2),
            MSE = mean(error^2),
            RMSE = sqrt(mean(error^2)))
```
...Dividing by n-2 instead of n...

```r
fair %>%
  summarize(SSE = sum(error^2),
            adj_MSE = SSE/(length(error)-2),
            standard_error = sqrt(adj_MSE))
```
* Now read Thursday, Lesson 4

Create a histogram of the errors of our best-fit line:

```r
fair %>% ggplot(aes(error)) + geom_histogram(binwidth=2)
```
___
**Question 4:** Do the errors appear to be roughly normally distributed?

___

Calculate the portion of the errors are within 1 standard error.

```r
fair %>% summarize(mean(abs(error)<4.9))
```
Calculate the portion of the errors are within 2 standard error2.

```r
fair %>% summarize(mean(abs(error)<2*4.9))
```

___
**Question 5:** In a normal distribution roughly 68% of values should be within 1 standard deviation and roughly 95% within 2.  Is this what you found for the errors of our best-fit line?

___

## Find the standard error in the slope.

The output of the following code shows that the slope of the best-fit-line for incumbent vote share versus GDP growth is 0.89 with a standard error of 0.18.

```r
summary(best_fit_line)
```

How many standard errors is our slope away from zero?

```r
0.8787/0.1761
```
___
**Question 6:** Our slope is roughly 5 standard errors away from zero.  How would you explain what that means?

___

* Now read Friday, Lesson 5

___
**Question 7:** Can you relate what Ray Fair says in Friday's lesson to confounding variables and spurious correlation?   (What example does Fair give of a possible confounding variable and what example does he give of a possible spurious correlation?)

___

* Now read Saturday, Lesson 6

___
**Question 8:**
According to the best fit line:

```r
summary(best_fit_line)
```

a. What vote share would you predict for an incumbent with 0% GDP growth?

b. What vote share would you predict for an incumbent with 10% GDP growth?

___

* Now read Sunday, Lesson 7
(Note: Our data is more up to date than the data discussed in the reading.)

## Adding More Variables

Our data, doesn't have the inflation rate in each election year.  Instead it has the a value "P" which is the absolute value of inflation (based on Fair's reasoning that people like neither inflation nor deflation) during the first 3.75 year's of the incumbent's term and this number is zeroed-out during elections of 1920, 1944, and 1948 (where voters, might, according to Fair. excuse inflation during war time).  [Could Ray Fair be accused of data mining (a pitfall he mentioned in Lesson 5 here?].  We can try to predict incumbent vote share from G and P:

```r
model = lm(incumbent_vote_share ~ G + P, data=fair)

summary(model)
```

Since we are using slightly different variables and a larger set of elections, our results table (that you generating above) looks a bit different from the one Fair shows in Box 2-3 on page 39.  

## Testing for Other Variables

___
**Question 9:** Can you find a variable that seems more important in predicting elections than inflation?  Try using the code above used to create "model" but replacing "P" with one of the other variables described (way!) above ("IDUR", "IPER", "Z" or "WAR").  Look at a summary of your model and pay particular attention to the t-statistics!

___
