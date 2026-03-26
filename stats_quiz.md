# Multiple Regression Quiz

## Getting the Data

Run the following code to acquire the datasets you need:

```r
memory = read.csv("https://raw.githubusercontent.com/jfcross4/stats/refs/heads/master/memory_quiz.csv")
covid = read.csv("https://raw.githubusercontent.com/jfcross4/stats/refs/heads/master/covid_data.csv")
air_bnb = read.csv("https://raw.githubusercontent.com/jfcross4/stats/refs/heads/master/chicago_air_bnb.csv")
```

---

# 1. Memory Tests

A stats class was asked to memorize lists of nouns.  In the **pre-test**, all students had 20 seconds to memorize nouns and then write down as many of these nouns as they could. Then students were split into two groups.  The "short" time group had another 30 seconds to memorize a second group of nouns while the "long" time group was given 60 seconds.  The number of nouns they recalled among this second batch is their "post-test score".

Run the following code to predict post-test scores from pre-test scores:

```r
m = lm(Post.test.Score ~ Pre.test.Score, data=memory)
summary(m)
```

a. Write an equation to predict post-test score from pre-test score.

b. What does the intercept of this equation mean?

c. Interpret the slope (the coefficient of pre-test score) in this equation.

Now, let's add time group to the model:

```r
m = lm(Post.test.Score ~ Pre.test.Score + time, data=memory)
summary(m)
```

d. According to this model, what is the estimated effect of having more time to memorize nouns?

e. Predict post-test score for someone with a pre-test score of 10 who was in the "long"/60 second group.

# 2. Covid

This (fake) covid dataset has the ages of people who had covid, their vaccination status (0 or 1) and a numerical assessment of their covid severity no doubt made by medical professionals.  We are going to make two models to predict covid severity, one that uses just vaccination status and one that uses vaccination status and age.  Please run the code to make and look at both models:

```r
m1 = lm(severity ~ vaccinated, data = covid)
m2 = lm(severity ~ vaccinated + age, data = covid)

summary(m1)
summary(m2)
```

The coefficient of "vaccinated" is quite different in these two models.  Why?  Please interpret these results.

# 3. Air BnB

The Air BnB data is from rentals in Chicago.  We'll build models to predict the price of air bnb rentals.

Let's start by building a model to predict the price using the number of reviews and the number of bedrooms:

```r
m = lm(price ~ reviews + bedrooms, data=air_bnb)
summary(m)
```

a. Which of these two variables appears to be a more important predictor of price?  How do you know?


**Bonus Questions** (you'll need to write your own code)

You may want to look at the data before attempting these:

```r
View(air_bnb)
```

b. Build a model to predict price from bedroom and rating.  What price do you predict for a place with 3 bedrooms and a 4.0 rating?

c. Build model to predict price using, first, only "bedrooms" and, second, "bedrooms" and "accommodates".  How does the coefficient of "bedrooms" differ between these models and why might it differ?
