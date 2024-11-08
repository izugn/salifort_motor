# Analysis of employee data and modeling (source: HR department at Salifort Motor Co.; Google Advanced Data Analytics course capstone project)

# Objective:
The HR department at Salifort Motors wants to take some initiatives to improve employee satisfaction levels at the company. 
Main problem: What are the major drivers of employee turnover? What’s likely to make an employee leave the company?
Goal: Data-driven suggestions to HR management, based EDA findings and modeling results.

# Dataset Description:
Describe the dataset and any preprocessing steps.

| Column name | Type | Description |
|:------------|:-----|:------------|
| satisfaction_level | int64 | The employee's self-reported satisfaction level [0-1] |
| last_evaluation | int64 | Score of employee's last performance review [0-1] |
| number_project | int64 | Number of projects employee contributes to |
| average_monthly_hours | int64 | Average number of hours employee worked per month |
| time_spend_company | int64 | How long the employee has been with the company (years) |
| work_accident | int64 | Whether or not the employee experienced an accident while at work (binary) |
| left | int64 | Whether or not the employee left the company (binary) |
| promotion_last_5years | int64 | Whether or not the employee was promoted in the last 5 years (binary) |
| department | str | The employee's department |
| salary | str | The employee's salary (low, medium, or high) |

Descriptive stats:

| #  | Column                 | Non-Null Count | Dtype  |
|:--|:----------------------|:---------------|:-------|
| 0  | satisfaction_level     | 14999 non-null | float64|
| 1  | last_evaluation        | 14999 non-null | float64|
| 2  | number_project         | 14999 non-null | int64  |
| 3  | average_montly_hours   | 14999 non-null | int64  |
| 4  | time_spend_company     | 14999 non-null | int64  |
| 5  | Work_accident          | 14999 non-null | int64  |
| 6  | left                   | 14999 non-null | int64  |
| 7  | promotion_last_5years  | 14999 non-null | int64  |
| 8  | Department             | 14999 non-null | object |
| 9  | salary                 | 14999 non-null | object |


+-------+---------------------+---------------------+--------------------+----------------------+--------------------+---------------------+--------------------+-----------------------+
|       | satisfaction_level  |   last_evaluation   |   number_project   | average_montly_hours | time_spend_company |    Work_accident    |        left        | promotion_last_5years |
+-------+---------------------+---------------------+--------------------+----------------------+--------------------+---------------------+--------------------+-----------------------+
| count |       14999.0       |       14999.0       |      14999.0       |       14999.0        |      14999.0       |       14999.0       |      14999.0       |        14999.0        |
| mean  | 0.6128335222348156  | 0.7161017401160078  |  3.80305353690246  |  201.0503366891126   | 3.498233215547703  | 0.1446096406427095  | 0.2380825388359224 | 0.021268084538969265  |
|  std  | 0.24863065106114257 | 0.17116911062327533 | 1.2325923553183522 |  49.94309937128408   | 1.4601362305354812 | 0.35171855238017985 | 0.4259240993802994 |  0.14428146457858232  |
|  min  |        0.09         |        0.36         |        2.0         |         96.0         |        2.0         |         0.0         |        0.0         |          0.0          |
|  25%  |        0.44         |        0.56         |        3.0         |        156.0         |        3.0         |         0.0         |        0.0         |          0.0          |
|  50%  |        0.64         |        0.72         |        4.0         |        200.0         |        3.0         |         0.0         |        0.0         |          0.0          |
|  75%  |        0.82         |        0.87         |        5.0         |        245.0         |        4.0         |         0.0         |        0.0         |          0.0          |
|  max  |         1.0         |         1.0         |        7.0         |        310.0         |        10.0        |         1.0         |        1.0         |          1.0          |
+-------+---------------------+---------------------+--------------------+----------------------+--------------------+---------------------+--------------------+-----------------------+

Shape of df: (14999, 10)

# EDA Findings:
A brief summary of your Exploratory Data Analysis (EDA) insights. Include a few key graphs (saved as images in the images/ folder).
# Modeling Approach:
Describe the machine learning models you used (e.g., Logistic Regression, Random Forest), including any preprocessing, class balancing, and hyperparameter tuning steps.
# Key Metrics:
Summarize the model performance metrics (e.g., accuracy, AUC, F1-score). You can present them in tables or with confusion matrix images.
# Graphs:
Include important graphs like ROC curves, feature importance plots, and confusion matrices. These can be stored in the images/ folder and linked in the README.
# Business Recommendations:
Based on your model, summarize your findings and recommendations (e.g., which factors most influence employee turnover).
# Future Work:
Suggest improvements or future analysis.
# Ethical Considerations:
Add any ethical concerns you encountered.
# Code:
Briefly explain where to find the key parts of your code (e.g., data preprocessing, modeling, evaluation).
