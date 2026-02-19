# A/B Testing Project: Ad Campaign Conversion Analysis

## Dataset

In this A/B test, users are divided into two groups:

- **Experimental Group** – Exposed to ads  
- **Control Group** – Shown a Public Service Announcement (PSA) or no ad at all  

**Goal:**  
1. **Campaign Success** – Determine if the ads led to a significant increase in conversions  
2. **Attributable Impact** – Quantify how much of the success can be directly linked to the ads  

**Data Dictionary:**

| Column           | Description                                                                 |
|-----------------|----------------------------------------------------------------------------|
| Index / user     | Row index                                                                   |
| id               | User ID (unique)                                                            |
| test_group       | `"ad"` = saw advertisement, `"psa"` = only saw PSA                          |
| converted        | `True` if user bought the product, `False` otherwise                        |
| total_ads        | Number of ads seen by the user                                             |
| most_ads_day     | Day the user saw the most ads                                              |
| most_ads_hour    | Hour of day the user saw the most ads                                      |

**Data File:**  
- `marketing_AB.csv`

---

## Quick Stats

| Metric                        | Value                  |
|--------------------------------|----------------------|
| Ad Group Conversion Rate       | 2.55%                 |
| PSA Group Conversion Rate      | 1.79%                 |
| Incremental Lift               | 0.76 percentage points|
| Z-Statistic                    | 7.37                  |
| P-Value                        | 1.71 × 10⁻¹³          |
| Incremental Revenue Generated  | $140,030              |

---

## Project Description

For this project, I am analyzing the results of an A/B test run by an e-commerce website. The goal is to utilize practical statistics, regression, and other data analysis tools to help the company determine whether they should implement the new page, keep the old page, or perhaps run the experiment longer to make a decision.

---

## Part I - Probability

- Load data into a dataframe and clean  
- Obtain basic proportions for statistics, including:  
  1. Conversion rate regardless of treatment or control  
  2. Probability an individual received the treatment page  
  3. Conversion rate of individuals who received the control page

---

## Part II - A/B Test

- Assume under the null hypothesis that the new page (`p_new`) and old page (`p_old`) have "true" success rates equal to the overall conversion rate regardless of page  
- Use a **5% Type I error rate**  
- Create a sampling distribution to show differences in conversion between groups  
- Compare results of the hypothesis test with `statsmodels.api` z-test

---

## Part III - Regression Approach

- Fit a **logistic regression model** to determine if there is a significant difference in conversion based on page type or ad exposure  
- Find p-value and determine if it supports the null hypothesis or rejects it in favor of the alternative hypothesis  
- Investigate whether time, day, hour, or total ad exposures influence conversion

## Conclusion

- The A/B test indicates that the ad campaign significantly increased conversions.  
- Regression results support the significance of both ad exposure and group assignment.  
- The campaign generated measurable incremental revenue, confirming a **successful experiment**.


**Python Implementation Example:**

```python
import statsmodels.api as sm

df["group_binary"] = df["test_group"].map({"psa":0, "ad":1})
X = sm.add_constant(df[["group_binary", "total_ads"]])
y = df["converted"]

model = sm.Logit(y, X).fit()
print(model.summary())





