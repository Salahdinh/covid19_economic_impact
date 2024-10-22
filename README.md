# S&P 500 and Unemployment Rate Analysis Using FRED API

## Project Overview
In this project, I used the FRED (Federal Reserve Economic Data) API to analyze the S&P 500 index and general and state-level unemployment rates. I explored how to extract, clean, and visualize this data using Python, focusing on revealing economic trends and comparisons.

## Python libraries installed:
- `pandas`
- `numpy`
- `matplotlib`
- `fredapi`
I had my FRED API key set up, replacing the placeholder in the script with the actual key.

## Data Analysis Steps

### 1. Setting Up the FRED API
I began by importing essential libraries and setting up the FRED API using my API key. This allowed me to access a variety of economic datasets directly.

```python
!pip install fredapi > /dev/null

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Set style and display options
plt.style.use('fivethirtyeight')
pd.set_option('display.max_columns', 500)
color_pal = plt.rcParams["axes.prop_cycle"].by_key()["color"]

# Import FRED API key
from fredapi import Fred
fred_key = '[API-KEY]'
```

### 2. Create The Fred Object
With the FRED API key, I created a `Fred` object, which enabled me to interact with the FRED database.

```python
fred = Fred(api_key=fred_key)
```

### 3. Pulling and Visualizing S&P 500 Data
I fetched the S&P 500 data using the FRED API. Then, I used `matplotlib` to create a time series plot. This visualization helped me observe the overall growth and the dips caused by economic events, particularly the impact of COVID-19. 

```python
sp500 = fred.get_series(series_id='SP500')
sp500.plot(figsize=(10, 5), title='S&P 500', lw=2)
plt.show()
```
![image](https://github.com/user-attachments/assets/1ad57669-7f82-4988-860d-d00793fcfb40)

### 4. Pulling and Analyzing General Unemployment Rate Data
Next, I analyzed the national unemployment rate. I searched for the relevant data and visualized it to see fluctuations over time, which helped me understand how economic conditions changed during the pandemic.

```python
un_emp = fred.search('unemployment')
unrate = fred.get_series('UNRATE')
unrate.plot()
plt.title('National Unemployment Rate')
plt.show()
```
![image](https://github.com/user-attachments/assets/aa86884b-ad01-44d8-80ef-aaf637324dcd)

### 5. Analyzing May 2020 Unemployment Rate per State
To examine how different states were impacted during the early months of the COVID-19 pandemic, I focused on the unemployment rates in May 2020. This analysis revealed the disparities among states, helping me identify which states were hit hardest.

```python
unemp_df = fred.search('unemployment rate state', filter=('frequency','Monthly'))
unemp_df = unemp_df.query('seasonal_adjustment == "Seasonally Adjusted" and units == "Percent"')
unemp_df = unemp_df.loc[unemp_df['title'].str.contains('Unemployment Rate')]
all_results = []

for myid in unemp_df.index:
    results = fred.get_series(myid)
    results = results.to_frame(name=myid)
    all_results.append(results)
    time.sleep(0.1)  # Avoiding too fast requests to prevent being blocked

uemp_results = pd.concat(all_results, axis=1)

# Dropping unnecessary columns
cols_to_drop = [col for col in uemp_results if len(col) > 4]
uemp_results = uemp_results.drop(columns=cols_to_drop, axis=1)
uemp_states = uemp_results.dropna().copy()

# Converting FRED IDs to state names for clarity
id_to_state = unemp_df['title'].str.replace('Unemployment Rate in ', '').to_dict()
uemp_states.columns = [id_to_state.get(c, c) for c in uemp_states.columns]

# Analyzing unemployment rates in May 2020
ax = uemp_states.loc[uemp_states.index == '2020-05-01'].T \
    .sort_values('2020-05-01') \
    .plot(kind='barh', figsize=(8, 12), width=0.7, edgecolor='black',
          title='Unemployment Rate by State, May 2020')
ax.legend().remove()
ax.set_xlabel('% Unemployed')
plt.show()
```
![image](https://github.com/user-attachments/assets/4bed77d3-594b-4a02-85d1-97fac69be5e6)

## Conclusions
1. **S&P 500 Trends**: COVID-19 caused a sharp drop, but the market recovered faster than after the 2008 crash due to the COVID relief bill.
2. **General Unemployment**: Unemployment spiked higher than in 2008 but bounced back quickly.  
3. **State-Level Unemployment (May 2020)**: Nevada, Hawaii, and Michigan had the highest rates, showing COVID-19’s uneven impact across states.
