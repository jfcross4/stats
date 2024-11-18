Plotting, Grouping and Summarizing *Other* Data
--------------------------------------------

Today, we're going to practice the same types of plotting that we did in [last week's lab](https://github.com/professorkalim/stats22-23/blob/cross/census_plotting.md).  But, this week, you'll get to pick your own data (from five choices) and have to figure out how to graph it on your own.

First, load the tidyverse.

```r
library(tidyverse)
```

Next pick *one* of the five following datasets.  


# Data Choices (and code to acquire it and view it)

1. Data about flowers
```r
data(iris)
View(iris)
```

2. Data about who lived and die on the titanic
```r
install.packages("titanic")
library(titanic)
View(titanic_train)
```

3. Data about the Health and Wealth of Nations
```r
install.packages("gapminder")
library(gapminder)
View(gapminder)
```

4. Airbnb Data from Chicago

```r
install.packages("bayesrules")
library(bayesrules)
View(airbnb)
```


5. Volleyball Stats (player-game level data from Germany 2013/2014 - 2018/2019)

```r
install.packages("volleystat")
library(volleystat)
View(matchstats)
```

# Graphing the Data

The following is example code that you could use to make bar plots, histograms, box plots, and scatter plots.  You'll need to replace "data" with the name of your data set and "discrete_variable" and "continuous_variable" with the names of discrete and continuous variables in your data set.  Good luck!

```r
# bar plot
data %>% 
  ggplot(aes(discrete_variable)) +
  geom_bar() + 
  theme(axis.text.x = element_text(angle = 90))

# histogram
data %>% 
  ggplot(aes(continuous_variable))+
  geom_histogram(binwidth = 1, color="red")
  
# boxplot by group
data %>% 
  ggplot(aes(continuous_variable, 
             discrete_variable))+
  geom_boxplot()
  

# scatterplot
data %>% 
  ggplot(aes(x_continuous_variable, y_continuous_variable))+
  geom_point() + 
  geom_smooth()

```
