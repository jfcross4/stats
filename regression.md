Regression Towards the Mean Lab
--------------------------------------

In this lab, we'll learn about regressions towards the mean and see it in practice by looking at the results of NFL games.  Let's start by loading everyone's favorite package and reading in NFL game data.

Please jot down your answers to these questions and we'll compare our answers in class tomorrow.

```r
library(tidyverse)

games = read.csv("https://raw.githubusercontent.com/jfcross4/stats/refs/heads/master/nfl_game_results.csv")
```

# The Data

Let's take a look at the raw data.  This data includes the result for every team in every game in each season from 1999 through 2024.

```r
View(games)
```
The two columns that most likely need explanation are "half" and "even_wk".  "Half" denotes whether the game took place in the first or second half and "even_wk" whether it was an even game (2nd, 4th, 6th, 8th...) or an odd game (1st, 3rd, 5th...) in the season.

# Grouping the Data by "half" and "even_ek"

We can group by these variables and get results by half...

```r
results_by_half = 
games %>%
  group_by(season, team, half) %>%
  summarize_at(c("win", "loss", "tie", "result"), sum) %>%
  mutate(games = win+loss+tie, 
         win_pct = (win + 0.5*tie)/games,
         pt_dff = result/games)

View(results_by_half)
```

... or results split by even/odd:

```r
results_by_even = 
  games %>%
  group_by(season, team, even_wk) %>%
  summarize_at(c("win", "loss", "tie"), sum) %>%
  mutate(games = win+loss+tie, 
         win_pct = (win + 0.5*tie)/games)

View(results_by_even)
```

In the data frame, "results_by_half" that you created above, each team has two rows for each team-season, one for each half of the season.  For each half of the season, it has a number of wins, losses and ties and their winning percentage.

Since we want to compare first half results to second half results, we want a table that has one row for each team-season with two columns for winning percentage, one showing their winning percentage in the first half and the other showing their winning percentage in the second half.  The table we want, with two winning percentages, is "wider" but, with only half as many rows, not as long.  We can create it using "pivot_wider".

```r
results_by_half_wide = 
results_by_half %>%
pivot_wider(id_cols = c(season, team),
            names_from=half,
            values_from = win_pct) %>%
  ungroup()
  
View(results_by_half_wide)
```
The table we just created has winning percentages in the first and second half for every team-season.  We can create the same table with results split into even and odd numered games:

```r
results_by_even_wide = 
  results_by_even %>%
  pivot_wider(id_cols = c(season, team),
              names_from=even_wk,
              values_from = win_pct) %>%
  ungroup()
  
View(results_by_even_wide)
```

# Correlations and Scatterplots



Let's make a scatter plot of second half winning percentage versus first half winning percentage.

```r
results_by_half_wide %>%
  ggplot(aes(first, second)) + 
  geom_point()
```

This plot has the disadvantage that points may be on top of each other.  We can avoid this by "jittering" the points (scattering them slightly) around their actual locations.  We'll also add a best-fit line.

```r
results_by_half_wide %>%
  ggplot(aes(first, second)) + 
  geom_jitter() + 
  geom_smooth(method="lm")
```

This best-fit line doubles as a prediction line.  For every possible first half winning percentage the line shows the team's predicted winning percentage in the second half.

**Question 1:**
For a team with a 75% winning percentage in the first half, what (roughly) is their predicted winning percentage in the second half?

**Question 2:**
For a team with a 25% winning percentage in the first half, what (roughly) is their predicted winning percentage in the second half?

**Question 3:**
For a team with a 50% winning percentage in the first half, what (roughly) is their predicted winning percentage in the second half?

When can also find the correlation between first and second half winning percentages:

```r
results_by_half_wide %>%
  summarize(cor(first, second))
```

Now, let's try the same thing with even and odd games:

```r
results_by_even_wide %>%
  ggplot(aes(even, odd)) + 
  geom_jitter() + 
  geom_smooth(method="lm")
```
**Question 4:**
Write code to find the correlation between winning percentage in even games and winning percentage in odd games.

# Regression Towards the Mean

Now, let's look only at the team's that were the worst performers in the first half.  We'll limit the analysis to the teams that had a winning percentage less than 1/3 in the first half, and look at their collective winning percentage in the first and second halves:

```r
results_by_half_wide %>%
  filter(first < 1/3) %>%
  summarize_at(c("first", "second"), mean)
```

We can also look at the teams who had the most success in the first half:

```r
results_by_half_wide %>%
  filter(first > 2/3) %>%
  summarize_at(c("first", "second"), mean)
```

**Question 5:**
How would you summarize these results?

We can also look at the teams that were most successful in even games and see how well they performed in both odd and even games. 

```r
results_by_even_wide %>%
  filter(even > 2/3) %>%
  summarize_at(c("even", "odd"), mean)
```
**Question 6:**
How would you explain this?  Are good teams getting worse?


# Equations for Best-Fit Lines

Take a look at the best-fit line for first/second half games again:

```r
results_by_half_wide %>%
  ggplot(aes(first, second)) + 
  geom_jitter() + 
  geom_smooth(method="lm")
```

**Question 7:**
Try to write an equation in the form of $y = mx + b$ for this line.  Remember, if you know two points that lie on the line, you can find the equation for the line.

# Checking your answer

You can check your equation by running the following code which will give you the intercept and the slope of the best fit line:

```r
lm(second ~ first, data=results_by_half_wide)
```

Have your equation (roughly) correct?

**Question 8:**

Modify the code above to find the equation for the best fit line of the odd v. even weeks graph.