02 Introduction to Making Graphs in R
================
Statistics

Welcome back to RStudio!
Today, you will make your first graph in R. To do this, we will begin by creating a new R Script file. First, open RStudio on your computer using the magnifying glass. Then, navigate to "File>New File>R Script" in menu bar on the top left side.

**Note:** As you work through this lab, take the time to make sure that you understand what each line of code is doing. There is a help feature in RStudio that you can access by clicking Help in the bottom right pane and typing in the topic. You can also type the name of any function with a "?" in front in the console. 

**Sharing your Work** 
Please write your answers to the questions in this lab in a Google Doc that you share with me.  If you have time to save a graph (this is one of the steps towards the end of this lab) please drag it into the Google Doc.

Example: Type ?c() into the console and click return.


# Creating x and y coordinates to make a line graph

Last time you created some vectors, assigned them to variables, and then did operations on them. This time we are going to create two vectors for the purpose of plotting them on a line graph.

Once we have these vectors saved as objects, we will use them as our x-coordinates and y-coordinates of the graph.

Begin by creating a vector with numbers 1 through 10 for the x-axis of your graph. Save the vector in an object called X. You can use any of the methods you learned about in Lab 1 to create this vector:

```r
X <- 
```

Now we need some values for the y-axis! Create a list of predicted daily high temperatures for the next 10 days, including today. You can get this data for most cities in the world using your phone's weather app or <a href="https://weather.com/" target="_blank">weather.com</a>
website. Once you have the data, put your phone away and continue with the activity.

This time record the values using the 'c' function and store it in Y

```r
Y <- c()
```

# Making the graph using the plot function

Use the Help feature to look up what the plot function does. Type "?plot()" in the console and read the contents of the "Generic X-Y Plotting" help screen. Make sure you have 10 to 15 minutes of class time remaining before starting the next part. It can take some time to understand what is going on with the different arguments if this is your first time with R.

``` r
plot(X, Y, 
      type="p",
      xlab = "Days", 
      ylab = "Daily High Temperature (F)", 
      main = "_______________ 10 Day Forecast")
```

**Question 1:** Try replacing the **p** in *type="p"* in the code above with **l**, **b**, **o** or **n**.  What kinds of graphs do each letter produce?


# Making the graph look nice

R is a data visualization tool. You can make additional changes to how the graph looks by including more arguments in the plot() function. Here is how that could look

``` r
plot(X, Y, 
     xlab = "Days", 
     ylab = "Daily High Temperature (F)", 
     main = "Antarctica 10 Day Forecast", 
     type = "o", pch = 18, lwd = 3, 
     col = "red", cex = 3)
```

**Question 2:** What does changing the values in the arguments 'pch', 'lwd', 'col', and 'cex' do to the graph?

We can also get rid of the numbers on the x-axis completely because the default is showing only the even numbers. The 'xaxt=n' will do this for us. To replace it with all numbers 1 through 10, we use the 'axis' function. Here is how that code will look:

``` r
plot(X, Y, 
     xlab = "Days", 
     ylab = "Daily High Temperature (F)", 
     main = "NYC 10 Day Forecast", 
     type = "o", pch = 18, lwd = 3, 
     col = "red", cex = 3, xaxt="n")
          
axis(1, at = seq(1, 10, by = 1))
```

With so many arguments, it is important to make your code legible to others and for yourself when you return to the file at a later time. One method I will expect you to use is to click the return key every time you use a comma to separate arguments. Try it out with this line of code:

``` r
plot(X, Y, xlab = "Days", ylab = "Daily High Temperature (F)", main = "NYC 10 Day Forecast", type = "o", pch = 18, lwd = 3, col = "red", cex = 3, xaxt="n")
        
axis(1, at = 1:10, labels = c('Today','Day 2','Day 3','Day 4','Day 5','Day 6','Day 7','Day 8','Day 9','Day 10'), las=2)
```

# Challenge
Plot the weather forecast in Antarctica for the next 10 days using what you've learned above. So that you don't replace your work from above, give the X and Y vectors names other than X and Y. Before producing the plot you like best enter the following into the console.

```r
png(file="temp_forecast.png")
```

Then produce your plot and finally type:

```r
dev.off()
```
and your plot will be saved as a .png file.

**Super Challenge Question:** 

Use the following code to read in monthly atmospheric carbon dioxide levels data from 1958-2007 and assign dates to X and carbon dioxide levels to Y.

```r
co2 <- read.csv("https://raw.githubusercontent.com/professorkalim/stats22-23/main/monthly_CO2.csv")

X <- co2$Date
Y <- co2$CO2
```

Now, use what you've learned about plotting to plot this data in a compelling graph.
