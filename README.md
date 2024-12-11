# My Project Title

by Suraj Rampure (rampure@ucsd.edu)

***Note***: If you choose a repo name and title as uninspired as the ones here, I will be quite sad.

---

## Introduction

Power outages are a growing concern in the United States, impacting millions of people each year and revealing significant weaknesses in the nation’s infrastructure. These events can disrupt daily routines, create safety risks, and have far-reaching economic consequences. With the increasing frequency of extreme weather and the challenges posed by aging infrastructure, it’s essential to examine the factors driving these outages and the patterns they reveal over time.

In this project, we analyze power outage data across the United States to identify trends in the frequency, causes, and impact of these events. A key focus of our work is determining whether 2022 experienced an unusually high number of outages compared to previous years. By using statistical methods and hypothesis testing, we aim to uncover meaningful insights that can help predict future outages and guide decision-making for energy providers, policymakers, and communities.

---
| Column                     | Description                                                                                   |
|----------------------------|-----------------------------------------------------------------------------------------------|
| `YEAR`                     | Year an outage occurred                                                                       |
| `MONTH`                    | Month an outage occurred                                                                      |
| `U.S._STATE`               | State the outage occurred in                                                                  |
| `NERC.REGION`              | North American Electric Reliability Corporation (NERC) regions involved in the outage event   |
| `CLIMATE.REGION`           | U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions)|
| `ANOMALY.LEVEL`            | Oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season         |
| `OUTAGE.START.DATE`        | Day of the year when the outage event started                                                 |
| `OUTAGE.START.TIME`        | Time of the day when the outage event started                                                 |
| `OUTAGE.RESTORATION.DATE`  | Day of the year when power was restored to all customers                                       |
| `OUTAGE.RESTORATION.TIME`  | Time of the day when power was restored to all customers                                       |
| `CAUSE.CATEGORY`           | Categories of all the events causing the major power outages                                  |
| `CAUSE.CATEGORY.DETAIL`    | Detailed information about the cause category of the outage                                   |
| `OUTAGE.DURATION`          | Duration of outage events (in minutes)                                                        |
| `DEMAND.LOSS.MW`           | Amount of peak demand lost during an outage event (in Megawatts)                              |
| `CUSTOMERS.AFFECTED`       | Total number of customers affected during the outage                                           |
| `RES.CUSTOMERS`            | Number of residential customers affected                                                      |
| `COM.CUSTOMERS`            | Number of commercial customers affected                                                       |
| `IND.CUSTOMERS`            | Number of industrial customers affected                                                       |
| `TOTAL.CUSTOMERS`          | Total number of customers affected                                                            |

## Cleaning and EDA

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>
## Data Cleaning and Exploratory Data Analysis

The first step is to clean the dataset to ensure it is organized and free of inconsistencies, preparing it for analysis.

## Cleaning

1. **Select Relevant Columns**  
   I started by dropping columns that are not relevant to this analysis and keeping only the features essential for understanding power outages. The columns I retained include:  
   `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `CLIMATE.CATEGORY`, `CAUSE.CATEGORY`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`, `TOTAL.CUSTOMERS`.  
   These columns provide information about when and where outages occurred, their causes, durations, and the number of customers affected.

2. **Combine Date and Time Columns**  
   I combined the `OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns into a single datetime column called `OUTAGE.START`. Similarly, I combined `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into a column called `OUTAGE.RESTORATION`. This formatting ensures that temporal information is easy to analyze while preserving important details. Afterward, I dropped the original `DATE` and `TIME` columns to avoid redundancy.

3. **Handle Missing or Incorrect Values**  
   I checked key columns such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` for invalid values. For example, rows where these columns contained `0` were flagged as potentially missing data, since a valid outage cannot have zero duration or customers affected. These values were replaced with `np.nan` to ensure they were handled properly during analysis.

4. **Normalize Population and Urban Data**  
   I combined the columns `POPULATION_URBAN`, `POPULATION_DENSITY_URBAN`, and `AREA_PERCENT_URBAN` into a single column named `URBAN`. This column accounts for the percent of a state’s population in urban areas, population density, and the proportion of the state classified as urban. This allows for better comparisons between states with different urbanization levels.

5. **Validate Data Integrity**  
   Finally, I checked all columns to ensure they had the correct data types. For instance:
   - Numerical columns such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` were converted to `float`.
   - Categorical columns, such as `CAUSE.CATEGORY`, were converted to categorical data types to optimize memory usage and processing speed.
  
## Outage Data

| OBS  | OUTAGE.START.DATE | OUTAGE.START.TIME | OUTAGE.START         | OUTAGE.RESTORATION.DATE | OUTAGE.RESTORATION.TIME | OUTAGE.RESTORATION       |
|------|-------------------|-------------------|-----------------------|-------------------------|-------------------------|--------------------------|
| 1.0  | 2011-07-01        | 17:00:00         | 2011-07-01 17:00:00  | 2011-07-03              | 20:00:00               | 2011-07-03 20:00:00     |
| 2.0  | 2014-05-11        | 18:38:00         | 2014-05-11 18:38:00  | 2014-05-11              | 18:39:00               | 2014-05-11 18:39:00     |
| 3.0  | 2010-10-26        | 20:00:00         | 2010-10-26 20:00:00  | 2010-10-28              | 22:00:00               | 2010-10-28 22:00:00     |
| 4.0  | 2012-06-19        | 04:30:00         | 2012-06-19 04:30:00  | 2012-06-20              | 23:00:00               | 2012-06-20 23:00:00     |
| 5.0  | 2015-07-18        | 02:00:00         | 2015-07-18 02:00:00  | 2015-07-19              | 07:00:00               | 2015-07-19 07:00:00     |


## Exploratory Data Analysis

### Univariate Analysis

**Uneven Distribution:**

The dataset shows that some years, such as 2010, have a noticeably higher number of records compared to others. This could point to either an unusual event occurring in those years or an increase in reporting activity. On the other hand, earlier years like 2000-2004 and later years such as 2016 have fewer records, which might be due to incomplete data collection during those periods.

**Potential Data Bias:**

The uneven distribution of records across years could introduce bias into the analysis or modeling. If the events in heavily represented years are not reflective of broader patterns, the model may overfit to those dominant years. This could affect how well the model performs on less-represented periods or in real-world applications.

**Dataset Coverage:**

The dataset covers the years 2000 to 2016, but the record counts are unevenly distributed. This imbalance may impact the model’s ability to generalize, especially if underrepresented years play a critical role in understanding long-term trends. For example, when using temporal features like `YEAR`, it’s essential to consider the potential impact of this imbalance and take steps to address it, such as applying weighting or resampling methods. Ensuring the dataset aligns with the modeling goals is crucial to prevent overfitting or misrepresenting trends driven by dominant years.

## Bivariate Analysis

### Analysis of Box Plot: YEAR vs. TOTAL CUSTOMERS
The box plot illustrates how the total number of customers affected by power outages varied across different years. Each box represents the range of values for a specific year, with the length of the box (the interquartile range, or IQR) capturing the middle 50% of the data. The whiskers extend to show the minimum and maximum values, giving a clear picture of how spread out the data is within a year.

The line inside each box represents the median, which shows the typical number of customers affected in that year. Outliers, represented as dots outside the whiskers, point to extreme events where significantly more customers were affected than usual. These outliers often reflect major disruptions or unusual circumstances in those years.

Some years, like 2004, 2012, and 2014, show larger ranges of variability, with wider IQRs. This suggests those years had both small, localized outages and larger, widespread ones. On the other hand, years like 2000, 2010, and 2016 have tighter distributions, indicating more consistent levels of customer impact across events.

Certain years also stand out for their high-impact outliers. For instance, 2002, 2014, and 2008 had significant events that affected a much larger number of customers compared to the typical outages of those years. These spikes likely align with major weather events or infrastructure issues that need further investigation.

This analysis highlights how the severity of power outages changes over time. By focusing on years with greater variability or extreme events, we can dig deeper into the factors—like weather patterns or infrastructure failures—that contribute to significant disruptions.

---

## Grouping and Aggregates 

## Cause Category by Year

| YEAR    | Equipment Failure | Fuel Supply Emergency | Intentional Attack | Islanding | Public Appeal | Severe Weather | System Operability Disruption |
|---------|-------------------|-----------------------|--------------------|-----------|---------------|----------------|-------------------------------|
| 2000.0  | 218273.0          | 0.0                   | 0.0                | 0.0       | 0.0           | 1420308.0      | 2632000.0                    |
| 2001.0  | 0.0               | 0.0                   | 0.0                | 0.0       | 0.0           | 36073.0        | 1395338.0                    |
| 2002.0  | 0.0               | 0.0                   | 0.0                | 0.0       | 0.0           | 6102586.0      | 280000.0                     |
| 2003.0  | 325531.0          | 0.0                   | 0.0                | 0.0       | 0.0           | 5399727.0      | 6737850.0                    |
| 2004.0  | 354124.0          | 0.0                   | 0.0                | 0.0       | 55800.0       | 13018174.0     | 164458.0                     |
| 2005.0  | 951500.0          | 0.0                   | 0.0                | 0.0       | 0.0           | 11929634.0     | 670950.0                     |
| 2006.0  | 0.0               | 0.0                   | 0.0                | 0.0       | 8000.0        | 9166910.0      | 977182.0                     |
| 2007.0  | 0.0               | 0.0                   | 0.0                | 9600.0    | 0.0           | 5735133.0      | 228700.0                     |
| 2008.0  | 616980.0          | 0.0                   | 0.0                | 40046.0   | 8000.0        | 18100814.0     | 1199086.0                    |
| 2009.0  | 256301.0          | 0.0                   | 0.0                | 70.0      | 0.0           | 6256406.0      | 300700.0                     |
| 2010.0  | 62303.0           | 0.0                   | 0.0                | 41117.0   | 32759.0       | 9613738.0      | 354572.0                     |
| 2011.0  | 11000.0           | 0.0                   | 4920.0             | 19814.0   | 47500.0       | 14665579.0     | 1689856.0                    |
| 2012.0  | 0.0               | 0.0                   | 0.0                | 128314.0  | 2163.0        | 12484131.0     | 81430.0                      |
| 2013.0  | 262055.0          | 1.0                   | 0.0                | 12802.0   | 90874.0       | 6469781.0      | 182885.0                     |
| 2014.0  | 0.0               | 0.0                   | 0.0                | 0.0       | 0.0           | 7994197.0      | 28000.0                      |
| 2015.0  | 0.0               | 0.0                   | 0.0                | 47066.0   | 1765.0        | 5296731.0      | 283649.0                     |
| 2016.0  | 0.0               | 0.0                   | 0.0                | 163213.0  | 4300.0        | 1514571.0      | 311824.0                     |

## Insights from the Pivot Table

This pivot table aggregates the total number of customers affected by power outages each year for various cause categories. Below are the key insights:

### 1. Dominant Cause Categories
The largest values in each row reveal which cause category impacted the most customers in a given year. For instance:
- **Severe Weather** consistently stands out as the primary cause, with the highest impact in most years. This highlights its significant role in driving outages.

### 2. Trends Over Time
The table shows how the number of customers affected changes year to year for each cause. For example:
- **Severe Weather** affects customers consistently but has notable spikes in certain years, such as 2008 and 2012, which likely correspond to major events like hurricanes or severe storms.
- Less frequent causes, like **Fuel Supply Emergency** and **Intentional Attack**, rarely appear, indicating they are uncommon contributors to outages.

### 3. Emerging Patterns
Some categories, such as **System Operability Disruption**, show significant variation across years. This could suggest isolated, major incidents that caused large impacts during specific periods. In contrast, categories like **Public Appeal** and **Islanding** consistently have minimal impact, affecting very few customers overall.

### 4. High-Impact Years
Certain years, such as 2008 and 2012, stand out for their overall high customer impact across all cause categories. These years may align with severe events like hurricanes, ice storms, or other extreme weather conditions that significantly disrupted power.

### 5. Gaps or Missing Data
Many rows show `0` values for certain cause categories, which could indicate missing data or simply that those causes were not factors in those years. Understanding whether these are true gaps or the absence of events is crucial for accurate interpretation.



---

### Applications of These Insights

- **Resource Allocation:** Since **Severe Weather** is the most dominant cause, resources should prioritize weatherproofing infrastructure and improving resilience to extreme weather conditions.
- **Policy Planning:** Spikes in specific causes during certain years can guide strategic planning for future preparedness and targeted investments.
- **Further Analysis:** Trends in the table could be compared with external factors like climate anomalies, policy changes, or regional infrastructure improvements to uncover additional insights.

This table is a valuable tool for identifying patterns, understanding causes, and prioritizing focus areas to mitigate the impacts of power outages effectively.


## Assessment of Missingness



## Hypothesis Testing 

### Null Hypothesis (H₀)
The mean number of customers affected in 2011 is less than or equal to the mean for other years.

### Alternative Hypothesis (H₁)
The mean number of customers affected in 2011 is greater than the mean for other years.

### Test
We performed a permutation test to determine if the mean number of customers affected in 2011 is significantly greater than the mean for other years.



