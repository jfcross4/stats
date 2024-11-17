Multiple Regression Lab
---------------------------------

```{r}
library(openintro)
library(datarium)
# The openintro and datarium packages have datasets that we will use

library(tidyverse)
# You know what this does!
```

# 1. Why do some mammals sleep more than others?

```{r}
data("mammals", package = "openintro")
View(mammals)
```
Predict total_sleep from some combination of life_span, gestation, predation, exposure, danger, body_wt and brain_wt (it would be cheating to use “non_dreaming” and “dreaming” sleep because these are amounts of two types of sleep).

You should start by reading a description of the data: [Mammals Description]("https://www.openintro.org/data/index.php?data=mammals")

Then let’s start building our model by predicting total_sleep from predation.

```{r}
colnames(mammals)
m <- lm(total_sleep ~ predation, data=mammals)
summary(m)
```

**Question 1:** How would you interpret this model summary? Does this make intuitive sense? Roughly how much less sleep would a mammal with a predation level of 5 be expected to get than an animal with a predation level of 1?

We might also be interested in using mammals’s brain weights. There’s a bit of a problem with this, however, which is that the animals that are big (and big-brained) are orders of magnitude bigger than the smaller animals and the differences between smaller animals are irrelevant by comparison. We can see this in a histogram of brain weights and in a plot of sleep versus brain weight.

```{r}
mammals %>% ggplot(aes(brain_wt))+geom_histogram()

mammals %>% 
  ggplot(aes(brain_wt, total_sleep)) + 
  geom_point()+
  geom_smooth(method="lm", color="red")
```
We can address this issue by creating a new variable which is the logarithm of brain weight. If you haven’t yet learned about logarithms, don’t fret! We are just creating a new variable, log_brain_wt, such that:

$$10^{log_{10}(brain\ wt)}=brain\ wt$$

so that if log_brain_wt is 2, then brain_wt must be $10^2$
 or 100 grams (the information page says that these brain weights are in kilograms but that’s ridiculous).

```{r}
mammals <- mammals %>% 
  mutate(log_brain_wt = log(brain_wt, base=10))

mammals %>% ggplot(aes(log_brain_wt))+geom_histogram()


mammals %>% 
ggplot(aes(log_brain_wt, total_sleep)) + 
  geom_point()+
  geom_smooth(method="lm", color="red")

mammals %>% 
  ggplot(aes(log_brain_wt, total_sleep, label=species)) + 
  geom_text()+
  geom_smooth(method="lm", color="red")
```

Now, we can add log_brain_wt to our prediction model:

```{r}
m <- lm(total_sleep ~ log_brain_wt + predation, data=mammals)

summary(m)
```

If we want, we can add the expectations (or predictions) of our model to the data set.

```{r}
mammals$pred_sleep <- predict(m, mammals)
```

and graph the predictions agains the actual amounts of sleep:

```{r}
mammals %>% 
  ggplot(aes(pred_sleep, total_sleep, label=species)) + 
  geom_text()+
  geom_smooth(method="lm")
```

**Question 2:** Which mammals sleep more than we’d expect based on our model? Which mammals sleep less than expected?

Try adding other variables to your sleep model. 

**Question 3:** Are any other variables important?  What are they?

# 2. Stress

```{r}
data("stress", package = "datarium")
```
Researchers want to evaluate the effect of a new “treatment” on the stress scores of subjects after adjusting for “age” and “exercise”.



```{r}
m <- lm(score ~ treatment, data=stress)
summary(m)
```
The coefficient of “treatmentno” shows the difference in stress scores between “no” treatment and “yes” treatment. In other words, folks with no treatment had stress scores 4.8 points higher than folks with treatment.

The “residuals” of this model are the actual stress levels less the predicted stress levels. Let’s plot these residuals against the ages of our subjects.


```{r}
plot(stress$age, residuals(m))
```

**Question 4:** What does this residual plot show us? Does it seems like subject age is related to stress level?

We can build a new version of this model with an age adjustment.

```{r}
m_age_adjusted <- lm(score ~ treatment+age, data=stress)
summary(m_age_adjusted)
```

**Question 5:** Can you build a model that adjusts for age and exercise level? How would you interpret this model?

# 3. Surviving the Titanic

```{r}
data("titanic.raw", package = "datarium")

titanic.raw <- titanic.raw %>% mutate(SurvivedTF = Survived=="Yes")
```

This data provides information on the passengers onboard the Titanic and, in paricular, who lived and who died.

The first thing, we need to do is tranform the “Yes”/“No” Survived column into TRUE/FALSE or 1/0 values. The code to do this is shown above. We’ll need to try to predict this “SurvivedTF” value we created rather than “Survived”.

First, let’s look at a summary of the data:

```{r}
summary(titanic.raw)
```

This shows how many passengers fall into each category. When you build regression models with categorical data, the model will choose one value from each category you use in your model as the baseline and show the affects of other values relative to that value. For instance, if we run the code:

```{r}
m <- lm(SurvivedTF ~ Class, data=titanic.raw)
summary(m)
```

R chooses 1st class as the default/baseline Class. The regression summary shows that 2nd class passengers were 21% less likely to survive than 1st class passengers and that 3rd class passengers were 37% less likely to survive than 1st class passengers. Crew members were the least likely to survive.

**Question 6:** Which other factors were important in determining who survived the sinking of the Titanic? Try building regression models to find out and report your findings.

# 4. MLB Stats

This data contains Major League Baseball Player Hitting Statistics for 2018.

```{r}
data("mlb_players_18", package = "openintro")
```

[MLB Bat Description]("https://www.openintro.org/data/index.php?data=mlb_players_18")

You can calculate singles as follows:

```{r}
mlb_players_18 = 
mlb_players_18 %>%
  mutate(singles = H - doubles - triples - HR)

```

Try to predict Runs, “R”, from singles, doubles, triples, home runs, walks, stolen bases and caught stealing. 

**Question 7:** Does the model you made sense?  In other words, do the relative values of these events match your expectations?

# 5. Duke Students GPA

"A survey of 55 Duke University students asked about their GPA, number of hours they study at night, number of nights they go out, and their gender."

```{r}
data("gpa", package = "openintro")
```
[Duke Student's GPA Description]("https://www.openintro.org/data/index.php?data=gpa")

**Question 8:** Try to predict the gpa of Duke students using data on their studying, sleeping, partying and gender.  What does your model say are the most important predictors of student GPA (at Duke)?  Try to explain your results as clearly as possible.
