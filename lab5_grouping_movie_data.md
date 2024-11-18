Lab 5: Grouping and Summarizing Movie Data
--------------------------------------------

Today we're going to delve deeper into movie data using R and our new understanding of `group_by()` and `summarize()` from our recent DataCamp session.

Let's start by loading the packages and data we need.

```r
library(tidyverse)
library(ggplot2movies)
```

First, you may want to take a look at the movie data to job your memory:

```r
View(movies)
```

Next, let's add a decade column the same way we did in our prior movie lab.

```r
movies = movies %>% 
mutate(decade = round(year-5, -1))
```

# Movie Lengths

Now, let's look at the length of movies in every decade.  We'll group by decade and then find the number of movies in each decade, the mean movie length, the standard deviation in movie lengths and the 10th and 90th percentile movie lengths.

```r
movies %>% 
  group_by(decade) %>% 
  summarize(num_movies=n(), 
            MeanMovieLength = mean(length),       
            SDMovieLength = sd(length),
            percentile10 = quantile(length, 0.1),
            percentile90 = quantile(length, 0.9)
            )
```

We could also take this data and graph it:

```r
movies %>% 
  group_by(decade) %>% 
  summarize(num_movies=n(), 
            MeanMovieLength = mean(length),       
            SDMovieLength = sd(length),
            percentile10 = quantile(length, 0.1),
            percentile90 = quantile(length, 0.9)
            ) %>%
  ggplot(aes(x = decade, y=MeanMovieLength, size=num_movies)) +
  geom_point()
```

We could also make a version that has lines showing the 10th and 90th percentile movie lengths over time.

```r
movies %>% 
  group_by(decade) %>% 
  summarize(num_movies=n(), 
            MeanMovieLength = mean(length),       
            SDMovieLength = sd(length),
            percentile10 = quantile(length, 0.1),
            percentile90 = quantile(length, 0.9)
            ) %>%
  ggplot(aes(x = decade, y=MeanMovieLength)) +
  geom_point() +
  geom_line(aes(decade, percentile90))+
  geom_line(aes(decade, percentile10))

```

**Question 1:** How have movie lengths have changed over time?



# Movies Genres

Take a look at the columns labelled "Action", "Animation", "Comedy" and so one.  These columns have values of 1 (indicating that a movie belongs to a certain genre) or 0 (indicating that it does not).  A movie can belong to multiple genres.  If we average the values of these columns, we'll find the propotion of movies that below to a certain genre.  I'm going to use the "summarize_at" verb will allows me to use the same function (in this case "mean") on multiple columns at once:

```r
movies %>% 
    summarize_at(vars(Action:Short), mean) 
```

This shows that 37.1% of these movies are Dramas and 8.1% are Romance (among other things).

Now, let's look at this grouped by decade:

```r
movies %>% 
    group_by(decade) %>% 
    summarize_at(vars(Action:Short), mean) 
```

It might help to plot this data (don't worry if this code doesn't make sense):

```r
movies %>% 
    group_by(decade) %>% 
    summarize_at(vars(Action:Short), mean) %>%
    pivot_longer(cols=Action:Short, names_to="genre", values_to="prop") %>%
    ggplot(aes(x=decade, fill=genre, y=prop))+
    geom_bar(position="stack", stat="identity")
```

**Question 2:** How have the genres of movies changed over time?

We could also limit ourselves to highly rated movies:

```r
movies %>% 
    filter(votes>=10, rating>=8) %>%
    group_by(decade) %>% 
    summarize_at(vars(Action:Short), mean) 
```

**Question 3:** How do the genres of highly rated movies compare to the genres of all movies?

# Ratings over Time

In the same way we looked at movie lengths over time we can look at movie ratings over time.  Here, I'll limit this to movies with at least 5 ratings.

```r
movies %>% 
    filter(votes>=5) %>%
    group_by(decade) %>% 
    summarize(num_movies=n(), 
              MeanMovieRating = mean(rating),       
              SDMovieRating = sd(rating),
              MaxMovieRating = max(rating), 
              percentile10 = quantile(rating, 0.1),
              percentile90 = quantile(rating, 0.9))
```

We can graph this data, just as we did with movie lengths:

```r
movies %>% 
    filter(votes>=5) %>%
    group_by(decade) %>% 
    summarize(num_movies=n(), 
              MeanMovieRating = mean(rating),       
              SDMovieRating = sd(rating),
              MaxMovieRating = max(rating), 
              percentile10 = quantile(rating, 0.1),
              percentile90 = quantile(rating, 0.9)) %>%
  ggplot(aes(x = decade, y=MeanMovieRating)) +
  geom_point() +
  geom_line(aes(decade, percentile90))+
  geom_line(aes(decade, percentile10))
```
**Question 4:** How have movie ratings have changed over time?

# Now, you're on your own!

**Question 5:** Ask you own question about movies and try to answer it with this data.  Please, fiddle!  Don't by shy about trying things without knowing whether they will work.
