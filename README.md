# Bank Customer Churn Analysis: Investigating Key Churn Drivers and the Financial Impact on the Bank

## Executive Summary
Our current churn rate stands at 20.4% over the past 10 tenures, indicating 1 in 5 customers leave the bank. This represents a significant risk to our deposit base. The analysis reveals a "High-Value Flight" pattern: we are disproportionately losing older, wealthier customers, particularly in the German market. These customers are predominantly female and hold 1 product. The bank must implement the following strategic recommendations to reduce churn to 15%, potentially retaining $15,805,282 balance at risk in the next tenure: Audit the German Market, Re-engage Affluent Customers, Optimize Product Mix, Enhance Wealth Management for Seniors, and offer improved Gender-Inclusive Services for women.

## Business Problem
* **Stakeholders:** Chief Operating Officer (COO) / Head of Retail Banking 

* **The Challenge:** The bank is observing an uptick in customer attrition (churn), but the raw churn rate doesn't tell the full story. If the bank is losing "High-Value" customers (those with significant deposits, multiple product holdings, or high credit scores) while retaining "Low-Value" or inactive accounts, the bank’s liquidity and lending capacity are at risk.

* **The Business Need:** We need to move beyond identifying who is leaving and what drives them to understanding the economic impact of their departure. The bank requires a data-driven profile of the "At-Risk High-Value Customer" to deploy targeted retention strategies.

* **Success Metrics:** 
  * Identification of key churn drivers (Demographic, Financial, etc.). Communicate findings through an Executive Report and Presentation.
  * Quantification of "Total Balance at Risk."
  * Recommendation of a Retention Strategy to guide the Customer Success team.

* **Objective:** To identify actionable insights and recommendations that can reduce churn to 15% in the next tenure.

## Tech Stack and Skills
* Communication: Storytelling, Excel Charts, PowerPoint Presentation, Executive Reporting.
* Analytical Techniques: Exploratory Data Analysis (EDA), Data Cleaning, Churn and Retention Rates, Doughnut Chart, Bar Chart, Histogram, Density Distribution, Hypothesis Testing (Chi-Squared and T-Test of independence), Univariate and Bivariate Statistical Analysis, Correlation Matrix, Scatterplot, Heatmap, Data Tables.
* Python: Pandas, Matplotlib, Seaborn, Numpy, Pingouin (for Hypothesis Test), Pivot Tables, User Defined Functions.
* Tools: Jupyter Notebook, VS Code, GitHub, Loom (for asynchronous video presentation).

## Methodology

### Source of Dataset
The dataset was downloaded from [_kaggle_](https://www.kaggle.com/datasets/shantanudhakadd/bank-customer-churn-prediction). It contains 10,000 rows and 14 columns. The attributes fall into 5 categories:

* Customer Identification: `RowNumber`, `CustomerId`, and `Surname`
* Demographics: `Geography` (France, Spain, Germany), `Gender`, and `Age`
* Financial Profile: `CreditScore`, `Balance`, and `EstimatedSalary`
* Account Relationship: `Tenure`, `NumOfProducts`, `HasCrCard`, and `IsActiveMember`
* Target Variable: `Exited`

### Constraints
* **Balance Skew:** In the `Balance`column, a significant number of customers had a zero balance which suggests inactivity or low-engagement accounts. This is a valid observation in banking data; however, the volume in this dataset is quite high.
* **Lack of Transactional Depth:** The dataset lacks granular details such as transactional history, behavioral trends over time, specific product types, or customer service interactions. Without knowing how a customer uses their account, I could not drill down to uncover broad demographics and reasons why customers churned.
* **Unclear Definition of “Active”:** The “IsActiveMember” column is a simple binary indicator (0 or 1). It does not specify what constitutes activity, whether it’s logging into an app, a minimum number of monthly transactions, or something else.

### Data Analysis Process

#### Data Cleaning 
I begun by assessing data quality. Data completeness and validation checks revealed there were no missing values or duplicates. Categorical and numeric variables were appropriately represented. However, there was one anomaly detected in the `Surname` column as ‘H?’. Surnames are generally excluded from churn analysis to ensure privacy. Hence, this did not impact the quality of the analysis. `RowNumber`, `CustomerId`, and `Surname` columns were not analytically useful for this purpose; therefore, they were excluded.

#### Exploratory Data Analysis (EDA)
The first step was to calculate descriptive statistics, the number of customers who churned (2,037), and those who were retained (7,963) over the period. Subsequently, I computed the churn rate (20.4%) and retention rate (79.6%). This revealed that 20% of the bank’s customers have left, while 80% remain. Approximately 1 in 5 customers has churned, which is substantial from a revenue standpoint.

</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/9390abf7-1b90-40a4-a6a5-e38567fa9180" width="350">
</p>

Calculation of churned and retained averages of numerical attributes revealed that, regarding `Age`, older customers are higher risk churners. Larger balance correlates with higher churn risk, that is customers with higher account balance are more likely to leave. The churned and retained averages for `Credit Score`, `Tenure`, and `EstimatedSalary` were negligible.

Subsequent plotting of distributions (histograms, count plots, and density plots) of numerical and categorical variables to  assess the spread of data presented the following insights:

<p align="center">
  <em>Distribution of numeric variables</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/30fb38cf-9e09-4e6b-97b7-cfce8d90121f" width="1000">
</p>

* **Credit Score:** Follows a near-normal distribution centered around 650, with a notable spike at the maximum score of 850.
* **Age:** Most customers are aged between 30 and 45. The distribution is slightly right-skewed, with fewer customers in the older age brackets.
* **Tenure:** Almost uniformly distributed.
* **Balance:** A high frequency of customers have $0 balance. For those with a balance, it is normally distributed around a mean of $120,000.
* **Number of Products:** High peaks at 1 and 2 products but relatively flat at 3 and 4.
* **Estimated Salary:** Near uniform distribution.

<p align="center">
  <em>Distribution of categorical variables</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/31a37551-c8be-47d6-89cc-bbd3422cac5a" width="1000">
</p>

* **Geography:** France represents about $50% of the data, while Germany and Spain each account for approximately $25%.
* **Gender:** There are more male customers than female. The difference is however, not wide.
* **Credit Card:** An overwhelming majority of customers have credit cards.
* **Activity:** There is a relatively even split between active and inactive members, though slightly more are active.

<p align="center">
  <em>Density distribution of churned and retained customers</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/5b3c4b4f-0a27-479f-a942-5ef574e2618f" width="1000">
</p>

By comparing the density distributions of churned (Exited = 1) and retained (Exited = 0) customers, several patterns emerged:

* **Age as a Key Driver:** There is a distinct shift in the age distribution. Churned customers are notably older (mean age: approx. 45 years) compared to retained customers (mean age: approx. 37 years). The churn probability increases widely after age 40.
* **The Balance Paradox:** Interestingly, customers who churned tend to have higher average balances (approx. $91,108) than those who stayed (approx. $72,745). A large volume of retained customers have $0 balance, suggesting that empty accounts are not necessarily the primary drivers of churn.
* **Number of Products:** Customers with only one product are more likely to churn compared to those with two. However, having 3 or 4 products is almost exclusively associated with churn, likely indicating a "saturated" or dissatisfied customer.
* **Credit Score, Tenure, and Salary:** These show very little divergence in density between the two groups, suggesting they may not be strong individual predictors of churn on their own.

#### Hypothesis Test of Independence: Are the explanatory variables independent from churn (response variable)?
Following the distributions, I conducted hypothesis tests to find out if the variables have a relationship or association with churn (`Exited`). The statistical investigation revealed strong evidence of behavioral and demographic drivers associated with churn. There was evidence of a statistically significant relationship between these 7 variables and churn:`Geography`, `Gender`, `IsActiveMember` (Activity Status), `NumOfProducts` (Number of Products), `CreditScore`, `Age`, and `Balance`. 

Conversely, there was not enough evidence of a statistically significant relationship between `HasCrCard` (Credit Card Ownership), `Tenure`, `EstimatedSalary`, and `churn`. These 3 variables do not necessarily drive customer churn on their own. This is also confirmed by the density distribution plots.

#### Correlation Analysis
I plotted a correlation heatmap to further investigate the relationship between numerical variables and churn. Gender was encoded as a binary variable (GenderMapped) to be included in this analysis (Female: 1, Male: 0). None of the variables exhibit a particularly strong relationship with the target variable (Exited) or with each other. However, the following is worth noting:

* **Age (+0.29) and Exited:** The most influential positive correlation. As age increases, the likelihood of exit increases.
* **Balance (+0.12) and Exited:** Higher balances are weakly associated with higher churn risk.
* **IsActiveMember (-0.16) and Exited:** Being an active member has a protective effect, reducing the probability of exit.
* **GenderMapped (+0.11) and Exited:** Indicates a correlation where females (encoded as 1) have a higher propensity to churn.
* **NumberOfProducts and Balance (-0.30):** Customers with more products have moderately low account balance.

#### How Key Factors Interact to Drive Customer Churn

<p align="center">
  <em>Scatterplots of Age vs Balance by Churn Status, and Age vs Credit Score by Churn Status</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/1b8f2f2e-6598-410d-80e4-a77a6b94b631" width="1000">
</p>

* **Age and Balance Interaction:** The scatter plot of Age vs. Balance shows a dense cluster of churned customers (red) in the 45–60 age range with balances between $100,000 and $150,000. This confirms that the bank is specifically losing its more mature, wealthier clients.

* **Age and Credit Score Interaction:** Though credit score is a weak standalone driver of churn, customers with credit score below 400 will undoubtedly leave the bank. This is depicted by the red dots below credit score of 400. This could be due to weak economic status of the customer.

<p align="center">
  <em>Heatmap of Geography vs. Gender, Geography vs. Activity Status, and Number of Product vs. Activity Status</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/529aa534-9a46-4fdb-8238-755d44883871" width="1000">
</p>

* **German Market Crisis (Interaction between Geography, Gender, and Activity Status):** The interaction between Geography and other variables reveal that Germany is a significant outlier.
* **Gender Interaction:** Female customers in Germany exhibit the highest risk across the entire bank, with a churn rate of 37.6%. For context, this is more than double the churn rate of males in France (12.7%).
* **Activity Interaction:** Inactive members in Germany have a staggering 41.1% churn rate. Even being an "Active Member" in Germany (23.7%) is riskier than being an "Inactive Member" in France (21.1%).
* **Product Saturation & Inactivity:** Customers with 3 products are highly likely to leave regardless of activity, but inactive members are at extreme risk (88.2% churn). Every single customer with 4 products in this dataset has churned (100% churn rate), suggesting that this segment may consist of customers who were mis-sold products or encountered severe service friction/failure. Customers with 2 products who are Active Members have the lowest churn rate in the entire dataset (5.56%). This is the optimal segment.

<p align="center">
  <em>Boxplot of Age vs. Geography, Number of Products, Activity Status, and Gender</em><br>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/3332ce8e-4985-4248-a402-272815869179" width="1000">
</p>

* **Age as a Universal Multiplier:** The boxplots show that the "Age Effect" is consistent across every demographic and behavioral segment. Regardless of Geography, Gender, Number of Products, or Activity level, the median age of churned customers is consistently around 45, whereas the median age of retained customers is approximately 35. This suggests the bank’s value proposition or service model may be failing to retain customers as they transition into older age brackets.

## Insights and Financial Impact

Investigation into the bank's customer churn reveals the profile of “At-Risk High-Value Customers” that are disproportionately impacting the bank’s most valuable assets. These customers are characterized by key churn drivers.

### Key Drivers of Churn
The analysis identified 5 critical factors that drive customers to leave:

* **The "Age Trap" (40+ years):** This is the strongest predictor of churn. While younger customers (18-39) are highly stable, the churn rate rises for customers aged 40+ and alarmingly higher for the 45-60 category. This suggests a problem in the bank's long-term retention or wealth management value proposition.

* **The German Market Crisis:** Customers in Germany are twice as likely to churn (32.4%) compared to France or Spain. Specifically, the bank has lost 32.6% of its total capital held in Germany.

* **Product (Fewer Touchpoints and Over-Saturation):** A huge number of customers with 1 product (1,409) left the bank at a churn rate of 27.7%. This suggests customers with 1 product have fewer touchpoints with the bank making it easier to switch to competitors. While having 2 products is the optimal for retention (7.6% churn), moving to 3 or 4 products leads to alarming churn rates (82%+ and 100%, respectively). This indicates that high-product-count customers may be experiencing service friction, high fees, or "forced" bundling.

* **Inactive Affluent Customers:** Inactive customers are significantly more likely to exit. Notably, churned customers have 25% higher average balances (approx. $91,109) than retained customers (approx. $72,745).

* **Gender Disparity:** Female customers churned at a higher rate (25.1%) compared to Males (16.5%). This suggests the bank's products and communications may not be well aligned with female customer needs or preferences.

### Non-Drivers of Churn
These variables did not show significant explanatory power as factors of customer churn: Tenure, Credit Card Ownership, Credit Score, Balance, and Estimated Salary.

### Financial Loss
The financial loss associated with churn is substantial and targets the bank's liquidity.

* **Total Balance Lost (Churned):** A total of $186 million in customer balances has been lost in the period under analysis.
* **Loss of Affluent Clients:** The bank is not just losing customers; it is losing its wealthiest ones. The average churned customer leaves with nearly $19,000 more in their account than the average customer who stays.
* **Regional Revenue Risk:** In Germany alone, the bank has seen nearly $98 million in balances exit, threatening the viability of operations in that region.
* **Total Balance at Risk:** The bank risks losing $15,805,282 in the current tenure if churn is not curbed.

## Strategic Recommendations
To mitigate the losses, the bank should implement the following retention strategies: 

* **Wealth Management for Seniors** 
  * Redesign our value proposition for the 45–60 demographic. This should include loyalty interest rates, dedicated financial advisors, and specialized retirement planning to counter the near 50% churn rate in this group.
* **German Market Audit**
  * Conduct a deep-dive competitive analysis and service audit in Germany to identify if the high churn is due to localized competitor offers or poor regional service quality.
* **Optimization of Product Mix**
  * Pivot marketing efforts and incentives to move 1 Product customers to a 2-Product "Anchor" status. Investigate the 3+ product segment to remove service friction or fee structures that are driving customers away. Incentives include Discount Programs at retail stores, Exclusive Investments, and Tier-Based benefits that unlock with account growth (e.g., free travel insurance).
* **Affluence Customer Re-engagement**
  * Use predictive triggers to flag inactive customers with balance of $100K+ for immediate personal outreach and "active status" incentives (e.g., fee waivers for 6 months, premium loan rates, free credit scores, and bonuses for customer referrals).
* **Gender-Inclusive Service**
  * Tailor communication and product benefits to address the specific needs of female professionals and heads of households to recoup significant lost revenue.

## Conclusion and Next Step
By stabilizing the German market, re-engaging the 45–60-year-old high-balance segment, optimizing our product mix, and enhancing benefits and product appeal to women, the bank can protect its core deposits and stop the flight of high-value balance. Retention should be our primary engine for growth in the next tenure. Every 1% reduction in churn preserves approximately $9 Million in deposits. Therefore, we must stop the exit of our best customers today to stabilize the bank's future.

* **Next Analytics Step**
  * Build a predictive Machine Learning model to flag customers who are likely to churn for retention interventions.
