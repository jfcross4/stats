Lab 6: Plotting, Grouping and Summarizing Kaggle Data
--------------------------------------------

This week's lab is the third lab in a sequence geared at helping you become an independent data visualizer.  In the first lab, I gave you code to graph census data.  In the second lab you need to modify that code to graph data of your choice.  In this third lab, I'm hoping that you can acquire your own data, load it into R and then visualize it.

There are a number of good places to find data.  Today we'll focus on Kaggle Datasets.

* Step 1: Go to [Kaggle Datasets](https://www.kaggle.com/datasets?fileType=csv)

* Step 2: Search for a data set that interests you.

It doesn't really matter which data set you pick but the type of data might well determine which types of graphs are appropriate.

* Step 3: Download the data
This isn't quite as easy as it seems you'll have to sign into with your Saint Ann's (gmail) email to make an account in order to do this.

* Step 4: Open posit.cloud.  You can start a new project or continue in an existing project.

* Step 5: Upload your data.

In the lower right hand corner of your posit.cloud screen (in the same panel you use to install package) you should see a small yellow circle with a upwards pointing white arrow inside.  Click on that circle and then select "Choose File" and find the .csv file that you downloaded from Kaggle and click "Okay".

Your data should now be uploaded!

* Step 6: Create a new Rscript and write code to read in the data your downloaded:

```r
## You can use file.choose() to location the data file you downloaded on 
## your computer and save it's location:
location = file.choose()

location #shows the location

data = read.csv(location) # reads in the data
# you can use a name that's more descriptive
# than "data"
```

An alternative way to read in the data is to read it in by name.  Replace "my_file_name" in the code below with the name of your data file.

```r
data = read.csv("my_file_name.csv")
```

Please let me know if you need help with this step.

* Step 7: Look at your data!

If you've read in your data correctly, you should now be able to use Rstudio to look at your data:

```r
View(data)
```

* Step 8: Try plotting your data!

Take a look at the code from your [census plotting lab](https://github.com/professorkalim/stats22-23/blob/cross/census_plotting.md).

You can start by making a univariate plot, either a bar plot for a categorical variables or a histogram for a numeric/continuous variable.

Next, try making a bivariate plot.

* Step 7: Challenge: Grouping and summarizing!

Time permitting, try modifying some of the code from our [movie labs earlier in the year](https://github.com/professorkalim/stats22-23/blob/main/lab5_grouping_movie_data.md) to filter, group and summarize your data in some interesting way!



