Analysis-03
================
Darren Wang (<hsiangw2@illinois.edu>)
13 January, 2020

------------------------------------------------------------------------

Abstract
========

> Credit card frauds cost banks a lot of troubles and money. Statistical learning techniques are used to determine if it is possible to effectively identify whether a given transaction is fraudulent from existing data. The model built in this study provides solid prediction on the presence of transaction. Given the result, the future of predicting credit card fraud is promising.

------------------------------------------------------------------------

Introduction
============

Credit card fraud happens everyday, everywhere. According to Wikipedia [1], although incidences of credit card fraud are limited to about 0.1% of all card transactions, they have resulted in huge financial losses as the fraudulent transactions have been large value transactions. In 1999, out of 12 billion transactions made annually, approximately 10 million—or one out of every 1200 transactions—turned out to be fraudulent. Banks today have state-of-the-art methods to prevent frauds from happening, fruad detection is one of them.

Given the amount of transactions data and customer information banks have gathered, statistical learning techniques that built on existing data could help to build tools for identify fraudulent transactions. The goal of this model built with statistical learning techniques would be to predict whether a given transaction is fraudulent based on a numbers of variables that were produced by dimensionality reduction methods, as well as transaction amount and time.

------------------------------------------------------------------------

Methods
=======

Data
----

The data originates from a research collaboration of Worldline and the Machine Learning Group [2] of ULB (Université Libre de Bruxelles) on big data mining and fraud detection. The data was accessed via the magic function provide by professor David Dalpiaz from Kaggle's Credit Card Fraud Detection Competition. [3]

The data contained 284,807 transaction records, each record had 31 attributes, including:

-28 variables denoted by V1 through V28, these variables were produced by unspecified dimensionality reduction technique. -Amount: Transaction amount -Time: Number of seconds elapsed between this transaction and the first transaction in the dataset -Class: 1 for fraudulent transactions, 0 otherwise

The data was further sliced into a subset that contained 50,000 transaction because of my limited computational power, and Time variable was leave out of this analysis.

Modeling
--------

In order to predict whether a transaction is fraudulent, four binary calssification techniques were applied to the data:

-   Logistic model, data was scaled and centered before modeling.
-   k-nearest neighbors models with and without scaling training data were considered. Models were trained using all available predictor variables. The choice of k was chosen using cross-validation.
-   Random forest models were trained using all available predictors. The choice of the number of features chosen at each attempt were chosen using cross-validation.
-   Boosted models(gbm) were trained using all available predictors. The choice of the number of trees and interaction depth were choosen using cross-validation.

### Resampling

Given the fact that our dataset contained only 0.2 percent of fraudulent transactions, such inbalance in the data would lead to the distortion of our models prediction. In order to deal with that problem, SMOTE, a minority oversampling technique were applied to the training data before modeling.

Evaluation
----------

In order to evaluate the ability of predicting whether a given transaction is fraudulent, the data was split into training, and testing sets, with a 80:20 proportion. In order to validate the models, the models were fitted with 10 folds cross-validation on the training data, in each step of cross-validation, the fraudulent transactions in estimation dataset was oversampled by SMOTE. Considering the cost of identifying gunuine transaction as fraudulent is much lower than identifying fraud transactions as gunuine. (The cost of false negative is much higher than false positive), the models were trained to optimized thier sensitivities, in other words, their ability to correctly identify fraudulent transactions as fraudulent. The models were further compared with each other with by considering AUC, sensitivity and Specificity at once. The best model was further evaluate by predicting on test data and going through a custom loss function specified below:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
Actual
</th>
<th style="text-align:left;">
Predicted
</th>
<th style="text-align:left;">
Loss
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Fraud
</td>
<td style="text-align:left;">
Genuine
</td>
<td style="text-align:left;">
0.5 x (Actual Amount)
</td>
</tr>
<tr>
<td style="text-align:left;">
Fraud
</td>
<td style="text-align:left;">
Fraud
</td>
<td style="text-align:left;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
Genuine
</td>
<td style="text-align:left;">
Genuine
</td>
<td style="text-align:left;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
Genuine
</td>
<td style="text-align:left;">
Fraud
</td>
<td style="text-align:left;">
1
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Results
=======

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
Model
</th>
<th style="text-align:right;">
AUC
</th>
<th style="text-align:right;">
Sensitivity
</th>
<th style="text-align:right;">
Specificity
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Logistic
</td>
<td style="text-align:right;">
0.921
</td>
<td style="text-align:right;">
0.841
</td>
<td style="text-align:right;">
0.958
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN
</td>
<td style="text-align:right;">
0.915
</td>
<td style="text-align:right;">
0.778
</td>
<td style="text-align:right;">
0.944
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN with Standardized Data
</td>
<td style="text-align:right;">
0.947
</td>
<td style="text-align:right;">
0.872
</td>
<td style="text-align:right;">
0.954
</td>
</tr>
<tr>
<td style="text-align:left;">
Random Forest
</td>
<td style="text-align:right;">
0.961
</td>
<td style="text-align:right;">
0.842
</td>
<td style="text-align:right;">
0.981
</td>
</tr>
<tr>
<td style="text-align:left;">
Gradient Boosting
</td>
<td style="text-align:right;">
0.962
</td>
<td style="text-align:right;">
0.872
</td>
<td style="text-align:right;">
0.979
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Discussion
==========

By the result table, the random forest model with mtry = 29, and gradient boosting model with n.trees = 50, interaction.depth = 3, shrinkage = 0.1 and n.minobsinnode = 10 shared a AUC value of 0.984, which outperformed the other models. Even though the random forest model had a slightly smaller specificity value comapring to gradient boosting model, its sensitivity is higher. We cared more about the sensitivity in the settings of this analysis therefore the ramdom forest model with mtry = 29 was the best model here. Furthermore, the random forest model produced a 97.5% accuracy on test data, followed by 82% of sensitivity and 97.5% specificity. The average loss per transaction on test data for random forest model was 0 and the maximum loss was 64.

Given the solid numbers on testing data the random forest model produced, the model was not recommend to be put into practice. First, taking the fact that frauds evolve overtime into account, how long will the model keep producing reliable result remains unknown. Moreover, in terms of scalability of the model, the dimensionality reduction technique applied to raw data might cause problems in the future, banks collect new transaction attributes overtime and the same technique might fail to deal with bigger dimensionality of data.

It is certain that this model and analysis could still be improved, first of all, we used only a small subset of data to fit our models, fitting models to a larger dataset could substantailly boost the prediciton. Second, 80% of the transactions in our data had transaction amount under 100, whereas the maximum amount of transaction was 25691, this kind of inbalance in the data could cause our model to have weak predictive power when encounter transactions with higher amount. Last but not the least, the sensitivity of our model could still be improved by trading off some overall accurarcy.

------------------------------------------------------------------------

Reference
=========

[1] [Credit Card Fraud](https://en.wikipedia.org/wiki/Credit_card_fraud)

[2] [Worldline and the Machine Learning Group](http://mlg.ulb.ac.be)

[3] [Credit Card Fraud Detection](https://www.kaggle.com/mlg-ulb/creditcardfraud)
