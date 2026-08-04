# 📊 Telco Customer Churn Prediction

An end-to-end Machine Learning project that predicts customer churn using a telecommunications customer dataset. This project demonstrates a complete machine learning workflow—from data preprocessing and feature engineering to model training, evaluation, and comparison of multiple classification algorithms.

---

## 📌 Project Overview

Customer churn is one of the most important business metrics in the telecommunications industry. Acquiring new customers is significantly more expensive than retaining existing ones, making churn prediction a valuable business problem.

The objective of this project is to develop predictive models that identify customers who are likely to discontinue their services. These predictions enable businesses to implement targeted retention strategies, reduce customer loss, and improve long-term revenue.

> **Business Goal:** Predict customer churn and identify high-risk customers before they leave.
---

# 🛠️ Technologies Used

- **Python 3.13**
- **Pandas** – Data manipulation and analysis
- **NumPy** – Numerical computing
- **Scikit-learn** – Machine learning, preprocessing, and evaluation
- **Jupyter Notebook**

---

# 📂 Dataset Overview

The project uses the **Telco Customer Churn** dataset containing information on customer demographics, account details, subscribed services, billing information, and churn status.

### Dataset Summary

- **Rows:** 7,043 customers
- **Columns:** 30 features
- **Target Variable:** `Churn Value`
  - `0` = Customer Retained
  - `1` = Customer Churned

### Features Used

#### Categorical Features

- Internet Service
- Online Security
- Online Backup
- Device Protection
- Tech Support
- Payment Method
- Contract
- Senior Citizen

#### Numerical Features

- Tenure Months
- Monthly Charges
- Total Charges

![Dataset Preview](Load_data.png)

---

# ⚙️ Data Preprocessing

Before building the models, the dataset underwent several preprocessing steps to improve model performance.

### Data Cleaning

- Filled missing values in **Total Charges** with `0` (customers with zero tenure)
- Separated numerical and categorical features
- Defined the target variable (`Churn Value`)

### Train-Test Split

The dataset was split into:

- **80% Training Data**
- **20% Testing Data**

using **stratified sampling** to preserve the class distribution.

![Train Test](train_test.png)

### Feature Engineering

A `ColumnTransformer` pipeline was used to preprocess the data.

#### Numerical Features

Standardized using:

- `StandardScaler`

This scales all numerical variables to the same range, improving model performance.

#### Categorical Features

Encoded using:

- `OneHotEncoder(drop='first')`

This converts categorical variables into numerical representations while avoiding multicollinearity.

![Encoding](encoding.png)

---

# 🤖 Machine Learning Models

Three classification algorithms were trained and evaluated.

## 1️⃣ Logistic Regression

| Metric | Score |
|---------|-------|
| Accuracy | **74%** |
| Precision | **50%** |
| Recall | **80%** |
| F1 Score | **0.62** |


![LR](LR.png)

![LR report](lRreport.png)

### Observation

Although Logistic Regression had the lowest overall accuracy, it achieved the **highest recall**, successfully identifying **80% of customers who actually churned**.

This makes it valuable when minimizing missed churn cases is the priority.

---

## 2️⃣ Random Forest Classifier

| Metric | Score |
|---------|-------|
| Accuracy | **78%** |
| Precision | **62%** |
| Recall | **46%** |
| F1 Score | **0.53** |

![RF](RF.png)
### Observation

Random Forest improved overall accuracy and precision but identified fewer churned customers than Logistic Regression.

---

## 3️⃣ Gradient Boosting Classifier

| Metric | Score |
|---------|-------|
| Accuracy | **80%** |
| Precision | **65%** |
| Recall | **52%** |
| F1 Score | **0.58** |

![GB](GB.png)

### Observation

Gradient Boosting delivered the best overall performance by achieving the highest accuracy and precision while maintaining a balanced recall.

---

# 📈 Model Comparison

| Model | Accuracy | Precision | Recall | F1 Score |
|------|---------:|----------:|--------:|---------:|
| Logistic Regression | 74% | 50% | **80%** | **0.62** |
| Random Forest | 78% | 62% | 46% | 0.53 |
| Gradient Boosting | **80%** | **65%** | 52% | 0.58 |

---

# 💡 Key Findings

- Gradient Boosting achieved the highest overall accuracy (**80%**).
- Logistic Regression achieved the highest recall (**80%**) and was most effective at identifying customers likely to churn.
- The choice of model depends on business priorities:
  - If the goal is **catching as many churners as possible**, Logistic Regression is preferred.
  - If overall predictive accuracy is more important, Gradient Boosting performs best.

---
---

# 👨‍💻 Author

**Samuel Nkansah**

Data Analyst | Python | SQL | Power BI | Machine Learning

