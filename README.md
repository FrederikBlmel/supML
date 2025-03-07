﻿#  
# Supervised Machine Learning
**University of Lucerne | HS21** 
**by Dr. Massimo Mannino**

### <u> A project by Dominik Walter, Frederik Blümel & Ludwig Kraft


## Overview
1. Introduction and Motivation
2. Data Cleaning
3. Modeling
4. Comparison and Conclusion


## 1 Introduction and Motivation

In the course Supervised Machine Learning we (the authors) have gained valueable insights into various spheres of supervised machine learning (ML). 

In greater detail we first dove into linear and logistic regression models, learned to assess model accuracy and evaluate models based on metrics like Confusion Matrix and its implicated metrics like Precision, Recall and the  ROC curve. We later looked into sampling strategies like cross validation and bootstrapping to handle large datasets more efficiently. Model selection procedures like lasso and ridge regression led us further to the decision tree techniques of bagging, random forests and boosting.

Accompanying the theoretical set-up of the course we enjoyed (and endured) a practical application of those supervised ML models. 

The project aims to train a decent model and predict rent prices of Switzerland's housing market based on a variety of variables.

In this paper we follow along our path of code and display central thoughts of why we proceded as we did. This includes some descriptive statistics, graph interpretations and derived measures of aforementioned. 
Additionally, we briefly explain the techniques we utilized to underline our comprehensive understanding of the models' conceptual baselines. 
Finally, we assess our results and apply our best model to a new dataset to predict the outcome. 

The dataset in question contains 90001 real-life observations of Switzerland's housing market described and structured into 100 different variables. 


## 2 Data Cleaning

The dataset is not clean in its original form and requires a decent amount of cleaning. 

We want to run our model on a holistic set of data and therefore aim towards a reduced dataset with relevant variables, predominantly meaningful observations and entries in every cell.

Obviously, we cannot discuss each and every discussion on which variables to delete and which not due to the given limited extent of this paper. However, we attempt to be as detailed as possible. 

#### <u>War on redundant variables

We need to understand that many of the collected observations have not been filled entirely by the creators of the observations. 
Does an NA in *bath* entail that the flat has no bathroom? There rather is a bathroom, but maybe not three as three bathrooms would have been valuable to mention when uploading housing file.

There exist some extreme cases as the variables *public_transport*, *wheelchair* or *garden_m2* with no entry at all. We delete those variables just like the ones containing less than 500 entries as we do not assess them to add any value to the model. Those 10 include heating options, *middle_house*, *sunny*, *furnished*, *bath* or *bright*. 

Another 34 variables have less than 5000 entries. We delete them as well. They include *shower*, *middle_house*, *oven*, etc.
We assume many of those variables not to be properly filled by the observations' creators. Otherwise a majority of Swiss houses and flats would not have, e.g. a shower.

We also delete the subcategories of *Mirco_rating*, but leave itself. This leaves us with 51 variables. 

#### <u> Topic of Multicollinearity

Two of the variables being highly correlated induces the possibiliy of multicollinearity.
We check for collinearity amongst the remainder variables. 

![enter image description here](https://github.com/kraftl-UL/supML/blob/main/images/corrplot_v1.png?raw=true)

We note several highly correlated variables as a supermarket and restaurant per km2 counter or the month and quarter of the time the observation was created. There even exist variables holding the same information twice as the *sonnenklasse* descriptor. We delete amongst the aforementioned or *avg_bauperiode* and *dist_to_school_1* 20 more variables. Which leaves us with 31. 

Multicollinearity appear not that intensively anymore as before. Strong correlation remains with variables that are obvious as with *rooms* and *area* or variables that will be omitted in the final model as *GDENR* and *msregion*, which are representatives of the observations's location. We also note the high correlation between *area* and the dependent variable *rent_full*.

![Collinearity after deleting redundant variables](https://github.com/kraftl-UL/supML/blob/main/images/corrplot_v2.png?raw=true)

Testing statistically on Multicollinearity we utilize the vif-test test. Variance inflation factor. With a simple linear model we check the 1/vif(model) values not be less than 0.1. This holds. 


Ultimately the following variables remain:

*GDENAMK*, *GDENR*,  *KTKZ*, *area*  , *balcony*,  *elevator*,*floors*,  *home_type*,  *msregion*, *parking_indoor* , *parking_outside*,  *rent_full*, *rooms* , *year_built*,  *Micro_rating*, *wgh_avg_sonnenklasse_per_egid*, *Anteil_auslaend*,  *Avg_age*, *Avg_size_household*, *Noise_max*,  *anteil_efh*, *avg_anzhl_geschosse*, *dist_to_4G*,  *dist_to_5G*, *dist_to_haltst*  , *dist_to_highway* , *dist_to_lake*, *dist_to_main_stat*,  *dist_to_train_stat* , *superm_pix_count_km2*, *dist_to_river*  and *na_count*.  


#### <u> War on NAs 

However, with those 31 variables we still have 18 with NAs. 

Then, we we tailor the way to substitute the missing values for each variable - mostly based on an idea of the variable's distribution (histograms, boxplots, unique values, common sense) .

Tackling sparse observations, we delete these rows as we would create synthetic rows by imputing. Also, we delete rows which contain NAs in area and rooms, since we seek to keep the risk low to be biased, especially in the regarding the variables  of rooms and area.

For discrete values as *balcony*, *elevator*, *parking_indoor*, *parking_outside* we consider an NA to be 0 as it is likely to not type anything when creating an observation, than typing 0 for non-existent paramters.
For missing *floor* values we considered the median. 

From external research we know that there doesn't exist any location in Switzerland with a distance of more than 16km to the next lake. Everything above this distance in our dataset - concerning *dist_to_lake* - is set to NA and later all NAs are replaced by the median, which is ~1206m.

We also assume the average age of people living in a hectare around a certain ovservation can be lower than 18 as before adulthood people cannot rent an appartment or flat. The average of those people in a certain area will be even higher. Same applies for the upper egde of *Avg_age*. So we first reduce the top and bottom outliers by more appropriate values of 25 & 58 and fill the remaining NAs with the (more representative) median value, which is 41.84 years.

Observations with *area* values lower than 6.0m2 were handled as outliers and deleted.

As we deleted the few observations with NAs in both, *area* and *rooms*, we refill *area*  depending on the number of rooms and a average room size computed based on observations with both entires. Vice versa for NAs in *rooms*. 

In *rooms* there were many obviously wrong entries (e.g. 1.6, 2.4, 3.1 or 5.4). We rounded those to the nearest .5 value. 

This was a short overview of methods and cherry-picking of variables how we handled NA substitution. 

We end the cleaning process with a dataset of 88921 observations and 31 variables. 

#### <u> Descriptive statistics

@ Dominik put in some of your screenshots. 






