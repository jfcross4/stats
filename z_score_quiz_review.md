Variance, Binomial and z Score mixed review REDUX
-------------------------------------------------

Today, we're going to review for the quiz by using R to solve review problems.  We'll also check the accuracy of our normal approximations.

You have have the "Variance, Binomial and z Score mixed review REDUX" sheet in hand and log in to rstudio.cloud.  You can continue an earlier project in the Statistics workspace.

Please write your answers on your worksheet.

# Section 1: Measures of Center and Spread

First create a vector of values

```r
values = c(0, 2, 2, 2, 2, 10)
```

Then, you can answer several parts of questions using:

```r
summary(values)
```

Now, let's write functions for variance and standard deviation and then use them:

```r
variance = function(x){ mean((x - mean(x))^2)}
standard_dev = function(x){sqrt(variance(x))}

variance(values)
standard_dev(values)
```

# Section 2: z-scores and a standard normal distribution

Women’s shoe sizes are roughly normally distributed with a mean of 8 and a standard deviation of 1.5.  

a. Find the z-score for a shoe that is size 7.

```r
z = (7 - 8)/1.5
print(z)
```

b. Find the z-score for a shoe that is size 10.

```r
# write this code on your own

```

c. Estimate the % of women with a shoe size less than 6.5.  (Assume that shoes only come in half sizes meaning that 6, 6.5 and 7 are possible sizes but 6.3214… is not)

In this case, "less than 6.5" is the same as "6 or less", since both mean that we want to include 6 but exclude 6.5. To do a continuity correction *in this case* we should make our cut at 6.25.  We'll start by getting the z-score for 6.25:

```r
z = (6.25 - 8)/1.5
print(z)
```

Next, we find the area to the left of this z-score in the standard normal distribution (we can use R's built in function instead of consulting our paper tables):

```r
pnorm(z)
```

Write R code to answer the following questions:

d. Estimate the % of women with a shoe size greater than 9.

e. Estimate the % of women with a shoe size between 6.5 and 9 (inclusive).

f. Estimate the % of women whose shoe size is 8.

g. What is the smallest interval of shoe sizes (I’m looking for a lower bound and an upper bound here) that would contain at least 80% of women’s shoe sizes?

# Section 3: Random Variables

A Saint Ann’s student has a 40% chance of arriving late to school 0 times over the course of a week, a 30% chance of arriving late once, a 20% chance of arriving late twice and a 10% chance of arriving late three times.  They would never be late more than three times in a week.

Let's create the outcomes and probabilities based on this problem (you can imagine drawing the table):

```r
outcomes = c(0, 1, 2, 3)
probs = c(0.40, 0.30, 0.20, 0.10)
```

A. What is the expected value of the number of times they arrive late in a week?

since,

$$E[x] = \Sigma\ x \cdot p(x)$$

we'll use that formula

```r
sum(outcomes*probs)
```
B. What is the variance and standard deviation in the number of times they arrive late in a week?

$$Var[x] = \Sigma\ (x - E[X])^2 \cdot p(x)$$


```r
ex = sum(outcomes*probs)

sum(((outcomes - ex)^2)*probs)

# and for the standard deviation:
sqrt(sum(((outcomes - ex)^2)*probs))
```

Try these on your own:

C. What is the expected value of the number of times they arrive late in a 33 week school year?

D. What is the variance and standard deviation in the number of points they arrive late in a 33 week school year?

E. What is the chance that this student arrives late more than 40 times in a 33 week school year?

(Use your answers to C and D and the code you used above in Section 2.  Remember to use a continuity correction!)








# Double Checking your work!

```r
library(discreteRV)

lateness = RV(outcomes=outcomes, probs=probs)
yearly_lateness = SofIID(lateness, n=33)

P(yearly_lateness > 40)
```

# Binomial Random Variables
(Try to answer these on your own using the methods we learned in class.)

Let’s assume that T is a true .800 batter, meaning that she gets a hit in 80% of her at bats.

For a *binomial* random variable:

$$E[X] = np$$
$$Var[X] = np(1-p)$$

If T has 75 at bats…

a. find the expected value and the standard deviation in the number of hits she gets?

b. What is the chance that T gets a hit in at least 63 of her 75 at bats?  (Remember to use a continuity correction.)

## Checking your work

```r
sum(dbinom(63:75, 75, 0.8))
```