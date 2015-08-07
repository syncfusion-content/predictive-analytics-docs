---
layout: post
title: Working-with-PMML-Execution-Engine
description: working with pmml execution engine
platform: ug
control: Essential Predictive Analytics
documentation: predective-analysis
---

# Working with PMML Execution Engine

PMML Execution engine is a PMML consumer that consumes the input PMML and creates an evaluator instance for the appropriate model, which in turn invokes the respective scoring procedure for prediction when GetResult method is called. PMML Execution Engine is easy to understand and are split into separate classes for all the models within a common namespace, Syncfusion.PMML.

## PMMLEvaluatorFactory

The PMMLEvaluatorFactory is a class that represents methods to instantiate respective PMML Evaluator.

### Properties and Methods

_Table_ _5__: Property Table_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
GetPMMLEvaluatorInstance(string)</td><td>
Returns the respective PMMLEvaluatorinstance for the given string PMML file path</td></tr>
<tr>
<td>
GetPMMLEvaluatorInstance(PMMLDocument)</td><td>
Returns the respective PMMLevaluator for the given PMMLDocument.</td></tr>
</table>
The following code example illustrates you the PMMLEvaluatorFactory that renders PMML file path using the GetPMMLEvaluatorInstance() method and passes it to PMMLEvaluator instance. Here a record from Iris dataset is passed as anonymous type and the predicted result is obtained.

{% highlight r %}

[C#]            
//Sample record passed as anonymous type            
 var anonymousType = new           
 {                
 Sepal_Length = 4.3,                
 Sepal_Width = 3.2,               
 Petal_Length = 0.4,               
 Petal_Width = 1.1            
 };            
 string PmmlFilePath = "../../Iris.pmml";  
 
 //Create instance for PMML             
 PMMLEvaluator PMMLEvaluator = new PMMLEvaluatorFactory().GetPMMLEvaluatorInstance(PmmlFilePath);            
 
 //Gets the predicted result            
 PredictedResult predictedResult = PMMLEvaluator.GetResult(anonymousType, null);            
 
 //Displays the predicted result            
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: SetosaThe predicted output (Setosa) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal - width and length, the iris species is more likely to be Setosa.


{% endhighlight %}

>![](Working-with-PMML-Execution-Engine_images/Working-with-PMML-Execution-Engine_img1.jpeg)  _Note: The above code example uses PMMLEvaluator instance. In case when the model to be evaluated is known before, then you can call the model evaluator directly._

## PMMLEvaluator

The PMMLEvaluator is an abstract class and it represents the base methods and properties for all model evaluators.

Properties and Methods

_Table_ _6__: PMMLEvaluator Public Properties/Methods_

<table>
<tr>
<td>
Property/ Method</td><td>
Description</td></tr>
<tr>
<td>
PMMLDocument</td><td>
Gets and Sets the PMMLDocument</td></tr>
<tr>
<td>
PMMLModel</td><td>
Gets the PMMLModel</td></tr>
<tr>
<td>
GetResult(object, IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the PMML model and returns the PredictedResult.</td></tr>
<tr>
<td>
Dispose</td><td>
Disposes the memory occupied by objects</td></tr>
</table>

### Regression Model Evaluator

Regression Model Evaluator represents the evaluator for Regression models. It inherits all the properties and methods of PMMLEvaluator.

#### Regression Model

The general purpose of regression model is to learn more about the relationship between several independent or predictor variables and a dependent variable. In the social and natural sciences multiple regression procedures are very widely used in research. 

_Table_ _7__: Property Table_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
RegressionModelEvaluator(PMMLDocument)</td><td>
Creates Instance for Regression model evaluator.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the Regression model and returns the PredictedResult.</td></tr>
</table>

Consider the Tips dataset taken from the “reshape2” R package that has information about each tip received by a waiter over a period of few months. A regression model is created based on this dataset and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with your installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Regression\Tips_

The following code example illustrates the procedure to call Regression Model Evaluator directly without the help of PMMLEvaluatorFactory.


{% highlight r %}

[C#]  
//Sample data is passed as anonymous type            
var anonymousType = new            
{              
total_bill=23.68,             
sex="Male",              
smoker="No",              
day="Sun",             
time="Dinner",              
size=2            
};            
string pmmlFilePath = "../../Tips.pmml";            

//Create instance for PMML Document            
PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);           

//Create instance for Mining model            
RegressionModelEvaluator regressionModel = new RegressionModelEvaluator(pmmlDocument);            

//Gets the predicted result            
PredictedResult predictedResult = regressionModel.GetResult(anonymousType, null);           

 regressionModel.Dispose();            
 //Displays the predicted result    
 
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: 3.50716866881611The predicted output (3.5) obtained from PMML Execution engine gives us a clear picture that based on given information (independant variables), the waiter may get a tip of 3.5$.In Regression Model Evaluator the term regression usually refers to the prediction of numeric values based on the Scoring procedure used in Regression Model.Here we considered the independent variables(total_bill,sex,smoker,day,time,size) and by using these independent variables we can find the result of dependent variable (tip) which is mentioned above .
{% endhighlight %}

### General Regression Model Evaluator

General Regression Model Evaluator represents the evaluator for General Regression model and it inherits all the properties and methods of PMMLEvaluator.

#### General Regression Model

General Regression Model is a supervised learning model used for both classification and regression. 

#### Properties and Methods



_Table_ _8__: General Regression Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
GeneralRegressionModelEvaluator (PMMLDocument)</td><td>
Creates Instance for GeneralRegression model evaluator for the given PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the General Regression model and returns the PredictedResult</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. A General regression model based on it is created and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\General Regression\Iris Logit_

The following code example illustrates the procedure to call General Regression Model Evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]
//Sample record is passed as anonymous type           
var anonymousType = new            
{                
SepalLength = 5.6,                
SepalWidth = 2.5,                
PetalLength = 3.9,                
PetalWidth = 1.1            
};            
string pmmlFilePath = "../../Iris Logit.pmml";    
        
//Create instance for PMMLDocument            
PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath); 
           
//Create instance for GeneralRegressionModelEvaluator            
GeneralRegressionModelEvaluator generalRegression = new GeneralRegressionModelEvaluator(pmmlDocument);  
          
//Gets the predicted result            
PredictedResult predictedResult = generalRegression.GetResult(anonymousType, null);          
generalRegression.Dispose();   
  
//Displays the predicted result            
Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: 1Here the predicted output value 1 represents the Iris category “Versicolor”. (i.e.) In Iris Datset we have considered the target value as Versicolor and if the Predicted Probability value of target is above 0.5 then by Binary Classification it is predicted as 1 (Versicolor) and the value below 0.5 is predicted as 0 (Not Versicolor).
{% endhighlight %}

### Naïve Bayes Model Evaluator

Naïve Bayes Model Evaluator represents the evaluator for the Naïve Bayes model and it inherits all the properties and methods of PMMLEvaluator.

#### Naïve Bayes Model

Naive Bayes classifier is a simple probabilistic classifier based on applying Bayes' theorem with strong and naive independence assumptions between the features. Naive Bayes is a popular method for text categorization. With appropriate pre-processing, it is competitive in this domain with more advanced methods including support vector machines. 

Bayesian classifiers have been used for text classification, such as junk email filtering such as spam, author identification, topic categorization and intrusion detection or anomaly detection in computer networks.

_Table_ _9__: NaiveBayes Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
NaiveBayesModelEvaluator(PMMLDocument)</td><td>
Creates Instance for Naïve Bayes model evaluator for the given PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the Naïve Bayes model and returns the PredictedResult. Currently NaiveBayes options available to specify whether there is a need to apply Laplace smoothing or not. We can apply Laplace smoothing by setting ApplyLaplace to true.</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. A Naïve Bayes model is created here, based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Naive Bayes\Iris_

The following code example illustrates the procedure to call Naïve Bayes model evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]     
//Sample record is passed as anonymous type           
 var anonymousType = new            
 {                
 SepalLength = 7.2,               
 SepalWidth = 3.6,                
 PetalLength = 6.1,               
 PetalWidth = 2.5            
 };            
 string pmmlFilePath = "../../Iris.pmml";            
 
 //Create instance for PMMLDocument            
 PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);            
 
 //Create instance for NaiveBayesModelEvaluator            
 NaiveBayesModelEvaluator naiveBayes = new NaiveBayesModelEvaluator(pmmlDocument);            
 
 //Gets the predicted result            
 PredictedResult predictedResult = naiveBayes.GetResult(anonymousType, null);            
 naiveBayes.Dispose();            
 
 //Displays the predicted result            
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: VirginicaThe predicted output (Virginica) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal - width and length, the iris species is more likely to be Virginica.</td></tr>
{% endhighlight %}

### Classification and Regression Tree Model Evaluator

Tree Model Evaluator represents the evaluator for tree models and it inherits all the properties and methods of PMMLEvaluator.

_Table_ _10__: Tree Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
TeeeModelEvaluator(PMMLDocument)</td><td>
Creates Instance for Tree model evaluator for the given PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the Tree model and returns the PredictedResultCurrently there is no Model options available for Tree Model.</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. Created here is a tree model based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Tree Model\Iris_

The following code example illustrates the procedure to call TreeModel evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]
//Sample values are passed as anonymous type            
var anonymousType = new            
{                
SepalLength = 6.2,                
SepalWidth = 2.9,                
PetalLength = 4.3,                
PetalWidth = 1.3            
};            
string pmmlFilePath = "../../Iris.pmml";  
          
//Create instance for PMMLDocument            
PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath); 
           
//Create instance for TreeModel Evaluator            
TreeModelEvaluator treeEvaluator = new TreeModelEvaluator(pmmlDocument);
           
//Gets the predicted result            
PredictedResult predictedResult = treeEvaluator.GetResult(anonymousType, null);            
treeEvaluator.Dispose();            

//Displays the predicted result            
Console.WriteLine(predictedResult.PredictedValue);           
{% endhighlight %}

{% highlight text %}
Output: VersicolorThe predicted output (Versicolor) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal width and length, the iris species is more likely to be Versicolor.</td></tr>
{% endhighlight %}

### Support Vector Machine Model Evaluator

Support Vector Machine Model Evaluator represents the evaluator for support vector machine models and it inherits all the properties and methods of PMMLEvaluator.

#### Support Vector Machines 

Support Vector Machines (SVM) model is a set of related supervised learning model with associated learning algorithms that analyse data and recognize patterns, used for both classification and regression analysis. It has gained popularity due to its potential for high accuracy.

_Table_ _11__: SupportVectorMachine Model Public Properties/Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
SupportVectorMachineModelEvaluator (PMMLDocument)</td><td>
Creates Instance for SupportVectorMachine model evaluator for the given PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the SupportVectorMachine model and returns the PredictedResult.Currently there is no Model options available for Support vector machine Model.</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. Created here is a Support Vector Machine model based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Support Vector Machine\Iris Sigmoid_

The following code example illustrates the procedure to call Support Vector Machine model evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#] 
//Sample values are passed as anonymous type            
var anonymousType = new            
{               
 SepalLength = 5.1,               
 SepalWidth = 3.8,                
 PetalLength = 1.6,                
 PetalWidth = 0.2            
 };            
 string pmmlFilePath = "../../Iris Sigmoid.pmml";           

 //Create instance for PMML Document            
 PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);           

 //Create instance for SupportVectorMachine model            
 SupportVectorMachineModelEvaluator supportVector = new SupportVectorMachineModelEvaluator(pmmlDocument);            
 
 //Gets the predicted result           
 PredictedResult predictedResult = supportVector.GetResult(anonymousType, null);           
 supportVector.Dispose();            
 
 //Displays the predicted result            
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: SetosaThe predicted output (Setosa) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal - width and length, the iris species is more likely to be Setosa.</td></tr>
{% endhighlight %}

### Mining Model or Random Forest Evaluator

Mining Model Evaluator represents the evaluator for mining models and it inherits all the properties and methods of PMMLEvaluator.

#### Mining Model

Mining model operates by constructing multiple decision trees. It draws random samples from the original data and for each random sample it grows a tree. It is used in both classification and regression cases.

_Table_ _12__: Mining Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
MiningModelEvaluator (PMMLDocument)</td><td>
Creates Instance for MiningModel evaluator for the given input PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the MiningModel and returns the PredictedResult.Currently there is no Model options available for Mining Model.</td></tr>
</table>


> ![](Working-with-PMML-Execution-Engine_images/Working-with-PMML-Execution-Engine_img2.jpeg) _Note: Gradient Boosting Model also initializes the Mining Model Evaluator as the PMML structure is similar._ 



Consider the Iris dataset taken from the "datasets" R package. Created here is a Random Forest model based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Random Forest\Iris_ 

The following code example illustrates the procedure to call Mining Model Evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]

//Sample values are passed as anonymous type           
 var anonymousType = new            
 {                
 SepalLength = 6.8,                
 SepalWidth = 3,               
 PetalLength = 5.5,               
 PetalWidth = 2.1            
 };            
 string pmmlFilePath = "../../Iris.pmml";            
 
 //Create instance for PMML Document            
 PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);            
 
 //Create instance for Mining model            
 MiningModelEvaluator miningModel = new MiningModelEvaluator(pmmlDocument);            
 
 //Gets the predicted result            
 PredictedResult predictedResult = miningModel.GetResult(anonymousType, null);
 miningModel.Dispose();            
 
 //Displays the predicted result            
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: VirginicaThe predicted output (Virginica) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal width and length, the iris species is more likely to be Virginica.</td></tr>
{% endhighlight %}

### Neural Network Model Evaluator

Neural Network Model Evaluator represents the evaluator for neural network models and it inherits all the properties and methods of PMMLEvaluator.

#### Neural Network Model

A Neural network also called an ANN or an Artificial Neural Network is an artificial system made of artificial neuron cells. It is modeled after the way the human brain works through imitating how the brain's neurons are fired or activated. The idea of neural networks is that, they are able to learn by themselves, an ability that makes them remarkably distinctive in comparison to normal computers, that cannot do anything for that they are not programmed. It is used in both classification and regression cases.

_Table_ _13__: Neural Network Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
NeuralNetwokModelEvaluator (PMMLDocument)</td><td>
Creates Instance for NeuralNetworkModel evaluator for the given input PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the NeuralNetworkModel and returns the PredictedResult. Currently there is no Model options available for Neural Network Model.</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. Created here is a Neural Network model based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Neural Networks\Iris_ 

The following code example illustrates the procedure to call Neural Network Model Evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]
//Sample values are passed as anonymous type            
var anonymousType = new            
{                
SepalLength = 5.2,                
SepalWidth = 3.3,                
PetalLength = 4.5,               
PetalWidth = 1.2            
};            
string pmmlFilePath = "../../Iris.pmml";            

//Create instance for PMML Document            
PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);            

//Create instance for Mining model            
NeuralNetworkModelEvaluator neuralNetworkModel = new NeuralNetworkModelEvaluator(pmmlDocument);            

//Gets the predicted result            
PredictedResult predictedResult = neuralNetworkModel.GetResult(anonymousType, null);
neuralNetworkModel.Dispose();            

//Displays the predicted result            
Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: VersicolorThe predicted output (Versicolor) obtained from PMML Execution engine provides a clear picture that is based on given sepal and petal width and length, the iris species is more likely to be Versicolor.</td></tr>
{% endhighlight %}


### Clustering Model Evaluator

Clustering Model Evaluator represents the evaluator for clustering models and it inherits all the properties and methods of PMMLEvaluator.

#### Clustering Model

Clustering is the task of grouping a set of objects in such a way that objects in the same group called a cluster are more similar in some sense or another to each other than to those in other groups (clusters). For each cluster a center vector can be provided. In center-based models a cluster is defined by a vector of center coordinates. Some distance measure is used to determine the nearest center that is the nearest cluster for a given input record.

_Table_ _14__: Clustering Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
ClusteringModelEvaluator (PMMLDocument)</td><td>
Creates Instance for ClusteringModel evaluator for the given input PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input against the scoring procedure of the ClusteringModel and returns the PredictedResult. Currently there is no Model options available for Clustering Model.</td></tr>
</table>


Consider the Iris dataset taken from the "datasets" R package. Created here is a Clustering model based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Clustering Model\Iris_ 

The following code example illustrates the procedure to call Clustering Model Evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]
//Sample values are passed as anonymous type            
var anonymousType = new            
{                
SepalLength = 5.4,                
SepalWidth = 3.7,                
PetalLength = 3.5,                
PetalWidth = 1.8            
};            
string pmmlFilePath = "../../Iris.pmml";           

 //Create instance for PMML Document            
 PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);            
 
 //Create instance for Mining model            
 ClusteringModelEvaluator clusteringModel = new ClusteringModelEvaluator (pmmlDocument);            
 
 //Gets the predicted result            
 PredictedResult predictedResult = clusteringModel.GetResult(anonymousType, null);
 clusteringModel.Dispose();           

 //Displays the predicted result            
 Console.WriteLine(predictedResult.PredictedValue);
{% endhighlight %}

{% highlight text %}
Output: 2The predicted output (2) obtained from PMML Execution engine gives us a clear picture that based on given sepal and petal width and length, the iris species is more likely belongs to the Cluster 2.</td></tr>

{% endhighlight %}

### Association Rules Model Evaluator

Association Rules Model Evaluator represents the evaluator for the Association rules model and it inherits all the properties and methods of PMMLEvaluator.

#### Association Rules Model

Association rule mining finds elements or attributes that frequently occur together—for example, products that are often bought together during a shopping session. Such information can be used to recommend products to shoppers, to place frequently bundled items together on store shelves. Machine learning methods for identifying associations among items in transactional data called market basket analysis in retail stores.

_Table_ _9__: Association Rules Model Public Properties or Methods_

<table>
<tr>
<td>
Property/Method</td><td>
Description</td></tr>
<tr>
<td>
AssociationRulesModelEvaluator(PMMLDocument)</td><td>
Creates Instance for Association Rules model evaluator for the given PMML document object.</td></tr>
<tr>
<td>
GetResult(object,IModelOptions)</td><td>
Evaluates the given input items against the scoring procedure of the Association rules model and returns the recommendations, exclusive recommendations and rule associations.</td></tr>
</table>


Consider the Groceries dataset taken from the "arules" R package. An Association rules model is created here, based on it and saved in PMML. PMML files, input dataset and a complete C# sample illustrating this model is shipped with the installer.

#### Samples location:

_%LOCALAPPDATA%\Syncfusion\EssentialStudio\%version%\Common\Analytics\Association Rules\Groceries_

The following code example illustrates the procedure to call Association rules model evaluator directly without the help of PMMLEvaluatorFactory.

{% highlight r %}

[C#]            
//Sample input items is passed as list of string            
List<string> input = null;            
input.Add(“citrus fruit”);            
input.Add(“semi-finished bread”);            
input.Add(“margarine”);            
input.Add(“ready soups”);            
string pmmlFilePath = "../../Groceries.pmml";           

 //Create instance for PMMLDocument            
 PMMLDocument pmmlDocument = new PMMLDocument(pmmlFilePath);            
 
 //Create instance for AssociationRulesModelEvaluator            
 AssociationRulesModelEvaluator associationRules = new AssociationRulesModelEvaluator(pmmlDocument);            
 
 //Gets the predicted result            
 PredictedResult predictedResult = associationRules.GetResult(input, null);            
 associationRules.Dispose();            
 string[] recommendations = predictedResult.GetRecommendations();            
 string[] exclusiveRecommendations = predictedResult.GetExclusiveRecommendations();            
 string[] ruleAssociations = predictedResult.GetRuleAssociations();            
 
 //Displays the recommended results            
 Console.WriteLine(“Recommendations: [” + string.Join(“,”,recommendations) + “]”);            
 Console.WriteLine(“ExclusiveRecommendations: [” + string.Join(“,”,exclusiveRecommendations) + “]”);           
 Console.WriteLine(“RuleAssociations: [” + string.Join(“,”,ruleAssociations) + “]”);
{% endhighlight %}

{% highlight text %}
Output: Recommendations: [whole milk,rolls/buns,other vegetables,yogurt]ExclusiveRecommendations: [whole milk,rolls/buns,other vegetables,yogurt]RuleAssociations: []The Recommendations, ExclusiveRecommendations and RuleAssociations above obtained from PMML Execution engine gives us a clear picture that based on given input items association rules suggests the recommendations that are often bought together during a shopping session.</td></tr>
{% endhighlight %}


## PMMLDocument

The PMMLDocument class is used to represent the object model for PMML document. We can load the input PMML file in both constructor as well as the open methods. Currently all properties provided with the object model are read only and allows us to check the values represented in the input PMML (XML) file.

### Properties and Methods

The PMMLDocument contains properties with read only option. 

_Table_ _15__: PMMLDocument Properties_

<table>
<tr>
<td>
Properties/Methods</td><td>
Description</td></tr>
<tr>
<td>
Header</td><td>
Gets the Header values</td></tr>
<tr>
<td>
DataDictionary</td><td>
Gets the DataDictionary values</td></tr>
<tr>
<td>
SupportVectorMachineModel</td><td>
Gets the SupportVectorMachineModelelements values</td></tr>
<tr>
<td>
TreeModel</td><td>
Gets the TreeModel values</td></tr>
<tr>
<td>
RegressionModel</td><td>
Gets the RegressionModel values</td></tr>
<tr>
<td>
GeneralRegressionModel</td><td>
Gets the GeneralRegressionModel values</td></tr>
<tr>
<td>
NaiveBayesModel</td><td>
Gets the NaiveBayesModel values</td></tr>
<tr>
<td>
MiningModel</td><td>
Gets the MiningModel values</td></tr>
<tr>
<td>
NeuralNetworkModel</td><td>
Gets the NeuralNetworkModel values</td></tr>
<tr>
<td>
ClusteringModel</td><td>
Gets the ClusteringModel values</td></tr>
<tr>
<td>
AssociationRulesModel</td><td>
Gets the AssociationRulesModel values</td></tr>
<tr>
<td>
PMMLDocument</td><td>
Initializes the class PMMLDocument</td></tr>
<tr>
<td>
PMMLDocument(string)</td><td>
Initializes PMMLDocument with string parameter</td></tr>
<tr>
<td>
PMMLDocument(Stream)</td><td>
Initializes PMMLDocument with stream parameter</td></tr>
<tr>
<td>
OpenPMMLDocument(string)</td><td>
Opens the input PMML string file path</td></tr>
<tr>
<td>
OpenPMMLDocument(Stream)</td><td>
Opens the input PMML stream  file path </td></tr>
<tr>
<td>
Dispose()</td><td>
Releases the memory occupied by objects</td></tr>
</table>
The following code example illustrates you on how the PMML file is loaded. Here the input file path is read in string format.

{% highlight r %}

[C#]

//Gets the filepath of the PMML file as String

            string PmmlFilePath = "../../Iris.pmml";

            PMMLDocument pmmlDocument = new PMMLDocument();

            pmmlDocument.OpenPMMLDocument(PmmlFilePath);



            //Filepath directly passed to its instance 

            string PmmlFilePath = "../../Iris.pmml";

            PMMLDocument pmmlDocument = new PMMLDocument(PmmlFilePath);

pmmlDocument.Dispose();
{% endhighlight %}


The following code example illustrates you the same method OpenPMMLDocument but the file path is in stream type.

{% highlight r %}

[C#]

// Gets the Stream filepath of the PMML file 

            string PmmlFilePath = "../../Iris.pmml";

            PMMLDocument pmmlDocument = new PMMLDocument();

            FileStream pmmlStream = File.Open(PmmlFilePath, FileMode.Open, FileAccess.Read);

            pmmlDocument.OpenPMMLDocument(pmmlStream);





            //Filestream directly passed to the Instance of PMMLDocument

            string PmmlFilePath = "../../Iris.pmml";

            FileStream pmmlStream = File.Open(PmmlFilePath, FileMode.Open, FileAccess.Read);

            PMMLDocument pmmlDocument = new PMMLDocument(pmmlStream);

pmmlDocument.Dispose();
{% endhighlight %}

## PredictedResult

The PredictedResult class is used to represent the predicted output for all PMMLEvaluator.

### Properties and Methods

_Table_ _13__: PredictedResult__Properties_

<table>
<tr>
<td>
Properties/Methods</td><td>
Description</td></tr>
<tr>
<td>
PredictedField</td><td>
Gets the Predicted field (Column) name</td></tr>
<tr>
<td>
PredictedDoubleValue</td><td>
Gets the predicted value as double data type. It returns predicted value only when predicting numeric field.</td></tr>
<tr>
<td>
PredictedStringValue</td><td>
Gets the predicted value as string data type. It returns predicted value only when predicting categorical field.</td></tr>
<tr>
<td>
PredictedValue</td><td>
Gets the predicted value. It returns the predicted value as object data type.Note: For association rules model, Object will return string array of exclusive recommended items.</td></tr>
<tr>
<td>
PredictedDataType</td><td>
Gets the predicted data type It’s an enumeration illustrating the return type of predicted value. Return type of predicted data type is one of the following: Double, String or Array</td></tr>
<tr>
<td>
GetPredictedProbability</td><td>
Gets the predicted probability for the input category</td></tr>
<tr>
<td>
GetPredictedProbabilities</td><td>
Get the predicted probabilities for all categories as Key value pair</td></tr>
<tr>
<td>
GetPredictedCategories</td><td>
Gets the predicted categories</td></tr>
<tr>
<td>
GetRecommendations</td><td>
Gets the array of recommended items.(This method should be used only for AssociationRules Model)</td></tr>
<tr>
<td>
GetExclusiveRecommendations</td><td>
Gets the array of exclusive recommended items.(This method should be used only for AssociationRules Model)</td></tr>
<tr>
<td>
GetRuleAssociations</td><td>
Gets the array of Rules associated items.(This method should be used only for AssociationRules Model)</td></tr>
</table>

## Prediction Sensitivity in Binomial Classification

Binary classification is used to classify cases into two categories/groups. You are provided the option to increase or decrease the sensitivity of the binary prediction in the following models.

1. General Regression model
2. Naïve Bayes model
3. Neural Networks model

Using BinomialThreshold property in Model options of General Regression, that is GeneralRegressionOptions, and Naïve Bayes, that is NaiveBayesOptions, and Neural Networks, that is NeuralNetworkOptions you can change the threshold value between 0 to 1 as a threshold to increase or decrease the sensitivity of the binary prediction.

Based on the value, probability (0 - 1) is split into two boundary regions.

_Table_ _16__: Binary Prediction_

<table>
<tr>
<td>
Category 1</td><td>
0 to BinomialThreshold</td></tr>
<tr>
<td>
Category 2</td><td>
BinomialThreshold to 1</td></tr>
</table>
The following code example illustrates the procedure to call General Regression Model Evaluator by passing the binomial threshold value in GeneralRegressionOptions.

{% highlight r %}

[C#]
var iris = new            
{                
Sepal_Length = 6.5, 
Sepal_Width = 2.8, Petal_Length = 4.6,
Petal_Width = 1.5            
};            
var PMMLEvaluator = new PMMLEvaluatorFactory().GetPMMLEvaluatorInstance("../../Model/Iris Logit.pmml");            
GeneralRegressionOptions generalRegressionOptions = new GeneralRegressionOptions();            
generalRegressionOptions.BinomialThreshold = 0.8;            

//Get predicted result with Binomial Threshold = 0.8            
PredictedResult predictedResult = PMMLEvaluator.GetResult(iris, generalRegressionOptions);           
 string[] fieldNames = predictedResult.GetPredictedCategories();            
 for (int i = 0; i < fieldNames.Length; i++)            
 {               
 Console.WriteLine(string.Format("The probability of {0}({1}) is {2}",  fieldNames[i], fieldNames[i] == "0" ? "not versicolor" : "versicolor",
 predictedResult.GetPredictedProbability(fieldNames[i])));     
 }            
 Console.WriteLine(string.Format("The PredictedResult with Binomial threshold value (0.8) is {0} ({1})",               
 predictedResult.PredictedValue, predictedResult.PredictedValue.ToString() == "0" ? "not versicolor" : "versicolor"));           
 generalRegressionOptions.BinomialThreshold = 0.2;            
 
 //Get predicted result with Binomial Threshold = 0.2            
 predictedResult = PMMLEvaluator.GetResult(iris, generalRegressionOptions);            
 Console.WriteLine(string.Format("The PredictedResult with Binomial threshold value (0.2) is {0} ({1})",               
 predictedResult.PredictedValue, predictedResult.PredictedValue.ToString() == "0" ? "not versicolor" : "versicolor"));
{% endhighlight %}

{% highlight text %}
Output:The probability of 0 (not versicolor) is 0.424140583239206The probability of 1 (versicolor) is 0.575859416760794The PredictedResult with Binomial threshold value (0.8) is 0 (not versicolor)The PredictedResult with Binomial threshold value (0.2) is 1 (versicolor)When Threshold value = 0.8Here the probability value of 1 (versicolor) is 0.5758 which is less than our binomial threshold value (0.8) so the predicted result with binomial threshold value 0.8 is 0 (i.e.) not versicolor.When Threshold value = 0.2Here the probability value of 1 (versicolor)  is 0.5758 which is greater than our binomial threshold value (0.2) so the predicted result with binomial threshold value 0.2 is 1 (i.e.) versicolor.</td></tr>
{% endhighlight %}


