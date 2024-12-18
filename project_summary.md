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

# Steps of EDA:
[Link to notebook](notebooks/salifort_hr_eda.ipynb)

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
Turnover rates varied by department between 11.93% - 18.8%.
- Highest: HR (18.8%), accounting (17.5%), technical (17.4%), support (17,1%) and sales (17%). 
- Lowest: Management (11.93%) and R&D (12.2%).

![image](https://github.com/user-attachments/assets/d6268a30-4a1d-49e2-b1ae-9e78fb9f45d2)

## Satisfaction levels accross departments:
- Across all departments, employees who stayed (blue) generally show higher median satisfaction levels (middle line in box) than those who left (orange);
- The satisfaction levels for those who left are consistently lower and show more variation (longer boxes) in most departments;
- At 'IT' and 'R&D' the distributions for those who stayed is more compact, suggesting more consistent satisfaction levels;
- The presence of outliers (dots beyond the whiskers) in some departments indicates individual cases that deviate significantly from the typical pattern. 'HR' boxplot shows, a more consistent and but lower satisfaction level for people who left. Those who stayed show more variable satisfaction levels, suggesting that employees might stay in HR even with varying levels of satisfaction. The department shows one of the larger gaps between median satisfaction of stayers versus leavers.
![image](https://github.com/user-attachments/assets/74caf2bf-10c1-4a42-9759-4608da96a634)

## Satisfaction and tenure:
- Years 2-4: High variation in satisfaction for those who stayed, while leavers show declining satisfaction;
- Year 4: Notable drop in satisfaction for leavers, with tight clustering around 0.1;
- Years 5-6: Interesting reversal - leavers show higher satisfaction (0.7-0.9) than stayers (0.2-0.7);
- Years 7+: No employees left the company, with satisfaction levels stabilizing around 0.5-0.8.

Highest employee count is at 3 years tenure (~5000 total), turnover peaks at years 3-5, with significant proportions leaving.
Sharp contrast after 7 years - zero turnover and smaller employee population, year 10 shows stable retention with about 100 employees.
- Critical retention period is years 3-5;
- After 7 years, turnover drops to zero, suggesting strong long-term retention;
- Mid-tenure (5-6 years) shows counterintuitive pattern where highly satisfied employees are leaving.

![image](https://github.com/user-attachments/assets/9e1ab386-027d-4ceb-8ee6-fb30646a022c)
![image](https://github.com/user-attachments/assets/9fdef7c9-b72d-4241-9d5e-42272fd80857)

## Satisfaction, evaluation and average monthly working hours:
The three charts reveal concerning patterns about work hours and employee satisfaction.

- All departments show median monthly hours around 200;
- Employees who left (orange) consistently worked longer hours across departments;
- R&D shows highest median hours for leavers, followed by technical and accounting;
- Considerable overtime across all departments (standard is 160 hrs/month).

We can identify two distinct groups of leavers:
  1. Low hours (140-160) with moderate satisfaction (0.3-0.4)
  2. High hours (250-310) with very low satisfaction (0.1-0.2)

The red line at 160 hours shows standard monthly workload (calculated with 40 hours workweek, and 4 weeks vacation / year.)
- Most employees work above standard hours;
- High concentration of stayed employees (blue) between 0.6-0.8 satisfaction;
- We might assume correlation between excessive hours and low satisfaction leading to turnover.

![image](https://github.com/user-attachments/assets/c42930db-3b6a-4213-baca-4af800b0d545)
![image](https://github.com/user-attachments/assets/d85498b0-1234-4d18-a35c-de82e7565b59)
![image](https://github.com/user-attachments/assets/2008415b-68bb-40e8-83b7-5c3c011c53e9)

## Insights about project workload and turnover:
- 7 projects: 100% turnover, extremely high hours (250-300);
- 6 projects: High turnover, working 225-275 hours;
- 2-3 projects: Lower hours (150-200) but still some turnover.

Project Distribution (Right Chart):
- Most employees handle 3-4 projects, these people seem to stay the most;
- Sharp increase in turnover for 5+ projects, no retention at 7 projects.

![image](https://github.com/user-attachments/assets/d70a3463-16bd-4116-bbb3-65705b905be0)

### Additional factors - work accident, promotion and salary levels:
#Analysis of promotion and work accident patterns:
- Very few employees (represented by 1.0 on y-axis) received promotions in past 5 years;
- Promoted employees generally stayed (blue dots);
- Most promoted employees worked standard hours (around 160/month);
- Almost no promotions for those working excessive hours (250-300);
- Only few employees who left were promoted.
![image](https://github.com/user-attachments/assets/0302bb00-f58d-49a9-8478-e4879c0415ad)

#Low incidence of work accidents overall:
- Slight increase in accidents for those working longer hours (250-300);
- Most employees with accidents stayed (blue dots);
- Some correlation between overwork and accidents for those who left;
![image](https://github.com/user-attachments/assets/f26b091d-288c-40a8-8376-8cbdf93e1a34)

#Salary distribution charts:
- Highest concentration of employees at 2-3 years, low and medium salaries dominate early years;
- High salaries are rare and decrease with tenure, sharp decline in all salary levels after year 3;
- Medium salary level dominates at 7+ years, more balanced distribution of high salaries here.

![image](https://github.com/user-attachments/assets/d61df3ba-0719-4b43-afce-923437833bfb)

# Modeling Approach 1. Logistic Regression Model:
[Link to notebook](notebooks/salifort_logreg.ipynb)

We can utilize logistic regression model in this binary classification task. Before splitting the data, the following preparations are done:
- Categorical variables converted: 'salary' is ordinal, with hierarchy in the catergories, so it is encoded using LabelEncoder; 'department' is one-hot encoded using pd.get_dummies();
- Data types are optimized for memory efficiency: Binary columns are converted to uint8; Float columns remain as float64; Integer columns are converted to uint8

- A correlation matrix is created to understand relationships between numerical features;
- The model is sensitive to outliers: outliers in 'time_spend_company' are removed.
- Data is checked for class imbalance in the target variable ('left'): There is an approximately 83%-17% split, so the data is not perfectly balanced, but it is not too imbalanced. Resampling the data is necessary.

## Model Development
- Data is split into three sets: 60% training, 20% validation, 20% test;
- Features are standardized using StandardScaler;
- Implements GridSearchCV, using 5-fold cross validation to find optimal hyperparameters;
- GridSearchCV uses recall to choose the "best" model parameters. It prioritizes identifying as many employees who will leave as possible.

## Model Evaluation
- The model's performance is evaluated using precision, recall, accuracy and F1-score of the 'Classification report';
![Screenshot 2024-12-08 at 09 03 01](https://github.com/user-attachments/assets/3ddf8333-d2b2-4a56-9b7e-cc7c3521d5ec)

- Confusion matrices for both validation and test sets
![Screenshot 2024-12-08 at 09 04 00](https://github.com/user-attachments/assets/20f2f33a-bb22-49fa-b665-768dcd52ce72)

[More about confusion matrix](https://www.studocu.com/row/document/king-fahd-university-of-petroleum-and-minerals/calculus-3/confusion-matrix-cheatsheet/40120103)

- ROC curves for both validation and test sets with AUC scores
![Screenshot 2024-12-08 at 09 04 24](https://github.com/user-attachments/assets/e8db6449-7326-49ed-98ab-f37d883dce3a)

## Model performance summary (Logistic regression):

### Class 0 (Employees who stayed):
Very high precision (1.00): When the model predicts an employee will stay, it's almost always correct
Moderate recall (0.65): The model identifies 65% of employees who actually stayed
Good F1-score (0.79): Balanced performance for predicting employees who stay

### Class 1 (Employees who left):
Low precision (0.37): Many false positives when predicting employee departures
Excellent recall (0.98): The model successfully identifies 98% of employees who actually left
Moderate F1-score (0.54): The low precision impacts overall effectiveness for this class

### AUC Scores and ROC curve:
- Both validation and test sets have AUC = 0.87. This indicates good but not excellent discriminative ability.
- Shows a good consistency between validation and test performance.

At a conservative threshold (around False positive rate = 0.2), the model catches ~90% of employees likely to leave, while the false positive rate is 20%.
This appears to be a sweet spot, where it balances catching most potential leavers while keeping false alarms manageable.
Going further, at a 40% false positive rate, it catches ~95% of likely leavers.

Advantages of the model:
- Catches 98% of employees who might leave - very few employees who leave are missed: Allows for early intervention.

Disadvantages:
- Many false alarms (63% of predicted leavers actually stay), could lead to unnecessary HR interventions;
- This may waste resources investigating false positives.

As an alternative approaches chosing 'precision' for scoring would result fewer false alarms, but might miss leavers. Scoring='f1' would balance between precision and recall. 'accuracy' might give an overall correctness but again, we might ignore minority class, which are the potential leavers.

### Overall Model Performance:
It indicates the model is reliable when predicting employees will stay, also good at identifying potential leavers, but with many false alarms. All in all, it is suited for initial screening than final decision-making.
This allows HR to proactively intervene with almost all potential leavers. On the other hand low precision (0.37) means the model flags many false positives.
This results in HR potentially spending resources on employees who weren't actually planning to leave.

# Modeling Approach 2. Random Forest Model:
[Link to notebook](notebooks/salifort_randomf.ipynb)

Secondly I chose building a random forests model. In general, it provides a robust measure of feature importance that shows the relative contribution of each variable to predicting employee turnover. Unlike simpler models, it captures non-linear relationships and complex interactions between variables.
It is not sensitive to outliers or unusual patterns in individual variables.
Employee turnover decisions typically involve multiple interacting factors, and a random forests model can capture complex patterns like:
- How satisfaction levels might interact with workload;
- How time spant at the company interacts with performance evaluations;
- How combinations of factors together influence turnover risk.

Before splitting the data, the following preparations are done:
- Categorical variables converted: 'salary' is ordinal, with hierarchy in the catergories, so it is encoded using LabelEncoder; 'department' is one-hot encoded using pd.get_dummies();
- A new variable is introduced: 'overworked' that comes from the conversion of 'average_monthly_hours'. This is an ordinal categorical variable, that takes 0 (below 161h) 1 (161 - 200h), or 2 (200+ hours). Hence 'average_monthly_hours' is dropped.
- Data types are optimized for memory efficiency: Binary columns are converted to uint8; Float columns remain as float64; Integer columns are converted to uint8.

## Model Development
- Data is split into 80% training, 20% test set, then the training data is further split (80/20) to training and validation sets.
- I implement GridSearchCV, using 5-fold cross validation to find optimal hyperparameters. GridSearchCV uses recall to choose the "best" model parameters. It prioritizes identifying as many employees who will leave as possible.

Optimized parameters found through GridSearchCV for this Random Forest model:
- Each tree in the forest is built using a random sample of the data. Some observations may be selected multiple times, while others may not be selected at all. This helps create diversity among the trees and reduces overfitting.
- Having Sets maximum depth (number of levels) of each decision tree to 20 also helps control model complexity and prevent overfitting.
When splitting a node, only a random subset of features is considered: the size of this subset is the square root of the total number of features. It helps ensure diversity among trees by making them focus on different features.
- Each leaf node (end node) must contain at least 1 sample.
- A node must contain at least 5 samples to be considered for splitting.
- The forest contains 300 individual decision trees, which is large enough to ensure stable predictions.

## Model Evaluation
- The model's performance is evaluated using precision, recall, accuracy and F1-score of the 'Classification report':

![Screenshot 2024-12-08 at 11 00 03](https://github.com/user-attachments/assets/f23626a4-33c6-49f0-90c5-6af2b23e15ab)

- Confusion matrices for both validation and test sets;

![image](https://github.com/user-attachments/assets/c7241957-392a-4e17-bd59-95dc5b1caf4b)
![image](https://github.com/user-attachments/assets/df7e8cc2-f360-4a54-90e7-fc93d38dc599)
[More about confusion matrix](https://www.studocu.com/row/document/king-fahd-university-of-petroleum-and-minerals/calculus-3/confusion-matrix-cheatsheet/40120103)

- ROC curves for both validation and test sets with AUC scores

![image](https://github.com/user-attachments/assets/e4112fdb-d0e9-41c2-867c-e07a550928f5)

- Feature importance chart (ordered from high to low)

![image](https://github.com/user-attachments/assets/da7d9eb9-7fe2-4bed-a456-463f30923146)


## Model performance summary (Random forest):

Overall Accuracy: The model performed well, achieving 98% accuracy on both validation and test sets. This indicates consistent and reliable performance.
### Class 0 (Employees who stayed) (Test Set):
- Precision: 99%
- Recall: 100%
- F1-score: 99%
### Class 1 (Employees who left) (Test Set):
- Precision: 98%
- Recall: 92%
- F1-score: 95%

The similar performance across validation and test sets suggests the model is robust and not overfitted. The high precision and recall values indicate reliable predictions for both employees staying and leaving.

### Confusion Matrix Analysis (Test Set):
- True Negatives (correctly predicted stays): 1994
- False Positives (incorrectly predicted departures): 7
- False Negatives (missed departures): 30
- True Positives (correctly predicted departures): 368

The model is particularly good at identifying employees likely to stay (very few false positives).

### ROC Curve Analysis:
- The model achieved an AUC-ROC score of 0.98;
- The ROC curve shows strong separation from the diagonal line, indicating good discriminative ability;
- A high true positive rate achieved with minimal false positive rate increase.

### Feature Importance (in descending order):
1. Satisfaction level (36.2%)
2. Number of projects (20.6%)
3. Time spent at company (18.3%)
4. Last evaluation (14.3%)
5. Overworked status (7.1%)
6. Other features (salary, work accident, department, etc.) each contributed less than 1%

## Key Insights:

- Employee satisfaction is by far the most crucial predictor of turnover;
- Workload (number of projects) and tenure are also significant factors;
- Performance evaluations have moderate importance;
- Department-specific factors have minimal impact on turnover decisions.

The results suggest the model would be a reliable tool for HR to identify employees at risk of leaving, with employee satisfaction and workload being the key areas to monitor.


# Conclusions, business recommendations:
1. Evidence that employees are overworked:

During EDA, we found average monthly hours of 200.47, significantly above the optimal 160 hours. The "overworked" feature ranked 5th in importance in the Random Forest model (7.1% importance). Strong correlation between working long hours and leaving the company. Data showed employees with 6-7 projects consistently worked excessive hours (240-315 hours/month).

2. Project cap recommendation is supported by:

Number of projects was the second most important feature (20.6% importance). EDA showed 100% turnover rate for employees with 7 projects. Optimal project load appeared to be 3-4 projects based on turnover rates. Strong correlation between high project numbers and excessive working hours.

3. Four-year tenure concern is validated:

Time spent at company was the third most important feature (18.3% importance). EDA revealed a spike in turnover at the 4-year mark (24.69% turnover rate). Clear pattern in satisfaction levels dropping at 4 years. Low promotion rates despite tenure (only 2.1% promoted in last 5 years).

4. Working hours and compensation recommendations:

Strong correlation between excessive hours (200+ monthly) and turnover. Many employees working significantly over standard hours (>200 monthly). Clear relationship between hours worked and evaluation scores. Evidence that high performers were often overworked but not necessarily rewarded.

5. Company culture and evaluation system concerns:

Satisfaction level was the most important predictor (36.2% importance). Clear pattern of high performers leaving when overworked. Last evaluation scores were the fourth most important feature (14.3%). Evidence of potential bias in evaluation system favoring long hours. 

6. Work-life balance issues:

Clear correlation between excessive hours and turnover. Departments showed similar patterns, indicating a company-wide issue. High performers often worked unsustainable hours. Low satisfaction scores correlated with excessive working hours.
