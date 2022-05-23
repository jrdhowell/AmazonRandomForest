# Data Science Shipment Acceptance Predictor: Project overview

* Created a tool that predicts whether a shipment will be accepted or rejected in an effort to help the small business reduce losses from rejected shipments.
* Data used is from a public Kaggle data set
* Engineered features from the text of the product descriptions and from the time and date data.
* Optimized a Random Forest to reach the best model

## Code and Resources Used
**R Version:** 4.1.2 (2021-11-01) <br/>
**Packages:** tidyverse, gridExtra, dplyr, lubridate, ggplot2, stringr, randomForest, fastDummies, janitor, caret, AUC, plotrix <br/>
**Data:** https://www.kaggle.com/datasets/pranalibose/amazon-seller-order-status-prediction 

## Kaggle Notebook Walkthrough

https://www.kaggle.com/code/jaredhowell/amazon-seller-data-random-forest 

## Data Cleaning

The data set from Kaggle required cleaning before it could be used. The following steps were taken:

* Removed, corrected and generally tidied variable entries as needed. For example, removed mistaken characters, changed the string date and time entries to the lubridate time and date data types, split the descriptions into 4 seperate columns based on the delimiters
* Corrected some one-off cities and states that were input incorrectly
* Corrected some one-off product descriptions
* Based on descriptive words in the description (ie man, woman, kids, etc), engineered "gender" variable to track targeted demographics
* Based on descriptive words in the description (ie lipstick, jewelry box, purse, etc), engineered "item_type" variable to track type of product
* Created variables to track the time of day and month an order was made

## Immputing missing data

There were null entries in the shipping_fee, item_total and cod variables. For both shipping_fee and item_total, an irritative process was used to determine how to imput the data.
For example, for item_total, if other observations shared the same SKU as the null data, the item_total of the shared SKU was used to imput the missing data. 
Then for the remainder of the missing data, the same process was used for matching descriptions, and finally, the most commone item_total among all the observations was used.

The same process was used for shipping_fee, but other variables were used to find the best fit, including matching cities, states, skus and finally the most common shipping_fee among all the observations.

For cod, the missing data represted when shipments were paid in advance, so the missing data was simply replaced with "Cash in advance"
 
## EDA 

Looked at the distributions of the data. This helped looked at any obvious relationships between the variables.

Highlights:

* About one third of total year sales are in the month of December
* The variables item_type and gender that were created represent the same information

## Modeling

Following the observations from the EDA and a Chi-Square test, the following variables were chosen to create the model:

* order_status
* item_total
* shipping_fee
* ship_city
* item_type
* cod
* month
* tod

The following steps were taken to create the model:

* create dummy variables for catagorical variables
* split the data into a train and test set
* performed upsampling on the data to address the imbalance to the target variable
* tune the model via gridsearch method to find optimal parameters for mtry, maxnodes and ntree count


## Results

The model created has a very good OOB estimate error rate of 1.79%. However, it had a Sepcificity value of 0. 
The model is very good at prediciting shipments that will be accepted, but has trouble predicting when a shipment will be rejected.
The reason for this is the exreme data imbalance in relation to the target variable. More data would be needed to help the model.
Different statistical models may prove beneficial to make a more accurate predicting tool.

According to the model, the item_total and tod (time of day) had the most influence on determining whether a shipment would be rejected or accepted. 
Combinded with the EDA, interestly, it would help reduce rejected shipments to have a mininum order price.
