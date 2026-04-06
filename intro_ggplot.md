Wrangling Data and ggplot
------------------------------------------------

Today we're going to practice wrangling and plotting data; perhaps learn something about movies too. 

We'll need three R packages for this lab.  Start by loading them:
```r
library(tidyverse)
library(ggplot2)
library(ggplot2movies)
```

Now, let's take a peak at the first few movies:
```r
glimpse(movies)
```


# Plotting the Data

## Histograms

Histograms are a way of showing the distribution of the outcomes from a random experiment (like rolling a die). It's also useful in representing numerical data like movie ratings. Run the code below and investigate the graphs that are generated. If you get an error, read the error message carefully and try to troubleshoot before asking for help. *(Example: Could not find function "ggplot" means that you forgot to run the line of code above to load the package)*

**Task:** Run the code for each histogram, analyze the graph and then do your best to write a description of what you think the graph is saying about the data. The description of Histogram 1 is already done for you.

```r
#Histogram 1
#Description: a histogram of the number of people who rated a movie.  Most movies have very few ratings but a small number of movies have many ratings!
movies %>% 
  ggplot(aes(imdb_num_votes)) + 
  geom_histogram()

#Histogram 2
#Description:
movies %>% 
  ggplot(aes(imdb_rating)) + 
  geom_histogram()

#Histogram 3
#Description:
movies %>% 
  ggplot(aes(thtr_rel_year)) +
  geom_histogram()

```

# Boxplots and Scatterplots

Boxplots and Scatterplots are two typical types of graphs that you will come across in the news, on social media, and in a Statistics classroom. They show the possible relationship between different variables.

**Observations:** Run the chunks of code below to generate these types of graphs. Look at the labels on the x- and y- axes of the graphs and determine what story the graphs might be telling about the data. 

## Boxplots 

We can plot a few distributions side by side with a box plot. Here are distribution of movie ratings for movies with different mpaa ratings.  
**Note:** The box covers the middle 50% of the data; extending from the 25th percentile rating up to the 75th percentile rating. The number in the middle where the line cuts the box is called the median.

```r
#MPAA Rating Boxplot
movies %>% 
  filter(mpaa_rating != "") %>%
  ggplot(aes(mpaa_rating, imdb_rating)) + 
  geom_boxplot()
```

## Scatterplots

Scatterplots help us determine if there is a strong correlation between two variables in a dataset.

This plot of the relationship between movie length and movie rating is unfortunately ruined by a few ridiculously long movies.

```r
#Scatterplot 1
movies %>%
  ggplot(aes(runtime, imdb_rating)) +
  geom_point()
```

Here is the same plot but you will limit it to movies with at least 10,000 votes. Edit the code to reflect this filter. Compe up with a short title to clarify what we're plotting; it should be something about movie ratings and lenght of movies. Edit ggtittle() to write your title.

```r
#Scatterplot 2
movies %>%
  filter(imdb_num_votes >= ________) %>%
  ggplot(aes(runtime, imdb_rating)) +
  geom_point() +
  ggtitle("____________")

```

Let's look at the relationship between critics score and audience score. I titled this one "Audience Score v. Critics Score, min 10,000 votes" but you can change it to be consistent with your naming convention above.

```r
#Scatterplot 3
movies %>%
  filter(imdb_num_votes >= 10000) %>%
  ggplot(aes(critics_score, audience_score)) +
  geom_point() +
  ggtitle("Audience Score v. Critics Score, min 10,000 votes")
 
```

and between number of votes and rating:

```r
#Scatterplot 4
movies %>%
  ggplot(aes(imdb_num_votes, imdb_rating)) +
  geom_point() +
  ggtitle("Movie Average Rating v. Number of Ratings")

```

Make you own scatter plot using a new combination of two variables.


# Data Wrangling
Let's practice with some of the data wrangling "verbs" we learned about on DataCamp and a new one called *top_n*. We've already used *filter()*. Before you run the code, type ?top_n in the console to read about this verb. 

After you run the code, you can click on the object name in the top right section of RStudio will open the dataframe.

## top_n and select


We can find the 10 highest rated movies and show the average rating in addition to whether the movies was nominated for best picture:

```r
movies %>%
  top_n(10, imdb_rating) %>%
  select(title, best_pic_nom, imdb_rating)
```

That list includes some obscure movies, let's use filter to limit our list to movies with at least 100 votes.  

```r
movies %>%
  filter(imdb_num_votes >= 100) %>%
  top_n(10, imdb_rating) %>%
  select(title, imdb_num_votes, imdb_rating)
```

Notice that to do this we filtered by votes before finding the top 10 in ratings.  

**Observation:** How is our result different if we swap the order of those steps (with the code below)? How do you explain this difference? First run the code below to see the dataframe. Then use the #note to answer the question.

```r
movies %>%
  top_n(10, imdb_rating) %>%
  filter(imdb_num_votes >= 100) %>%
  select(title, imdb_num_votes, imdb_rating)
```



## Arranging

You might also have been frustrated that when we looked at the top 10 movies by rating that they weren't ordered by rating (what kind of a top 10 list is that!?).  We can fix this using arrange:

```r
movies %>%
  filter(imdb_num_votes >= 100) %>%
  top_n(10, imdb_rating) %>%
  arrange(desc(imdb_rating)) %>%
  select(title, imdb_num_votes, imdb_rating)
```

**Task:** Write a chunk of code that will show the top 20 highest rated movies with at least 1000 votes. Make sure it runs properly and generates the correct dataframe. 







## Mutating

With *mutate* we can add new columns based on existing columns.  Suppose we're interest in how movies have changed over time and might be interested in looking at movies grouped by decade.  We can add a decade column.  To do this, I'll subtract 5 from the year and then round years to the 10's place.  I'll also use *factor* to tell R that the number that results from this calculation is a group or category.

Notice also that I'm starting this line of code with "movies = ...".  This is the way to overwrite the movies data with this new column added.

```r
movies = movies %>% 
mutate(decade = factor(round(thtr_rel_year-5, -1)))
```

Now, let's go back to our box plots and look at movie ratings by decade.  I'll limit this to movies with at least 10,000 ratings:

```r
movies %>%
  filter(imdb_num_votes >= 10000) %>%
  ggplot(aes(decade, imdb_rating)) +
  geom_boxplot()

```

**Challenge:** Upload a dataset (any dataset) into R and create a plot using that data.