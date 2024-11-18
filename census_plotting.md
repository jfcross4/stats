Plotting, Grouping and Summarizing School Census Data
--------------------------------------------

Please go to Rstudio cloud and open your school_census project. You can complete the following within that project.

# Getting the data:

The "clean_census" function cleans up some messy data.  You can look at the data with "View()" before and after cleaning it.

```r
library(tidyverse)

source("clean_census.R")

c2020 = read.csv("census_ny_all_grades_2020.csv")
c2022 = read.csv("census_ny_all_grades_2022.csv")
census = rbind(c2020, c2022)

View(census) #before cleaning

census = clean_census(census)

View(census) #after cleaning
```

# Univariate Plots
(one variable at a time)

We can plot categorical variables with bar plots.  Here's a bar plot of ways students traveled to school.

```r
census %>% ggplot(aes(Travel_to_School))+
  geom_bar()
```

And here it is with a more readable x-axis...

```r
census %>% ggplot(aes(Travel_to_School))+
  geom_bar() + 
  theme(axis.text.x = element_text(angle = 90))
```


Try:

```r
colnames(census)
```

to look through all of the variables.  

# Challenge 1: 
Pick out a different categorical variable and use it to make a bar plot.

# Histograms
We can use histograms to plot continuous/numerical variables.  Here's a histogram of the number of hours of sleep students get on school nights.

```r
census %>% ggplot(aes(Sleep_Hours_Schoolnight))+
  geom_histogram(binwidth = 1, color="red")
```

# Challenge 2: 

Pick out a different numeric variable and use it to make a histogram.

# Bivariate Plots
We can use bivarite plots to show the relationship between two variables.

The following boxplots show the relationship between mode of travel to school (categorial) and sleep hours (numeric).

```r
census %>% 
  ggplot(aes(Sleep_Hours_Schoolnight, 
             Travel_to_School))+
  geom_boxplot()
```

You might remember from our boxplot that four of the methods of transportation are far more common than the others.  Let's focus in on those four:

```r
census %>% 
  filter(Travel_to_School %in% 
           c("Rail (Train/Tram/Subway)", "Bus", "Car",
             "Walk")) %>%
  ggplot(aes(Sleep_Hours_Schoolnight, 
             Travel_to_School))+
  geom_boxplot()
```

# Question 1: 
How would you explain the difference between the median sleep hours between rail riders and bus riders?

# Challenge 3: 
Pick out a pair of variables, one numeric and one categorical and make a series of boxplots showing their relationship.

# Numeric or Categorical?

```r
census %>% 
  ggplot(aes(Sleep_Hours_Schoolnight, fill=ClassGrade))+
  geom_histogram(binwidth = 1) + 
  facet_grid(ClassGrade~.)
```

```r
census %>% 
  ggplot(aes(Sleep_Hours_Schoolnight, fill=as.factor(ClassGrade)))+
  geom_histogram(binwidth = 1)+
  facet_grid(ClassGrade~.)
```


# Scatterplots!

Scatterplots show the relationship between two continuous variables.  Here's a plot of sleep hours (on school nights) versus grade.  I've added a "smooth" best fit line.

```r
census %>% 
  filter(Gender!="") %>%
  ggplot(aes(ClassGrade, Sleep_Hours_Schoolnight))+
  geom_point() + 
  geom_smooth()
```

# Challenge 4: 
Pick out two numeric variables and make a scatterplot showing their relationship.


## Filtering, Grouping and Summarizing

Recently, we were tasked with finding summary statistics for different groups and I want to review how we can do that.

Let's suppose that we wanted to compared the sleep of younger high schoolers (9th and 10th graders) to that of upperclassmen (11th and 12th graders).   The following code will do that for us and it's worth trying to understand it piece by piece.

* First we filter out students younger than 9th grade and responses where *Sleep_Hours_Schoolnight* was missing.  Next, with the "group_by" line we split the remaining students (now, all high schoolers) up by whether they are in 11th grade or higher.  Finally, with "summarize" we calculate three things for each group.  First we find out how many responses we have from each next, then how many hours of sleep they average and finally, what proportion of the responses reported 8 or more hours of sleep.

```r
census %>%
  filter(ClassGrade >= 9,
         !is.na(Sleep_Hours_Schoolnight)) %>%
  group_by(ClassGrade>=11) %>%
  summarize(n=n(),
            mean_sleep = mean(Sleep_Hours_Schoolnight),
            good_sleep_prop = 
              mean(Sleep_Hours_Schoolnight>=8))
```

The following code splits middle-schoolers into 5th/6th graders in one group and 7th/8th graders in another.

```r
census %>%
  filter(ClassGrade <= 8,
         !is.na(Sleep_Hours_Schoolnight)) %>%
  group_by(ClassGrade>=7) %>%
  summarize(n=n(),
            mean_sleep = mean(Sleep_Hours_Schoolnight),
            good_sleep_prop = 
              mean(Sleep_Hours_Schoolnight>=8))
```

# Question 2: 
Based on the previous two data summaries, how would you describe how sleep hours changes from 5th to 12th grade?


# Challenge 5: 
Write code that compares the mean travel time to school middle schoolers to the mean travel time to school for high schoolers.
