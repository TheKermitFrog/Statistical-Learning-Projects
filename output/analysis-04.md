Analysis-04
================
Darren Wang (<hsiangw2@illinois.edu>)
13 January, 2020

------------------------------------------------------------------------

Abstract
========

> Statistical learning methods were applied to wine data in order to classify wine samples by their quality. Parametric model such as logistic regression and non-parametric models such as KNN, random forests were explored. None of the models provided predictions accurate enough to suggest its usage in real world. More data collection should be made to imrpove the study.

------------------------------------------------------------------------

Introduction
============

Studies show that drinking wine is benefiacial to individual's health in a variety of ways [1] when consume moderately, benefits includes reducing inflammation, maintaining heart health, etc... Nowadays, wine drinking much more than an activitiy to do in leisure, it's an representation of higher social class, and a form of art. However, as popular as drinking wine being, the method of determining qualities of wines remians the old-fashioned way - by the taste of wine experts. For wine brewers, quality of wine directly affect their price and sales. Relying fully on wine expert's subjective taste to determine wine's quality is very risky.

As technologies keep on progress, there are more and more attributes such as pH value, and amount of chemicals in wine can be percisely measured. The goal of this analysis is to predict the quality of wine based on its attributes. Statistical learning technique would be used to build models on existing data and make poredictions.

------------------------------------------------------------------------

Methods
=======

Data
----

The data originates from the UCI machine learning repository [2]. 6,497 red and white vinho verde wine samples, from the north of Portugal were included in the dataset. The data was accessed directly in R from source via the ucidata package on Github.

The data contained 13 varaibles including chemical attributes such as pH value, density. Amount of some matters that potentially affect the taste such as sulphates, sulfur dioxide, citric acid were also included. There was also a categorical variable indicating wheter a sample is red or white wine.

Theoretically, each sample of wine was evluated by quality measure ranging from 0 to 10. However, none of the wine sample in our data has quality 0, 1, 2, and 10. Moreover, there were only 30(0.5%) samples in quality level 3 and 9(0.1%) samples in quality level 9. Even adjusted with resampling methods, these extreme imbalanced was likley to distort our model. Therefore, I grouped quality level 9 and 8 samples together and grouped quality level 3 samples and 4 samples together.

Modeling
--------

In order to predict the quality of wine, serveral classification stategies were explored. Both multiclass models, using all 7 quality levels was considered. Modeling techniques used in this analysis are given as follows:

-   Multiclass logistic regression model through `nnet` package was considered. Data was scaled and centered before modeling.
-   k-nearest neighbors models with and without standardizing training data were considered. Models were trained using all available predictor variables. The choice of k was chosen using cross-validation.
-   Random forest models were trained using all available predictors. The choice of the number of features chosen at each attempt were chosen using cross-validation.
-   Boosted models(gbm) were trained using all available predictors. The choice of the number of trees and interaction depth were choosen using cross-validation.

### Resampling

As shown below, even after the data was regrouped, the numbers of samples in each quality level in our dataset was extremely imbalanced, such imbalance in the data would lead to the distortion of our models prediction. In order to deal with that potential problem, SMOTE, a minority oversampling technique was applied to the training data before multiclass model training. Models trained with and without resampling were considered and evaluated.

<img src="analysis-04_files/figure-markdown_github/quality hist-1.png" style="display: block; margin: auto;" />

    ## Loading required package: grid

Evaluation
----------

In order to evaluate the ability of predicting wine quality, the data was split into training, and testing sets, with a 80/20 proportion. To validate these models, the models were fitted with 5 folds cross-validation on the training data with and without SMOTE resampling. If modeled with SMOTE resampling, the minority quality levels in estimation dataset was oversampled by SMOTE in each step of cross-validation. The models were evaluted base on their accuracy of prediction.

------------------------------------------------------------------------

Results
=======

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Table: Accuracy of Models
</caption>
<thead>
<tr>
<th style="text-align:left;">
Model
</th>
<th style="text-align:right;">
Accuracy
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Logistic
</td>
<td style="text-align:right;">
0.547
</td>
</tr>
<tr>
<td style="text-align:left;">
Losgistic with SMOTE
</td>
<td style="text-align:right;">
0.355
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN
</td>
<td style="text-align:right;">
0.462
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN with SMOTE
</td>
<td style="text-align:right;">
0.262
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN with standardization
</td>
<td style="text-align:right;">
0.549
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN with standardization and SMOTE
</td>
<td style="text-align:right;">
0.364
</td>
</tr>
<tr>
<td style="text-align:left;">
Random Forest
</td>
<td style="text-align:right;">
0.667
</td>
</tr>
<tr>
<td style="text-align:left;">
Random Forest with SMOTE
</td>
<td style="text-align:right;">
0.474
</td>
</tr>
<tr>
<td style="text-align:left;">
GBM
</td>
<td style="text-align:right;">
0.589
</td>
</tr>
<tr>
<td style="text-align:left;">
GBM with SMOTE
</td>
<td style="text-align:right;">
0.430
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Discussion
==========

Surprisingly, models with SMOTE resampling within each fold of cross-validation were outperformed by models without resampling. I would claim that is because the extremely imbalance between each of the quality levels, and thus the lack of diversity within the vairbles of the minority quality levels.

Among all the models, random forest model with mtry = 2 clearly outperformed the others, giving a 68.2% overall accuracy. The model was further tested by making prediction on the test data set, giving a 68.2% of accuracy and confusion matrix as followed:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Table: Test Results, Random Forest
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
4 and under
</th>
<th style="text-align:right;">
5
</th>
<th style="text-align:right;">
6
</th>
<th style="text-align:right;">
7
</th>
<th style="text-align:right;">
over 8
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
4 and under
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
5
</td>
<td style="text-align:right;">
27
</td>
<td style="text-align:right;">
302
</td>
<td style="text-align:right;">
98
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
6
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
112
</td>
<td style="text-align:right;">
468
</td>
<td style="text-align:right;">
90
</td>
<td style="text-align:right;">
14
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
7
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
34
</td>
<td style="text-align:right;">
92
</td>
<td style="text-align:right;">
11
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
over 8
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
15
</td>
</tr>
</tbody>
</table>
As shown in the confusion matrix, despite the somewhat reliable predictions for quality 5, 6, and 7, however, it had a hard time identifying quality 4 and under as well as quality over 8. In fact, for 68 samples in the test data set that had quality 4 and under, only 4 were correctly classified as 4 and under, that's about 6% of accuracy. Therefore, I concluded that this model was not usefel, as it failed to produce reliable prediction.

There were other concerns about this model. First of all, the extremely imbalance between the overall quality should be addressed in order to improve the model, 9 and 30 samples within quality 4 and 9, would cause problems in prediction even with oversampling method(we were overfitting in those minority qualities). Moreover, there were no data from quality 3 and under and quality 10, which mean that our model would never predice a sample has quality below 3 or a perfect quality. Therefore, more data should be collected to improve the model.

Second, the data we used for modeling were all vinho verde wine samples from the north of Portugal, which was not generalized enough to produce reliable result when encounter wine from outside of Portugal, or wine that is not vinho verde. This further implies that we need to collect more data in order to generalized the prediction of our model.

------------------------------------------------------------------------

Reference
=========

[1] [Health Effects of Wine](https://en.wikipedia.org/wiki/Health_effects_of_wine)

[2] [Wine Quality Dataset](http://archive.ics.uci.edu/ml/datasets/Wine+Quality)
