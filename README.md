# Evaluating Customer Impact of Power Outages

Author: Kaijie Zhang

## Overview

This is the DSC project for DSC80. The project is investigating a dataset about significant power outages in US.

## Introduction

In the project, our dataset is about significant power outages experienced by various states across the continental U.S. In addition to details about these major outages, it provides information on the geographical locations of the outages, regional climate data, land-use features, electricity consumption trends, and the economic profiles of the affected states. The data is included in a single excel file containing 55 variables. 

The dataset is from https://engineering.purdue.edu/LASCI/research-data/outages. The definition reference of the data is from https://www.sciencedirect.com/science/article/pii/S2352340918307182.

During the project, I will have an overall ideas about the dataset and interesting features helping me to Evaluate Customer Impact of Power Outages.

The number of people affected has always been a very important indicator. When we assess the severity of a disaster or plan to compensate people, the number of people affected will determine the direction of decision-making.

These are the columns I selected for used. The original data includes 1534 rows and 57 columns.


| Column                     | Description                                                                                                                                    |
| :------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
| `'YEAR'`                   | Indicates the year when the outage event occurred                                                                                            |
| `'MONTH'`                  | Indicates the month when the outage event occurred                                                                                           |
| `'U.S._STATE'`             | Represents all the states in the continental U.S.                                                                                            |
| `'POSTAL.CODE'`            | Represents the postal code of the U.S. states                                                                                                |
| `'NERC.REGION'`            | The North American Electric Reliability Corporation (NERC) regions involved in the outage event                                              |
| `'CLIMATE.REGION'`         | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.) |
| `'ANOMALY.LEVEL'`          | Represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season                                          |
| `'CLIMATE.CATEGORY'`       | Represents the climate episodes corresponding to the years as "Warm", "Cold", or "Normal"                                                     |
| `'OUTAGE.START.DATE'`      | Indicates the day of the year when the outage event started                                                                                   |
| `'OUTAGE.START.TIME'`      | Indicates the time of the day when the outage event started                                                                                   |
| `'OUTAGE.RESTORATION.DATE'`| Indicates the day of the year when power was restored to all customers                                                                         |
| `'OUTAGE.RESTORATION.TIME'`| Indicates the time of the day when power was restored to all customers                                                                         |
| `'CAUSE.CATEGORY'`         | Categories of all the events causing the major power outages                                                                                 |
| `'HURRICANE.NAMES'`        | If the outage is due to a hurricane, this variable provides the hurricane name                                                                |
| `'OUTAGE.DURATION'`        | Duration of outage events (in minutes)                                                                                                        |
| `'DEMAND.LOSS.MW'`         | Amount of peak demand lost during an outage event (in Megawatt)                                                                               |
| `'CUSTOMERS.AFFECTED'`     | Number of customers affected by the power outage event                                                                                       |


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The columns I kept are in 3 groups:

Group 1: GENERAL INFORMATION: 
'U.S._STATE', 'NERC.REGION',
Group 2: REGIONAL CLIMATE INFORMATION:
'CLIMATE.REGION', 'ANOMALY.LEVEL', 'CLIMATE.CATEGORY',
Group 3:
'OUTAGE EVENTS INFORMATION'
'OUTAGE.START.DATE', 'OUTAGE.START.TIME', 'OUTAGE.RESTORATION.DATE',
'OUTAGE.RESTORATION.TIME', 'CAUSE.CATEGORY',
'HURRICANE.NAMES', 'OUTAGE.DURATION', 'DEMAND.LOSS.MW', 'CUSTOMERS.AFFECTED'.

Then, I combine the start and end time into two columns. But later I realize the datetime itself cannot be the features for later predictions. So I store them respectively into 2*5 features, which generates, for examples, 'OUTAGE.RESTORATION.YEAR', 'OUTAGE.RESTORATION.MONTH', 'OUTAGE.RESTORATION.DAY',
    'OUTAGE.RESTORATION.WEEKDAY', 'OUTAGE.RESTORATION.HOUR'.


### Univariate Analysis

<iframe
  src="Data/f0"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Data/f1"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Data/outage_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>






### Bivariate Analysis

<iframe
  src="Data/f3"
  width="800"
  height="600"
  frameborder="0"
></iframe>


<iframe
  src="Data/f4"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Interesting Aggregates

| IS_HURRICANE | Total_Customers_Affected | Average_Customers_Affected | Event_Count |
| :----------- | -----------------------: | -------------------------: | ----------: |
| False        |             1.29e+08    |                  126804.85 |        1462 |
| True         |             2.72e+07    |                  382673.06 |          72 |

---

| CLIMATE.REGION      | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
| :------------------ | ----------------: | ---------------------: | -----------------: | --------: | ------------: | -------------: | ---------------------------: |
| Central             |          87750.00 |                 0.00  |            110.71  |   9666.67 |          0.00 |       148706.65 |                     210450.00 |
| East North Central  |              0.00 |                 0.00  |            660.11  |      0.00 |       7600.00 |       134972.50 |                     759737.67 |
| Northeast           |          28575.75 |                 0.50  |           1055.58  |      0.00 |      18600.00 |       169466.93 |                     530758.86 |
| Southwest           |          55666.67 |                 0.00  |             327.42 |   35230.00 |          0.00 |        85138.43 |                     135655.57 |
| West                |         198608.14 |                 0.00  |            14060.0 |    5039.19 |          0.00 |       361041.40 |                     152040.45 |
| West North Central  |              0.00 |                 0.00  |               0.00 |       0.00 |      34500.00 |        74178.00 |                          0.00 |




## Assessment of Missingness

### NMAR Analysis

By observing this dataset, the column CUSTOMERS.AFFECTED is likely to have NMAR (Not Missing At Random) data. The reasons for these missing values cannot be inferred from other columns and are likely related to the values themselves. For example, if certain companies did not report the number of customers affected, it would result in missing values. If the number of affected people is small, the government might choose to report it as zero or not report it at all. Conversely, if the number of affected people is very large, the difficulties in accurate statistics might lead to the results being withheld. Additionally, due to the inconsistency in data collection methods, CUSTOMERS.AFFECTED may be based on various data sources. Significant discrepancies in the reported numbers of affected customers across different sources might render the data invalid, resulting in NaN values.

### Missingness Dependency

<iframe
  src="Data/f5"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Data/f6"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Data/f7"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Data/f8"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Hypothesis Testing

<iframe
  src="Data/f9"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem



## Baseline Model




## Final Model



## Fairness Analysis

<iframe
  src="Data/f10"
  width="800"
  height="600"
  frameborder="0"
></iframe>
