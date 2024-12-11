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

### Cleaning

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

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---
