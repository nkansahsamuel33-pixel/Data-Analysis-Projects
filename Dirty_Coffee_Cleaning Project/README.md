# ☕ Dirty Café Sales Data Cleaning Project

## 📌 Project Overview
This project focuses on cleaning and preprocessing a raw, unstructured dataset containing 10,000 transaction records from a café (`dirty_cafe_sales.csv`)[cite: 1]. The dataset contained various data quality issues, including improper column headers, missing values (`NaN`), system noise (`ERROR`, `UNKNOWN`), inconsistent casing, and incorrect data types[cite: 1]. 

Using **Python** and **Pandas**, this notebook systematically cleans the dataset, imputes missing financial figures using mathematical calculations, standardizes categorical text, and exports a clean dataset ready for downstream analysis[cite: 1].

---

## 🛠️ Tools & Libraries Used
* **Python**
* **Pandas** for data manipulation and cleaning
* **NumPy** for conditional calculations (`np.where`)

---

## 🚀 Data Cleaning Steps

### 1. Initial Exploration & Column Standardization
* Imported raw transaction data and examined structure using `df.info()` and missing value percentages (`df.isna().mean()`).
* Replaced spaces in column names with underscores to streamline variable referencing.

import pandas as pd
import numpy as np

df = pd.read_csv('dirty_cafe_sales (1).csv')
df.columns = df.columns.str.replace(' ', '_')

> 📸 [INSERT SCREENSHOT HERE: Notebook Output of `df.info()` or initial dataset state]

---

### 2. Standardizing the `Item` Column
* Cleared leading/trailing whitespace and normalized text capitalization using `.str.title().
* Replaced explicit string values like `'Error'` and missing `NaN` values with `'Unknown'`.

df['Item'] = df['Item'].str.strip().str.title().fillna('Unknown').replace('Error', 'Unknown')

---

### 3. Handling Unrecoverable Missing Records
* Evaluated numeric relationship across `Quantity`, `Price_Per_Unit`, and `Total_Spent`.
* Dropped records where **both** key fields needed for mathematical calculation (e.g., `Quantity` and `Price_Per_Unit`, or `Quantity` and `Total_Spent`) were completely missing, as these values could not be accurately imputed[cite: 1].

# Drop rows missing both key metrics
df = df.dropna(subset=['Quantity', 'Price_Per_Unit'], how='all')

df = df.dropna(subset=['Quantity', 'Total_Spent'], how='all')

---

### 4. Numeric Type Conversion & Mathematical Imputation
* Replaced string noise (`'ERROR'`) and missing values (`NaN`) in numeric columns with `0` temporarily to allow type casting[cite: 1].
* Converted `Quantity`, `Price_Per_Unit`, and `Total_Spent` to numeric data types using `pd.to_numeric()`[cite: 1].
* Used NumPy conditional statements (`np.where`) to mathematically calculate missing values across related financial metrics[cite: 1]:
  - Quantity = Total Spent / Price Per Unit
    
  - Price Per Unit = Total Spent / Quantity
    
  - Total Spent = Quantity * Price Per Unit

# Cast columns to numeric
df['Quantity'] = pd.to_numeric(df['Quantity'].replace('ERROR', 0).fillna(0), errors='coerce')

df['Price_Per_Unit'] = pd.to_numeric(df['Price_Per_Unit'].replace('ERROR', 0).fillna(0), errors='coerce')

df['Total_Spent'] = pd.to_numeric(df['Total_Spent'].replace('ERROR', 0).fillna(0), errors='coerce')

# Recalculate missing figures mathematically
df['Quantity'] = np.where(df['Quantity'] == 0.0, df['Total_Spent'] / df['Price_Per_Unit'], df['Quantity'])

df['Price_Per_Unit'] = np.where(df['Price_Per_Unit'] == 0.0, df['Total_Spent'] / df['Quantity'], df['Price_Per_Unit'])

df['Total_Spent'] = np.where(df['Total_Spent'] == 0.0, df['Quantity'] * df['Price_Per_Unit'], df['Total_Spent'])

> 📸 [INSERT SCREENSHOT HERE: Code block showing numeric calculations or updated dataframe]

---

### 5. Cleaning Categorical & Date Columns
* **`Payment_Method` & `Location`**: Stripped excess spaces, standardized casing to Title Case, and replaced `'ERROR'` or missing values (`NaN`) with `'Unknown'`[cite: 1].
* **`Transaction_Date`**: Standardized invalid strings (`'ERROR'`) and missing dates with `'Unknown'`[cite: 1].

# Clean Payment Method
df['Payment_Method'] = df['Payment_Method'].str.title().str.replace('ERROR', 'Unknown').fillna('Unknown').str.strip()

# Clean Location
df['Location'] = df['Location'].str.title().str.replace('ERROR', 'Unknown').fillna('Unknown').str.strip()

# Clean Transaction Date
df['Transaction_Date'] = df['Transaction_Date'].fillna('Unknown').str.replace('ERROR', 'Unknown')

---

## 📊 Final Dataset Preview

After completing the pipeline, the dataset was inspected to verify clean data types, complete numerical fields, and consistent categorical formatting[cite: 1].

df.head(20)

> 📸 [INSERT SCREENSHOT HERE: Output of `df.head(20)` showing clean columns and data]

---

## 💾 Output
The cleaned dataset was exported to a clean CSV file ready for visualization and reporting[cite: 1]:

df.to_csv('Coffee_clean.csv', index=False)
