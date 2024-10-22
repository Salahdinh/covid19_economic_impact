# S&P 500 and Unemployment Rate Analysis Using FRED API

## Overview
This project demonstrates how to use Python and the [FRED API](https://fred.stlouisfed.org/) to fetch and analyze economic data, focusing on the S&P 500 index and state-level unemployment rates. It provides a step-by-step guide to data fetching, processing, and visualization using `pandas`, `matplotlib`, and `plotly`. The aim is to make data analysis approachable for beginners and to offer insights into the U.S. economy, particularly during the COVID-19 pandemic.

## Project Structure


## Requirements
Ensure you have the following Python libraries installed:
- `pandas`
- `numpy`
- `matplotlib`
- `plotly`
- `fredapi`

## Installation
1. Clone the repository:
    ```
    git clone https://github.com/your-username/your-repository.git
    ```
2. Navigate to the project directory:
    ```
    cd your-repository
    ```
3. Install the required packages:
    ```
    pip install -r requirements.txt
    ```

## Usage
To run the analysis, execute:

Make sure you have your FRED API key available and replace the placeholder in the code with your actual key.

## Data Analysis Steps

### 1. Setting Up the FRED API
We begin by importing essential libraries and setting up the FRED API using an API key, which provides access to various economic datasets directly from the Federal Reserve.

### 2. Searching for Economic Data
The `fred.search()` function is used to find relevant economic data, such as the S&P 500 and unemployment rates. For example:
```python
sp_search = fred.search('S&P', order_by='popularity')

This makes it easy to identify and retrieve datasets for analysis.

### 3. Visualizing S&P 500 Data
We pull the S&P 500 data and use `matplotlib` to create a time series graph that visualizes trends over time:
```python
sp500 = fred.get_series(series_id='SP500')
sp500.plot(figsize=(10, 5), title='S&P 500', lw=2)
```
This helps us understand market fluctuations and overall trends.

### 4. Analyzing Unemployment Rate Data
We fetch state-level unemployment data, filter it, and consolidate it into a single `pandas` DataFrame. Using `plotly`, we can create interactive line plots for in-depth exploration:
```python
unemp_df = fred.search('unemployment rate state', filter=('frequency','Monthly'))
unemp_df = unemp_df.query('seasonal_adjustment == "Seasonally Adjusted" and units == "Percent"')
```
Data cleaning and transformation ensure that we have accurate and useful information.

### 5. Mapping FRED IDs to State Names
To make the data easier to understand, we map the original FRED IDs to state names using a dictionary (`id_to_state`):
```python
unemployment_states.columns = [
    id_to_state.get(col, col) for col in unemployment_states.columns
]
```
This step replaces cryptic IDs with recognizable state names, clarifying the data when we analyze or plot it.

### 6. Analyzing April 2020 Unemployment Rates
We analyze unemployment rates for April 2020 to understand the early impact of the COVID-19 pandemic. The data is sorted and visualized in a horizontal bar plot:
```python
april_2020_data = unemployment_states.loc['2020-04-01'].sort_values()
fig, ax = plt.subplots(figsize=(10, 5))
april_2020_data.plot(kind='bar', ax=ax)
ax.set_title("Unemployment Rate by State - April 2020")
ax.set_xlabel("State")
ax.set_ylabel("Unemployment Rate (%)")
ax.legend().remove()
plt.show()
```
This plot clearly illustrates how different states were affected during the peak of the pandemic, providing valuable insights.

## Results
The project yields several key insights:
1. **S&P 500 Trends**: Clear visualization of the S&P 500 over time, showing how the market has performed historically.
2. **State-Level Unemployment**: Analysis of unemployment rates across states, highlighting regional economic differences.
3. **COVID-19 Impact**: Specific focus on unemployment rates during April 2020, showing which states were hardest hit by the pandemic.
