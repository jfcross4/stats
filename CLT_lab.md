Central Limit Theorem Lab
----------------------------

In this lab we'll lab we'll learn about the Central Limit Theorem.  Once again, we'll use the **DiscreteRV** package.  

First, log in to *rstudio.cloud*.  If you're using the same project as last week you'll already have **DiscreteRV** installed.  If not, you'll need to install it and then load it:


```r
library(discreteRV)
```

# Creating More Random Variables

Let's create random variables for one die roll as well as the sum and mean of fifty die rolls:

```r
one_die_roll = RV(outcomes = 1:6, probs = 1/6)

sum_of_fifty_dice = SofIID(one_die_roll, n=50)

mean_of_fifty_dice = sum_of_fifty_dice/50
```
Let's also create random variables for one roulette spin and the sum and mean of thirty roulette spins:

```r
one_roulette_spin = RV(outcomes = c(100, -100), 
  probs = c(18/38, 20/38))

sum_of_thirty_spins = SofIID(one_roulette_spin, n=30)

mean_of_thirty_spins = sum_of_thirty_spins/30

```
Let's also create a basketball possession random variable:

```r
basketball_possession = 
RV(outcomes = 0:3, 
  probs = c(0.5, 0.1, 0.3, 0.1))
```
**Challenge 1:**
Using the code above as your guide, try making two new random variables, "sum_of_100_possessions" which is the sum of 100 (independent) basketball possessions and "mean_of_100_possessions"

# Plotting

Now, let's plot everything!

```r
plot(one_die_roll)
plot(sum_of_fifty_dice)
plot(mean_of_fifty_dice)
```
**Challenge 2**
Plot all of the other random variables you have created.

**Question 1**
What do you notice about the plots of all of the random variables that are sums and means of simple random variables?


# Z-Scores

As you know, we can calculate z-scores of values as:

$$z(x) = \frac{x-E(X)}{SD(X)}$$

Let's create a new random variable that is z-scores of the sums of fifty die rolls.

```r
z_score_fifty_dice =
  (sum_of_fifty_dice - E(sum_of_fifty_dice))/SD(sum_of_fifty_dice)
```
**Challenge 3**

Create random variables for the z-scores of thirty roulette spins and of 100 basketball possessions.

We can also plot these new random variables:
  
```R
plot(z_score_fifty_dice)
```
Try plotting the new z-score random variables you just created.

Lastly, let's find out how often our new z-score random variables are more than 1, 2 and 3 standard deviations from the mean (Here the "abs" function is for "absolute value")

```r
P(abs(z_score_fifty_dice)>=1)
P(abs(z_score_fifty_dice)>=2)
P(abs(z_score_fifty_dice)>=3)
```

**Challenge 3**/**Question 2**:

Try these same calculations with the other z-score random variables you created (for roulette spins and basketball possessions).  How do the numbers compare?

**Challenge 4**/**Question 2**:
Lastly (if you have time), try recreating all of this but with larger numbers of die rolls, spins and basketball possessions.  Maybe 10 times more (but don't go overboard or else you will slow down your R session).  Calculate the z-scores for these random variables and see how often they are 1, 2 and 3 standard deviations from the mean.  What do you notice?
