---
layout: post
title: Statistics-for-Associations
description: statistics for associations 
platform: ug
control: Essential Predictive Analytics
documentation: predective-analysis
---

## Statistics for Associations 

In the previous section, you have learnt to explore methods for visualizing the association between two variables. In this section, you can learn to provide the complement to those analyses by discussing statistical procedures for describing bivariate associations. This chapter includes some of the most familiar inferential statistics: correlation, regression, t-tests for two samples and paired observations, the one-factor analysis of variance, tests to compare multiple proportions, and the chi-squared test for independence. 

### Correlations 

Correlations are a mainstay of statistical analysis. R provides several approaches to correlations and the external packages provide an almost unlimited choice of procedures. In this section, you can start with cor(), the default correlation function in R. You can use the swiss data from R’s datasets package. This data contains fertility and socio-economic variables for 47 Frenchspeaking provinces of Switzerland in 1888. 



(See ?swiss for more information.) 

#### Sample: sample_7_1.R 

# LOAD DATA require(“datasets”)  # Load the datasets 

package. data(swiss)

Once the data are loaded into R, you can simply call the cor() function on the entire data frame. Also, to make the printout less busy, you can wrap the cor() function with the round() function to round the output to two decimal places. 

# CORRELATION MATRIX cor(swiss) 

round(cor(swiss), 2)  # Rounded to 2 decimals 

The first few rows and columns of that output are: 

Fertility Agriculture Examination Education Catholic 

   Fertility             1.00        0.35       -0.65     -0.66     0.46 

  Agriculture           0.35        1.00       -0.69     -0.64     0.40

Missing from this printout are probability values. R does not have a built-in function to get pvalues—or even asterisks indicating statistical significance—for a correlation matrix. Instead, you can test each correlation separately with cor.test(). This function gives the correlation value, a hypothesis test, and a confidence interval for the correlation. 

# INFERENTIAL TEST FOR SINGLE CORRELATION cor.test(swiss$Fertility, swiss$Education) 



 	Pearson's product-moment correlation data:  swiss$Fertility and swiss$Education t = -5.9536, df = 45, p-value = 3.659e-07 

alternative hypothesis: true correlation is not equal to 0 95 percent confidence interval: 

 -0.7987075 -0.4653206 sample estimates: 

       cor  -0.6637889 

This is useful information but it is cumbersome to test correlations one at a time. The external package Hmisc provides an alternative, making it possible to get two matrices at once: one matrix with correlation values, and a second matrix with two-tailed p-values. The first step is to install and load the Hmisc package. 

# INSTALL AND LOAD "HMISC" PACKAGE

 install.packages("Hmisc") require("Hmisc")

When you load Hmisc, you can receive message that R is also loading several other packages—grid, lattice, survival, splines, and Formula—that Hmisc relies on. You may also receive a message that a few functions, such as format.pval, round.POSIXt, trunc.POSIXt, and units, are masked from package:base. This happens because Hmisc is temporarily overriding those functions that is supposed to happen, so you can ignore those messages. 



Before you can run the rcorr() function from Hmisc, however, you need to make a change to your data. The swiss data is a data frame but Hmisc operates on matrices. All that you need to do is wrap swiss with the as.matrix() function that can then be nested within rcorr(). 



# COERCE DATA FRAME TO MATRIX 

# GET R & P MATRICES 

rcorr(as.matrix(swiss))

Here are the first few rows and columns of each resulting matrix. The first matrix consists of correlation coefficients and is identical to your rounded matrix from R’s cor() function. 

Fertility Agriculture Examination Education Catholic 

Fertility             1.00        0.35       -0.65     -0.66     0.46 Agriculture           0.35        1.00       -0.69     -0.64     0.40

The second matrix consists of two-tailed probability values, with blanks on the diagonal: 

P 

                 Fertility Agriculture Examination Education Catholic 

Fertility                  0.0149      0.0000      0.0000    0.0010   

Agriculture      0.0149                0.0000      0.0000    0.0052

Using the standard criterion of _p_ < .05, all of these correlations are statistically significant. 

For more information on Hmisc, enter help(package = "Hmisc"). And, as before, when it is completed, you should unload the external packages and clear out any variables or objects that you have created. 

# CLEAN UP detach("package:datasets", unload=TRUE)  # Unload the

 package. detach("package:Hmisc", unload=TRUE)  # Unload the package. 

rm(list = ls())  # Remove the objects.

### Bivariate regression 

Bivariate regression is a linear regression between two variables and a very common, flexible, and powerful procedure. R has excellent built-in functions for bivariate regression that can provide an enormous amount of information.  

For this example, you can use the trees data from R’s datasets package. (See ?trees for more information.) 

#### Sample: sample_7_2.R 

# LOAD DATA 

require(“datasets”)  # Load the datasets package. data(trees)  # Load data into the workspace. trees[1:5, ]  # Show the first 5 lines. 

  Girth Height Volume 

1	8.3     70   10.3 

2	8.6     65   10.3 

3	8.8     63   10.2 

4	10.5     72   16.4 

5	10.7     81   18.8

You can use bivariate regression to see how a tree’s girth can predict its height. But first, it is always a good idea to check the variables graphically to see how well they meet the assumptions of your intended procedure. 

# GRAPHICAL CHECK

 hist(trees$Height) hist(trees$Girth) 

plot(trees$Girth, trees$Height)

To save space, you can’t print those graphs here but you can mention that both variables are approximately normally distributed, with some positive skew on girth, and the scatter plot appears to be approximately linear. The variables appear well suited for regression analysis. 



The command for linear regression is lm(). It requires only two arguments: the outcome variable and the predictor variable, with the tilde operator, ~, between them. For this example, it appears as lm(Height ~ Girth, data = trees). The third argument here, data = trees, is optional. It is there to specify the data set from which the variables are drawn. As a note, it is also possible to give the full address for each variable, like trees$Height and trees$Girth. The separate data statement is just a convenience. 



In this example, you can save the regression model into reg1. By saving the model into an object, you can then able to call several functions on the model without having to specify it again. In the code that follows, you can run lm() and then get information on the model with summary(). 

# BASIC REGRESSION MODEL reg1 <- lm(Height ~ Girth, data = trees)  # Save the model. summary(reg1)  # Get regression output. 

Call: 

lm(formula = Height ~ Girth, data = trees)



Residuals: 

     Min       1Q   Median       3Q      Max  

-12.5816  -2.7686   0.3163   2.4728   9.9456  



Coefficients: 

            Estimate Std. Error t value Pr(>|t|)     

(Intercept)  62.0313     4.3833  14.152 1.49e-14 *** Girth         1.0544     0.3222   3.272  0.00276 **  

--- 

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1  

Residual standard error: 5.538 on 29 degrees of freedom 

Multiple R-squared:  0.2697, Adjusted R-squared:  0.2445  

F-statistic: 10.71 on 1 and 29 DF,  p-value: 0.002758

The summary() function gives nearly everything that is needed for most analyses: the regression coefficients, their statistical significance, the R2 and adjusted R2, the _F_-value and _p_value for the entire model, and information about residuals. (See ?lm and ?summary for more information.) 

It is also possible to get confidence intervals for the regression coefficients with the confint() function: 

# CONFIDENCE INTERVALS confint(reg1) 

                 2.5 %    97.5 % 

(Intercept) 53.0664541 70.996174 

Girth        0.3953483  1.713389

Other functions that can be useful when evaluating a regression model are: 

* predict() that gives the predicted values on the outcome variable for different values on the predictor (see ?predict). 
* lm.influence() that gives basic quantities used in forming a wide variety of diagnostics for checking the quality of regression fits (see ?lm.influence). 
* influence.measures() that gives information on residuals, dispersion, hat values, and others (see ?influence.measures). 

You can complete by unloading packages and cleaning the workspace: 

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads data sets package.

 rm(list = ls())  # Remove all objects from workspace.

### Two-sample t-test 

When you want to compare the means of two different groups, then the most common choice is the two-sample t-test. R provides several options for this test and several ways to organize the data for analysis. For this example, you can use the sleep data from R’s datasets package. This data set that is gathered by the creator of the t-test, William Gosset, who published under the pseudonym Student, shows the effect of two soporific drugs on 10 patients. The data set has three columns: extra that is the increase in hours of sleep; group that indicates the drug given; and ID that is the patient ID. To make the analysis simpler, you can open the data, remove the ID variable, and save it as a new data frame called sd for “sleep data.” 

#### Sample: sample_7_3.R 

# LOAD DATA & SELECT VARIABLES require(“datasets”)  # Load 

the datasets package. sleep[1:5, ]  # Show the first 5 cases. 

sd <- sleep[, 1:2]  # Save just the first two variables. 

sd[1:5, ]  # Show the first 5 cases.

Once the data are loaded into the new data frame, you can create some basic graphs, a histogram and a boxplot, to check on the shape of the distribution and how well it meets the statistical assumptions of the t-test. 

# GRAPHICAL CHECKS hist(sd$extra)  # Histogram of extra sleep.

 boxplot(extra ~ group, data = sd)  # Boxplots by group.

To save space, you can’t reprint the graphics here, but you can mention that the histogram is approximately normal and the boxplots that are separated by group, do not show any outliers. With this preliminary check done, you can move ahead to the default t-test, using R’s t.test() function. 

# TWO-SAMPLE T-TEST WITH DEFAULTS 

t.test(extra ~ group, data = sd) 

 	Welch Two Sample t-test 

data:  extra by group 

t = -1.8608, df = 17.776, p-value = 0.07939 

alternative hypothesis: true difference in means is not equal to 0 95 percent confidence interval: 

 -3.3654832  0.2054832 sample estimates: 

mean in group 1 mean in group 2             0.75            2.33

These results show that while there is a difference between the two group’s means—2.33 vs. 0.75—the difference does not meet the conventional levels of statistical significance, as the observed p-value of .08 is greater than the standard cut-off of .05. As a note, R uses the Welch two-sample t-test by default that is often used for samples with unequal variances. One consequence of this choice is that the degree of freedom is a fractional value, 17.776 in this case. For the more common, equal-variance t-test, just add the argument var.equal = TRUE to the function call (see ?t.test for more information). 



R’s t.test() function also provides a number of options, such as one-tailed tests and different confidence levels, as show in the following code. 

# TWO-SAMPLE T-TEST WITH OPTIONS 

t.test(extra ~ group,  # Same: Specifies variables. 

       data = sd,  # Same: Specifies data set.        alternative = "less",  # One-tailed test.        conf.level = 0.80)  # 80% CI (vs. 95%) 



 	Welch Two Sample t-test 

data:  extra by group 

t = -1.8608, df = 17.776, p-value = 0.0397 

alternative hypothesis: true difference in means is less than 0 80 percent confidence interval: 

       -Inf -0.8478191 sample estimates: 

mean in group 1 mean in group 2             0.75            2.33 

As expected, when a one-tailed or directional hypothesis test is applied to the same data, the results are statistically significant with a p-value of .04 (exactly half of the p-value for the twotailed test). 

In the previous examples with the sleep data, the outcome variable is in one column and the grouping variable is in a second column. It is, however, also possible to have the data for each group in separate columns. In the following example, you can create simulated data using R’s rnorm() function that draws data from a normal distribution. One advantage of using simulated data is that it is possible to specify the differences between the groups exactly. It is also important to note that the data in this example is slightly different every time the code is ran, so your results should not match these exactly. 

In this case, where the data for the two groups are in difference variables, the function call is simpler: just give the names of the two variables as the arguments to t.test(), as shown in the following code. 

# SIMULATED DATA IN TWO VARIABLES x <- rnorm(30, mean = 20, sd = 5) y <- rnorm(30, mean = 24, sd = 6) 

t.test(x, y) 



 	Welch’s Two Sample t-test data:  x and y 

t = -2.6208, df = 56.601, p-value = 0.01124 

alternative hypothesis: true difference in means is not equal to 0 95 percent confidence interval: 

 -6.8721567 -0.9186296 sample estimates: mean of x mean of y   20.18359  24.07898 

The results in this case indicates a statistically significant difference with a p-value of .01. Finally, you should orderly remove the packages and objects used in this section.  

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads data sets package. 

rm(list = ls())  # Remove all objects from workspace.

### Paired t-test 

The t-test is a flexible test with several variations. In the previous section you used the t-test to compare the means of two groups. In this section, you can use the t-test to examine how scores change over time for a single group of subjects. This is known as paired t-test because each participant’s observation at time 2 is paired with their own observation at time 1. The paired t-test is also known as the matched-subjects t-test or the within-subjects t-test. 

In this example, you can use simulated data. In the following code example, a variable called t1, for time 1, is created using the rnorm() function to generate normally distributed data with a specified mean and standard deviation (see ?rnorm for more information). To simulate changes over time, another random variable, called dif for difference, is generated. These two variables are then summed to yield t2, the scores at time 2 for each subject. 

#### Sample: sample_7_4.R 

# CREATE SIMULATED DATA 

t1 <- 

rnorm(50, mean = 52, sd = 6)  # Time 1 dif <- rnorm(50, mean = 6, sd = 12)  # Difference t2 <- t1 + dif 

 # Time 2

Once the data is generated, the t.test() function is called. The arguments in this case are the variables with the data at the two times, as well as paired = TRUE that indicates that the paired t-test should be used. 

# PAIRED T-TEST WITH DEFAULTS 

t.test(t2, t1, paired = TRUE)  # Must specify "paired" 



 	Paired t-test 

data:  t2 and t1 

t = 4.457, df = 49, p-value = 4.836e-05 

alternative hypothesis: true difference in means is not equal to 0 95 percent confidence interval: 

  4.078233 10.775529 sample estimates: 

mean of the differences                 7.426881 

In this example, the change in scores over time is statistically significant. Note that your own results are different because the data is generated randomly. This effect can be seen in the following code example, the t-test is ran again with several options: a non-zero null value is specified, a one-tailed test is called, and a different confidence level is used. The mean difference is smaller in this example (5.81 vs. 7.43) because the data is generated again before running this code. 

# PAIRED T-TEST WITH OPTIONS 

t.test(t2, t1,         paired = TRUE,        mu = 6,  # Specify a non-0 null value.        alternative = "greater",  # One-tailed test        conf.level = 0.99)  # 99% CI (vs. 95%) 



 	Paired t-test 

data:  t2 and t1 

t = -0.1027, df = 49, p-value = 0.5407 

alternative hypothesis: true difference in means is greater than 6 99 percent confidence interval: 

 1.529402      Inf sample estimates: 

mean of the differences                 5.816891 

In this case the difference is not statistically significant, but that is primarily due to different null value used—6 vs. 0—and not to the variations in the datat. 

You can complete by clearing the workspace. 

# CLEAN UP rm(list = ls())  # Remove all objects from the

 workspace.

### One-factor ANOVA 

One-factor analysis of variance, or ANOVA, is used to compare the means of several different groups on a single, quantitative outcome variable. In this example, you can again use simulated data from R’s rnorm() function. As before, this means that your data is slightly different, but the overall pattern should be the same. After the four variables are created with one variable for each group, they are combined into a single data frame using the data.frame() and cbind() functions. Then the data is reorganized. The four separate variables are then stacked into a single outcome variable called values with the stack() function that also creates a column called ind to indicate their source variable; this variable functions as the grouping variable. 

#### Sample: sample_7_5.R 

# CREATE SIMULATION DATA 

# Step 1: Each group in a separate variable. 

x1 <- rnorm(30, mean = 40, sd = 8)  # Group 1, mean = 40 x2 <- rnorm(30, mean = 41, sd = 8)  # Group 1, mean = 41 x3 <- rnorm(30, mean = 44, sd = 8)  # Group 1, mean = 44 x4 <- rnorm(30, mean = 45, sd = 8)  # Group 1, mean = 45  

# Step 2: Combine vectors into a single data frame. 

xdf <- data.frame(cbind(x1, x2, x3, x4))  # xdf = "x data frame" 



# Step 3: Stack data to get the outcome column and group column. 

xs <- stack(xdf)  # xs = "x stack" 

At this point, the data is ready to analyze with R’s aov() function. The default specification is simple, with just two required arguments: the outcome variable that is values in this case, and the grouping variable that is ind in this case. The two variables are joined by the tilde operator, ~. (The argument data = xs simply indicates the data frame that holds the variables.) 

# ONE-FACTOR ANOVA anova1 <- aov(values ~ ind, data = xs)  # Basic model. summary(anova1)  # Model summary. 



             Df Sum Sq Mean Sq F value Pr(>F)   ind           3    579  192.97   3.188 0.0264 * Residuals   116   7022   60.53                  

--- 

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

These results indicate that the omnibus analysis of variance is statistically significant, with a pvalue of .03. However, this test does not indicate where the group differences lie. To find them, you need a post-hoc comparison. The default post-hoc procedure in R is Tukey’s Honestly Significant Difference (HSD) test or TukeyHSD().Because you have saved the ANOVA model into an object named anova1, you can just use that object as the argument in the code that follows: 

# POST-HOC COMPARISONS 

TukeyHSD(anova1) 



  Tukey multiple comparisons of means 

    95% family-wise confidence level 

Fit: aov(formula = values ~ ind, data = xs) 

 $ind 

           diff        lwr       upr     p adj x2-x1  1.132450 -4.1039279  6.368829 0.9425906 x3-x1  5.502549  0.2661705 10.738927 0.0354399 x4-x1  4.004412 -1.2319658  9.240791 0.1964255 x3-x2  4.370098 -0.8662799  9.606477 0.1362299 x4-x2  2.871962 -2.3644162  8.108340 0.4835521 x4-x3 -1.498136 -6.7345146  3.738242 0.8782931 

The TukeyHSD() function reports observed differences between paired groups, as well as the lower and upper bounds for confidence intervals and adjusted p-values. The interesting thing in this particular situation is that the only statistically significant difference is between groups 1 and 3. This is surprising because the commands that generated the random data actually called for a larger difference between groups 1 and 4. However, given the vicissitudes of random data generation, such quirks are to be expected. This is also an indication that sample size of 30 scores per group is probably not sufficiently large enough to give stable results. In future research, it is beneficial to at least double the sample size. 



You can finish by cleaning the workspace of the variables and objects we generated in this exercise. 

# CLEAN UP 

rm(list = ls())  # Remove all objects from workspace

### Comparing proportions 

In the last five procedures that you have examined—correlations, regression, two-sample t-tests, paired t-tests, and ANOVA—the outcome variable is quantitative (i.e., an interval or ratio level of measurement). This made it possible to calculate means and standard deviations that are then used in the inferential tests. However, there are situations where the outcome variable is categorical (i.e., a nominal or possibly ordinal level of measurement.) The last two sections in this chapter address those situations. In the case of a dichotomous outcome—that is, an outcome with only two possible values, such as yes or no, on or off, and click or don’t click, then a proportion test is an ideal way to compare the performance of two or more groups. 



In this example, you can again use simulated data. R’s prop.test() function needs two pieces of information for each group: the number of trials or total observations, usually referred to as n, and the number of trials with positive outcome, usually referred to as X. In the code that follows, a vector called n5 of five 100s for the number of trials by using R’s rep() function is created(see ?rep for more information). A second vector called x5 is then created with the number of positive outcomes for each group. 

#### Sample: sample_7_6.R 

# CREATE SIMULATION DATA 

# Two vectors are needed: 

# One specifies the total number of people in each group. 

# This creates a vector with 5 100s in it, for 5 groups. 

n5 <- c(rep(100, 5)) 

# Another specifies the number of cases with positive outcomes.  x5 <- c(65, 

60, 60, 50, 45)

The next step is to call R’s prop.test() function with two arguments: the variable with the X data and the variable with the n data. 

# PROPORTION TEST prop.test(x5, n5) 

 	5-sample test for equality of proportions without continuity  	correction 



data:  x5 out of n5 

X-squared = 10.9578, df = 4, p-value = 0.02704 alternative hypothesis: two.sided sample estimates: 

prop 1 prop 2 prop 3 prop 4 prop 5    0.65   0.60   0.60   0.50   0.45 

The output includes a chi-squared test—shown here as X-squared—and the p-value that in this case is below the standard cutoff of .05. It also gives the observed sample proportions. 



In the case where there are only two groups being compared, R also gives confidence interval for the difference between the groups’ proportions. The default value is .95 but that can be changed with the conf.level argument. 

<table>
<tr>
<td>
# CREATE SIMULATION DATA FOR 2 GROUPS n2 <- c(40, 40)  # 40 trials x2 <- c(30, 20)  # Number of positive outcomes # PROPORTION TEST FOR 2 GROUPS prop.test(x2, n2, conf.level = .80)  # With CI  	2-sample test for equality of proportions with continuity  	correction data:  x2 out of n2 X-squared = 4.32, df = 1, p-value = 0.03767 </td></tr>
<tr>
<td>
alternative hypothesis: two.sided 80 percent confidence interval:  0.09097213 0.40902787 sample estimates: prop 1 prop 2    0.75   0.50 </td></tr>
</table>
In this case, despite the relatively small samples, the difference between the sample proportions is statistically significant, as shown by both the p-value and the confidence interval. 



You can finish by clearing the workspace. 

# CLEAN UP rm(list = ls())  # Remove all objects from the 

workspace.

### Cross-tabulations 

The final bivariate procedure that you can see in this section is the chi-squared inferential test for crosstabulated data. In this example you can use the Titanic data from R’s datasets package. This data contains the survival data from the wreck of the Titanic, broken down by sex, age (child or adult), and class of passenger (1st, 2nd, 3rd, or crew). See ?Titanic for more information. The data is in a tabular format that, when displayed, consists of four 4 x 2 tables, although it is also possible to display it as a “flat” contingency table with the ftable() function.  

#### Sample: sample_7_7.R 

# LOAD DATA require(“datasets”)  # Load the datasets package. Titanic              # Show complete data in tables. 

ftable(Titanic)      # Display a "flat" contingency table. 

                   Survived  No Yes 

Class Sex    Age                    

1st   Male   Child            0   5 

             Adult          118  57 

      Female Child            0   1 

             Adult            4 140 

2nd   Male   Child            0  11 

             Adult          154  14 

      Female Child            0  13 

             Adult           13  80 

3rd   Male   Child           35  13 

             Adult          387  75 

      Female Child           17  14 

             Adult           89  76 

Crew  Male   Child            0   0 

             Adult          670 192 

      Female Child            0   0 

             Adult            3  20

While the tabular format is a convenient way of storing and displaying the data, you can restructure the data for your analysis so there is one row per observation. You can do this using a string of nested commands that you saw in sample_1_1.R: 

# RESTRUCTURE DATA 

tdf <- as.data.frame(lapply(as.data.frame.table(Titanic),       function(x)rep(x, as.data.frame.table(Titanic)$Freq)))[, -5] tdf[1:5, ]  # Check the first five rows of data. 

  Class  Sex   Age Survived 

1	3rd Male Child       No 

2	3rd Male Child       No 

3	3rd Male Child       No 

4	3rd Male Child       No 

5	3rd Male Child       No

For this example, you can focus on just two of the four variables: Class and Survived. To do this, you can create a reduced data set by using the table() function and save it into a new object, ttab for Titanic Table.

# CREATE REDUCED TABLE ttab <- table(tdf$Class, tdf$Survived)  # Select two variables. 

ttab  # Show the new table. 

        No Yes 

  1st  122 203 

  2nd  167 118 

  3rd  528 178 

  Crew 673 212

The raw frequencies are important and easier to understand the pattern by looking at the row percentages. You can get these by using the prop.table() function and requesting the first column. In the following code example, commands are wrapped in the round() function to reduce the output to two decimal places, then multiply by 100 to get percentages. 

# PERCENTAGES 

rowp <- round(prop.table(ttab, 1), 2) * 100 # row % colp <- round(prop.table(ttab, 2), 2) * 100 # column % totp <- round(prop.table(ttab), 2) * 100    # total % rowp 

       No Yes 

  1st  38  62 

  2nd  59  41 

  3rd  75  25   Crew 76  24

From these results it is clear that first-class passengers have much higher survival rate at 62%, while third-class passengers and crew have much lower rate at 25% and 24%, respectively. The chi-squared test for independence, or R’s chisq.test() function, can then test these results against a null hypothesis: 

# CHI-SQUARED TEST FOR INDEPENDENCE 

tchi <- chisq.test(ttab)  # Compare two variables in ttab tchi 

 Pearson's Chi-squared test data:  ttab 

X-squared = 190.4011, df = 3, p-value < 2.2e-16 

These group differences are highly significant, with a p-value that is nearly zero. In addition to this basic output, R is able to give several other detailed tables from the saved chisquared object: 

* tchi$observed gives the observed frequencies that is as same as data in ttab. 
* tchi$expected gives the expected frequencies from the null distribution. 
* tchi$residuals give the Pearson's residuals. 
* tchi$stdres  gives the standardized residuals based on the residual cell variance. 

Finally, you can unload the packages and clear the workspace before moving on.

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads the datasets package.

 rm(list = ls())  # Remove all objects from the workspace.





