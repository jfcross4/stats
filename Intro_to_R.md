01 Introduction to R
================
Statistics

Welcome to RStudio Day 1! 

You will see several panes on your screen. For now, we will focus our attention on the console window on the left pane. Down the line you will likely find yourself spending most of your time writing scripts, which will appear in a new pane in the upper left side.

**Note: As you work through this lab, take the time to make sure that you understand what each line of code is doing by typing out your own lines of code with different numbers.**


# Computation

R can be used for computation, data manipulation, visualizations and
simulations (among other things) but, mostly simply, R can be used like a calculator.

The following are some common arithmetic operators (symbols) you can use in R:
* add using +
* subtract using -
* divide using /
* multiply using *
* exponent using ^
* and many more!

Type the following lines into the console below, and hit "return" after each expression like you would on a calculator. 

``` r
3*2
4^2
5/2
5 %/% 2
5 %% 2
5*(3+7)^2
sqrt(25)
25^(1/2)
```

Once you've calculated all of the above, choose some of your own numbers and calculate those. Have fun with it! Try zero to the zero power, a number divided by zero, or one of the functions you learned in trigonometry.

**Question 1: How are /, %/% and %% different from each other?  Try some computations to find out!**

## Assignment

The assignment operator "<-" is the same as "=".  Your first assignment is to use the assignment operator.

```r
x <- 3
print(x)

y = 12
print(y)

```

# Vectors

R is an object oriented code, which means you "create" objects that hold different bits of information. One type of object is a vector, which is simply a string of values. You will learn several different ways to make vectors and save them as an object. Once you have an object, you can then do calculations and operations with that object. The best way to understand what this means is to try it out!

Let’s try creating vectors, assigning them to objects and then performing calculations on them.



## Creating a Vector


```r
x = 1:10
print(x)

y = seq(from=10,to=100,by=5)
print(y)

z = c(2,3,5)
print(z)
```

You can use functions to investigate the vectors and find different information about them.

```r
length(y)

summary(y) 

mean(y)
```

**Question 2: How many values are in vector y?  What is the mean of vector y?**

Before you move on to the next section, try making some vectors of your own. Save them into objects using letters of the alphabet (other than x,y,z). Investigate them and let me know if you have questions.

## Operations on Vectors

R can also perform operations on vectors.  Try the following:

```r
x^2

1/y

10*z

2^x
```

**Question 3: Use R to calculate: a) The first 10 multiples of 9.  b) The first 20 perfect squares.**


# Challenge Problems

R has many built in functions and many more that can be added as parts
of packages. To find out how a function works you can type “?” and then
the name of the function. This will pull up instructions within the help
tab in your lower right window. The most useful parts of these
instructions are often the examples given at the bottom. Try typing in the following lines one at a time

``` r
A1 <- 2:10
A1

sum(A1)

?sum
```

Look at the bottom right pane. A help window should have appeared telling you what the sum function does. Read through it and let me know if you have any questions.

# Functions

For the two challenge problems, you will need to figure out which function will help you calculate the answer. Do a little reading in the help section about the following functions by using the ? 

```r
?min
?max
?length
?mean
?cumsum
?cumprod
```

**Challenge 1:**

If you place one grain of rice on the first space on a chessboard and
two on the second space and four on the third and so on (doubling every
time), how many grains of rice will you need to fill the chessboard
(which has 64 spaces)? Try writing one line of R code that will solve
this problem.

# Logical Statements

You can also enter logical statements into R – that is, statements that
will return true or false. You can then do mathematics on these values
with true taking the value 1 and false the value 0. Try entering each of
the following statements in turn and make sure that you understand why
you are getting each output.

```r
x <- c(20,25,23,27, 28)
x > 23
x >= 23
x == 20
7*(x >= 23)+3
sum(x>=23)
x %% 5
(x %% 5) == 0
sum((x %% 5) == 0)
```

You can make your logical statements more complicated by introducing
ands and ors (& and \| in R).

```r
x <- c(20,25,23,27, 28)
(x == 20) & (x == 28)
(x == 20) | (x == 28)

(x %% 5 ==0 ) | (x %% 7 ==0)

sum((x %% 5 ==0) | (x %% 7 ==0))
```

**Challenge 2:** Going back to the rice and chessboard scenario, if you have one million
grains of rice how many spaces can you fill before running out? Try to
write one line of code to solve this problem.


