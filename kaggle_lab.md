Lab 6: Plotting, Grouping and Summarizing Kaggle Data
--------------------------------------------

* Step 1: Go to [Kaggle Datasets](https://www.kaggle.com/datasets?fileType=csv)

* Step 2: Search for a dataset that interests you.

* Step 3: Download the data
This isn't quite as easy as it seems you'll have to sign into with your Saint Ann's (gmail) email to make an account in order to do this.

* Step 4: Open the local version of Rstudio on your computer.

* Step 5: Create a new Rscript and write code to read in the data your downloaded:

```r
## You can use file.choose() to location the data file you downloaded on 
## your computer and save it's location:
location = file.choose()

location #shows the location

data = read.csv(location) # reads in the data
```

* Step 6: Look at your data!

```r
View(data)
```

* Step 6: Try plotting your data!

Take a look at the code from your [last lab](https://github.com/professorkalim/stats22-23/blob/main/census_plotting.md).

You can start by making a univariate plot, either a bar plot for a categorical variables or a histogram for a numeric/continuous variable.

Next, try making a bivariate plot.

* Step 7: Challenge: Grouping and summarizing!

Time permitting, try modifying some of the code from last week's lab to filter, group and summarize your data in some interesting way!



