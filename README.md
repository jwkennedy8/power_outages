# How long will your Power Be Out?
Power Outage Data Analysis and Predictive Models

Created by: Jack Kennedy

# Table of Contents

- [Introduction](#introduction)

    -[Relevant Columns](#relevant-columns)
- [Data Cleaning and Exploratory Data Analysis](#data-cleaning-and-exploratory-data-analysis)

    -[Data Cleaning](#data-cleaning)

    -[Univariate Analysis](#univariate-analysis)

    -[Bivariate Analysis](#bivariate-analysis)

    -[Interesting Aggregate](#interesting-aggregate)

- [Framing a Prediction Problem](#framing-a-prediction-problem)
- [Baseline Model](#baseline-model)
- [Final Model](#final-model)

# Introduction

As the world becomes increasingly dependent on technology for everyday life, the costs associated with technology disruptions will also keep increasing. Power outages affect heating/cooling systems for homes, pause production for many businesses, and places people that rely on electricity for life support at exceptional risk. Giving an accurate estimate on how long an outage may last would have significant benefits for the decision making of affected individuals/businesses.

**In this project, I will determine how related information affects the duration of an outage.** To do this, I will explore a dataset that includes additional information on the circumstances of major outages in the U.S from January 2000 to July 2016. This dataset was introduced in “A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S” (Mukherjee et all., 2018). It was collected from the U.S department of Energy. There are 1534 unique outages recorded that include 58 pieces of information related to each one. In order to create a predictive model, I will focus only on information that may be relevant to predict duration. 

## Relevant Columns

| Column Name | Description                      |
|------------|----------------------------------|
| OUTAGE.START.DATE     |  Outage start date (YY-MM-DD)   |
| OUTAGE.START.TIME      | Outage start time of day (hour,min,sec)  |
| OUTAGE.RESTORATION.DATE     | Restoration Date (YY-MM-DD)   |
| OUTAGE.RESTORATION.TIME    | Restoration time of day (hour,min,sec)   |
| CAUSE.CATEGORY     | Generic cause of outage (vandalism, weather etc.)  |
| CUSTOMERS.AFFECTED     | Total customers affected by outage   |
| CLIMATE.REGION     | U.S. Climate regions as specified by National Centers for Environmental Information  |
| US._STATE     | US State  |
| IND.PERCEN     | Percentage of Industrial power consumption in state |
| POPPCT_URBAN     | 	Percentage of the total population of the U.S. state represented by the urban population (in %)   |
| POPDEN_RURAL    | Population density of the rural areas (persons per square mile) |

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

Before starting my analysis, I needed to transform a few of the relevant features to datatypes suitable for analysis/regression. First, I found that 58 observations were missing the outage restoration dates and also could not be determined by duration because their corresponding durations were null. I removed all cases where duration could not be determined because that is the feature I am trying to predict. Next, I transformed **OUTAGE.START.TIME** into a new column **OUTAGE.START.HOUR** which is 24 hour format rounded to the nearest full hour. I wanted to explore the relationship between day of the week and duration as well so I derived the day of week from **OUTAGE.START.DATE** to create **start_day**.

The only missing values for **POPDEN_RURAL** where outages in Washington D.C. Since there is no rural population there I filled in 0 for each of these observations. Similiarly, the only missing values for **CLIMATE.REGION** were outages located in Hawaii. Since all cases in Hawaii were missing a region, I filled in 'hawaii' for missing **CLIMATE.REGION**. 

The last relevant columns that had missing values were **CUSTOMERS.AFFECTED** and **IND.PERCEN**. For these columns I used mean value imputation by state.

|   YEAR | start_day   |   start_time | CLIMATE.REGION     |   CUSTOMERS.AFFECTED | CAUSE.CATEGORY     |   IND.PERCEN |   POPPCT_URBAN |   POPDEN_RURAL |   OUTAGE.DURATION |
|-------:|:------------|-------------:|:-------------------|---------------------:|:-------------------|-------------:|---------------:|---------------:|------------------:|
|   2011 | Friday      |      17      | East North Central |                70000 | severe weather     |      32.2024 |          73.27 |           18.2 |              3060 |
|   2014 | Sunday      |      18.6333 | East North Central |               124007 | intentional attack |      35.7276 |          73.27 |           18.2 |                 1 |
|   2010 | Tuesday     |      20      | East North Central |                70000 | severe weather     |      37.366  |          73.27 |           18.2 |              3000 |
|   2012 | Tuesday     |       4.5    | East North Central |                68200 | severe weather     |      34.4393 |          73.27 |           18.2 |              2550 |
|   2015 | Saturday    |       2      | East North Central |               250000 | severe weather     |      29.7795 |          73.27 |           18.2 |              1740 |

## UNIVARIATE ANALYSIS

I plotted a historgram of the number of outages with respect to the Industrial power consumption relative to state power. As seen in the plot below, a majority of the outages affect places where the industrial power consumption is around 20-40% of total state consumption. This distribution makes sense because this is in alignment with the proportion of national power consumption by the industiral power sector.

 <iframe
 src="assets/univariate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 
## BIVARIATE ANALYSIS

I also plotted a histogram for the mean customers affected by state which revealed that there were no values for **CUSTOMERS.AFFECTED** in the initial dataset for Montana and South Dakota. This was problematic at first because I had originally used mean value imputation by state for all **CUSTOMERS.AFFECTED**. I used the mean for North Dakota (34.5k) as a value to impute for both states due to similar population sizes and location.

 <iframe
 src="assets/by_state_hist.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Next, I plotted the distribution of outage durations by climate region to see how outages look by region. I noticed the fourth quartile of the East North Central region was much wider than any of the other regions. This may be due to a combination of the winter months being much harsher in this region as well as a less reliable energy infrastructure. Although not as big as East North Central, the Southwest, Northeast, and West regions also have wide distributions than the remaining regions. Since these three are similar, I can group each set of regions into similar categories based on distribution to simplify my predictive model. 

 <iframe
 src="assets/bivariate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


## INTERESTING AGGREGATE


 The table below shows the occurances of each type of outage by climate region. Across all regions, we can see severe weather and intentional attacks account for most of the outages. The West region stands out for **equipment failure**,**islanding**, and **system operability disruption** having way more for these categories than all other regions. 

 | CAUSE.CATEGORY                |   Central |   East North Central |   Northeast |   Northwest |   South |   Southeast |   Southwest |   West |   West North Central |   hawaii |
|:------------------------------|----------:|---------------------:|------------:|------------:|--------:|------------:|------------:|-------:|---------------------:|---------:|
| equipment failure             |         5 |                    3 |           5 |           2 |       9 |           4 |           5 |     21 |                    1 |      nan |
| fuel supply emergency         |         4 |                    4 |          14 |           1 |       4 |         nan |           1 |     10 |                  nan |      nan |
| intentional attack            |        34 |                   20 |         131 |          85 |      28 |           9 |          61 |     31 |                    4 |      nan |
| islanding                     |         3 |                    1 |           1 |           3 |       2 |         nan |           1 |     28 |                    5 |      nan |
| public appeal                 |         2 |                    2 |           4 |           2 |      42 |           5 |           1 |      9 |                    2 |      nan |
| severe weather                |       133 |                  104 |         175 |          25 |     106 |         116 |          10 |     67 |                    4 |        4 |
| system operability disruption |        10 |                    3 |          14 |           4 |      27 |          16 |           9 |     39 |                  nan |        1 |

# Framing a Prediction Problem

From the data collected, I am creating a regression problem to predict power outage duration based contextual data. The relevant columns that I have selected would be features known at the time of the outage which makes this appropriate for prediciton. I chose to predict duration because it can be a very useful estimate for a supplier to accurately inform customers of the outage duration. As stated earlier, the duration of an outage can significantly affect consumer decision making. To evaluate my models, I will use the **mean squared error** to capture the error because I want my model to be sensitive to large errors as longer outages are the most consequential.

# Baseline Model

To get started, I built a linear regression model using **CLIMATE.REGION**, **CAUSE.CATEGORY**, and **CUSTOMERS.AFFECTED**.
This model used the **sklearn LinearRegression** module. I one hot encoded **CLIMATE.REGION** and  **CAUSE.CATEGORY** and used an 80-20 train-test split. The training MSE was **18390775.11776536 minutes^2** and the test MSE was **73532246.3912735 minutes^2**

These results mean that this models predictions on unseen data are **~8575 minutes** off on average which is around 6 days! Given the fact that the data has a mean outage duration of **~2625 minutes**, this model performs poor on both seen and unseen data.

# Final Model

To improve my model, I include more features that I explored earlier such as **IND.PERCEN** and a custom feature **start_hour** which was derived from **OUTAGE.START.TIME**. Since my objective is to reduce error on unseen data in particular, I chose to use Ridge Regression with a range of regularization penalties along with turning **IND.PERCEN**, **CUSTOMERS.AFFECTED**, and **YEAR** into polynomial features. I also transformed **CLIMATE.REGION** into three categories to simplify the encoding since the box plots from earlier showed that many of the distributions looked similiar by region. Each region was classified into 'high', 'med', or 'low' risk of having a extended outage based on the distribution of the region.

To implement the model, I used sklearns **GridSearchCV** module to search for the best parameters for the regularization penalties and polynomial feature degrees. I created function transformers for **start_hour** and **CLIMATE.REGION** so that the model can make predictions based on the same features in the original dataset. 

The fit model had the following results: 

| Dataset | Minutes^2                      |
|------------|----------------------------------|
| Training     |  17621599.81673342  |
| Test     | 70651184.39905676  |

Although the model showed improvement from the baseline, it only predicted **~170 minutes** closer on average for the test dataset. For future improvement, I would recommend investigating energy suppliers by area of outage to supplement this dataset as well as energy infrastucture data such as type of equipment used, and how recently it was updated. 




