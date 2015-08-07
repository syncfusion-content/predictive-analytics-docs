---
layout: post
title: Working-with-the-Data-File
description: working with the data file 
platform: ug
control: Essential Predictive Analytics
documentation: predective-analysis
---

# Working with the Data File 

####The univariate graphs and statistical procedures, as well as the methods for modifying data, suggest important ways to focus your analysis. Three of the most important methods of focusing include selecting subgroups for individual analysis, comparing subgroups, and integrating additional data, either new cases or new variables, into your dataset. 

## Selecting cases 

####When you are working with your data, you may want to focus on certain subgroups of cases or variables to get better insight. R makes it simple to select cases and variables. The general syntax is this: data set[rows, columns] or data set[cases, variables]. To select all of the rows or columns, just leave the attribute empty. To select an adjacent set of cases or variables, give the index number of the first and last items with a colon between them. For example, this command would select rows 10-20 and columns 2-5: data set[10:20, 2:5]. To select nonadjacent cases or variables, use the concatenate function, c(). For example, to select all of the cases but just variables 1-4, 6, and 8-12, use this command: data set[, c(1:4, 6, 8:12)]. 



####In this example is used the mtcars data from R’s datasets package. This dataset contains road test data from a 1974 issue of Motor Trend__magazine. In the code that follows, first load the package and the data, and then display the first three cases.

### Sample: sample_5_1.R 
{% highlight r %}
# LOAD DATA require(“datasets”)  # Load datasets package.  

data(mtcars)  # 1974 road test data from Motor Trend. 

mtcars[1:3, ]  # Show all variables for the first three cars.                mpg cyl disp  hp drat    wt  qsec vs am gear carb Mazda RX4     21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 

Mazda RX4 Wag 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 

Datsun 710    22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
{% endhighlight %}

####Next, you get the mean horsepower for all of the cars in the dataset. The only thing to remember in this command is how to specify a single variable in a data set with the $ operator. 

####In this way, the horsepower variable is mtcars$hp. 
{% highlight r %}
# ALL CASES mean(mtcars$hp)  # Mean horsepower for all cars. [1] 146.6875
{% endhighlight %}

####Now you can get the mean horsepower for just the eight cylinder cars. Put the name of the selection variable in square brackets and use the double equal signs, ==, indicating logical equality. 
{% highlight r %}
# SELECT ON SINGLE VARIABLE 

# Mean horsepower (for 8-cylinder cars). mean(mtcars$hp[mtcars$cyl == 8])  # Select rows where cyl = 8 [1] 209.2143
{% endhighlight %}

####If you plan on doing several analyses with the same subgroup, it may be helpful to create a new data frame based on that selection. In that case, make the selection and assign it to a new variable. In the following code, a data frame called v8 is created for all the eight cylinder cars. The first part of the selection in square brackets selects the rows for cars with eight cylinder engines and the blank space after the comma selects all of the variables.
{% highlight r %}
# CREATE NEW DATA FRAME WITH SELECTION 

v8 <- mtcars[mtcars$cyl == 8, ]  # 8-cylinder cars, all variables
{% endhighlight %}

####You can now select on two other variables: cars with 5-speed transmission and cars that weigh less than 4000 pounds. The average horsepower for this group is 299.5 that is very high for 1974, so it is followed by a list of all the cars that met these criteria and selecting a few variables to display. 
{% highlight r %}
# SELECT ON TWO VARIABLES 

# Mean horsepower for cars with v8, 5-speed, and weigh < 4000 lbs. mean(v8$hp[v8$gear == 5 & v8$wt < 4])  # Show the mean horsepower. 

[1] 299.5 v8[v8$gear == 5 & v8$wt < 4, c(2, 10, 6, 4)]  # List the cars included. 

               cyl gear   wt  hp Ford Pantera L   8    5 3.17 264 

Maserati Bora    8    5 3.57 335


{% endhighlight %}

####Only two cars made this list: the De Tomaso Pantera, incorrectly listed here as a Ford, although it had a Ford engine, and the Maserati Bora. 

####Once you have saved your work, you can clear the workspace of unneeded variables, objects, or packages.

{% highlight r %}

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads data sets package. rm(list = ls())  # Remove all objects from workspace.
{% endhighlight %}

## Analyzing by subgroups 

####In this section you can learn about the methods to include all of the cases in the analyses but to organize the results by subgroups. 



####In this example, is used the Iris dataset that was collected by botanist Edgar Anderson but made famous by statistician Ronald Fisher. This dataset consists of four measurements for three species of Iris. 

### Sample: sample_5_2.R 
{% highlight r %}
# LOAD DATA require(“datasets”)  # Load the data sets package. 



# PREVIEW DATA iris[1:3, ]  # Show first three rows, all variables. 

  Sepal.Length Sepal.Width Petal.Length Petal.Width Species 

1	5.1         3.5          1.4         0.2  setosa 

2	4.9         3.0          1.4         0.2  setosa 

3	4.7         3.2          1.3         0.2  setosa

{% endhighlight %}

####For this example, you need to compare the petal widths of the three species. To do this, you can use the aggregate() function, which is used to compute summary statistics for subgroups. The function takes three arguments: 

1. The variable to be analyzed. In this case, iris$Petal.Width. 
2. The variable that specified group membership. In this case, iris$Species. 
3. The function or statistics to be used. In this case, FUN = mean. 

####The tilde operator, ~, is used to separate the left and right sides of a model formula. R organizes the output like this:
{% highlight r %}
# COMPARE GROUPS ON ONE VARIABLE 

aggregate(iris$Petal.Width ~ iris$Species, FUN = mean)   iris$Species iris$Petal.Width 1       setosa            0.246 

2	versicolor            1.326 

3	virginica            2.026

{% endhighlight %}

####virginica

####To compare the groups on more than one outcome variable, replace the single outcome variable with the column binding function cbind() and list the desired outcomes as arguments. cbind() makes it possible to combine several vectors or variables into a single, new data frame. In this case, R does not give the variable names but uses generic labels—V1, V2, etc.—so you must note the order in which you entered the variables.
{% highlight r %}
# COMPARE GROUPS ON TWO VARIABLES aggregate(cbind(iris$Petal.Width,                 iris$Petal.Length) 

          ~ iris$Species,           FUN = mean)   iris$Species    V1    V2 1       setosa 0.246 1.462 

2	versicolor 1.326 4.260 

3	virginica 2.026 5.552


{% endhighlight %}

####Once you have saved your work, you can clean the workspace by removing any variables or objects you created. 

{% highlight r %}

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads data sets package. rm(list = ls())  # Remove all objects from workspace.
{% endhighlight %}

## Merging files 

####Analyses are often much more powerful if data from different sources are combined. For example, joining data on Internet search trends with data on demographics can give important insights for marketing researchers. In this section, you can examine the longley data from R’s datasets package. This is a data frame with seven economic variables, observed yearly from 1947 to 1962. After loading the data and displaying a few cases, you can then split the data set into three parts and then join them again to demonstrate the process. 

### Sample: sample_5_3.R 
{% highlight r %}
# LOAD DATA require(“datasets”)  # Load the data sets package. 

# DISPLAY DATA longley[1:3, ]  # Display the first three rows, all variables. 

     GNP.deflator     GNP Unemployed Armed.Forces Population Year Employed 

1947	83.0 234.289      235.6        159.0    107.608 1947   60.323 

1948	88.5 259.426      232.5        145.6    108.632 1948   61.122 

1949	88.2 258.054      368.2        161.6    109.773 1949   60.171
{% endhighlight %}

####Now split the data into three data sets. First, create a dataset called a1 with the first six of seven variables for the first 14 of 16 cases. Second, create another dataset called a2 that has the last two of seven variables for the same 14 cases. This means that datasets a1 and a2 share one variable: year. This variable will serve as the index variable that makes it possible to line up cases when adding variables. The third dataset, called b, has all seven variables but adds two new cases. 



####Although it is not necessary to first save the datasets as new objects in the workspace, it is a convenient way of checking the process. After the datasets have been created, you can then use write.table() to save them as text files on the host computer. To do so, you must provide the name of the object to be written and its file path. The file path specification works slightly differently on Macintosh and Windows PCs. On Mac, write "~/Desktop/longley.a1.txt" to save the file to the desktop. On a Windows PC, write "c:/longley.a1.txt" to save the file to the C drive. It is also important to specify that values in the table are separated by tabs by adding sep = "\t". Once everything has been written correctly, then you can use rm(list=ls()) to clear the workspace and start the imports with a clean space. 


{% highlight r %}
# SPLIT & EXPORT DATA a1 <- longley[1:14, 1:6]  # First 14 cases, first 6 variables. a2 <- longley[1:14, 6:7]  # First 14 cases, last 2 variables. b <- longley[15:16, ]     # Last 2 cases, all variables. write.table(a1, "~/Desktop/longley.a1.txt", sep = "\t") write.table(a2, "~/Desktop/longley.a2.txt", sep = "\t") write.table(b, "~/Desktop/longley.b.txt", sep = "\t") 

# On PC, use "c:/longley.a1.txt" 

rm(list=ls()) # Clear out everything to start fresh

{% endhighlight %}

####Once all of the data files have been exported and the workspace has been cleaned, you can start over by importing them one at a time and putting them together. You can start by importing and combining the two data sets with the variables for the first 14 cases: a1, which has the first six variables, including year, and a2, which has the last two variables, also including year. To do this use read.table() to import the data sets. Then you can use merge() to match the cases in the data sets. merge() takes three arguments:  

1. The name of the first data set. 
2. The name of the second data set.  
3. The variable used to match cases, with the argument by.  



####Feed the merge into a new object a.1.2 and then display the first few cases to check the outcome.

{% highlight r %}

# IMPORT & COMBINE FIRST TWO DATA SETS # Add columns for same cases. 

a1t <- read.table("~/Desktop/longley.a1.txt", sep = "\t") a2t <- read.table("~/Desktop/longley.a2.txt", sep = "\t") # Take early years (a1t) and add columns (a2t). 

# Must specify the variable to match cases ("Year" in this case). 

a.1.2 <- merge(a1t, a2t, by = "Year")  # Merge two data frames 

a.1.2[1:3, ]  # Check results for the first three cases. 

  Year GNP.deflator     GNP Unemployed Armed.Forces Population Employed 

1	1947         83.0 234.289      235.6        159.0    107.608   60.323 

2	1948         88.5 259.426      232.5        145.6    108.632   61.122 

3	1949         88.2 258.054      368.2        161.6    109.773   60.171

{% endhighlight %}

####Notice addition of the index variable on left, numbered 1, 2, 3. In the original data set the index variable was the year of the observation, with year also appearing as the sixth variable. In the new data set, year was used to match observations and so it now appears as the first variable. 

####To add the new cases from the data set b, first import the dataset with read.table(). Then we use the row binding function rbind() to join the two datasets. The interesting thing is that this works even though the variables are currently in a different order, with year moving to the front on the first dataset.


{% highlight r %}
# IMPORT & COMBINE LAST DATA SET # Add two more cases at the bottom. b <- read.table("~/Desktop/longley.b.txt", sep = "\t") all.data <- rbind(a.1.2, b)  # "Row Bind" all.data[12:16, ]  # Check last four rows, all variables. 

     Year GNP.deflator     GNP Unemployed Armed.Forces Population Employed 

13	1959        112.6 482.704      381.3        255.2    123.366   68.655 

14	1960        114.2 502.601      393.1        251.4    125.368   69.564 

1961	1961        115.7 518.173      480.6        257.2    127.852   69.331 

1962	1962        116.9 554.894      400.7        282.7    130.081   70.551

{% endhighlight %}

####There is one problem with this process. Notice the mismatch of index variables on the left of the previous output. You can fix this by resetting the row names with row.names() <- NULL, making sure to insert the name of the data set, as shown in the following code: 

{% highlight r %}

# CLEAN DATA row.names(all.data) <- NULL  # Reset row names. all.data[13:16, ]  # Check last four rows, all variables. 

   Year GNP.deflator     GNP Unemployed Armed.Forces Population Employed 

13	1959        112.6 482.704      381.3        255.2    123.366   68.655 

14	1960        114.2 502.601      393.1        251.4    125.368   69.564 

15	1961        115.7 518.173      480.6        257.2    127.852   69.331 

16	1962        116.9 554.894      400.7        282.7    130.081   70.551

{% endhighlight %}

####At this point, the three datasets have been successfully joined and you can proceed with our analyses. And, as before, once you have saved our work, you can clean the workspace by removing any variables or objects you created. 

{% highlight r %}

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads data sets package. rm(list = ls())  # Remove all objects from workspace.

{% endhighlight %}

