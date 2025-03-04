Correlations
-------------------------------

In this lab, we'll look at some new data sets and find correlations.

There are some packages you'll need, which means that you'll want to open posit.cloud and work within our Statistics workspace where those packages are already installed.

Let's load the packages we need:

```r
library(openintro)
library(datarium)
# The openintro and datarium packages have datasets that we will use

library(tidyverse)
# You know what this does!
```

# 1. Why do some mammals sleep more than others?

```r
data("mammals", package = "openintro")
View(mammals)
```

Here's a link to a description of the data: [Mammals Description]("https://www.openintro.org/data/index.php?data=mammals").  (Dream sleep and non-dream sleep times can be determined using electroencephalogram and electromyography.)

# Mammal Scatterplots

Let's look at the relationship between some other variables and amounts of sleep.

```r
mammals %>% 
  ggplot(aes(gestation, total_sleep)) +
  geom_point()
  
mammals %>% 
  ggplot(aes(body_wt, total_sleep)) +
  geom_point()
```

**Challenge 1:** Create a plot showing the relationship between brain_wt and total_sleep.

One problem with this plot is that the massive elephants crunch all of the other mammal weights together and so it's hard to see the relation between size and amount of sleep.  One solution is to use *mutate* to create a new variable "log_brain_wt" and look at the relationship between that variable and sleep.

```r
mammals =
  mammals %>%
  mutate(log_brain_wt = log(brain_wt))

mammals %>% 
  ggplot(aes(log_brain_wt, total_sleep)) +
  geom_point()

```

We could also use *mutate* to create a new variable "percent_brain" which is the percentage of body weight that is brain weight.  The formula for this new variable will look strange.  Brain weights are in grams and body weights in kilograms so to get the right proportion we need divide brain_wt/body_wt by 1000 and then to turn it into a percentage we have to multiply by 100.  This explains why we use 0.1*brain_wt/body_wt.

```r
mammals = 
mammals %>%
  mutate(percent_brain = 
           0.1*brain_wt/body_wt)
           
mammals %>% 
  ggplot(aes(percent_brain, total_sleep)) +
  geom_point()           
```

We can also add labels and best-fit lines to each of the last two plots (run these one at a time):

```r
mammals %>% 
  ggplot(aes(log_brain_wt, total_sleep)) +
  geom_label(aes(label=species))+
  geom_smooth(method="lm")


mammals %>% 
  ggplot(aes(percent_brain,total_sleep)) +
  geom_label(aes(label=species))+
  geom_smooth(method="lm")
```

**Question 1:** What do the two graphs showing the relationship of total sleep to "brain_percent" and "log_brain_wt" tell you?  Do big brained mammals sleep more than small brained mammals?  What about big *"brain percent"* mammals, do they sleep more or less?


**Question 2:** Which mammals sleep more than we’d expect based on their brain size (or brain percent)? Which mammals sleep less than expected?


**Question 3:** Are any other variables that can be used to predict amounts of sleep?  What are they?

# Mammal Correlations

Which variable do you think has the strongest correlation with sleep time?

You can find the correlation between percent_brain and total sleep as follows:

```r
mammals %>%
  summarize(cor(percent_brain, total_sleep, use="pairwise.complete"))
```

**Challenge 2:*** 
Find the variable with the strongest (largest positive or negative) correlation with total_sleep.



# 2. MLB Stats

This data contains Major League Baseball Player Hitting Statistics for 2018.

```r
data("mlb_players_18", package = "openintro")
```

[MLB Bat Description]("https://www.openintro.org/data/index.php?data=mlb_players_18")

You can calculate singles as follows:

```r
mlb_players_18 = 
mlb_players_18 %>%
  mutate(singles = H - doubles - triples - HR)

```

**Challenge 3:**
Try to creating four plots showing the relationship of each of singles, doubles, triples, and home runs with runs

**Challenge 4:**
Predict which amount singles, doubles, triples and HR will have the strongest correlation with runs and then write code to find all four correlations.  Were you correct?

**Challenge 5:**
Predict the correlation between strikeouts and runs.  Do you expect it to be positive or negative?  Now write code to find the correlation.  Were you correct?

**Challenge 5:**
You can find the correlation between singles and runs for regular players (players who played in at least 120 games) as follows:

```r
mlb_players_18 %>% 
  filter(games >= 120) %>% 
  summarize(cor(singles, R))
```

How does this compare to the correlation between singles and runs for all players?  Can you explain the difference?

**Challenge 6:**
Among players who played in at least 120 games, find the variable that has the strongest correlation with runs.

**Challenge 7:**
Can you find a pair of variables that has a negative correlation?  What if you limit the data to players who have played in at least 120 games?  Or at least 140 games?