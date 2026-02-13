# ✈️ U.S. Airline Flight Delay Analysis & Prediction

---

## 📌 Project Overview

This project analyzes U.S. airline flight delays and builds predictive models to identify flights with a high risk of significant arrival delays (≥ 15 minutes).

The objective is to combine:

- Exploratory Data Analysis (EDA)  
- Feature Engineering  
- Machine Learning Modeling  
- Model Interpretability  

to uncover delay patterns and evaluate predictive performance across multiple models.

The project consists of two main components:

- Data cleaning and preparation using SQL  
- Exploratory analysis, visualization, and modeling using Python  

---

## 📊 Interactive Dashboard (Tableau)

An interactive dashboard with key visualizations is available on Tableau Public.

🔗 **View Dashboard:**  
https://public.tableau.com/views/FlightDelays_17683171909260/Dashboard1

The dashboard includes:

- Monthly delay trends  
- Airline delay comparison  
- Airport-level delay performance  
- Year-over-year flight volume analysis  

This dashboard complements the Python-based EDA and provides an interactive exploration layer.

---

## 🗄️ Data Preparation (SQL)

Data cleaning was performed in DBeaver to ensure data quality before modeling.

### 🔍 Missing Values

**01_check_missing_values.sql**

Identified:

- 76 missing values in `arr_flights` (0.11% of 69,978 rows)  
- 92 missing values in `arr_del15`  

**02_remove_null_flights.sql**

- Removed 76 rows where `arr_flights IS NULL`

**03_analyze_null_del15.sql**

- Found 16 rows where:
  - `arr_del15` and delay-related columns were NULL  
  - `arr_cancelled` or `arr_diverted` were non-zero  

**04_fill_null_del15.sql**

- Filled those 16 rows with zeros  

Justification: delays corresponded to cancelled/diverted flights.

---

### ✅ Data Validation

**05_check_negative_delays.sql**

- Verified no negative delay values exist.

**06_check_delay_causes.sql**

- Checked that the sum of delay causes matched total delay minutes.  
- Minor rounding discrepancies observed (e.g., 99.00 vs 97.99).  
- No corrections required (dataset documentation confirmed rounding behavior).

Additional validation checks:

- Cases where `arr_del15 > arr_flights`  
- Duplicate records  

---

### 📤 Output

**09_create_view_and_export.sql**

- Created cleaned view: `flights_cleaned`  
- Added calculated metrics:
  - `delay_rate`
  - `cancel_rate`
- Exported final dataset to CSV for Python analysis  

---

## 📊 Exploratory Data Analysis (Python)

Using `pandas`, `matplotlib`, and `seaborn`, the following analyses were performed:

- Monthly trends in average delay rates  
- Airports with the highest average delay rates  
- Airlines with the highest delay proportions  
- Breakdown of delay causes for the most delayed airlines  
- Year-over-year flight volume patterns  

All visualizations were saved and later used to support feature engineering and model interpretation.

---

## ⚙️ Feature Engineering

Features were engineered to capture:

- Seasonality  
  - Cyclical encoding of month (sin, cos)  

- Operational scale  
  - Log-transformed flight volume  
  - Airport size proxy (average arrivals)  

- Historical performance  
  - Average historical delay rate by carrier  

- Seasonal effects  
  - Holiday season indicator  

### 🎯 Target Variable

delayed_flag = 1 if delay_rate ≥ 0.20


---

### 🔧 Encoding & Scaling

- Categorical features (airport, carrier) → One-hot encoded  
- Numerical features → Standardized  

---

## 🤖 Modeling & Evaluation

The problem was formulated as a binary classification task.

A stratified train–test split was used.

### 📈 Model Performance (ROC-AUC)

| Model | ROC-AUC |
|-------|---------|
| Logistic Regression | 0.74 |
| Random Forest | 0.77 |
| Gradient Boosting | 0.78 |
| XGBoost | 0.80 |

🏆 **XGBoost achieved the best performance and was selected as the final model.**

---

### 📊 Evaluation Metrics

- Classification reports (Precision, Recall, F1-score)  
- ROC curves  
- Confusion matrices  

---

## 🔎 Model Interpretability

To interpret the final model, SHAP (SHapley Additive exPlanations) was used:

- Global feature importance (SHAP summary plots)  
- Feature impact & directionality (dependence plots)  
- Identification of key delay drivers  

### 🔑 Most Influential Drivers

- Historical carrier delay performance  
- Flight volume (airport size proxy)  
- Seasonal effects  
- Carrier- and airport-specific characteristics  

---

## 🛠️ Tech Stack

- SQL (DBeaver) – Data cleaning & validation  
- Python  
  - pandas  
  - matplotlib  
  - seaborn  
  - scikit-learn  
  - XGBoost  
  - SHAP  
- Tableau Public – Interactive dashboard  

---

## 🚀 Usage

This project demonstrates a full end-to-end analytics pipeline:

- SQL data validation & transformation  
- Tableau interactive visualization  
- Python exploratory analysis  
- Feature engineering  
- Machine learning modeling  
- Model interpretation  

It is designed as a portfolio project showcasing:

- Data engineering skills  
- Analytical reasoning  
- Applied machine learning  
- Model interpretability  
- Data visualization  

