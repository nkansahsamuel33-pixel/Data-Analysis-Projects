# ☕ Coffee Sales Data Cleaning with Pandas

A complete data cleaning project using **Python, Pandas, and NumPy** to prepare a messy coffee sales dataset for analysis.

The objective of this project was to identify and resolve data quality issues such as missing values, inconsistent formatting, invalid entries, incorrect data types, and incomplete transactions.

---

## 📊 Project Overview

Real-world datasets are rarely clean. This dataset contained:

- Missing values
- Inconsistent text formatting
- Invalid values (`ERROR`)
- Incorrect data types
- Missing transaction information
- Missing numerical values that could be calculated from existing data

The dataset was cleaned step-by-step to make it suitable for further analysis and visualization.

---

## 🛠 Technologies Used

- Python
- Pandas
- NumPy
- Jupyter Notebook

---

# Dataset Loading

The dataset was imported using Pandas.

```python
import pandas as pd
import numpy as np

df = pd.read_csv("dirty_cafe_sales (1).csv")
```

📸 **Screenshot:** Dataset preview (`df.head()`)

---

# Data Inspection

Before cleaning, the dataset structure and missing values were examined.

```python
df.info()
df.isna().mean()*100
```

This helped identify:

- Missing values
- Incorrect data types
- Columns requiring cleaning

📸 **Screenshot:** `df.info()`

📸 **Screenshot:** Missing value percentages

---

# Cleaning Column Names

Column names contained spaces, which were replaced with underscores.

```python
df.columns = df.columns.str.replace(" ", "_")
```

### Before

```
Price Per Unit
```

### After

```
Price_Per_Unit
```

---

# Cleaning the Item Column

The **Item** column contained:

- Missing values
- Extra spaces
- Inconsistent capitalization
- "ERROR" values

The cleaning process:

```python
df["Item"] = (
    df["Item"]
    .str.strip()
    .str.title()
    .fillna("Unknown")
    .replace("Error", "Unknown")
)
```

This ensured all product names were standardized.

📸 **Screenshot:** Before and after cleaning

---

# Removing Invalid Records

Some transactions had both:

- Quantity missing
- Price per unit missing

These records could not be recovered and were removed.

```python
df = df.dropna(
    subset=["Quantity","Price_Per_Unit"],
    how="all"
)
```

Likewise, records missing both:

- Quantity
- Total Spent

were also removed.

```python
df = df.dropna(
    subset=["Quantity","Total_Spent"],
    how="all"
)
```

---

# Handling ERROR Values

Several numeric columns contained the string:

```
ERROR
```

These were replaced with **0** before converting to numeric values.

```python
df["Quantity"] = df["Quantity"].replace("ERROR",0).fillna(0)

df["Price_Per_Unit"] = df["Price_Per_Unit"].replace("ERROR",0).fillna(0)

df["Total_Spent"] = df["Total_Spent"].replace("ERROR",0).fillna(0)
```

---

# Converting Data Types

The numeric columns were converted from strings to numeric values.

```python
df["Quantity"] = pd.to_numeric(df["Quantity"], errors="coerce")

df["Price_Per_Unit"] = pd.to_numeric(df["Price_Per_Unit"], errors="coerce")

df["Total_Spent"] = pd.to_numeric(df["Total_Spent"], errors="coerce")
```

Using `errors="coerce"` converts invalid values into `NaN` instead of causing an error.

---

# Recovering Missing Numeric Values

Rather than leaving missing values empty, they were calculated using:

```
Total Spent = Quantity × Price Per Unit
```

The notebook recovered missing values with:

```python
Quantity = Total Spent / Price Per Unit

Price Per Unit = Total Spent / Quantity

Total Spent = Quantity × Price Per Unit
```

using `numpy.where()`.

This preserved as many records as possible instead of deleting them.

📸 **Screenshot:** Code cell calculating missing values

---

# Cleaning Payment Method

The Payment Method column contained:

- ERROR
- Missing values
- Inconsistent capitalization
- Leading/trailing spaces

Cleaning performed:

```python
df["Payment_Method"] = (
    df["Payment_Method"]
    .str.title()
    .str.replace("ERROR","Unknown")
    .fillna("Unknown")
    .str.strip()
)
```

---

# Cleaning Location

The Location column contained similar issues.

```python
df["Location"] = (
    df["Location"]
    .str.title()
    .str.replace("ERROR","Unknown")
    .fillna("Unknown")
    .str.strip()
)
```

---

# Cleaning Transaction Dates

Missing transaction dates and invalid values were replaced with:

```
Unknown
```

```python
df["Transaction_Date"] = (
    df["Transaction_Date"]
    .fillna("Unknown")
    .str.replace("ERROR","Unknown")
)
```

---

# Final Dataset

After cleaning, the dataset contained:

- Standardized text values
- Correct column names
- Numeric data types
- Reduced missing values
- Consistent categorical values
- Recovered numerical information
- Invalid records removed

The cleaned dataset was then ready for:

- Exploratory Data Analysis (EDA)
- Visualization
- Dashboard creation


---

# Skills Demonstrated

- Data Cleaning
- Missing Value Treatment
- Feature Standardization
- String Manipulation
- Data Type Conversion
- Conditional Data Recovery
- Pandas
- NumPy
- Data Validation

---


```

---

## 👤 Author

**Samuel Nkansah**

Data Analyst | Python | SQL | Power BI | Excel

---

## 💾 Output
The cleaned dataset was exported to a clean CSV file ready for visualization and reporting[cite: 1]:

df.to_csv('Coffee_clean.csv', index=False)
