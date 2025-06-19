# power_outages
Power Outage Data Analysis and Predictive Models

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

|    |   YEAR | start_day   |   start_time | CLIMATE.REGION     |   CUSTOMERS.AFFECTED | CAUSE.CATEGORY     |   IND.PERCEN |   POPPCT_URBAN |   POPDEN_RURAL |   OUTAGE.DURATION |\n|---:|-------:|:------------|-------------:|:-------------------|---------------------:|:-------------------|-------------:|---------------:|---------------:|------------------:|\n|  0 |   2011 | Friday      |      17      | East North Central |                70000 | severe weather     |      32.2024 |          73.27 |           18.2 |              3060 |\n|  1 |   2014 | Sunday      |      18.6333 | East North Central |               124007 | intentional attack |      35.7276 |          73.27 |           18.2 |                 1 |\n|  2 |   2010 | Tuesday     |      20      | East North Central |                70000 | severe weather     |      37.366  |          73.27 |           18.2 |              3000 |\n|  3 |   2012 | Tuesday     |       4.5    | East North Central |                68200 | severe weather     |      34.4393 |          73.27 |           18.2 |              2550 |\n|  4 |   2015 | Saturday    |       2      | East North Central |               250000 | severe weather     |      29.7795 |          73.27 |           18.2 |              1740 |

## UNIVARIATE ANALYSIS

I plotted a historgram of the number of outages with respect to the Industrial power consumption relative to state power. As seen in the plot below, a majority of the outages affect places where the industrial power consumption is around 20-40% of total state consumption. This distribution makes sense because this is in alignment with the proportion of national power consumption by the industiral power sector.

 <iframe
 src="assets/univariate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

## BIVARIATE ANALYSIS

I plotted the distribution of outage durations by climate region to see how outages look by region. I noticed the fourth quartile of the East North Central region was much wider than any of the other regions. This may be due to a combination of the winter months being much harsher in this region as well as a less reliable energy infrastructure. Although not as big as East North Central, the Southwest, Northeast, and West regions also have wide distributions than the remaining regions. Since these three are similar, I can group each set of regions into similar categories based on distribution to simplify my predictive model. 

 <iframe
 src="assets/bivariate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 # Framing a Prediction Problem



 # Baseline Model

 # Final Model

