---
layout: post
title: Statistics-for-Three-or-More-Variables
description: statistics for three or more variables 
platform: ug
control: Essential Predictive Analytics
documentation: predective-analysis
---

## Statistics for Three or More Variables 

The final analytic section addresses a few common methods for exploring and describing the relationships between multiple variables. These methods include the single most useful procedure in any analyst’s toolbox, multiple regression. Other methods including the two-factor analysis of variance, the cluster analysis, and the principle components, or factor, analysis are covered. 

### Multiple regression 

The goal of regression is simple: take a collection of predictor variables and use them to predict scores on a single, quantitative outcome variable. Multiple regression is the most flexible approach that is covered in this section. All the other parametric procedures that you have covered—t-tests, ANOVA, correlation, and bivariate regression—can all be seen as special cases of multiple regression. 

In this section, you can start by looking at the simplest version of multiple regression: simultaneous entry. This is when all the predictors are entered as a group and all of them are retained in the equation. 

In this particular example, you are going to look at the most basic form of multiple regression, where all the variables are entered at the same time in the equation (it's the variable selection and entry that causes most of the fuss in statistics). You can begin by loading the 

USJudgeRatings data from R’s datasets package. See USJudgeRatings for more information. 

#### Sample: sample_9_1.R 

# LOAD DATA require("datasets")  # Load the datasets package. data(USJudgeRatings)  # Load data into the workspace. 

USJudgeRatings[1:3, 1:8]  # Display 8 variables for 3 cases. 

               CONT INTG DMNR DILG CFMG DECI PREP FAMI 

AARONSON,L.H.   5.7  7.9  7.7  7.3  7.1  7.4  7.1  7.1 

ALEXANDER,J.M.  6.8  8.9  8.8  8.5  7.8  8.1  8.0  8.0 

ARMENTANO,A.J.  7.2  8.1  7.8  7.8  7.5  7.6  7.5  7.5 

The default function in R for regression is lm() that stands for linear model (see ?lm for more information). The basic structure is lm(outcome ~ predictor1 + predictor2). You can run this function on the outcome variable, RTEN (i.e., “worthy of retention”), and the eleven predictors using the code that follows and save the model to an object that you call reg1, for regression 1. Then, by calling only the name reg1, you can get the regression coefficients, and by calling summary(reg1), you can get several statistics on the model. 

# MULTIPLE REGRESSION: DEFAULTS 

# Simultaneous entry 

# Save regression model to the object. 

reg1 <- lm(RTEN ~ CONT + INTG + DMNR + DILG + CFMG +             DECI + PREP + FAMI + ORAL + WRIT + PHYS,            data = USJudgeRatings)

Once you have saved the regression model, you can just call the object’s name, reg1, and get a list of regression coefficients: 

Coefficients: 

(Intercept)         CONT         INTG         DMNR         DILG   

   -2.11943      0.01280      0.36484      0.12540      0.06669   

       CFMG         DECI         PREP         FAMI         ORAL   

   -0.19453      0.27829     -0.00196     -0.13579      0.54782   

       WRIT         PHYS   

   -0.06806      0.26881

For more detailed information about the model, including descriptions of the residuals, confidence intervals for the coefficients, and inferential tests, we can just type summary(reg1): 

Residuals: 

     Min       1Q   Median       3Q      Max  -0.22123 -0.06155 -0.01055  0.05045  0.26079  



Coefficients: 

            Estimate Std. Error t value Pr(>|t|)     

(Intercept) -2.11943    0.51904  -4.083 0.000290 *** 

CONT         0.01280    0.02586   0.495 0.624272     

INTG         0.36484    0.12936   2.820 0.008291 **  

DMNR         0.12540    0.08971   1.398 0.172102     

DILG         0.06669    0.14303   0.466 0.644293     

CFMG        -0.19453    0.14779  -1.316 0.197735     DECI         0.27829    0.13826   2.013 0.052883 .   

PREP        -0.00196    0.24001  -0.008 0.993536     

FAMI        -0.13579    0.26725  -0.508 0.614972     ORAL         0.54782    0.27725   1.976 0.057121 .   

WRIT        -0.06806    0.31485  -0.216 0.830269     PHYS         0.26881    0.06213   4.326 0.000146 *** 

--- 

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1  

Residual standard error: 0.1174 on 31 degrees of freedom 

Multiple R-squared:  0.9916, Adjusted R-squared:  0.9886  

F-statistic: 332.9 on 11 and 31 DF,  p-value: < 2.2e-16

All the predictor variables are included in this model that means their coefficients and probability values are only valid when taken together. Two things are curious about this model. First, it has an extraordinarily high predictive value, with an R2 of 99%. Second, the two most important predictors in this simultaneous entry model are (a) INTG, or judicial integrity that makes obvious sense, and (b) PHYS, or physical ability that has a t-value that’s nearly twice as large as the integrity. This second one doesn’t make sense but is supported by the data. 

Additional information on the regression model is available with these functions, when the model’s name is entered in the parentheses: 

* anova() that gives an ANOVA table for the regression model 
* coef() or coefficients() that gives the same coefficients that you got by calling the model’s name, reg1. 
* confint() that gives confidence intervals for the coefficients. 
* resid() or residuals() that gives case-by-case residual values.  hist(residuals()) that gives a histogram of the residuals. 

Multiple regression is potentially a very complicated procedure with an enormous number of variations and much room for analytical judgment calls. The version that you conducted previously is the simplest version: all of the variables are entered at once in their original state (i.e. without any transformations), no interactions are specified, and no adjustments are made once the model is calculated. 

R’s base installation provides many other options and the available packages give hundreds, and possibly thousands, of other options for multiple regression.21 you can just mention two of R’s built-in options, both of that are based on stepwise procedures. Stepwise regression models work by using a simple criterion to include or exclude variables from a model, and they can greatly simplify analysis. Such models, however, are very susceptible to capitalizing on the quirks of data, leading one author, in exasperation, to call them “positively satanic in their temptations toward Type I errors.”22 



With those stern warnings in mind, you can nonetheless take a brief look at two versions of stepwise regression because they are very common—and commonly requested—procedures. The first variation you can examine is backwards removal, where all possible variables are initially entered, and then variables that do make statistically significant contributions to the overall model is removed one at a time.  



The first step is to create a full regression model, just like you did for simultaneous regression. Then, the R function step() is called regression model as its first argument and direction = "backward" as the second. An optional argument, trace = 0, prevents R from printing out all of the summary statistics at each step. Finally, you can use summary() to get summary statistics on the new model that is saved as regb, as in “regression backwards.” 



# MULTIPLE REGRESSION: STEPWISE: BACKWARDS REMOVAL reg1 <- lm(RTEN ~ CONT + INTG + DMNR + DILG + CFMG +             DECI + PREP + FAMI + ORAL + WRIT + PHYS,            data = USJudgeRatings) regb <- step(reg1,  # Stepwise regression, starts with the full model. 

             direction = "backward",  # Backwards removal              trace = 0)  # Don't print the steps. summary(regb)  # Give the hypothesis testing info. Residuals: 

      Min        1Q    Median        3Q       Max  

-0.240656 -0.069026 -0.009474  0.068961  0.246402  

~~~~

21. See the Regression Modeling Strategies package rms for one excellent example. 
22. That line is from page 185 of Norman Cliff’s 1987 book, _Analyzing Multivariate Data_. Similar sentiments are expressed by Bruce Thompson in his 1998 talk “Five Methodology Errors in Educational Research: The Pantheon of Statistical Significance and Other Faux Pas,” that lists stepwise methods as Error #1 (see 

[http://people.cehd.tamu.edu/~bthompson/aeraaddr.htm](http://people.cehd.tamu.edu/~bthompson/aeraaddr.htm)[)](http://people.cehd.tamu.edu/~bthompson/aeraaddr.htm), or his 1989 journal editorial entitled “Why won't stepwise 

methods die?” in _Measurement and Evaluation in Counseling and Development_ (see 

[http://myweb.brooklyn.liu.edu/cortiz/PDF%20Files/Why%20Wont%20Stepwise%20Methods%20Die.pdf)](http://myweb.brooklyn.liu.edu/cortiz/PDF%20Files/Why%20Wont%20Stepwise%20Methods%20Die.pdf). In simpler terms: analysts beware. 





Coefficients: 

            Estimate Std. Error t value Pr(>|t|)     

(Intercept) -2.20433    0.43611  -5.055 1.19e-05 *** 

INTG         0.37785    0.10559   3.579 0.000986 *** 

DMNR         0.15199    0.06354   2.392 0.021957 *   

DECI         0.16672    0.07702   2.165 0.036928 *   

ORAL         0.29169    0.10191   2.862 0.006887 **  PHYS         0.28292    0.04678   6.048 5.40e-07 *** 

--- 

Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1  

Residual standard error: 0.1119 on 37 degrees of freedom 

Multiple R-squared:  0.9909, Adjusted R-squared:  0.9897  

F-statistic: 806.1 on 5 and 37 DF,  p-value: < 2.2e-16 

Using a stepwise regression model with backwards removal, the predictive ability or R2 is still 99%. Only five variables remained in the model and, as with the simultaneous entry model, physical ability is still the single biggest contributor. 

A more common approach to stepwise regression is forward selection that starts with no variables and then adds them one at a time when they make statistically significant contributions to predictive ability. This approach is slightly more complicated in R because it requires the creation of a “minimal” model with nothing more than the intercept that is the mean score on the outcome variable. This model is created by using the number 1 as the only predictor variable in the equation. Then the step() function is called again, with the minimal model as the starting point and direction = "forward" as one of the attributes. The possible variables to include are listed in scope. Finally, trace = 0 prevents the intermediate steps from being printed. 

<table>
<tr>
<td>
# MULTIPLE REGRESSION: STEPWISE: FORWARDS SELECTION # Start with a model that has nothing but a constant. reg0 <- lm(RTEN ~ 1, data = USJudgeRatings)  # Intercept only regf <- step(reg0,  # Start with intercept only.              direction = "forward",  # Forward addition              # scope is a list of possible variables to include.              scope = (~ CONT + INTG + DMNR + DILG + CFMG + DECI +                         PREP + FAMI + ORAL + WRIT + PHYS), </td></tr>
<tr>
<td>
             data = USJudgeRatings,              trace = 0)  # Don't print the steps. summary(regf)  # Statistics on model. Residuals:       Min        1Q    Median        3Q       Max  -0.240656 -0.069026 -0.009474  0.068961  0.246402  Coefficients:             Estimate Std. Error t value Pr(>|t|)     (Intercept) -2.20433    0.43611  -5.055 1.19e-05 *** ORAL         0.29169    0.10191   2.862 0.006887 **  DMNR         0.15199    0.06354   2.392 0.021957 *   PHYS         0.28292    0.04678   6.048 5.40e-07 *** INTG         0.37785    0.10559   3.579 0.000986 *** DECI         0.16672    0.07702   2.165 0.036928 *   --- Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1  Residual standard error: 0.1119 on 37 degrees of freedom Multiple R-squared:  0.9909, Adjusted R-squared:  0.9897  F-statistic: 806.1 on 5 and 37 DF,  p-value: < 2.2e-16 </td></tr>
</table>
Given the possible fluctuations of stepwise regression, it is reassuring to know that both approaches finished with the same model, although they are listed in a different order. 

Again, it is important to remember that multiple regression can be very complicated and subtle procedure and that many analysts have criticized stepwise methods vigorously. Fortunately, R and its available packages offer many alternatives—and more are added on a regular basis—it is encouraged you to explore options before committing to a single approach. 

Once you have saved your work, you should clean the workspace by removing any variables or objects you created. 

# CLEAN UP detach("package:datasets", unload = TRUE)  # Unloads the datasets package. 

rm(list = ls())  # Remove all objects from the workspace.

### Two-factor ANOVA 

The multiple regression procedure that you have seen in the previous section is enormously flexible, and the procedure that you can learn in this section is the two-factor analysis of variance (ANOVA), can accurately be described as a special case of multiple regression. There are, however, advantages of using the specialized procedures of ANOVA. The most important advantage is that it is developed specifically to work in situations where two categorical variables—called factors in ANOVA—are used simultaneously to predict a single quantitative outcome. ANOVA gives easily interpreted results for the main effect of each factor and a third result for their interaction. You can examine these effects by using the warpbreaks data from R’sdatasets package. 

