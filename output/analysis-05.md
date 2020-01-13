Analysis-05
================
Darren Wang (<hsiangw2@illinois.edu>)
13 January, 2020

------------------------------------------------------------------------

Abstract
========

> Statistical learning methods were applied to images of hand-written digits in order to classify them by thier flattened attributes. Model such as KNN, random forests, boosted models and neural networks were explored and compared based on their accuracy. Out of these models, random forest model produced the best result.

------------------------------------------------------------------------

Introduction
============

In recent years, with the progress of machines' computational power, neural network, deep learning methods, those long established methods nowadays are widely applied to problems such as audiovisual recognition, natural language processing, and image recognition. However, these methods require a lot of computational power, and if the data is large, these methods often require commercial level computational power.

Other than state-of-the-art methods such as deep learning, traditional statistical learning methods such as regression, K-nearest neighbors can be used by individuals with limited computational power to build simple but useful machine learning models. In this analysis, statistical learning methods were utilized to build classification models on a moderate size data set containing hand written digits, in order to recognize written digits. Performance of various models were compared in this analysis.

------------------------------------------------------------------------

Methods
=======

Data
----

The data was originated from the MNIST database [1], it contained images of 60,000 and 10,000 hand writting single digits from 0 to 9. The images, in 28 x 28 pixels, were flattened into 784 scalar. The data was directly downloaded from the MNIST database through download.file function in R, and was unzip utilizing gunzip in R.utils package. The data was further decoded and arrange through two self define function written by the great Doctor David Dalpiaz. Consider my very limited computational power, the data was further sliced into by stratified sampling into a subset containing 1,205 hand written digits, in order to ensure balance between differnt digits.

Modeling
--------

In order to predict which number a given hand written digit is, serveral classification models were explored. Multiclass models, predicting all 10 different digits were considered. Modeling techniques used in this analysis are given as follows:

-   k-nearest neighbors models were considered. The choice of k was chosen using cross-validation.
-   Random forest models were considered. The choice of the number of features chosen at each attempt were chosen using cross-validation.
-   Boosted models(gbm) were considered. The choice of the number of trees and interaction depth were choosen using cross-validation.
-   Neural Network models were considered. The choice of parameters size, decay were choosen using cross-validation.

Evaluation
----------

In order to evaluate the ability of classifiying hand written digits, models were built on the training data subset. To validate these models, the models were fitted with 5 folds cross-validation on the training data. The models were evaluted base on their accuracy of prediction.

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
KNN
</td>
<td style="text-align:right;">
0.881
</td>
</tr>
<tr>
<td style="text-align:left;">
Random Forest
</td>
<td style="text-align:right;">
0.906
</td>
</tr>
<tr>
<td style="text-align:left;">
GBM
</td>
<td style="text-align:right;">
0.861
</td>
</tr>
<tr>
<td style="text-align:left;">
Neural Network
</td>
<td style="text-align:right;">
0.181
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

    ## [1] 0.9081

Discussion
==========

Surprisingly, our best neural networks model showed an accuracy score significantly lower than other models. The other 3 models, random forest with mtry = 39, KNN with 5, gbm model with n.trees = 150, interaction.depth = 3, shrinkage = 0.1 and n.minobsinnode = 10 showed similar well-performed accuracy. This fortified the idea that more complex models may not neccessarily yield better result than simple model, because our least parametrized model - the KNN model, with a training accuracy of 0.8813467, is higher than 0.8614329, from the most complex gbm model.

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Table: Test Results, Random Forest
</caption>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
0
</th>
<th style="text-align:right;">
1
</th>
<th style="text-align:right;">
2
</th>
<th style="text-align:right;">
3
</th>
<th style="text-align:right;">
4
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
8
</th>
<th style="text-align:right;">
9
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
0
</td>
<td style="text-align:right;">
968
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
26
</td>
<td style="text-align:right;">
23
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
12
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1117
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
944
</td>
<td style="text-align:right;">
33
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
38
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
884
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
39
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
19
</td>
<td style="text-align:right;">
11
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
874
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
30
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
11
</td>
<td style="text-align:right;">
37
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
754
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
879
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
7
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
905
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
11
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
8
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
863
</td>
<td style="text-align:right;">
23
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
9
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
58
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
41
</td>
<td style="text-align:right;">
22
</td>
<td style="text-align:right;">
893
</td>
</tr>
</tbody>
</table>
Predicting on testing set, our best model, random forest model with mtry = 39, yielded a testing accuracy of 0.9081, considering the inbalance between trianing and testing set (1205 v.s. 10000), this is pretty convincing accuracy. This phenomenon could be argued by the well-structured nature of hand-written digits(most people write number in a same way), however, looking at the confusion matrix above, interesting facts could be observed. 0, 1, 6 are always classified corretly, 2, 3, 5, 8, 9 are sometimes classified incorrectly. 4 is often classified wrongly as 9. This occured because the similarity between 4 and 9, we could further observe that 8 and 3 are sometimes mixed together and same for 2 and 7. In order to improve classification on these particular digits, more data should be collected and used for training.

------------------------------------------------------------------------

Reference
=========

[1] [THE MNIST DATABASE](http://yann.lecun.com/exdb/mnist/)
