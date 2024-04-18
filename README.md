# Module 5 challenge - Analyzing Athletic Sales Data for Women's Footwear


## Table of Contents
1. [Usage and Introduction](#usage-and-introduction)
2. [Data Preparation](#data-preparation)
   - [Combine and Clean the Data](#combine-and-clean-the-data)
   - [Check for Null Values](#check-for-null-values)
   - [Convert Data Types](#convert-data-types)
3. [Determine the Region with the Most Products Sold](#determine-the-region-with-the-most-products-sold)
   - [Using Groupby](#using-groupby)
   - [Using Pivot Table](#using-pivot-table)
4. [Determine the Region with the Most Sales](#determine-the-region-with-the-most-sales)
   - [Using Groupby](#using-groupby-1)
   - [Using Pivot Table](#using-pivot-table-1)
5. [Determine the Retailer with the Most Sales](#determine-the-retailer-with-the-most-sales)
   - [Using Groupby](#using-groupby-2)
   - [Using Pivot Table](#using-pivot-table-2)
6. [Determine the Retailer that Sold the Most Women's Athletic Footwear](#determine-the-retailer-that-sold-the-most-womens-athletic-footwear)
   - [Using Groupby](#using-groupby-3)
   - [Using Pivot Table](#using-pivot-table-3)
7. [Determine the Day with the Most Women's Athletic Footwear Sales](#determine-the-day-with-the-most-womens-athletic-footwear-sales)
8. [Determine the Week with the Most Women's Athletic Footwear Sales](#determine-the-week-with-the-most-womens-athletic-footwear-sales)

## 1. Usage and Introduction

This project aims to analyze the athletic sales data for women's footwear. The analysis will focus on determining the region, retailer, and time periods with the highest sales and product demand.

## 2. Data Preparation - Combine and Clean the Data

The code starts by importing the necessary libraries, including Pandas and NumPy. It then reads the CSV files containing the 2020 and 2021 sales data into separate DataFrames (`df_2020` and `df_2021`).

### Import Libraries and Dependencies

```python
import pandas as pd
import numpy as np
```

### Read the CSV files into DataFrames.

```python
df_2020 = pd.read_csv('Resources/athletic_sales_2020.csv')
df_2021 = pd.read_csv('Resources/athletic_sales_2021.csv')
```

### Combine the 2020 and 2021 sales DataFrames on the rows and reset the index.

The code then combines the two DataFrames into a single DataFrame called `combined_df` using the `pd.concat()` function.

`combined_df = pd.concat([df_2020, df_2021], ignore_index=True)`

## Check for Null Values

The code checks if the `combined_df` contains any null values. In this case, the DataFrame does not contain any null values.

```python
if combined_df.isnull().values.any():
    print("The DataFrame contains null values.")
else:
    print("The DataFrame does not contain null values.")
```


## Convert Data Types

The code checks the data types of the columns in the `combined_df` and converts the `invoice_date` column to a datetime data type using `pd.to_datetime()`.

`combined_df['invoice_date'] = pd.to_datetime(combined_df['invoice_date'], format='%m/%d/%y')
`
## 3. Determine the Region with the Most Products Sold - Using Groupby

The code uses the `groupby()` function to group the `combined_df` by region, state, and city, and then sums the `units_sold` column to get the total products sold for each group. The resulting DataFrame is sorted in descending order by the `Total_Products_Sold` column, and the top 5 results are displayed.

### Rename the sum to "Total_Products_Sold"
`products_sold = region_sold.rename(columns={'units_sold': 'Total_Products_Sold'})
`
### Sort the DataFrame in descending order based on "Total_Products_Sold"
`products_sold = products_sold.sort_values(by='Total_Products_Sold', ascending=False)
`
### Show the top 5 results
`products_sold.head(5)
`

## Using Pivot Table

The code uses the `pivot_table()` function to create a pivot table with the region, state, and city as the index, and the `units_sold` column as the values, which are summed. The resulting DataFrame is sorted in descending order by the `Total_Products_Sold` column, and the top 5 results are displayed.

### Show the number products sold for region, state, and city
```python

`product_sales_piv = pd.pivot_table(combined_df, 
                                   values='units_sold', 
                                   index=['region', 'state', 'city'], 
                                   aggfunc='sum')

product_sales_piv.rename(columns={'units_sold': 'Total_Products_Sold'}, inplace=True)
product_sales_piv = product_sales_piv.sort_values(by='Total_Products_Sold', ascending=False)
`
```
## Show the top 5 results
`print(product_sales_piv.head(5))`


## 4. Determine the Region with the Most Sales - Using Groupby

The code uses the `groupby()` function to group the `combined_df` by region, state, and city, and then sums the `total_sales` column to get the total sales for each group. The resulting DataFrame is sorted in descending order by the `Total Sales` column, and the top 5 results are displayed.

### Show the total sales for the products sold for each region, state, and city.
`total_sales = combined_df.groupby(['region', 'state', 'city'])['total_sales'].sum().reset_index()`

### Rename the "total_sales" column to "Total Sales"
`total_sales = total_sales.rename(columns={'total_sales': 'Total Sales'})
total_sales = total_sales.sort_values(by='Total Sales', ascending=False)
`
### Show the top 5 results.
`print("Top 5 retailers with the most sales:")
 print(total_sales.head(5))
`

## Using Pivot Table

The code uses the `pivot_table()` function to create a pivot table with the region, state, and city as the index, and the `total_sales` column as the values, which are summed. The resulting DataFrame is sorted in descending order by the `Total Sales` column, and the top 5 results are displayed.

### Show the total sales for the products sold for each region, state, and city.
```python
sales_pivot = pd.pivot_table(combined_df, 
                             values='total_sales', 
                             index=['region', 'state', 'city'], 
                             aggfunc='sum')
```

### Rename the "total_sales" column to "Total Sales"
```python
sales_pivot = sales_pivot.rename(columns={'total_sales': 'Total Sales'})
sales_pivot = sales_pivot.sort_values(by='Total Sales', ascending=False)
```

### Show the top 5 results
`print("Top 5 regions with the most sales:")
print(sales_pivot.head(5))`

## 5. Determine the Retailer with the Most Sales - Using Groupby

*** _I received help from the BCS tutor for this code below on 4/16/2024_ ***

The code uses the `groupby()` function to group the `combined_df` by retailer, region, state, and city, and then sums the `total_sales` column to get the total sales for each group. The resulting DataFrame is sorted in descending order by the `total_sales` column, and the top 5 results are displayed.
Use either the groupby function to create a multi-index DataFrame 
with the "retailer", "region", "state", and "city" columns.

```python
most_sales = combined_df.groupby(['region', 'state', 'city'])['total_sales'].agg(total_sales=("sum"))
most_sales = most_sales.sort_values(by=("total_sales"), ascending=False)

```
### Show the top 5 results
`most_sales.head(5)
`

## Using Pivot Table

The code uses the `pivot_table()` function to create a pivot table with the retailer, region, state, and city as the index, and the `total_sales` column as the values, which are summed. The resulting DataFrame is sorted in descending order by the `Total Sales` column, and the top 5 results are displayed.

### Show the total sales for the products sold for each retailer, region, state, and city.
```python
sales_pivot = pd.pivot_table(combined_df, values='total_sales', index=['retailer', 'region', 'state', 'city'], aggfunc='sum')
```

### Optional: Rename the "total_sales" column to "Total Sales"
```python
sales_pivot = sales_pivot.rename(columns={'total_sales': 'Total Sales'})
sales_pivot = sales_pivot.sort_values(by='Total Sales', ascending=False)
```

### Show the top 5 results
`sales_pivot.head(5)`

## 6. Determine the Retailer that Sold the Most Women's Athletic Footwear - Using Groupby

The code first filters the `combined_df` to only include rows where the `product` column is "Women's Athletic Footwear". It then uses the `groupby()` function to group the filtered DataFrame by retailer, region, state, and city, and sums the `units_sold` column to get the total units of women's athletic footwear sold for each group. The resulting DataFrame is sorted in descending order by the `Womens_Footwear_Units_Sold` column, and the top 5 results are displayed.

### Show the total number of women's athletic footwear sold for each retailer, region, state, and city.
```python
womens_sales = womens_total_sales.groupby(['retailer','region','state','city'])['units_sold'].agg(Womens_Footwear_Units_Sold=("sum"))
```

### Sort the top 5 results
`womens_sales.sort_values(by=(['Womens_Footwear_Units_Sold']), ascending=False).head()
`

## Using Pivot Table

The code first filters the `combined_df` to only include rows where the `product` column is "Women's Athletic Footwear". It then uses the `pivot_table()` function to create a pivot table with the retailer, region, state, and city as the index, and the `units_sold` column as the values, which are summed. The resulting DataFrame is sorted in descending order by the `Womens_Footwear_Units_Sold` column, and the top 5 results are displayed.

### Create the pivot table
```python
womens_sales_pivot = combined_df.loc[combined_df['product'] == "Women's Athletic Footwear"].pivot_table(
    index=['retailer', 'region', 'state', 'city'],
    values='units_sold',
    aggfunc='sum',
    fill_value=0
)
```

### Reset the index to make it a DataFrame
`womens_sales_pivot = womens_sales_pivot.reset_index()
`
### Rename the 'units_sold' column
```python
womens_sales_pivot = womens_sales_pivot.rename(columns={'units_sold': 'Womens_Footwear_Units_Sold'})
```

### Sort the pivot table by 'Womens_Footwear_Units_Sold' in descending order
```python
womens_sales_pivot = womens_sales_pivot.sort_values(by='Womens_Footwear_Units_Sold', ascending=False)
```

### Show the top 5 results
`womens_sales_pivot.head()`

## 7. Determine the Day with the Most Women's Athletic Footwear Sales

*** _Alberto helped me fix/troubleshoot my original code below during office hours on 4/17/24_ ***

The code first filters the `combined_df` to only include rows where the `product` column is "Women's Athletic Footwear". It then creates a pivot table with the `invoice_date` as the index and the `total_sales` column as the values, which are summed. The resulting DataFrame is sorted in descending order by the `Total_Sales` column, and the top 10 results are displayed.

### Create a pivot table with the 'invoice_date' column is the index, and the "total_sales" as the values
`pivot_table = womens_total_sales.pivot_table(index=['invoice_date'], values='total_sales', aggfunc='sum')`

### Rename the "total_sales" column to "Total Sales"
`pivot_table = pivot_table.rename(columns={'total_sales': 'Total_Sales'})
`
### Resample the pivot table into daily bins, and get the total sales for each day.
`daily_sales = pivot_table.resample('D').sum()
`
### Sort the resampled pivot table in ascending order on "Total Sales"
`daily_sales_sorted = daily_sales.sort_values(by='Total_Sales', ascending=False)
`
### Show the top 10 results
`daily_sales_sorted.head(10)
`

## 8. Determine the Week with the Most Women's Athletic Footwear Sales

*** _Alberto helped me fix/troubleshoot this code below as well during office hours on 4/17/24_ ***

The code first filters the `combined_df` to only include rows where the `product` column is "Women's Athletic Footwear". It then creates a pivot table with the `invoice_date` as the index and the `total_sales` column as the values, which are summed. The resulting DataFrame is resampled to a weekly frequency using the `resample('W')` function, and the resulting weekly sales are sorted in descending order by the `Total_Sales` column. The top 10 results are displayed.

### Resample the pivot table into weekly bins, and get the total sales for each week
`weekly_sales = pivot_table.resample('W').sum()`

### Sort the resampled pivot table in ascending order on "Total Sales" and show top 10 results
```python
weekly_sales = weekly_sales.sort_values(by='Total_Sales', ascending=False).head(10)
weekly_sales.head(10)
```

## License
[NA]

```