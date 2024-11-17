---
output:
  pdf_document: default
  html_document: default
---
Lab 3: Vectors, Simulation and the Two Daughter Problem
--------------------------------------------

In this lab we'll use vectors and simulation in R to solve probability problems!

Please sign on to [RStudio Cloud](https://posit.cloud/).  Once again, you can either start a new project or continue on in the project you used for the last (non-Data Camp) lab.

# Coin Flips

We can start by creating a vector that has the two outcomes for a coin flip.

```r
coin = c("H", "T")
```

The our "flips" will be values plucked from this vector.  When we pluck (or "sample") values from this vector we'll want to do it with replacement meaning we can get the same result repeatedly (after we pull out "H", for example, we replace it before sampling again).

Here's what three coin flips looks like:

```r
sample(coin, size=3, replace=TRUE)
```

**Try running the code above repeatedly.**  You'll get different results!  In this lab, you likely won't get precisely the same results as your neighbor.

Now, let's try flipping 100 coins and assigning the entire sequence of flips to a variable (called "flips").

```r
flips = sample(x=coin, size=100, replace=TRUE)
```

We can summarize our flips in a number of ways:

```r
# to get a summary of the outcomes
table(flips)
```

```r
# to calculate the number of heads
num_heads = sum(flips == "H")
num_heads
```

```r
# to calculate the proportion of flips that were heads
proportion_heads = mean(flips == "H")
proportion_heads
```

If we want to flip 100 coins repeatedly and track how many of the 100 flips are heads each time, we could run the code above repeatedly and jot down the number of heads each time on a piece of paper.  **Or**, we could have R keep track for us.  We'd start by creating an empty vector in which to store our results:

```r
head_counts = c() #creates a vector with no values
```

Now, we can "flip" a coin a 100 times and count the number of heads and add that value to head_counts:

```r
flips = sample(x=coin, size=100, replace=TRUE)
num_heads = sum(flips == "H")

# "head_counts" becomes whatever "head_counts" was but with 
# one additional value:

head_counts = c(head_counts, num_heads)
```

You can run the code immediately above repeatedly to keep adding values.  Be careful not to recreate the empty vector or else you'll overwrite your previous results and will be starting from scratch!

# For Loops: A Programming Interlude

We can ask R to repeat the same code again and again using a *for loop*.  Here's an easy way to say hello ten times:

```r
for (i in 1:10){
print("Hello")
}
```

or you want the first 20 letters in the alphabet:

```r
for (i in 1:20){
print(letters[i])
}

```

or the first 15 multiple of 5:

```r
for (i in 1:20){
print(5*i)
}
```

# Using For Loops for Simulation

We can also use a for loop to repeat our coin flipping experiment many times over.  Let's try flipping 100 coins, 1000 times over (for 100,000 total flips!).  

```r
head_counts = c()

for (i in 1:1000){
  flips = sample(x=coin, size=100, replace=TRUE)
  num_heads = sum(flips == "H")
  head_counts = c(head_counts, num_heads)
}

head_counts
```

Now's let's summarize our results in a few ways!  

**Try to make sure that you understand each of the following:**

```r
mean(head_counts)

max(head_counts)

min(head_counts)

mean(head_counts >= 50)

mean(head_counts >= 60)

table(head_counts)

hist(head_counts)
```

## Question 1:
Roughly how likely is someone to flip 45 or more heads in 100 coin tosses?  (You can write a piece of code similar to one of the lines above to answer this question.)

# Simulating the Two Daughter Problem

**You (being forward):** How many children do you have?

**Stranger on the subway:** Two.

**You:**  Do you have a daughter, yes or no?

**Stranger (looking around for a cop):** Yes…

**Question:**  What (approximately) are the chances that this stranger has two daughters?

The Two Daughter Problem assumed that children genders are like flips of a coin.  So, we can create a child vector just like we made a coin vector:

```r
child = c("D", "ND")
```

Now, let's create 1000 imaginary two child families and record the number of daughters just like we record the numbers of heads.

```r
daughter_counts = c()

for (i in 1:1000){
  two_child_fam = sample(x=child, size=2, replace=TRUE)
  num_daughters = sum(two_child_fam == "D")
  daughter_counts = c(daughter_counts, num_daughters)
}
```

We can use some of the same summaries:

```r
mean(daughter_counts)

max(daughter_counts)

min(daughter_counts)

table(daughter_counts)

mean(daughter_counts >= 1)

mean(daughter_counts == 2)

```
We're looking for:

$$P(two\ daughters\ |\ at\ least\ one\ daughter)$$

and using our formula for conditional probability:

$$P(A | B) = \frac{P(A \cap B)}{P(B)}$$

we can rewrite this as:

$$P(A | B) = \frac{P(two\ daughters\  \cap \ at\ least\ one\ daughter)}{P(at\ least\ one\ daughter)} = \frac{P(two\ daughters)}{P(at\ least\ one\ daughter)}$$

and we can calculate that in R with:

```r
mean(daughter_counts == 2)/mean(daughter_counts >= 1)
```

Based on the code above, what (roughly) is the solution to the original two daughter problem?

# A Three Daughter Problem

**You:** Do you have 3 children?

**Stranger:** Yes, exactly 3.

**You:** Do you have at least one daughter?
  
**Stranger:** Yes.

**Question:** What are the chances that this stranger has three daughters?
  
  
## Challenge 1: 
Try using simulation to answer this question.

## Challenge 2:

A large number of people each flip 100 coins and count the number of heads they flipped.  Everyone who flipped more heads than tails is asked how many heads they flipped.  Use simulation to estimate the average number of heads flipped by people who flipped more heads than tails.

## Appendix: The Alternative Two Daughter Problem

You (being forward): How many children do you have?

Stranger on the subway: Two.

You:  Do you have a daughter named Rosemary, yes or no?

Stranger (looking around for a cop): Yes…

Question:  What (approximately) are the chances that this stranger has two daughters?


```r
# This code assumes that 1 in every 50 girls is named Rosemary.
# This is a wild overestimate but a lower, more realistic
# estimate would require us to run even more simulations!


child = c("D", "ND", "DR")
daughter_counts = c()
rosmary_counts = c()

for (i in 1:10000){
  two_child_fam = sample(x=child, 
                         size=2, 
                         prob = c(0.49, 0.50, 0.01),
                         replace=TRUE)
  num_daughters = sum(two_child_fam == "D" | two_child_fam == "DR")
  num_rosmarys = sum(two_child_fam == "DR")
  daughter_counts = c(daughter_counts, num_daughters)
  rosmary_counts = c(rosmary_counts, num_rosmarys)
}

sum(daughter_counts==2 & rosmary_counts>=1)/sum(rosmary_counts>=1)
```
