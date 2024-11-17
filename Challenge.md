01 Introduction to R
================
Student Solutions

# Challenge Problems
**Challenge 1:**
If you place one grain of rice on the first space on a chessboard and
two on the second space and four on the third and so on (doubling every
time), how many grains of rice will you need to fill the chessboard
(which has 64 spaces)? Try writing one line of R code that will solve
this problem.

Here is one way to create the vector:
``` r
sum(((0:63)-(-2:61))^(0:63))
```
The most popular was this one line of code:
``` r
sum(2^(0:63))
```
or this one with two lines:
```r
A1 <- 2^(0:63)
sum(A1)
```

**Challenge 2:** 
Going back to the rice and chessboard scenario, if you have one million
grains of rice how many spaces can you fill before running out? Try to
write one line of code to solve this problem.

I did not record the responses! If you would like to share your response, let me know and I will include it here.
