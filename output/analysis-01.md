Brooklyn Airbnb Pricing
================
Darren Wang (<hsiangw2@illinois.edu>)
13 January, 2020

------------------------------------------------------------------------

Abstract
========

Online marketplaces are widely used by people nowadays. Airbnb is a popular online marketplace for arranging or offering lodging, primarily homestays, or tourism experiences. Statistical learning methods are used to determine can the rental prices on Airbnb be effectively predicted.

------------------------------------------------------------------------

Introduction
============

Online marketplaces nowadays play an important role in our everyday life. Successful online marketplaces such as Amazon, and eBay create billions of income every year. Airbnb is one of those successful online marketplaces, which provides a platform for hosts to accommodate guests with short-term lodging and tourism-related activities.

However, how these rental prices are calculated remains a mist to it users. While a house has many attributes that could be used to determine its rental price, there is yet a well recognized standard for rental pricing with these attributes. If we could capture the relationship between a house's attributes and its corresponding rental price, unfair priced items could be identified, thus allowed users of Airbnb to avoid unfair trades.

Statistical learning techniques were applied to Airbnb listings data in New York, NY during 2019. Rental attributes, and location was used to predict rental prices. The results indicate that this prediction can be made with a small amounts of error when predicting most rentals, but gives higher errors predicting the higher rentals. However, limitations of data and the involvment of subjective judgement in retal pricing suggest the need for further investigation.

------------------------------------------------------------------------

Methods
=======

Data
----

The data was accessed via Kaggle. [1] It contains information on Airbnb listings in New York, NY during 2019 including price, rental attributes, and location. For the purposes of this analysis, the data was restricted to short term (one week or less) rentals in Brooklyn that rent for less than $1000 a night. (Additionally, only rentals that have been reviewed are included.)

Modeling
--------

In order to predict the price of rentals, three modeling techniques were considered: linear models, k-nearest neighbors models, and decision tree models.

-   Linear models with and without log transformed responses were considered. Various subsets of predictors, with and without interaction terms were explored.
-   k-nearest neighbors models were trained using all available predictor variables. The choice of k was chosen using a validation set.
-   Decision tree models were trained using all available predictors. The choice of the complexity parameter was chosen using a validation set.

Evaluation
----------

To evaluate the ability to predict rental prices, the data was split into estimation, validation, and testing sets. Error metrics and graphics are reported using the validation data in the Results section.

------------------------------------------------------------------------

Results
=======

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>
Table 1: Validation RMSE for Best Models
</caption>
<thead>
<tr>
<th style="text-align:left;">
Model
</th>
<th style="text-align:right;">
Validation RMSE
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Linear Model with Two-ways Interactions and Backward Selection
</td>
<td style="text-align:right;">
69.49448
</td>
</tr>
<tr>
<td style="text-align:left;">
KNN Model with k = 44
</td>
<td style="text-align:right;">
85.10139
</td>
</tr>
<tr>
<td style="text-align:left;">
Tree Model with cp = 0.001
</td>
<td style="text-align:right;">
72.34499
</td>
</tr>
</tbody>
</table>
<img src="analysis-01_files/figure-markdown_github/graphical-results-1.png" alt="Figure 1: Best Models Actual v.s. Predicted Plots"  />
<p class="caption">
Figure 1: Best Models Actual v.s. Predicted Plots
</p>

------------------------------------------------------------------------

Discussion
==========

By assessing each model's performance on validation data, the best linear, K nearest neighbour and disicion tree model were found out and display in table 1. It is reasonable to choose the linear model with two-way interactions and using backwards selection with AIC as our final model as it provides the lowest validation error. Using the test data, we got a RMSE of 66.1789609. Compared to the range of price in this data, ranging from 10 to 999, this model seems to provide reasonable predictions. Even though the fact that 80% of the price are below 160 would makes the model seems unreliable in terms of prediction, the left most plot in Figure 1 tells that the predictions from this model are precise for majority of the data, but are less precise when predicting higher rentals. Thus, our test RMSE is distorted by failures to predict these high rentals.

Empirically, this problem should be alleviated by performing a log transformation on response variable. In fact, our linear model with log transformation on price, two-way interactions and using backwards selection with AIC gave us validation RMSE of 70.5038415, which is very similar to our chosen model's 69.4944835. Therefore, one possible direction upon improve this model would be transforming response variable.

In this analysis, we restricted our study to only short term rentals in Brooklyn that rent for less than $1000 a night, the nature of this data limited the possible applications of this model. By using data from only short term rentals in Brooklyn in 2019, it is very likely that the model would fail to predict rentals in another city. Considering only rentals in Brooklyn, the nature of this data would possibly lead to unreliable result when predicting rentals in a different year. Moreover, we are ignoring the possibility that there are outliers in this data, this might lead to biased result and our fitted models could be drastically affected by potentail outliers. In order to generalized this model, we should include data from more cities, years, and price range. In addition, to avoid the potential harm brought by outliers, exploratory data analysis should come in place before fitting models to data.

We are trying to capture the relationship between attributes of houses and their rental prices by models, which is often a very complicated relationship and a lot of subjective judgements from human are involved. A possible future study is formulating an effective way of reducing the noise brought by subjective thoughts. In addition, macroeconomics factors such as GDP, average income, unemployment rate, and how people react to these factors should play an important role in determining rental prices and therefore worth investigate.

------------------------------------------------------------------------

Appendix
========

Data Dictionary
---------------

-   `latitude` - latitude coordinates of the listing
-   `longitude` - longitude coordinates of the listing
-   `room_type` - listing space type
-   `price` - price in dollars
-   `minimum_nights` - amount of nights minimum
-   `number_of_reviews` - number of reviews
-   `reviews_per_month` - number of reviews per month
-   `calculated_host_listings_count` - amount of listing per host
-   `availability_365` - number of days when listing is available for booking

For additional background on the data, see the data source on Kaggle.

EDA
---

<img src="analysis-01_files/figure-markdown_github/eda-plots-1.png" style="display: block; margin: auto;" />

<img src="analysis-01_files/figure-markdown_github/price-map-1.png" style="display: block; margin: auto;" />

[1] [New York City Airbnb Open Data](https://www.kaggle.com/dgomonov/new-york-city-airbnb-open-data)
