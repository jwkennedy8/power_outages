# power_outages
Power Outage Data Analysis and Predictive Models

# Introduction

As the world becomes increasingly dependent on technology for everyday life, the costs associated with technology disruptions will also keep increasing. Power outages affect heating/cooling systems for homes, pause production for many businesses, and places people that rely on electricity for life support at exceptional risk. Giving an accurate estimate on how long an outage may last would have significant benefits for the decision making of affected individuals/businesses.

**In this project, I will determine how related information affects the duration of an outage.** To do this, I will explore a dataset that includes additional information on the circumstances of major outages in the U.S from January 2000 to July 2016. This dataset was introduced in “A Multi-Hazard Approach to Assess Severe Weather-Induced Major Power Outage Risks in the U.S” (Mukherjee et all., 2018). It was collected from the U.S department of Energy. There are 1534 unique outages recorded that include 58 pieces of information related to each one. In order to create a predictive model, I will focus only on information that may be relevant to predict duration. 

## Relevant Columns

| Column Name | Description                      |
|------------|----------------------------------|
| OUTAGE.START.DATE     |  Outage start date (Day,month,year)   |
| OUTAGE.START.TIME      | Outage start time of day (hour,min,sec)  |
| OUTAGE.RESTORATION.DATE     | Restoration Date (Day,month,year)   |
| OUTAGE.RESTORATION.TIME    | Restoration time of day (hour,min,sec)   |
| CAUSE.CATEGORY     | Generic cause of outage (vandalism, weather etc.)  |
| CAUSE.CATEGORY.DETAIL     | Short description of cause  |
| CUSTOMERS.AFFECTED     | Total customers affected by outage   |
| US._STATE     | desc3   |
| IND.PERCEN     | Percentage of Industrial power consumption in state |
| POPPCT_URBAN     | 	Percentage of the total population of the U.S. state represented by the urban population (in %)   |
| POPDEN_RURAL    | Population density of the rural areas (persons per square mile) |