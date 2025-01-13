Random Variable Lab
----------------------------

In this lab we'll lab we'll learn about Random Variables and use the **DiscreteRV** package.

First, log in to *rstudio.cloud* and then create a new project or continue working within an existing project that is within our class work space (that way the DiscreteRV package will already be installed).

Run each of the lines of code below and try answering each of the questions.

```r
library(discreteRV)
```

Using the RV function you can create a random variable -- a set of outcomes and their association probabilities.  Here's one random variable that looks like the roll of a die.

```r
X <- RV(outcomes = 1:6, probs = 1/6)
plot(X)
```

Here's another random variable representing the results of betting $100 on a roulette spin:

```r
R <- RV(outcomes = c(100, -100), 
  probs = c(18/38, 20/38))
  
plot(R)
```
# Expected Value, Variance and Standard Deviation

You can also find the expected value, variance and standard deviations of random variables.  The (long) way to get the expected value is to use the formula:

$$E[x] = \sum x \cdot P(x)$$ 

Here's the expected value of a die roll:

```r
sum(outcomes(X)*probs(X))
```

The easier way is to use the Expectation function in the *discreteRV* package:

```r
E(X)
```

Similarly, you can calculate variance by our formula: $$Var[X] = \sum P(x)(x-E[x])^2$$

```r
sum(probs(X)*(outcomes(X) - E(X))^2)
```

or you can use the Variance function and tqke the square root to get the standard deviation:

```r
V(X)

sqrt(V(X))
```

**1. Use the function above to get the expected value, variance and standard deviation of a $100 bet on a roulette spin.**

# Sums of Random Variables

What if you roll two dice and add the results?  

You could treat this as the sum of two *independent and identically distributed "iid" random variables*.  "Identically Distributed" means that the dice have the same possible outcomes and associated probabilities.  Independent, as you know, means that the result of one die tells you nothing about the result of the other die.

The function for creating the sum of *independent and identically distributed* random variables in the discreteRV package is "SofIID".  We can use it to create a random variable representing the results of two die rolls as follows (remember that X is the result of one die roll):

```r
two_dice <- SofIID(X, n=2)
plot(two_dice)
```
What if you roll 20 dice and add them all up?  What will this distribution look like?

```r
twenty_dice <- SofIID(X, n=20)
plot(twenty_dice)
```

You can also find the probability that a random variable will exceed a certain value or fall within a certain range.

```r
# the chance that the sum of twenty dice exceeds 80
P(twenty_dice > 80)

# the chance that the sum of twenty dice 
# is between 60 and 80 inclusive
P((twenty_dice>= 60) %AND% (twenty_dice <=80))

# the chance that the sum of twenty dice 
# is not between 60 and 80 inclusive
P((twenty_dice < 60) %OR% (twenty_dice > 80))
```

2. Find the probability that the sum of twenty dice equals 70 exactly

You can also get the expected value and variance of these sums of random variables:

```r
E(twenty_dice)
V(twenty_dice)
sqrt(V(twenty_dice))
```

3. How do these values compare with the expected value, variance and standard deviation of one die roll?

4. What is the probability that the sum of twenty die rolls is more than one standard deviation above its expected value?

5. Try to create and plot a random variable that represents betting $100 on a spin of roulette, 20 times.

6. What is the probability that you win money if you play roulette 20 times?

7. Simulating the *2054 Election* (with some stylized facts)

On the night before the election, according to your model, Charlotte has 15 states, the District of Columbia, and 199 electoral college votes locked up.  Rhea has 23 states and 188 electoral college votes in the bag.  The other 12 states can each be represented as a random variable (as shown below). The random variables are from Charlotte's point of view.  So, we think Charlotte is looking good in Virginia where she has a 95% chance of getting the 13 electoral college votes but Rhea is in command in Colorado, where Charlotte only has a 9% chance of getting the 9 electoral college votes.

```r


Virginia <- RV(outcomes=c(0,13), probs=c(0.05, 0.95))
New_Mexico <- RV(outcomes=c(0,5), probs=c(0.05, 0.95))
New_Hampshire <- RV(outcomes=c(0,4), probs=c(0.10, 0.90))
Nevada <- RV(outcomes=c(0,6), probs=c(0.15, 0.85))
Pennsylvania <- RV(outcomes=c(0,20), probs=c(0.20, 0.80))
Arizona <- RV(outcomes=c(0,11), probs=c(0.30, 0.70))
Michigan <- RV(outcomes=c(0,16), probs=c(0.35, 0.65))
Georgia <- RV(outcomes=c(0,16), probs=c(0.50, 0.50))
Florida <- RV(outcomes=c(0,29), probs=c(0.80, 0.20))
Oregon <- RV(outcomes=c(0,7), probs=c(0.85, 0.15))
North_Carolina <- RV(outcomes=c(0,15), probs=c(0.90, 0.10))
Colorado <- RV(outcomes=c(0,9), probs=c(0.95, 0.05))

```
You can then create a random variable that is the sum of Charlotte's electoral college votes and plot it:

```r
Charlotte = 199 + New_Mexico + 
  Michigan + Pennsylvania + New_Hampshire +
  Nevada + Oregon + Colorado + Virginia +
  Florida + North_Carolina +
  Arizona + Georgia

plot(Charlotte)
```

a. According to this model, what is Charlotte's chance of winning the election (note that she needs 270 electoral college votes in order to win)?

b. According to this model, what is Charlotte's chance of winning 300 or more electoral college votes?

Note: Once you have a Charlotte random variable, it's easy to create a Rhea random variable...

```r
Rhea = 538 - Charlotte
```

you can also plot the difference:

```r
plot(Rhea-Charlotte)
```

c. What are the shortcomings of this model?  Assuming that the probabilities for each state are correct does this model accurately predict Charlotte's chances of winning, underestimate her chances or overestimate her chances?
