# Uber Dataset Analysis

## Project Description
This project analyzes Uber ride data to derive insights about travel patterns, usage, and other important metrics. It involves checking the dataset for missing values, duplicates, and generating meaningful insights about the data.

### 1. Libraries Used
The following libraries are required to run the analysis:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 2. Load the Data
Load the dataset using pandas:
```python
#dataset path 
df = pd.read_csv("D:\\YBI\\Uber Data\\UberDataset.csv") 
```

### 3. Data Checks to Perform
- **Check for missing values**
- **Check for duplicates**
- **Check data types**
- **Check the number of unique values for each column**
- **Check the summary statistics of the dataset**
- **Check the various categories in the categorical columns**

### 4. Initial Exploration
Preview the first few and last few rows of the dataset:
```python
df.head()
df.tail()
```

### 5. Dataset Information
Get a quick overview of the dataset:
```python
df.info()
```

### 6. Check for Missing Values
Identify missing values in the dataset:
```python
df.isnull().sum()
```

### Insights:
- There are some missing values, especially in the "PURPOSE" column, which contains a significant number of missing values.

### 7. Handling Missing Values
- Replace missing values in the `PURPOSE` column with "UNKNOWN":
```python
df["PURPOSE"].fillna("UNKNOWN", inplace=True)
df.isnull().sum()  # Confirm no missing values remain
```

### 8. Handling Duplicates
- Check for and remove duplicates:
```python
df[df.duplicated()]
df.drop_duplicates(inplace=True)
```

### 9. Convert Date Columns to Datetime Format
Convert the `START_DATE` and `END_DATE` columns to `datetime` format, and extract useful features like hour, day, and month:
```python
df['START_DATE'] = pd.to_datetime(df['START_DATE'])
df['END_DATE'] = pd.to_datetime(df['END_DATE'])

# Extract time, day, and month features
df['TIME_DAY'] = df['START_DATE'].apply(lambda i: i.hour)
df['TIME_OF_DAY'] = pd.cut(df['START_DATE'].apply(lambda i: i.hour),
                            bins=[0, 6, 11, 17, 21, 24],
                            labels=['Night', 'Morning', 'Afternoon', 'Evening', 'Night'])

df['DAY_OF_THE_RIDE'] = df['START_DATE'].apply(lambda i: i.weekday())
day_label = {0: 'Mon', 1: 'Tues', 2: 'Wed', 3: 'Thus', 4: 'Fri', 5: 'Sat', 6: 'Sun'}
df['DAY_OF_THE_RIDE'] = df['DAY_OF_THE_RIDE'].map(day_label)

df['MONTH_OF_THE_RIDE'] = df['START_DATE'].apply(lambda i: i.month)
month_label = {1: 'Jan', 2: 'Feb', 3: 'Mar', 4: 'April', 5: 'May', 6: 'June', 7: 'July', 8: 'Aug', 9: 'Sep', 10: 'Oct', 11: 'Nov', 12: 'Dec'}
df['MONTH_OF_THE_RIDE'] = df['MONTH_OF_THE_RIDE'].map(month_label)

# Calculate the duration of the ride in minutes
df["DURATION_OF_THE_RIDE_MINUTE"] = (df["END_DATE"] - df["START_DATE"]).astype('timedelta64[m]')
```

### 10. Statistical Summary
Generate descriptive statistics for the numerical columns:
```python
df.describe()
```

### 11. Final Cleaned Dataset
Preview the cleaned dataset:
```python
df.head()
```
