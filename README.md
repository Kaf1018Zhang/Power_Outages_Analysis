# Evaluating Customer Impact of Power Outages

Author: Kaijie Zhang

## Overview

This is the DSC project for DSC80. The project is investigating a dataset about significant power outages in US.

## Introduction

In the project, our dataset is about significant power outages experienced by various states across the continental U.S. In addition to details about these major outages, it provides information on the geographical locations of the outages, regional climate data, land-use features, electricity consumption trends, and the economic profiles of the affected states. The data is included in a single excel file containing 55 variables. 

The dataset is from https://engineering.purdue.edu/LASCI/research-data/outages. The definition reference of the data is from https://www.sciencedirect.com/science/article/pii/S2352340918307182.

---

During the project, we will have an overall ideas about the dataset and interesting features helping me to Evaluate Customer Impact of Power Outages.

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

Enlightly, there should be some values that make no sense if they are 0, like 'OUTAGE.DURATION', 'DEMAND.LOSS.MW'. The case might not be considered if the counted time is 0.

Finally, I tansform the hurricane names ('HURRICANE.NAMES') to whether caused by them ('IS_HURRICANE').

### Univariate Analysis
Let's look over some visualizations about the data.

The below graph shows the frequency of number of customers affected by the Power Outages, which looks strongly rigth skewed. So most numbers are less 0,5M.

<iframe
  src="data/f0.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The below graph shows the frequency of categories of all the events. They are not uniform and the severe weather occurs the most.

<iframe
  src="data/f1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The below graph shows the distribution of people affected throughout the US.

<iframe
  src="data/outage_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

Continue to observe visualizations with more features.

The below graph shows the total customers affected by years. The distribution is symmetric. 2008 is a secial year because the total people affected is about 20M.

<iframe
  src="data/f3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The below graph shows the mean customers affected by causing reasons. This shows that system issue and weather are the most influential reasons.

<iframe
  src="data/f4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Interesting Aggregates

Here are some interesting numbers about the data. 

This is an aggregation on whether the HURRICANE causes the case. We can see the HURRICANE, compare to the general reasons, is more inflential in average.

| IS_HURRICANE | Total_Customers_Affected | Average_Customers_Affected | Event_Count |
| :----------- | -----------------------: | -------------------------: | ----------: |
| False        |             1.29e+08    |                  126804.85 |        1462 |
| True         |             2.72e+07    |                  382673.06 |          72 |

---

This shows few lines of a pivot table. It includes the data distribution cross the causing reasons and climate region of the US. This table could reflect how climate might lead to higher probability of some particular reasons of Power Outages.


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

## Test 1

I observe the missingness of Amount of peak demand lost during an outage event. The possible missingness type of it is Missing At Random (MAR) depending on the causing reasons category. This makes sense. For example, is the power outage is caused by equipment failure. There should be no eletronic devices to record the specific amount of loss during the case. In this case, the Amount of peak demand lost might be biased and lower.

<iframe
  src="data/f5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

With X: Amount of peak demand lost during an outage event; Y: Reason category of power outage

H0: Any observed differences in missingness of X across reason categories Y are due to random chance.

H1: Any observed differences in missingness of X across reason categories Y are not due to random chance.

Test statistic: TVD

Singificant level: 0.05

Accoring to the graph, my idea is somehow reflected ( the Amount of peak demand lost might be biased and lower when the reason is equipment failure). By making permutation test with TVD as test statistic, I got a p-value of 0. So we reject the null hypothesis and conclude that the missingness of Amount of peak demand lost is likely to depend on the reason category of power outage. 

<iframe
  src="data/f6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Test 2

Here is an example checking whether the missingness of number of people affected by the power outage is MAR depending on the Day of restoration. Logically, these two vairables do not share strong relation. The probability of power outage is uniform across the days of a month according to the common sense.

<iframe
  src="data/f7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The visualization shows a chaotic but relatively uniform distribution of propotion. A permutation test with:

With X: Nmber of customers affected; Y: The day of restoration (1 to 31 in general)

H0: Any observed differences in missingness of X across days of restoration Y are due to random chance.

H1: Any observed differences in missingness of X across days of restoration Y are not due to random chance.

Test statistic: TVD

Singificant level: 0.05

is performed. This gives a P-value of 0.456. So I fail to reject the H0 and believe that it is likely that number of people affected by the power outage is NOT MAR depending on the Day of restoration. But it does not mean it is not MAR overall, the missingness of it might depend on, for example, Reason category of power outage again because it is also the recording of objective data during the cases.

<iframe
  src="data/f8.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Hypothesis Testing

Hurricanes are serious natural disasters, so I suspect that power outages caused by hurricanes will be more impactful than ordinary power outages: that is, they affect more people. Therefore, I will use a permutation test to observe the relationship between whether an event is caused by a hurricane and the number of customers affected.

With,

X: Whether power outages caused by hurricanes; Y: Nmber of customers affected

H0: Outages caused by hurricanes affect about same amount of customers on average compared to those not caused by hurricanes.

H1: Outages caused by hurricanes affect significantly more customers on average compared to those not caused by hurricanes.

Test statistic: difference in means of the number of customers affected between power outages caused by hurricanes and those not caused by hurricanes.

Singificant level: 0.05

<iframe
  src="data/f9.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After a permutation test, we recieve a P-value of 0.0002. Therefore, we reject the null hypothesis at the 0.05 significance level. This result suggests that power outages are more likely to be caused by hurricanes affect significantly more customers on average compared to those not caused by hurricanes.

## Framing a Prediction Problem

**Problem Type**: Binary Classification  
**Response Variable**: `'CUSTOMERS.AFFECTED_BINARY'`  
This variable indicates whether the number of customers affected by a power outage exceeds 50,000. This threshold was chosen to identify highly impactful outages, which are of greater interest for disaster response planning and resource allocation.

### Features Used for Prediction

### Numerical Features
- `'OUTAGE.RESTORATION.YEAR'`
- `'OUTAGE.RESTORATION.MONTH'`
- `'OUTAGE.RESTORATION.DAY'`
- `'OUTAGE.RESTORATION.WEEKDAY'`
- `'OUTAGE.RESTORATION.HOUR'`
- `'ANOMALY.LEVEL'`
- `'OUTAGE.DURATION'`
- `'DEMAND.LOSS.MW'`

### Categorical Features
- `'IS_HURRICANE'`
- `'NERC.REGION'`
- `'CLIMATE.REGION'`
- `'CLIMATE.CATEGORY'`
- `'CAUSE.CATEGORY'`

---

### Evaluation Metric

**Metric Used**: F1-Score  
**Reason for Choice**:  
- The dataset is imbalanced, with fewer instances of outages affecting more than 50,000 customers compared to less impactful outages.  
- F1-score balances precision and recall, making it a suitable choice to evaluate model performance in such cases. Accuracy alone would be misleading, as it could be high even if the model fails to predict the minority class correctly.

---
About the reason of choosen features: GENERAL INFORMATION gives an overall idea of the case. For example, some particular state has more people and lead to a higher predicted value. REGIONAL CLIMATE INFORMATION describe how some region with serious climate might lead to higher responded values. OUTAGE EVENTS INFORMATION describes the detailed records of the power outage, helping us to evaluate the seriousness of the case.

## Baseline Model

A basic model with random forest classifer, where,

### Numerical Features (7 - Quantitative)
The following features are continuous or discrete numerical values:
- `'OUTAGE.RESTORATION.YEAR'`
- `'OUTAGE.RESTORATION.MONTH'`
- `'OUTAGE.RESTORATION.DAY'`
- `'OUTAGE.RESTORATION.WEEKDAY'`
- `'OUTAGE.RESTORATION.HOUR'`
- `'ANOMALY.LEVEL'`
- `'OUTAGE.DURATION'`

**Encoding**: These features were scaled using a `StandardScaler` to normalize their values for better model performance.

---

### Categorical Features (3 - Nominal)
The following features represent unordered categories:
- `'NERC.REGION'`
- `'CLIMATE.REGION'`
- `'CLIMATE.CATEGORY'`

Classification Model Evaluation:
Accuracy: 0.76
Precision: 0.80
Recall: 0.85
F1 Score: 0.83

This is acutally a very impressive score. It implies that even with a relatively default model, the features are helpful to predict the numbers of affected people.

## Final Model

A improved model with GridSearchCV and more features. They includes 'DEMAND.LOSS.MW', 'IS_HURRICANE', an 'CAUSE.CATEGORY'. They make sense according to the correlation between them and the numbers of affected people shown in the analysis before.

Best Parameters: {'classifier__class_weight': None, 'classifier__criterion': 'gini', 'classifier__max_depth': 5, 'classifier__min_samples_leaf': 2, 'classifier__min_samples_split': 2}

Best F1 Score: 0.8811759938408972

Final Model Evaluation:
Accuracy: 0.81
Precision: 0.85
Recall: 0.88
F1 Score: 0.86

The F1 score improve about 3% from the baseline model. Notice that the hyper-parameter is changed only slightly from the previous one, with few addings of features could make limted improvement. Overall, the final model perform very well with a F1 score of 0.86.


## Fairness Analysis

In this analysis, I aim to ensure that my model is fair across different groups, particularly for smaller, unbalanced groups. For example, there are fewer than a hundred power outages caused by hurricanes, making this group significantly smaller. Moreover, based on our bivariate analysis, power outages caused by hurricanes tend to affect more people on average than those not caused by hurricanes. This raises the concern that the model might be unfair in such special cases. To address this, I chose Group X to be power outages caused by hurricanes (IS_HURRICANE = 1) and Group Y to be outages not caused by hurricanes (IS_HURRICANE = 0).

Null Hypothesis: The F1 scores for power outages caused by hurricanes and those not caused by hurricanes are the same, and any observed differences are due to random chance.

Alternative Hypothesis: The F1 score for power outages caused by hurricanes is significantly different from the F1 score for outages not caused by hurricanes.

Evaluation Metric:
My evaluation metric is the F1 score, as it accounts for both precision and recall and is well-suited for imbalanced datasets.

Test Statistic:
The test statistic is the absolute difference in F1 scores between the two groups (|F1_hurricane - F1_no_hurricane|).

Significant Level:
The p-value threshold/Significant Level is set at 0.05 rather than 0.01 to make it easier to reject the null hypothesis, ensuring that the fairness of the model is treated more strictly.

Result:
The p-value is 0.15. So we fail to rehject the null hypothesis. I could believe my model is more likely to be fair in its approach to events that were caused by hurricanes and those that were not.


<iframe
  src="data/f10.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
