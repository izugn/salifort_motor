# Analysis of employee data and modeling (source: HR department at Salifort Motor Co.; Google Advanced Data Analytics course capstone project)

# Objective:
The HR department at Salifort Motors wants to take some initiatives to improve employee satisfaction levels at the company.
- Main problem: What are the major drivers of employee turnover? What’s likely to make an employee leave the company?
- Goal: Data-driven suggestions to HR management, based on EDA findings and classification model results.

# Dataset Description:
Source: .CSV file; Shape: (14999, 10)

| Column name | Type | Description |
|:------------|:-----|:------------|
| satisfaction_level | float64 | The employee's self-reported satisfaction level [0-1] |
| last_evaluation | float64 | Score of employee's last performance review [0-1] |
| number_project | int64 | Number of projects employee contributes to |
| average_monthly_hours | int64 | Average number of hours employee worked per month |
| time_spend_company | int64 | How long the employee has been with the company (years) |
| work_accident | int64 | Whether or not the employee experienced an accident while at work (binary) |
| left | int64 | Whether or not the employee left the company (binary) |
| promotion_last_5years | int64 | Whether or not the employee was promoted in the last 5 years (binary) |
| department | str | The employee's department |
| salary | str | The employee's salary (low, medium, or high) |

# Data Exploration (Initial EDA and data cleaning):
Data exploration, cleaning, data manipulation and modeling performed in Python programming language. 
Libraries used for data manipulation and visualization: numpy, pandas, matplotlib and seaborn.

Purpose of exploratory data analysis in general:
- Identify potential predictive features for modeling;
- Understand data quality issues (duplicates, outliers);
- Discover patterns and relationships that contribute feature engineering;
- Establish baseline understanding for interpreting model results later on.

# [Steps of EDA](notebooks/salifort_hr_eda.ipynb):
- The first step is to understand variables, standardize and clean the dataset: find missing- and redundant data, detect outliers.
Number of duplicated rows in the dataset: 3008. With several continuous variables in 10 columns, it seems unlikely that these data entries are legitimate, so I decided to frop them.
- Outliers: I create a helper fuction to calculate number of rows containing outliers in each column, then use 'tabulate' library for representing the results.
Column 'time_spend_company' showing the lenght of tenure is the only non-binary column shownig 824 outliers. This can be a concern later, when I build a logistic regression model, which is more sensitive to out;iers that others. I shall consider removing these outliers based on the type of model I apply.

Next, I analyze relationships between variables:
- Calculate department size and turnover rates (how many employees left / stayed).

Approach further segmentation and see
- mean satisfaction level by department;
- satisfaction levels by number of years spent at the company;
- relation between years at the company and turnover rates;
- mean monthly worktime by department;
- satisfaction levels vs. average monthly workhours;
- average monthly worktime vs. employees' evaluation score;
- turnover rates by the number of projects employees were involved in;
- relations between salary levels and years spent at the company;
- tunrover by monthly workhours and promotion;
- turnover by monthly workhours and work accident suffered.  
 
## Distributions:
Turnover rates varied by department:
Highest: HR (18.8%)
Lowest: Management (11.93%)
![image](https://github.com/user-attachments/assets/d6268a30-4a1d-49e2-b1ae-9e78fb9f45d2)


Turnover rates by salary level:
Low: 20.45%
Medium: 14.62%
High: 4.85%

# Modeling Approach:
Describe the machine learning models you used (e.g., Logistic Regression, Random Forest), including any preprocessing, class balancing, and hyperparameter tuning steps.
# Key Metrics:
Summarize the model performance metrics (e.g., accuracy, AUC, F1-score). You can present them in tables or with confusion matrix images.
# Graphs:
Include important graphs like ROC curves, feature importance plots, and confusion matrices. These can be stored in the images/ folder and linked in the README.
# Business Recommendations:
Based on your model, summarize your findings and recommendations (e.g., which factors most influence employee turnover).

