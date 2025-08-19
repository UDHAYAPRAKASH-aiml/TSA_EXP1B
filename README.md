# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
## Date: 19-08-25

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
### Load dataset
data = pd.read_csv("C:\\Users\\admin\\time series\\housing_price_dataset.csv")
# Group by YearBuilt to create a time series of average prices per year
price_series = data.groupby("YearBuilt")["Price"].mean().reset_index()
# Convert YearBuilt to datetime for time series analysis
price_series['YearBuilt'] = pd.to_datetime(price_series['YearBuilt'], format='%Y')
price_series.set_index('YearBuilt', inplace=True)
# Original series
series = price_series['Price']
# 1. Regular Differencing
price_series['diff'] = series - series.shift(1)
# 2. Log Transformation
price_series['log'] = np.log(series)
# 3. Log Differencing
price_series['log_diff'] = price_series['log'] - price_series['log'].shift(1)
# 4. Seasonal Decomposition (using log transformed data)
result = seasonal_decompose(price_series['log'].dropna(), model='additive', period=5)
# 5. Seasonal Differencing
price_series['log_seasonal_diff'] = price_series['log_diff'] - price_series['log_diff'].shift(5)
# ---- Plotting ----
plt.figure(figsize=(16, 16))
plt.subplot(6, 1, 1)
plt.plot(series, label='Original')
plt.legend(loc='best')
plt.title('Original Data (Average Price per Year)')
plt.subplot(6, 1, 2)
plt.plot(price_series['diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')

plt.subplot(6, 1, 3)
plt.plot(result.trend, label='Trend (Decomposition)')
plt.legend(loc='best')
plt.title('Trend (from Seasonal Decomposition)')

plt.subplot(6, 1, 4)
plt.plot(price_series['log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')

plt.subplot(6, 1, 5)
plt.plot(price_series['log_diff'], label='Log Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Differencing')

plt.subplot(6, 1, 6)
plt.plot(price_series['log_seasonal_diff'], label='Log + Differencing + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log + Regular + Seasonal Differencing')

plt.tight_layout()
plt.show()
### OUTPUT:
```
<img width="1463" height="722" alt="Screenshot (163)" src="https://github.com/user-attachments/assets/8a7d9fa9-6cd3-42c6-a510-93e05b744266" />

<img width="1409" height="693" alt="Screenshot (164)" src="https://github.com/user-attachments/assets/22fd6638-b3d7-4c35-a07a-b19db0d4f141" />




RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger data.
