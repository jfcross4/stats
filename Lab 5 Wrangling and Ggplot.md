Lab 5: DataCamp Practice - Wrangling and Ggplot
------------------------------------------------

Today we're going to practice DataCamp skills with wrangling and plotting data; perhaps learn something about movies too. Remember to save your file in the Statistics Shared folder on your desktop before you begin and regularly click save throughout class. To share your folder with me, drag the folder into your Google Drive folder so it gets saved in the cloud.

We'll need three R packages for this lab.  Start by installing this one:
```r
install.packages("ggplot2movies")
```

Then load the three libraries we will need:
```r
library(tidyverse)
library(ggplot2)
library(ggplot2movies)
```

Now, let's take a peak at the first few movies:
```r
glimpse(movies)
```

**Question:** What are three or four other functions you have used to investigate dataframes and vectors? List all the ones you recall with a #note stating their purpose directly in your RScript. If you don't recall, then review your past DataCamp assignments and then come back to this.

Example: 
```r
glimpse(movies) 
#shows columns as rows so you can see each variable name and the first first few data points
```

# Plotting the Data

## Histograms

Histograms are a way of showing the distribution of the outcomes from a random experiment (like rolling a die). It's also useful in representing numerical data like movie ratings. Run the code below and investigate the graphs that are generated. If you get an error, read the error message carefully and try to troubleshoot before asking for help. *(Example: Could not find function "ggplot" means that you forgot to run the line of code above to load the package)*

**Task:** Run the code for each histogram, analyze the graph and then do your best to write a description of what you think the graph is saying about the data. The description of Histogram 1 is already done for you.

```r
#Histogram 1
#Description: a histogram of the number of people who rated a movie.  Most movies have very few ratings but a small number of movies have many ratings!
movies %>% 
  ggplot(aes(votes)) + 
  geom_histogram()

#Histogram 2
#Description:
movies %>% 
  ggplot(aes(rating)) + 
  geom_histogram()

#Histogram 3
#Description:
movies %>% 
  ggplot(aes(year)) +
  geom_histogram()

#Histogram 4 (Don't worry about explaining this one...it's a weird one. But still run the code.)
#Description: this is a histogram of the % of people who gave the movies a rating of 10 on a 1-10 scale.
#it's rounded in an unusual way as you'll see in the histogram.
movies %>% 
  ggplot(aes(r10)) + 
  geom_histogram()

```
**Task:** Begin by looking at the movies dataframe *(Do you remember how to do this? One way is to store 'movies' in an object so it appears in the top right corner of RStudio. Then you can click on it to see the dataframe with rows and columns.)*

Now that you see all of the data, pick two new variables (column headers) that seem interesting to you and make two new histograms using similar code as in the 4 histograms above. Write a description as you did with the previous histograms. I'll start you off with the first few lines:

```r
#Histogram 5
#Description:
movies %>%
  
  
#Histogram 6
#Description:
```

**Reflection:** Answer the following questions thoughtfully using a #note - (1) What do you notice about the histograms above? (2) What do you wonder about them? (3) If you could get your hands on any type of data, what would it be and why? i.e. climate data, sports data, etc. (4) What types of graphs have you made in previous math classes? i.e. pie chart, line graph, etc.

# Analyzing the Plot
Boxplots and Scatterplots are two typical types of graphs that you will come across in the news, on social media, and in a Statistics classroom. They show the possible relationship between different variables.

**Observations:** Run the chunks of code below to generate these types of graphs. Look at the labels on the x- and y- axes of the graphs and determine what story the graphs might be telling about the data and then write a short observation in your own words. This might be the first time you are seeing this type of graph; it is okay to ask me, your classmates, or the internet if you are unsure. It's also okay to be wrong! Do your best on this part.

## Boxplots 

We can plot a few distributions side by side with a box plot. Here are distribution of movie ratings for movies with different mpaa ratings.  
**Note:** The box covers the middle 50% of the data; extending from the 25th percentile rating up to the 75th percentile rating. The number in the middle where the line cuts the box is called the median.

```r
#MPAA Rating Boxplot
movies %>% 
  filter(mpaa != "") %>%
  ggplot(aes(mpaa, rating)) + 
  geom_boxplot()
#Observation: 
```

## Scatterplots

Scatterplots help us determine if there is a strong correlation between two variables in a dataset.

This plot of the relationship between movie length and movie rating is unfortunately ruined by a few ridiculously long movies.

```r
#Scatterplot 1
movies %>%
  ggplot(aes(length, rating)) +
  geom_point()
#Observation: 
```

Here is the same plot but you will limit it to movies with at least 10,000 votes. Edit the code to reflect this filter. Compe up with a short title to clarify what we're plotting; it should be something about movie ratings and lenght of movies. Edit ggtittle() to write your title.

```r
#Scatterplot 2
movies %>%
  filter(votes >= ________) %>%
  ggplot(aes(length, rating)) +
  geom_point() +
  ggtitle("____________")
#Observation: 
```

Let's look at the relationship between budget and rating. I titled this one "Movie Rating v. Budget, min 10,000 votes" but you can change it to be consistent with your naming convention above.

```r
#Scatterplot 3
movies %>%
  filter(votes >= 10000) %>%
  ggplot(aes(budget, rating)) +
  geom_point() +
  ggtitle("Movie Rating v. Budget, min 10,000 votes")
#Observation: 
```

and between number of votes and rating:

```r
#Scatterplot 4
movies %>%
  ggplot(aes(votes, rating)) +
  geom_point() +
  ggtitle("Movie Average Rating v. Number of Ratings")
#Observation:
```



# Data Wrangling
Let's practice with some of the data wrangling "verbs" we learned about on DataCamp and a new one called *top_n*. We've already used *filter()*. Before you run the code, type ?top_n in the console to read about this verb. 

After you run the code, you can click on the object name in the top right section of RStudio will open the dataframe.

## top_n and select

We can find the 5 movies with the largest budgets and show the movie name, the budget and the year:

```r
df1 <- movies %>%
  top_n(5, budget) %>%
  select(title, budget, year)
```

We can find the 10 highest rated movies and show the average rating in addition to the budget (note that this list runs past 10 due to ties):

```r
df2 <- movies %>%
  top_n(10, rating) %>%
  select(title, budget, rating)
```

That list includes some obscure movies, let's use filter to limit our list to movies with at least 100 votes.  

```r
df3 <- movies %>%
  filter(votes >= 100) %>%
  top_n(10, rating) %>%
  select(title, votes, rating)
```

Notice that to do this we filtered by votes before finding the top 10 in ratings.  

**Observation:** How is our result different if we swap the order of those steps (with the code below)? How do you explain this difference? First run the code below to see the dataframe. Then use the #note to answer the question.

***NOTE: If you are having a hard time with this part, stop here and review the DataCamp assignment in the Intro to Tidyverse sections on filter and select.***
https://campus.datacamp.com/courses/introduction-to-the-tidyverse/data-wrangling-1?ex=1
***Make sure you are logged into your SA account so you can see your completed work.***

```r
df4 <- movies %>%
  top_n(10, rating) %>%
  filter(votes >= 100) %>%
  select(title, votes, rating)
#Observation:
```



## Arranging

You might also have been frustrated that when we looked at the top 10 movies by rating that they weren't ordered by rating (what kind of a top 10 list is that!?).  We can fix this using arrange:

```r
df5 <- movies %>%
  filter(votes >= 100) %>%
  top_n(10, rating) %>%
  arrange(desc(rating)) %>%
  select(title, votes, rating)
```

**Task:** Write a chunk of code that will show the top 20 highest rated movies with at least 1000 votes. Make sure it runs properly and generates the correct dataframe. 

```r
df6 <- 

```





## Mutating

With *mutate* we can add new columns based on existing columns.  Suppose we're interest in how movies have changed over time and might be interested in looking at movies grouped by decade.  We can add a decade column.  To do this, I'll subtract 5 from the year and then round years to the 10's place.  I'll also use *factor* to tell R that the number that results from this calculation is a group or category.

Notice also that I'm starting this line of code with "movies = ...".  This is the way to overwrite the movies data with this new column added.

```r
movies = movies %>% 
mutate(decade = factor(round(year-5, -1)))
```

Now, let's go back to our box plots and look at movie ratings by decade.  I'll limit this to movies with at least 10,000 ratings:

```r
movies %>%
  filter(votes >= 10000) %>%
  ggplot(aes(decade, rating)) +
  geom_boxplot()

```

**Challenge:** In your Rscript file, create a scatterplot of any dataset we have looked at together. You can use the 'movies' data from this lab, the 'starwars' data we looked at a few weeks ago, or the Motor Trend magazie 'mtcars' data. In a few sentences, write a #note in your Rscript file about why you compared those two variables and what you think the graph is telling you about those data.
