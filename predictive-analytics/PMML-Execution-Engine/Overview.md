---
layout: post
title: Overview | PMML Execution Engine | Predictive Analytics | Syncfusion
description: overview
platform: predictive-analytics
control: Essential Predictive Analytics
documentation: ug
---

# Overview

This section explains you the PMML Execution Engine with its key features, prerequisites to use the API, its compatibilities with Visual Studio Frameworks and finally the documentation details complimentary with the product.

## Introduction to PMML Execution Engine

PMML Execution Engine is a C# library developed for predicting results based on predicted modelling done in PMML over the input data and you can run on the following .Net platforms – Windows forms, WinRT, WPF, UWP, ASP.NET and ASP.NET MVC. PMML stands for Predictive Model Markup Language. It is an XML-based file format developed by the [Data Mining Group](http://www.dmg.org) to provide a way for applications to describe and exchange models produced by data mining and machine learning algorithms.

## Use Case Scenario

PMML Execution Engine is used to make predictions based on the input PMML file. You can bind the predicted results to dashboard applications for intuitive understanding and decision making. 

## Key Features

Important features of PMML Execution Engine are as follows,

* Predicts the results for corresponding model mentioned in the PMML loaded.
* The results are exactly similar to that obtained from the [R](http://cran.r-project.org/) software.
* Predicts both classification (Categorical values) as well as regression (Numeric values).
* Calculates the probability of prediction in case of categorical values.
* The models currently supported in PMML Execution Engine are,



List of models currently supported in PMML Execution Engine

<table>
<tr>
<th>
Model  Name</th><th>
Function Name</th><th>
Algorithm Name</th></tr>
<tr>
<td>
Regression Model</td><td>
Regression/multinomial regression</td><td>
least squares/multinom</td></tr>
<tr>
<td>
Generalized Regression Model</td><td>
regression/classification</td><td>
logit/cloglog/log/identity/inverse/sqrt/probit/coxph</td></tr>
<tr>
<td>
Naive Bayes Model</td><td>
classification</td><td>
naive bayes</td></tr>
<tr>
<td>
Classification & Regression Tree Model</td><td>
regression/classification</td><td>
rpart</td></tr>
<tr>
<td>
Support Vector Machine Model</td><td>
regression/classification</td><td>
ksvm</td></tr>
<tr>
<td>
Random Forest (Mining Model).</td><td>
regression/classification</td><td>
randomForest</td></tr>
<tr>
<td>
Neural Networks Model</td><td>
regression/classification</td><td>
nnet</td></tr>
<tr>
<td>
Clustering Model</td><td>
classification</td><td>
kmeans</td></tr>
<tr>
<td>
Gradient Boosting Machine Model (Mining Model)</td><td>
regression</td><td>
</td></tr>
<tr>
<td>
Association Rules Model</td><td>
</td><td>
arules</td></tr>
<tr>
<td>
K-Nearest Neighbors Model</td><td>
regression/classification</td><td>
</td></tr>
</table>

## User Guide Organization

The product is derived with example of PMMLs and input data samples as well as an extensive documentation to guide you. This User guide provides you detailed information on the features and methodologies of PMML Execution Engine. It is organized into the following sections:

Overview-This section gives a brief introduction to your product and its key features.

Installation and Deployment-This section elaborates license, patches and information on adding or removing selective components.

System Requirements-This section covers the lists of IDE’s, PMML versions, Visual Studio Frameworks compatible with PMML Execution Engine.

Deploying PMML Execution Engine-This section elaborates the required assemblies to deploy the applications in the specific platforms.

Getting Started-This section guides you on getting started with PMML Execution Engine. 

Concepts and Features-The features of PMML Execution Engine are illustrated with use case scenarios, code examples and screenshots under this section.

Frequently Asked Questions-This section covers the list of questions with expert solutions.

## Document Conventions

The following conventions helps you in quickly identifying the important sections of information when using the content.

Document Conventions

<table>
<tr>
<th>
Convention</th><th>
Icon</th><th>
Description</th></tr>
<tr>
<td>
Note</td><td>

{{ 'Note:' | markdownify }}</td><td>
Represents important information</td></tr>
<tr>
<td>
Example</td><td>
Example</td><td>
Represents an example</td></tr>
<tr>
<td>
Tip</td><td>
{{ '![](Overview_images/img2.jpeg) '| markdownify }}

</td><td>
Represents useful hints that helps you in using the controls/features</td></tr>
<tr>
<td>
Additional Information</td><td>
{{ '![](Overview_images/img3.jpeg) '| markdownify }}

</td><td>
Represents additional information on the topic</td></tr>
</table>


