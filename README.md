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

<iframe
  src="bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Bivariate Analysis

### Analysis of Box Plot: YEAR vs. TOTAL CUSTOMERS
The box plot illustrates how the total number of customers affected by power outages varied across different years. Each box represents the range of values for a specific year, with the length of the box (the interquartile range, or IQR) capturing the middle 50% of the data. The whiskers extend to show the minimum and maximum values, giving a clear picture of how spread out the data is within a year.

The line inside each box represents the median, which shows the typical number of customers affected in that year. Outliers, represented as dots outside the whiskers, point to extreme events where significantly more customers were affected than usual. These outliers often reflect major disruptions or unusual circumstances in those years.

Some years, like 2004, 2012, and 2014, show larger ranges of variability, with wider IQRs. This suggests those years had both small, localized outages and larger, widespread ones. On the other hand, years like 2000, 2010, and 2016 have tighter distributions, indicating more consistent levels of customer impact across events.

Certain years also stand out for their high-impact outliers. For instance, 2002, 2014, and 2008 had significant events that affected a much larger number of customers compared to the typical outages of those years. These spikes likely align with major weather events or infrastructure issues that need further investigation.

This analysis highlights how the severity of power outages changes over time. By focusing on years with greater variability or extreme events, we can dig deeper into the factors—like weather patterns or infrastructure failures—that contribute to significant disruptions.

<iframe
  src="bivariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
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

### NMAR Analysis
The dataset contains missing values in various columns, with one notable candidate for NMAR (Not Missing At Random) being **CUSTOMERS.AFFECTED**. This likely arises due to the nature of data collection and reporting practices. For instance, companies may report outage events without consistently providing details on the number of affected customers, leading to gaps in this field.

To determine if the missingness in **CUSTOMERS.AFFECTED** is MAR (Missing At Random) or NMAR, additional data would be required. Specifically, collecting information on the reporting practices of individual companies could help identify whether the missingness is related to unreported customer impacts during outages.

### Dependency Testing for Missingness
To further explore the missingness patterns in the dataset, we investigated whether the missingness in certain columns is dependent on other features. The analysis included testing **CUSTOMERS.AFFECTED** against categorical variables such as **U.S._STATE** and **MONTH**.

#### Dependency with U.S._STATE
We tested whether the missingness in **CUSTOMERS.AFFECTED** is associated with the **U.S._STATE** column. Using a permutation-based approach, we calculated the observed Total Variation Distance (TVD) and compared it against the null distribution. The results showed an observed TVD of **0.3671** and a p-value of **0.0000**, indicating that the missingness in **CUSTOMERS.AFFECTED** is significantly dependent on **U.S._STATE**. This suggests that certain states might be more likely to report missing customer data.

#### Dependency with MONTH
Similarly, we examined whether the missingness in **DEMAND.LOSS.MW** is dependent on the **MONTH** column. The observed TVD for this test was **0.1021**, with a p-value of **0.0172**, leading us to conclude that the missingness in **DEMAND.LOSS.MW** is also MAR, but less strongly dependent on the **MONTH** variable.

### Imputation Strategy
To address the missing values in **CUSTOMERS.AFFECTED**, we implemented a probabilistic imputation approach. Missing values were filled by sampling from the observed data within the same **U.S._STATE** group. If a group contained entirely missing values, default values based on the global mean or median were used. This strategy ensures that the imputed values are contextually relevant while preserving overall data integrity.

### Conclusion
This assessment highlights the importance of understanding missingness patterns within the dataset. By identifying dependencies and implementing a thoughtful imputation strategy, we ensured that the dataset was ready for analysis while minimizing biases introduced by missing values. Future improvements could involve acquiring more comprehensive data to reduce reliance on imputation.


## Hypothesis Testing 

### Null Hypothesis (H₀)
The mean number of customers affected in 2011 is less than or equal to the mean for other years.

### Alternative Hypothesis (H₁)
The mean number of customers affected in 2011 is greater than the mean for other years.

### Test
We performed a permutation test to determine if the mean number of customers affected in 2011 is significantly greater than the mean for other years.


## Step 5: Framing a Prediction Problem

### Prediction Problem
The prediction problem is to determine the **cause category** of a major power outage based on historical data. This is a **multiclass classification problem**, as there are multiple possible causes (e.g., Weather, Equipment Failure, Human Error, etc.).

---

### Response Variable
The response variable is **CAUSE.CATEGORY**, which categorizes the primary reason for the outage.

**Reason for Choice:**  
This variable is critical for understanding and mitigating future power outages. Predicting the cause helps utility companies allocate resources, improve infrastructure, and plan for disruptions more effectively.

---

### Evaluation Metric
**Primary Metric:** F1-Score

**Reason for Choice:**
- The dataset likely has **imbalanced classes** (e.g., weather-related outages might dominate). Accuracy would not capture performance well in this case, as a model could simply predict the majority class and still achieve high accuracy.
- **F1-Score** balances **precision** and **recall**, ensuring the model performs well across all classes, including minority ones.
- A **weighted F1-Score** will be used to account for the class imbalances while still considering the overall performance.

---

### Type of Prediction and Justification of Features

**Type of Prediction:** Multiclass Classification  

At the time of prediction, we assume the following information is known:
- **Time-Related Features** (e.g., `YEAR`, `MONTH`, `SEASON`)—available at the start of the outage.
- **Geographic Information** (`U.S._STATE`, `NERC.REGION`)—inherent to the location of the outage.
- **Impact Features** (e.g., `TOTAL.CUSTOMERS`, `CUSTOMERS.AFFECTED`)—these are known during or immediately after the outage.
- **Anomaly Levels and Climate Categories**—typically associated with outage reports and can be available from weather systems.

**Note:**  
Features like **OUTAGE.RESTORATION** or exact durations will not be used for prediction, as these are not known at the time the outage occurs.

## Step 6: Baseline Model

### Logistic Regression

---

### Model Description

#### Prediction Problem
The goal of this project is to predict the **cause of a major power outage**. This is a **multiclass classification problem**, meaning the target variable includes several possible causes, such as **Weather, Equipment Failure,** and **Human Error**. Understanding these causes can help utility companies take preventive actions and improve their responses.

---

#### Response Variable
- **Response Variable:** `CAUSE.CATEGORY` (Multiclass Target)
- **Why It Was Chosen:** Identifying the cause of a power outage is essential for improving response times, optimizing resource allocation, and preventing similar outages in the future.

---

#### Features Used
1. **U.S._STATE** (Nominal, One-Hot Encoded)
   - **Why It Was Chosen:** States have unique weather patterns, infrastructure conditions, and risks, all of which can influence the cause of an outage.
   - **Transformation:** One-hot encoding was applied to convert the categorical state information into binary columns that the model can understand.

2. **TOTAL.CUSTOMERS** (Quantitative, Standardized)
   - **Why It Was Chosen:** The number of customers affected may provide insight into the scale of the outage and its likely cause. For example, larger outages might be tied to severe weather, while smaller ones could result from localized equipment issues.
   - **Transformation:** Standardized using `StandardScaler` to ensure that this feature is scaled consistently, preventing it from overpowering other variables during training.

---

### Pipeline Design

1. **Preprocessing:**
   - **One-Hot Encoding:** Converts `U.S._STATE` into binary columns for better representation in the model.
   - **Standardization:** Scales `TOTAL.CUSTOMERS` to have a mean of 0 and a standard deviation of 1 for uniformity.

2. **Model:**
   - **Logistic Regression (Multinomial):** This model was chosen to predict the cause of outages. It's straightforward, interpretable, and a solid choice for establishing a baseline.
   - **Why Logistic Regression?** It’s easy to implement, provides clear insights, and works well as an initial model before exploring more complex approaches.

---

### Performance Metrics
- **Accuracy:** Measures the percentage of correct predictions overall.
- **F1-Score:** A more balanced metric that considers both precision and recall, making it particularly useful for datasets with class imbalances (e.g., Weather-related outages may dominate over other causes).

---

### Summary
- **Problem:** Predicting the cause of a major power outage using multiclass classification.
- **Response Variable:** `CAUSE.CATEGORY`.
- **Features:** The features used include `U.S._STATE` (converted to binary columns) and `TOTAL.CUSTOMERS` (scaled for consistency).
- **Preprocessing Steps:** Applied one-hot encoding and standardization to prepare the data for modeling.
- **Model:** Multinomial Logistic Regression was used as the baseline model.
- **Evaluation Metrics:** Both Accuracy and Weighted F1-Score were selected to assess the model's performance, ensuring fair evaluation even with imbalanced classes.



## Final Model

For our final model, we used a **Random Forest Classifier** and incorporated the following features:  
**U.S._STATE, NERC.REGION, YEAR, TOTAL.CUSTOMERS, CUSTOMERS.AFFECTED**. These features were chosen because they capture geographic, temporal, and impact-related information that directly influence the cause of outages. Using this model, we were able to achieve an F1-Score of **0.702** on the test set, indicating a solid performance for our multiclass classification task.

We included the following transformations in our pipeline:
- **U.S._STATE** and **NERC.REGION** were one-hot encoded to represent categorical data effectively.
- **YEAR** was ordinally encoded to capture its sequential nature.
- **TOTAL.CUSTOMERS** and **CUSTOMERS.AFFECTED** were standardized using a scaler to ensure numerical features were on the same scale.

### Hyperparameter Tuning
To optimize our model, we used GridSearchCV to test a range of hyperparameters and find the best combination for performance. The optimal parameters were:
- `criterion`: `gini`
- `max_depth`: `25`
- `min_samples_split`: `5`
- `n_estimators`: `100`
- `bootstrap`: `True`

These parameters allowed the Random Forest Classifier to balance complexity and generalization, leading to improved results.

### Model Performance
The best cross-validated F1-Score achieved was **0.6827**, and the test set evaluation gave the following metrics:
- **Accuracy:** **0.739**
- **Weighted F1-Score:** **0.702**

Additionally, a confusion matrix revealed that the model performed particularly well for certain categories, such as **Intentional Attack** and **Severe Weather**, while struggling with less frequent causes like **Fuel Supply Emergency**. This reflects the challenges of working with imbalanced datasets.

### Conclusion
The Random Forest Classifier provided a robust framework for predicting outage causes. The use of GridSearchCV ensured that the model was well-optimized, and the inclusion of weighted F1-Score as a metric helped address class imbalances. While the results are promising, future improvements could involve additional feature engineering or trying ensemble methods to further enhance performance.

## Fairness Analysis

For the fairness analysis, we divided the data into two groups based on the total number of customers affected:  
- **Group X:** Areas with total customers affected below or equal to the median.  
- **Group Y:** Areas with total customers affected above the median.

This grouping was chosen because the number of customers impacted can influence the prioritization of resources and responses. By ensuring that the model performs well across both groups, we can better assess its fairness in predicting outage causes for both smaller-scale and larger-scale events.

The primary evaluation metric used for this analysis is the **F1-Score**, as it provides a balanced measure by combining precision and recall. This is particularly important due to the potential imbalance between groups. We calculated the weighted F1-Scores for both groups and used a permutation test to assess whether any observed differences in F1-Scores were statistically significant.

### Hypotheses:
- **Null Hypothesis (H₀):** The model is fair, meaning the F1-Scores for Group X and Group Y are similar, with any differences due to random chance.
- **Alternative Hypothesis (H₁):** The model is not fair, and the F1-Scores for Group X and Group Y differ significantly.

Using 10,000 trials in the permutation test, we determined a p-value of **0.0** at a significance level of **0.05**. This indicates that the differences in F1-Scores are statistically significant. As a result, we reject the null hypothesis and conclude that the model exhibits different levels of performance for areas with fewer customers affected versus those with more customers affected.

This analysis highlights the need to address performance disparities in the model to ensure equitable predictions across varying scales of customer impact.

