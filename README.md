
# Project Overview
This project analyzes U.S. airline flight delays and builds predictive models to identify flights with a high risk of significant arrival delays (≥15 minutes). The goal is to combine exploratory data analysis, feature engineering, and machine learning to uncover delay patterns and evaluate predictive performance across multiple models.
The project consists of two main parts:
	•	Data cleaning and preparation using SQL
	•	Exploratory analysis, visualization, and modeling using Python


## Data Preparation (SQL)
The dataset was cleaned using SQL in DBeaver to ensure data quality before analysis and modeling. The following steps were performed:
Missing Values
	•	01_check_missing_values.sql Identified 76 missing values in arr_flights (0.11% of 69,978 rows) and 92 missing values in arr_del15.
	•	02_remove_null_flights.sql Removed 76 rows where arr_flights IS NULL.
	•	03_analyze_null_del15.sql Found 16 rows where arr_del15 and delay-related columns were NULL, while arr_cancelled or arr_diverted were non-zero.
	•	04_fill_null_del15.sql Filled these 16 rows with zeros, as the delays corresponded to cancelled or diverted flights.
Data Validation
	•	05_check_negative_delays.sql Verified that no negative values exist in delay-related columns.
	•	06_check_delay_causes.sql Checked that the sum of delay causes matched total delay minutes. Minor discrepancies (e.g., 99.00 vs 97.99) were observed due to rounding, as documented in the dataset. No corrections were required.
	•	Additional checks (ongoing)
	◦	Cases where arr_del15 > arr_flights
	◦	Duplicate records
Output
	•	09_create_view_and_export.sql Created a cleaned view flights_cleaned with calculated metrics:
	◦	delay_rate
	◦	cancel_rate Exported the final dataset to CSV for Python-based analysis and modeling.


## Exploratory Data Analysis (Python)
Using pandas, matplotlib, and seaborn, the following analyses were performed:
	•	Monthly trends in average delay rates
	•	Airports with the highest average delay rates
	•	Airlines with the highest delay proportions
	•	Breakdown of delay causes for the most delayed airlines
	•	Year-over-year flight volume patterns
All visualizations were saved and used to support feature engineering and model interpretation.


## Feature Engineering
Several features were engineered to capture seasonality, operational scale, and historical behavior:
	•	Cyclical encoding of month (sin, cos)
	•	Log-transformed flight volume
	•	Average historical delay rate by carrier
	•	Airport size proxy based on average arrivals
	•	Holiday season indicator
	•	Binary target variable:
	◦	delayed_flag = 1 if delay_rate ≥ 0.20
Categorical features (airport, carrier) were one-hot encoded and numerical features were standardized.


## Modeling and Evaluation
The task was formulated as a binary classification problem. Four models were trained and compared using a stratified train–test split:
Model Performance Summary (ROC-AUC)
	•	Logistic Regression — AUC = 0.74
	•	Random Forest — AUC = 0.77
	•	Gradient Boosting — AUC = 0.78
	•	XGBoost — AUC = 0.80
XGBoost achieved the best overall performance and was selected as the final model.
Evaluation included:
	•	Classification reports (precision, recall, F1-score)
	•	ROC curves
	•	Confusion matrices


## Model Interpretability
To interpret the final model, SHAP (SHapley Additive exPlanations) was used:
	•	Global feature importance via SHAP summary plots
	•	Feature impact and directionality using dependence plots
	•	Identification of the most influential drivers of flight delays
Key drivers included:
	•	Historical carrier delay performance
	•	Flight volume (airport size)
	•	Seasonal effects
	•	Carrier- and airport-specific characteristics


## Key Insights
	•	Delay risk is strongly influenced by carrier history and airport congestion
	•	Seasonal patterns significantly affect delay probability
	•	Tree-based models outperform linear models in capturing nonlinear relationships
	•	XGBoost provides the best balance between performance and interpretability


## Usage
This project demonstrates a full analytics-to-ML pipeline:
	•	SQL for data cleaning and validation
	•	Python for analysis, visualization, modeling, and interpretation
It is designed as a portfolio project showcasing data engineering, analytical thinking, and applied machine learning.
