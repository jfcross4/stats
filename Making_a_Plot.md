02 Introduction to Making Graphs in R
================
Statistics

Welcome back to RStudio!
Today, you will make your first graph in R. First, open RStudio on your computer using the magnifying glass. You will write your code in the Console window as before. In future labs you will learn to save your code into an R Script file. For today, just make sure that you click "Save" when closing RStudio so that you can review your work with the up/down arrow keys next time.

**Note:** As you work through this lab, take the time to make sure that you understand what each line of code is doing. There is a help feature in RStudio that you can access by clicking Help in the bottom right pane and typing in the topic. You can also type the name of any function with a "?" in front in the console. 


Example: Type ?c() into the console and click return.

**Sharing your Work** 
As before, write the answers to the lab questions on the worksheet I'll provide in class.  Save the graphs you generate in the folder that you created on your desktop. There will be instructions later in the lab on how to do this. In the last five minutes of class you will share that folder with your teacher through Google Drive.

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

# Making the graph using the default plot function

Use the Help feature to look up what the plot function does. Type "?plot()" in the console and **read** the contents of the "Generic X-Y Plotting" help screen.

**Question 1:** What do the first two arguments in the default plot function do? When you get to the examples at the bottom of the page, identify which one is going to produce a graph of the sine function. Write the code into your console and try to generate the plot. How are the arguments in the example different from the default plot function we will be using today?

Make sure you have 10 to 15 minutes of class time remaining before starting the next part! It can take some time to understand what is going on with the different arguments if this is your first time with R.

Now that you have the vectors X and Y, you can use them as arguments in the plot function to make your first graph! Edit the blank space in the "main" argument to read the name of the city you collected temperature data on. 

_Can you tell what the "xlab" and "ylab" arguments are used for?_

``` r
plot(X, Y, 
      type="p",
      xlab = "Days", 
      ylab = "Daily High Temperature (F)", 
      main = "_______________ 10 Day Forecast")
```
_Note: To go down to the next line without running your code, hold Shift and hit return._

**Question 2:** Try replacing the **p** in *type="p"* in the code above with **l**, **b**, **o** or **n**.  What kinds of graphs do each letter produce?


# Making the graph look nice

R is a data visualization tool. You can make additional changes to how the graph looks by including more arguments in the plot() function. Here is how that could look

``` r
plot(X, Y, 
     xlab = "Days", 
     ylab = "Daily High Temperature (F)", 
     main = "_______________ 10 Day Forecast", 
     type = "o", pch = 18, lwd = 3, 
     col = "red", cex = 3)
```

**Question 3:** What does changing the values in the arguments 'pch', 'lwd', 'col', and 'cex' do to the graph?

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

# Saving the graph as a png file

Remember when we created the folder on your computer desktop? It is time to start using it! 

1. Click the "Export" dropdown button and then "Save as Image..." to open the save screen. 
2. Choose the folder you created as the directory and save the image there. 
3. Take a look in the folder to make sure it worked before moving on.

# Legible code practice

With so many arguments, it is important to make your code legible to others and for yourself when you return to the file at a later time. One method I will expect you to use is to click the Shift+return key every time you use a comma to separate arguments. Try copying this line of code and trying it out.

``` r
plot(cos, -pi, 2*pi, xlab = "Inputs", ylab = "Outputs", main = "Legible Code Practice - Cos(x)", type = "o", pch = 1, lwd = 1, col = "purple", cex = 1, xaxt="n")
        
axis(1, at = -3:6, labels = c('Appx -Pi','-2','-1','0','1','2','3','4','5','Appx 2pi'), las=2)
```

Before you save the graph, change the number for the argument **pch** to change the type of plotting character. Try different values until you find one you like.

Save the graph in your folder using the same instructions as before.

# Challenge: Saving without clicks
**Challenge 1:** Plot the weather forecast in Antarctica for the next 10 days using what you've learned above. So that you don't replace your work from above, give the X and Y vectors names other than X and Y.

You can set your default directory in Preferences. Once you've done that, you can type the lines below before and after your code to save the file and name it what you like.

When you are satisfied with the plot from Challenge 1, enter the following into the console.

```r
png(file="temp_forecast.png")
```

Then reproduce your plot and finally type:

```r
dev.off()
```
and your plot will be saved as a .png file.

**Challenge 2:** Plotting Climate Data

Use the following code to read in monthly atmospheric carbon dioxide levels data from 1958-2007 and assign dates to D and carbon dioxide levels to C.

```r
co2 <- read.csv("https://raw.githubusercontent.com/professorkalim/stats22-23/main/monthly_CO2.csv")

D <- co2$Date
C <- co2$CO2
```

Now, use what you've learned about plotting to plot this data in a compelling graph. Save the graph in your lab folder and name it "co2_levels.png"

You can visit this site https://gml.noaa.gov/ccgg/trends/ to see more about where the data came from. 
