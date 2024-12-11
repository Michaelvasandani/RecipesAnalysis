## Analysis of Power Outages in the United States 

DSC 80 Project 
By Michael Vasandani & Omar Ali

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

## Cleaning

1. **Select Relevant Columns**  
   I started by dropping columns that are not relevant to this analysis and keeping only the features essential for understanding power outages. The columns I retained include: `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `CLIMATE.REGION`, `ANOMALY.LEVEL`, `OUTAGE.START.DATE`, `OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`, `CAUSE.CATEGORY`, `CAUSE.CATEGORY.DETAIL`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`, `RES.CUSTOMERS`, `COM.CUSTOMERS`, `IND.CUSTOMERS`, `TOTAL.CUSTOMERS`. These columns provide information about when and where outages occurred, their causes, durations, and the number of customers affected.

3. **Combine Date and Time Columns**  
   I combined the `OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns into a single datetime column called `OUTAGE.START`. Similarly, I combined `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into a column called `OUTAGE.RESTORATION`. This formatting ensures that temporal information is easy to analyze while preserving important details. Afterward, I dropped the original `DATE` and `TIME` columns to avoid redundancy.

4. **Handle Missing or Incorrect Values**  
   I checked key columns such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` for invalid values. For example, rows where these columns contained `0` were flagged as potentially missing data, since a valid outage cannot have zero duration or customers affected. These values were replaced with `np.nan` to ensure they were handled properly during analysis.

5. **Normalize Population and Urban Data**  
   I combined the columns `POPULATION_URBAN`, `POPULATION_DENSITY_URBAN`, and `AREA_PERCENT_URBAN` into a single column named `URBAN`. This column accounts for the percent of a state’s population in urban areas, population density, and the proportion of the state classified as urban. This allows for better comparisons between states with different urbanization levels.

  
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
<iframe
  src="univariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Uneven Distribution:**

The dataset shows that some years, such as 2010, have a noticeably higher number of records compared to others. This could point to either an unusual event occurring in those years or an increase in reporting activity. On the other hand, earlier years like 2000-2004 and later years such as 2016 have fewer records, which might be due to incomplete data collection during those periods.

**Potential Data Bias:**

The uneven distribution of records across years could introduce bias into the analysis or modeling. If the events in heavily represented years are not reflective of broader patterns, the model may overfit to those dominant years. This could affect how well the model performs on less-represented periods or in real-world applications.

**Dataset Coverage:**

The dataset covers the years 2000 to 2016, but the record counts are unevenly distributed. This imbalance may impact the model’s ability to generalize, especially if underrepresented years play a critical role in understanding long-term trends. For example, when using temporal features like `YEAR`, it’s essential to consider the potential impact of this imbalance and take steps to address it, such as applying weighting or resampling methods. Ensuring the dataset aligns with the modeling goals is crucial to prevent overfitting or misrepresenting trends driven by dominant years.

## Bivariate Analysis

<iframe
  src="bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Analysis of Box Plot: YEAR vs. TOTAL CUSTOMERS
The box plot illustrates how the total number of customers affected by power outages varied across different years. Each box represents the range of values for a specific year, with the length of the box (the interquartile range, or IQR) capturing the middle 50% of the data. The whiskers extend to show the minimum and maximum values, giving a clear picture of how spread out the data is within a year.

The line inside each box represents the median, which shows the typical number of customers affected in that year. Outliers, represented as dots outside the whiskers, point to extreme events where significantly more customers were affected than usual. These outliers often reflect major disruptions or unusual circumstances in those years.

Some years, like 2004, 2012, and 2014, show larger ranges of variability, with wider IQRs. This suggests those years had both small, localized outages and larger, widespread ones. On the other hand, years like 2000, 2010, and 2016 have tighter distributions, indicating more consistent levels of customer impact across events.

Certain years also stand out for their high-impact outliers. For instance, 2002, 2014, and 2008 had significant events that affected a much larger number of customers compared to the typical outages of those years. These spikes likely align with major weather events or infrastructure issues that need further investigation. 

This analysis highlights how the severity of power outages changes over time. By focusing on years with greater variability or extreme events, we can dig deeper into the factors—like weather patterns or infrastructure failures—that contribute to significant disruptions.

---

## Grouping and Aggregates 

### Cause Category by Year

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

### Significant Insights From the Table

This pivot table aggregates the total number of customers affected by power outages each year for various cause categories. Below are the key insights:

#### 1. Dominant Cause Categories
The largest values in each row reveal which cause category impacted the most customers in a given year. For instance:
- **Severe Weather** consistently stands out as the primary cause, with the highest impact in most years. This highlights its significant role in driving outages.

#### 2. Trends Over Time
The table shows how the number of customers affected changes year to year for each cause. For example:
- **Severe Weather** affects customers consistently but has notable spikes in certain years, such as 2008 and 2012, which likely correspond to major events like hurricanes or severe storms.
- Less frequent causes, like **Fuel Supply Emergency** and **Intentional Attack**, rarely appear, indicating they are uncommon contributors to outages.

#### 3. Emerging Patterns
Some categories, such as **System Operability Disruption**, show significant variation across years. This could suggest isolated, major incidents that caused large impacts during specific periods. In contrast, categories like **Public Appeal** and **Islanding** consistently have minimal impact, affecting very few customers overall.

#### 4. High-Impact Years
Certain years, such as 2008 and 2012, stand out for their overall high customer impact across all cause categories. These years may align with severe events like hurricanes, ice storms, or other extreme weather conditions that significantly disrupted power.

#### 5. Gaps or Missing Data
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

---
### Checking for MCAR and MAR data

#### Dependency of `CUSTOMERS.AFFECTED` Missingness on `U.S._STATE`

**Objective:**  
Determine whether the missingness of `CUSTOMERS.AFFECTED` is dependent on `U.S._STATE`.

**Hypotheses:**  
- **Null Hypothesis (H₀):** The missingness of `CUSTOMERS.AFFECTED` is **not** dependent on `U.S._STATE`.  
- **Alternative Hypothesis (H₁):** The missingness of `CUSTOMERS.AFFECTED` is dependent on `U.S._STATE`.

**Results:**  
- **p-value:** 0.00  
- **Decision:** p-value < 0.01, **reject** the null hypothesis.  
- **Conclusion:** The missingness of `CUSTOMERS.AFFECTED` **is** dependent on `U.S._STATE`.

<iframe
  src="marCUSTOMERS.AFFECTEDagainstU.S._STATE.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

#### Dependency of `DEMAND.LOSS` Missingness on `MONTH`

**Objective:**  
Assess whether the missingness of `DEMAND.LOSS` is dependent on `MONTH`.

**Hypotheses:**  
- **Null Hypothesis (H₀):** The missingness of `DEMAND.LOSS` is **not** dependent on `MONTH`.  
- **Alternative Hypothesis (H₁):** The missingness of `DEMAND.LOSS` is dependent on `MONTH`.

**Results:**  
- **p-value:** 0.0172  
- **Decision:** p-value > 0.01, **fail to reject** the null hypothesis.  
- **Conclusion:** The missingness of `DEMAND.LOSS` is **Missing Completely at Random (MCAR)** with respect to `MONTH`.

<iframe
  src="marDEMAND.LOSS.MWagainstMONTH.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Imputation Strategy
To address the missing values in **CUSTOMERS.AFFECTED**, we implemented a probabilistic imputation approach. Missing values were filled by sampling from the observed data within the same **U.S._STATE** group. If a group contained entirely missing values, default values based on the global mean or median were used. This strategy ensures that the imputed values are contextually relevant while preserving overall data integrity.

---

## Hypothesis Testing 

### Null Hypothesis (H₀)
The mean number of customers affected in 2011 is less than or equal to the mean for other years.

### Alternative Hypothesis (H₁)
The mean number of customers affected in 2011 is greater than the mean for other years.

### Test
We performed a permutation test to determine if the mean number of customers affected in 2011 is significantly greater than the mean for other years.

We performed a test with 
<iframe
  src="hypothesispermutation.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Framing a Prediction Problem

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

## Baseline Model

#### **Model Description**:

For the baseline model, we used **Logistic Regression** to predict the cause of power outages, utilizing two features from the dataset: **`U.S._STATE`** and **`YEAR`**.

- **Features Used**:
  - **`U.S._STATE`** (Nominal): This categorical feature represents the state where the outage occurred. Different states may experience different causes of outages due to varying weather patterns, infrastructure, and regulations.
  - **`YEAR`** (Ordinal): This feature indicates the year in which the outage occurred. The year might indicate trends or shifts in causes over time, such as changes in infrastructure or environmental factors.

#### **Feature Transformation**:
- **`U.S._STATE`**: This categorical feature was transformed using **One-Hot Encoding**, which converts the states into binary indicator variables. Since states do not have an inherent order, One-Hot Encoding is appropriate for this feature.
- **`YEAR`**: This ordinal feature was transformed using **Ordinal Encoding**, which assigns a unique integer to each year to preserve the chronological relationship between them. This encoding captures the trend or shifts over time in the causes of outages.

#### **Modeling Algorithm**:
- **Algorithm Chosen**: **Logistic Regression (Multinomial)**
  - Logistic regression was chosen as the baseline model for its simplicity and interpretability. It is well-suited for multiclass classification problems, such as predicting the cause of outages from multiple categories.
  - We used the **multinomial** setting of logistic regression, which allows the model to handle multiple possible outcomes (i.e., different causes of outages).

#### **Model Performance**:

| **Metric**                     | **Value**                             |
|---------------------------------|---------------------------------------|
| **F1-Score (weighted)**         | 0.5453                                |

The **F1-Score (weighted)** of 0.5453 indicates a moderate balance between precision and recall, particularly for the minority classes. This suggests that while the baseline model performs decently, there is room for improvement in handling class imbalances.

#### **Evaluation**:
While the baseline model performs reasonably well, it does not yet capture all of the underlying patterns in the data. The relatively low F1-score suggests that improvements can be made, especially in terms of precision and recall for minority classes. By improving feature engineering and tuning model hyperparameters, we can expect the performance to improve in subsequent models.


--
## Final Model

#### **Model Description**:

For the final model, we used a **Random Forest Classifier** to predict the cause of power outages, utilizing multiple features from the dataset: **`U.S._STATE`**, **`NERC.REGION`**, **`YEAR`**, **`TOTAL.CUSTOMERS`**, and **`CUSTOMERS.AFFECTED`**.

- **Features Used**:
  - **`U.S._STATE`** (Nominal): This categorical feature represents the state where the outage occurred. Different states may experience different causes of outages due to varying weather patterns, infrastructure, and regulations.
  - **`NERC.REGION`** (Nominal): This feature represents the NERC region, which groups states by their electric grid reliability. Different regions may experience outages due to local factors like weather patterns or infrastructure conditions.
  - **`YEAR`** (Ordinal): This feature indicates the year in which the outage occurred. The year might indicate trends or shifts in causes over time, such as changes in infrastructure or environmental factors.
  - **`TOTAL.CUSTOMERS`** (Quantitative): This feature indicates the total number of customers affected by the outage. Larger outages may have different causes, such as weather, while smaller outages might be due to equipment failures or human error.
  - **`CUSTOMERS.AFFECTED`** (Quantitative): This feature indicates how many customers were directly impacted by the outage, which can be a sign of the outage's scale and cause.

#### **Feature Transformation**:
- **`U.S._STATE`**: This categorical feature was transformed using **One-Hot Encoding**, which converts the states into binary indicator variables. Since states do not have an inherent order, One-Hot Encoding is appropriate for this feature.
- **`NERC.REGION`**: This categorical feature was transformed using **One-Hot Encoding**, which converts the regions into binary indicator variables, as there is no inherent order.
- **`YEAR`**: This ordinal feature was transformed using **Ordinal Encoding**, which assigns a unique integer to each year to preserve the chronological relationship between them.
- **`TOTAL.CUSTOMERS`** and **`CUSTOMERS.AFFECTED`**: These quantitative features were transformed using **StandardScaler**, which standardizes the values to have a mean of 0 and a standard deviation of 1.

#### **Modeling Algorithm**:
- **Algorithm Chosen**: **Random Forest Classifier**
  - Random Forest was chosen for its ability to handle both categorical and quantitative data and its capacity to capture complex, non-linear relationships in the data.
  - It is an ensemble learning method that aggregates the predictions of multiple decision trees to improve accuracy and reduce overfitting.

#### **Model Performance**:

| **Metric**                     | **Value**                             |
|---------------------------------|---------------------------------------|
| **F1-Score (weighted)**         | 0.7020                                |

The **F1-Score (weighted)** of 0.7020 indicates a strong balance between precision and recall, especially for the minority classes. The final model demonstrates an improvement in capturing the patterns in the data compared to the baseline model, with a more robust handling of class imbalances.

#### **Evaluation**:
The final model shows a marked improvement in performance over the baseline. The **F1-Score (weighted)** indicates that the model effectively balances precision and recall, addressing the shortcomings of the baseline model. The increase in performance is attributed to thoughtful feature engineering (e.g., `CUSTOMERS.PERCENT.AFFECTED`, `AFFECTED.CUSTOMERS.TYPES.COUNT`), data transformations (One-Hot Encoding, Standardization, and Ordinal Encoding), and hyperparameter tuning through **GridSearchCV**, which helped the model generalize better.

---
## Fairness Analysis of the Final Model

#### **Group Selection**:
- **Group X**: Areas with **Total Customers Below the Median** (Low Total Customers).
- **Group Y**: Areas with **Total Customers Above the Median** (High Total Customers).

#### **Evaluation Metric**:
- The evaluation metric used is **F1-Score (weighted)**, as it provides a better balance between precision and recall, particularly for imbalanced classes.

#### **Hypotheses**:
- **Null Hypothesis (H0)**: The model's F1-Score (weighted) is equal between areas with low total customers and areas with high total customers. Any observed differences in F1-Score are due to random chance.
- **Alternative Hypothesis (Ha)**: The model's F1-Score (weighted) is different between areas with low total customers and areas with high total customers. The observed differences represent a real disparity in model performance.

#### **Test Statistic and Permutation Test**:
- We used a **permutation test** to compare the **F1-Score (weighted)** between the two groups. The test calculates the observed F1-Score difference and compares it to 10,000 random permutations of the `is_low_customers` group to generate a distribution of F1-Score differences under the null hypothesis.
  
#### **p-value**:
- The **p-value** is calculated by comparing the observed F1-Score difference to the distribution of permuted F1-Score differences.
- **Original F1-Score Difference** (Low - High customers): 0.0929
- **p-value**: 0.0740

#### **Results**:
- **Group-wise F1-Scores**:
  - **Low Total Customers (≤ median)**: 0.6928
  - **High Total Customers (> median)**: 0.7857

- **Null Hypothesis**: We **fail to reject** the null hypothesis since the p-value (0.0740) is greater than the significance level of 0.01.

#### **Conclusion**:
- The fairness analysis suggests that there is **no significant disparity** in the model’s F1-Score between areas with low total customers and areas with high total customers, as the p-value is above the threshold of 0.01.

---

This fairness analysis confirms that, based on the available data and the **F1-Score (weighted)** metric, the final model performs similarly across both groups. Further analyses could explore other metrics or groups to provide deeper insights into model fairness.

