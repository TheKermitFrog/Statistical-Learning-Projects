Analysis-02
================
Darren Wang (<hsiangw2@illinois.edu>)
13 January, 2020

------------------------------------------------------------------------

Abstract
========

> Heart disease is one of the most lethal diseases in North America. Statistical learning techniques are used to determine if it is possible to effectively predict the presence of heart disease from existing data. The model built in this study provides solid prediction on the presence of heart disease. Given the result, the future of predicting the presence of heart disease is promising.

------------------------------------------------------------------------

Introduction
============

Heart is without doubt one of the most important organs in our body, that is why heart disease is one of the most lethal diseases. Heart disease is the leading cause of death for both men and women, every year, about 610,000 people die of heart disease in the United States - that is 1 in every 4 deaths. Among all kind of heart disease, coronary heart disease is the most common type, killing about 366,000 people in 2015. [1]

There are many risk factors for developing heart disease, some of them are controllable factors like smoking, alcohol intake, and various lifestyle habits, some of the risk factors are non-controllable, such as age, sex, familiy history, etc... [2] Even based on these risk factors, it is still hard for us to determine the probability of getting heart disease. Statistical learning methods can help us with calculating the odds of getting heart disease based on existing data. In this analysis, we will be building models on the Cleveland Heart Disease Database, the most popular database for heart disease research. [3]

------------------------------------------------------------------------

Methods
=======

Data
----

The data was accessed via UCI Machine Learning Repository, which was further processed to simplify the analysis. [4] It contained information on 740 patients from 4 different locations. Demographic attributes(age, sex), pathological attributes(serum cholestoral in mg/dl, etc...) as well as numbers of narrowed vessels were included in the dataset.

Modeling
--------

Our goal is to predict the presence of heart disease, therefore, in this analysis we focused on predicting whether a patient has heart disease, but not the exact number of shrunk vessels. In order to predict the presence of heart disease, four modeling techniques were considered: logistic linear models, k-nearest neighbors models, and decision tree models, and random forest models:

-   Logistic linear models with and without ridge, lasso and elastic net penalty terms were considered. Data was scaled and centered before modeling.
-   k-nearest neighbors models with and without scaling training data were considered. Models were trained using all available predictor variables. The choice of k was chosen using cross-validation.
-   Decision tree models were trained using all available predictors. The choice of the complexity parameter was chosen using cross-validation.
-   Random forest models were trained using all available predictors. The choice of the number of features chosen at each attempt were chsoen using cross-validation.

The final model was a random forest model built on top of the predictions of three best models above, that is, an ensemble model with logistic regression.

Evaluation
----------

To evaluate the ability to predict presence of heart disease, the data was split into training, and testing sets, with a 80:20 proportion. In order to validate the models, the models were fitted with 5 folds cross-validation on the training data. Considering the cost of falsely identify patients with heart disease as healthy is much higher than the other way around. (The cost of false negative is much higher than false positive), we might need to change the cutoff point, therefore, it would be more logical to compare models by the AUC to get the general callsification power under different cutoff point, instead of comparing accuracy. Average AUC and graphics for each model are reported using the 5 validation data in the Results section.

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
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Logistic
</td>
<td style="text-align:right;">
0.889
</td>
</tr>
<tr>
<td style="text-align:left;">
Losgistic with Penalty
</td>
<td style="text-align:right;">
0.889
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN
</td>
<td style="text-align:right;">
0.746
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN with Scaling and Centering Data
</td>
<td style="text-align:right;">
0.876
</td>
</tr>
<tr>
<td style="text-align:left;">
Tree
</td>
<td style="text-align:right;">
0.734
</td>
</tr>
<tr>
<td style="text-align:left;">
Random Forest
</td>
<td style="text-align:right;">
0.884
</td>
</tr>
</tbody>
</table>
<img src="Analysis-02_files/figure-markdown_github/graphical-results-1.png" style="display: block; margin: auto;" />

------------------------------------------------------------------------

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
Model
</th>
<th style="text-align:right;">
AUC
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Final Model
</td>
<td style="text-align:right;">
0.996
</td>
</tr>
</tbody>
</table>
<img src="Analysis-02_files/figure-markdown_github/best model AUC plot-1.png" style="display: block; margin: auto;" />

Discussion
==========

By the AUC area, the best three models chosen for ensembling the final model were: - Logistic model with a elastic net peanalty term alpha = 0.1, and lambda = 0.0005 - KNN with scaled and centered data, k = 7 - Rondom Forest with mtry = 2 The ensemble model with these three model has an AUC of 0.996, about 0.1 higher than three best models. It had a testing accuracy of 0.8027211. Overall, the predicting power of the final model was pretty solid. Even with a moderate size of data, the prediction of existing heart disease is reliable. When given bigger data, and more features, the future of prediction on the presence of heart disease with statistical learning methods is promising.

Given the fact that the cost involves in false positve is much lower than false negative, the best action to take next would be tuning cutoff point of our model, trading some overall accuracy with a higher specificity rate. Since we could successfully predict the presence of heart disease with simple statistical learning methods, prediction on the number of shrunk vessels is next to be considered, as it tells more information on the severity than only the presence of heart disease.

The deficiency of this analysis lies within the lack of data, model building on only 740 patients from 3 regions is obviously not going to provide reliable result for patients outside of these three regions. In order to get a more general result, some other possible directions to further this study include increasing the data size, adding features, adding data of patients outside of North America.

------------------------------------------------------------------------

Reference
=========

[1] [Heart Disease Fact Sheet](https://www.cdc.gov/dhdsp/data_statistics/fact_sheets/fs_heart_disease.htm)

[2] [Heart Disease Symptoms and Causes](https://www.mayoclinic.org/diseases-conditions/heart-disease/symptoms-causes/syc-20353118)

[3] [Cleveland Heart Disease Database](https://archive.ics.uci.edu/ml/datasets/Heart+Disease)

[4] [Cleveland Heart Disease Database](https://archive.ics.uci.edu/ml/datasets/Heart+Disease)
