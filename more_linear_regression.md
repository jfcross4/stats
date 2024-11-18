More Linear Models Lab
---------------------------------

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

Let's pick up where we left off in class!  We can use *mutate* to create a new variable "percent_brain" which is the percentage of body weight that is brain weight.  Olympus proposed this as a solution to the "elephant in the room" (hat tip, Sam) which was that two species had *much* larger (orders of magnitude larger) brains than all the other species.

We can see this in a histogram of brain weights and in a plot of sleep versus brain weight.

```r
mammals %>% ggplot(aes(brain_wt))+geom_histogram()

mammals %>% 
  ggplot(aes(brain_wt, total_sleep)) + 
  geom_point()+
  geom_smooth(method="lm", color="red")
```

Here's Olympus's solution:

```r
mammals = 
mammals %>%
  mutate(percent_brain = 
           0.1*brain_wt/body_wt)
```

Now, let's look at the relationship between "percent_brain" and sleep:

```r
mammals %>% 
  ggplot(aes(percent_brain,total_sleep)) +
  geom_label(aes(label=species))+
  geom_smooth(method="lm")
```

Alternatively, we could address this issue by creating a new variable which is the logarithm of brain weight. We will create a new variable, log_brain_wt, such that:

$$10^{log_{10}(brain\ wt)}=brain\ wt$$

so that if log_brain_wt is 2, then brain_wt must be $10^2$
 or 100 grams (the information page says that these brain weights are in kilograms but that’s ridiculous, these numbers are in grams).

Note: run these bits of code one at a time so that you see each plot.

```r
mammals <- mammals %>% 
  mutate(log_brain_wt = log(brain_wt, base=10))

mammals %>% 
ggplot(aes(log_brain_wt))+
geom_histogram()


mammals %>% 
ggplot(aes(log_brain_wt, total_sleep)) + 
  geom_point()+
  geom_smooth(method="lm", color="red")

mammals %>% 
  ggplot(aes(log_brain_wt, total_sleep, label=species)) + 
  geom_text()+
  geom_smooth(method="lm", color="red")
```

**Question 1:** What do the two graphs showing the relationship of total sleep to "brain_percent" and "log_brain_wt" tell you?  Do big brained mammals sleep more than small brained mammals?  What about big *"brain percent"* mammals, do they sleep more or less?


**Question 2:** Which mammals sleep more than we’d expect based on their brain size (or brain percent)? Which mammals sleep less than expected?


**Question 3:** Are any other variables that can be used to predict amounts of sleep?  What are they?

# 2. Surviving the Titanic

```r
data("titanic.raw", package = "datarium")

titanic.raw <- titanic.raw %>% mutate(SurvivedTF = Survived=="Yes")
```

This data provides information on the passengers onboard the Titanic and, in paricular, who lived and who died.

The first thing, we need to do is tranform the “Yes”/“No” Survived column into TRUE/FALSE or 1/0 values. The code to do this is shown above. We’ll need to try to predict this “SurvivedTF” value we created rather than “Survived”.

First, let’s look at a summary of the data:

```r
summary(titanic.raw)
```

This shows how many passengers fall into each category. When you build regression models with categorical data, the model will choose one value from each category you use in your model as the baseline and show the affects of other values relative to that value. For instance, if we run the code:

```r
m = lm(SurvivedTF ~ Class, 
          data=titanic.raw)

summary(m)
```

R chooses 1st class as the default/baseline Class. The regression summary shows that 2nd class passengers were 21% less likely to survive than 1st class passengers and that 3rd class passengers were 37% less likely to survive than 1st class passengers. Crew members were the least likely to survive.

**Question 4:** Which other factors were important in determining who survived the sinking of the Titanic? Try building regression models to find out and report your findings.

# 3. MLB Stats

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

Try to predict Runs, “R”, from each of singles, doubles, triples, and home runs. 

**Question 5:** Do you best fit lines make sense?  In other words, do the relative values of these events match your expectations?  

**Question 6:**
Try plotting your data.  Do linear best fit lines seem like the best way to model the relationships between these variables (singles, doubles, triples, and home runs) and runs?
