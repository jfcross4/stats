Movies, Cars and Best Fit Lines
-----------------------------

As per usual, let's start by loading tidyverse and getting the data:

```r
library(tidyverse)

movies <- read.csv('https://raw.githubusercontent.com/idc9/stor390/master/data/movies.csv')
```

and, now, let's take a look at the data:

```r
View(movies)
```

I'm interested in looking at the relationship between audience ratings of movies and critics ratings of the same movie.  Let's look at this graphically:

```r
movies %>% 
  ggplot(aes(critics_score, 
             audience_score)) + 
  geom_point()
```

**Question 1:** How would you describe this relationship?

One way to summarize or quantify this relationship (and far from the only way) is to add a best fit line.  Let's do that!

```r
movies %>% 
  ggplot(aes(critics_score, 
             audience_score)) + 
  geom_point() +
  geom_smooth(method="lm")
```

As you know from algebra, you can write equations for lines with the form y = mx + b.  We can get the equation for this line using the code:

```r
lm(audience_score ~ critics_score,
   data=movies)
```

R is telling us that the equation for this line is:

$$audience\ score\ = 0.5015 \cdot critics\ score + 33.4355$$

We can view this as a prediction line -- it tells us how to predict the audience score from the critics score.  In fact, this line is line that makes predictions with the lowest root mean square error.

If we want to know what this line predict for a movie with a critics score of 60, we can plug that into the equation:

```r
0.5014*60 + 33.4355
```
This equation predicts that the audience score would be 63.5.  Put another way, this line passes through the point (60, 63.5).

**Question 2:** What audience score does this line predict for a movie with a critics_score of 80?

**Question 3:** What audience score does this line predict for a movie with a critics_score of 20?

# IMDB ratings

Now, let's try to use ratings from the International Movie Database (IMDB) website to predict critic's ratings.  

```r
movies %>% 
  ggplot(aes(imdb_rating, 
             critics_score)) + 
  geom_point() +
  geom_smooth(method="lm")
```

**Question 4:** By modifying code from above, try to find the equation for this line.  Please write that equation here.

**Question 5:** Based on your equation, what critics_score should be expect for a movie with an imdb_rating of 7?  (You may want to make sure that the result of your calculation matches what you'd expect based on the plot.)

**Question 6:** Based on your equation, what critics_score should be expect for a movie with an imdb_rating of 3.  Does this make sense?  If not, what do you think went wrong?

# Weights of Cars and Gas Mileage

Let's shift gears to an entirely different data set that tells us about 32 cars from 1974.  

```r
data(mtcars)
View(mtcars)
```

Let's look at how the weight of cars (given in 1000's of pounds) predicts how many miles they can drive per gallon of gas (mpg).

```r
ggplot(mtcars, aes(wt, mpg))+
  geom_point() +
  geom_smooth(method="lm")
```

**Question 7:** Find an equation for this line and predict the **mpg** of a car that weights 2500 pounds.

**Question 8:** Would it be reasonable to use this equation to predict the **mpg** of a car that weighs 8000 pounds?  Why or why not?

Instead of miles per gallon we could consider the inverse gallons per mile, which we can calculate as follows:

```r
mtcars = 
  mtcars %>% 
  mutate(gpm = 1/mpg)
```

Let's graph gallons per mile against weight.

```r
ggplot(mtcars, aes(wt, gpm))+
  geom_point() +
  geom_smooth(method="lm")
```
**Question 9: (tricky)**. Try finding the equation for this line.  Does this make similar predictions to our mpg v. wt line for a car that weighs 2500 pounds?  Does it make more or less reasonable predictions for a car that weighs 8000 pounds?  Which of these two lines do you think does a better job of capturing the true relationship between car weights and gas mileage?


