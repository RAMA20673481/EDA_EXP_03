**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**

**Name : Ramanujam G**

**Reg No: 212224240129**


# Q1. Load the dataset and perform preprocessing
```
# Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler, StandardScaler

# Load data
df = pd.read_csv("del-sirifort-cpcb-2024-25 (1).csv")

# Select columns
cols = ['PM2.5 (µg/m³)', 'PM10 (µg/m³)', 'NO2 (µg/m³)', 'Ozone (µg/m³)']
data = df[cols].apply(pd.to_numeric, errors='coerce').fillna(0)

# 1. Preprocessing
print(data.head())
print("Shape:", data.shape)
print("Missing values:\n", data.isnull().sum())
```

# Q2. Distribution analysis using histograms
```
# 2. Distribution - Histogram
data.hist(figsize=(10,7))
plt.show()
```

# Q3. Calculate numerical summary
```
# 3. Numerical Summary
print("\nNumerical Summary:")
print(data.describe().T[['mean', 'min', '25%', '50%', '75%', 'max']])
print("\nMode:")
print(data.mode().iloc[0])
```
# Q4. Calculate measures of spread
```
# 4. Measures of Spread
print("\nMeasures of Spread:")
print("Range:\n", data.max() - data.min())
print("Variance:\n", data.var())
print("Standard Deviation:\n", data.std())
print("CV (%):\n", (data.std()/data.mean())*100)
```

# Q5. Construct box plots and detect outliers using IQR
# Box plot visualization
```
# 5. Boxplot + Outliers
data.plot(kind='box', figsize=(10,6))
plt.show()

for c in cols:
    Q1 = data[c].quantile(.25)
    Q3 = data[c].quantile(.75)
    IQR = Q3 - Q1
    out = data[(data[c] < Q1-1.5*IQR) | (data[c] > Q3+1.5*IQR)]
    print(c, "Outliers:", len(out))
```

# Q6. Perform Min-Max scaling
```
# 6. Min-Max Scaling
mm = MinMaxScaler()
print("\nMin-Max Scaling:")
print(pd.DataFrame(mm.fit_transform(data), columns=cols).head())
```

# Q7. Perform Z-score standardization using StandardScaler
```
# 7. Z-Score Standardization
ss = StandardScaler()
print("\nZ-Score:")
print(pd.DataFrame(ss.fit_transform(data), columns=cols).head())
```

# Q8. Calculate Gini coefficient
```
# 8. Gini Coefficient

def gini(x):
    x = np.sort(x)
    n = len(x)
    return (2 * np.sum((np.arange(1, n+1)) * x)) / (n * np.sum(x)) - (n+1)/n

print("\nGini Coefficient:")

for c in cols:
    print(c, ":", round(gini(data[c].values), 4))
```



**Output**
# Loading the dataset and  preprocessing
<img width="657" height="282" alt="Screenshot 2026-11-21 215820" src="https://github.com/user-attachments/assets/5280eb5f-19f6-48f1-8b8a-640cd79f494b" />

# Distribution analysis using histograms
<img width="740" height="507" alt="Screenshot 2026-11-21 215830" src="https://github.com/user-attachments/assets/ec1074c4-634a-4230-8be9-03e981d82afb" />


# Numerical summary
<img width="678" height="300" alt="Screenshot 2026-11-21 215840" src="https://github.com/user-attachments/assets/7351ee1a-2a94-40e4-9f4b-ee6f4de3a888" />

# Measures of spread
<img width="436" height="557" alt="Screenshot 2026-11-21 215849" src="https://github.com/user-attachments/assets/5c4843a6-cc03-4406-8fc4-7c819e4642e4" />

# Construct box plots and detect outliers using IQR
<img width="725" height="516" alt="Screenshot 2026-11-21 215901" src="https://github.com/user-attachments/assets/1ef0851e-cfe1-4282-ab7e-b244659afb65" />

# Min-Max scaling
<img width="667" height="168" alt="Screenshot 2026-11-21 215908" src="https://github.com/user-attachments/assets/6ff984e2-66e3-4d80-aa0b-937b2170d025" />

# Z-score standardization using StandardScaler
<img width="618" height="162" alt="Screenshot 2026-11-21 215915" src="https://github.com/user-attachments/assets/f02bb6ae-a1c5-4cb8-82d0-8bc7b2b69744" />

# Gini coefficient
<img width="260" height="117" alt="Screenshot 2026-11-21 215921" src="https://github.com/user-attachments/assets/1c48cbc8-9c8f-440b-8d5c-a3941a4e34b3" />



**Interpretation**

1) PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) Ozone levels are comparatively lower than PM2.5 and PM10

3) PM2.5 and PM10 pollutants show considerable variation in their recorded concentrations throughout the observation period.

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


