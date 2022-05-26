## This is an exploration of some company sales data
Huge thanks to freeCodeCamp.org for the dataset


```python
# Import my libraries
import os
import re
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
# Change to the downloads directory
os.chdir("Downloads")
```


```python
df = pd.read_csv("sales_data.csv")
```


```python
# Preview the dataframe
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Day</th>
      <th>Month</th>
      <th>Year</th>
      <th>Customer_Age</th>
      <th>Age_Group</th>
      <th>Customer_Gender</th>
      <th>Country</th>
      <th>State</th>
      <th>Product_Category</th>
      <th>Sub_Category</th>
      <th>Product</th>
      <th>Order_Quantity</th>
      <th>Unit_Cost</th>
      <th>Unit_Price</th>
      <th>Profit</th>
      <th>Cost</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-11-26</td>
      <td>26</td>
      <td>November</td>
      <td>2013</td>
      <td>19</td>
      <td>Youth (&lt;25)</td>
      <td>M</td>
      <td>Canada</td>
      <td>British Columbia</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>8</td>
      <td>45</td>
      <td>120</td>
      <td>590</td>
      <td>360</td>
      <td>950</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-11-26</td>
      <td>26</td>
      <td>November</td>
      <td>2015</td>
      <td>19</td>
      <td>Youth (&lt;25)</td>
      <td>M</td>
      <td>Canada</td>
      <td>British Columbia</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>8</td>
      <td>45</td>
      <td>120</td>
      <td>590</td>
      <td>360</td>
      <td>950</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-03-23</td>
      <td>23</td>
      <td>March</td>
      <td>2014</td>
      <td>49</td>
      <td>Adults (35-64)</td>
      <td>M</td>
      <td>Australia</td>
      <td>New South Wales</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>23</td>
      <td>45</td>
      <td>120</td>
      <td>1366</td>
      <td>1035</td>
      <td>2401</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-03-23</td>
      <td>23</td>
      <td>March</td>
      <td>2016</td>
      <td>49</td>
      <td>Adults (35-64)</td>
      <td>M</td>
      <td>Australia</td>
      <td>New South Wales</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>20</td>
      <td>45</td>
      <td>120</td>
      <td>1188</td>
      <td>900</td>
      <td>2088</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-05-15</td>
      <td>15</td>
      <td>May</td>
      <td>2014</td>
      <td>47</td>
      <td>Adults (35-64)</td>
      <td>F</td>
      <td>Australia</td>
      <td>New South Wales</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>4</td>
      <td>45</td>
      <td>120</td>
      <td>238</td>
      <td>180</td>
      <td>418</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-05-15</td>
      <td>15</td>
      <td>May</td>
      <td>2016</td>
      <td>47</td>
      <td>Adults (35-64)</td>
      <td>F</td>
      <td>Australia</td>
      <td>New South Wales</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>5</td>
      <td>45</td>
      <td>120</td>
      <td>297</td>
      <td>225</td>
      <td>522</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2014-05-22</td>
      <td>22</td>
      <td>May</td>
      <td>2014</td>
      <td>47</td>
      <td>Adults (35-64)</td>
      <td>F</td>
      <td>Australia</td>
      <td>Victoria</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>4</td>
      <td>45</td>
      <td>120</td>
      <td>199</td>
      <td>180</td>
      <td>379</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016-05-22</td>
      <td>22</td>
      <td>May</td>
      <td>2016</td>
      <td>47</td>
      <td>Adults (35-64)</td>
      <td>F</td>
      <td>Australia</td>
      <td>Victoria</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>2</td>
      <td>45</td>
      <td>120</td>
      <td>100</td>
      <td>90</td>
      <td>190</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2014-02-22</td>
      <td>22</td>
      <td>February</td>
      <td>2014</td>
      <td>35</td>
      <td>Adults (35-64)</td>
      <td>M</td>
      <td>Australia</td>
      <td>Victoria</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>22</td>
      <td>45</td>
      <td>120</td>
      <td>1096</td>
      <td>990</td>
      <td>2086</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2016-02-22</td>
      <td>22</td>
      <td>February</td>
      <td>2016</td>
      <td>35</td>
      <td>Adults (35-64)</td>
      <td>M</td>
      <td>Australia</td>
      <td>Victoria</td>
      <td>Accessories</td>
      <td>Bike Racks</td>
      <td>Hitch Rack - 4-Bike</td>
      <td>21</td>
      <td>45</td>
      <td>120</td>
      <td>1046</td>
      <td>945</td>
      <td>1991</td>
    </tr>
  </tbody>
</table>
</div>




```python
# First we check for any missing data
for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}%'.format(col, pct_missing))
```

    Date - 0.0%
    Day - 0.0%
    Month - 0.0%
    Year - 0.0%
    Customer_Age - 0.0%
    Age_Group - 0.0%
    Customer_Gender - 0.0%
    Country - 0.0%
    State - 0.0%
    Product_Category - 0.0%
    Sub_Category - 0.0%
    Product - 0.0%
    Order_Quantity - 0.0%
    Unit_Cost - 0.0%
    Unit_Price - 0.0%
    Profit - 0.0%
    Cost - 0.0%
    Revenue - 0.0%
    


```python
# Drop any duplicates
df = df.drop_duplicates()
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 112036 entries, 0 to 113035
    Data columns (total 18 columns):
     #   Column            Non-Null Count   Dtype 
    ---  ------            --------------   ----- 
     0   Date              112036 non-null  object
     1   Day               112036 non-null  int64 
     2   Month             112036 non-null  object
     3   Year              112036 non-null  int64 
     4   Customer_Age      112036 non-null  int64 
     5   Age_Group         112036 non-null  object
     6   Customer_Gender   112036 non-null  object
     7   Country           112036 non-null  object
     8   State             112036 non-null  object
     9   Product_Category  112036 non-null  object
     10  Sub_Category      112036 non-null  object
     11  Product           112036 non-null  object
     12  Order_Quantity    112036 non-null  int64 
     13  Unit_Cost         112036 non-null  int64 
     14  Unit_Price        112036 non-null  int64 
     15  Profit            112036 non-null  int64 
     16  Cost              112036 non-null  int64 
     17  Revenue           112036 non-null  int64 
    dtypes: int64(9), object(9)
    memory usage: 16.2+ MB
    


```python
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Day</th>
      <th>Year</th>
      <th>Customer_Age</th>
      <th>Order_Quantity</th>
      <th>Unit_Cost</th>
      <th>Unit_Price</th>
      <th>Profit</th>
      <th>Cost</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
      <td>112036.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>15.665607</td>
      <td>2014.400925</td>
      <td>35.919508</td>
      <td>11.904254</td>
      <td>267.819603</td>
      <td>453.850628</td>
      <td>286.035194</td>
      <td>471.103333</td>
      <td>757.138527</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.781485</td>
      <td>1.273327</td>
      <td>11.016543</td>
      <td>9.564877</td>
      <td>550.218722</td>
      <td>922.751848</td>
      <td>454.852634</td>
      <td>886.971635</td>
      <td>1312.061623</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>2011.000000</td>
      <td>17.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>-30.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>8.000000</td>
      <td>2013.000000</td>
      <td>28.000000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>5.000000</td>
      <td>29.000000</td>
      <td>28.000000</td>
      <td>64.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>16.000000</td>
      <td>2014.000000</td>
      <td>35.000000</td>
      <td>10.000000</td>
      <td>9.000000</td>
      <td>25.000000</td>
      <td>103.000000</td>
      <td>112.000000</td>
      <td>226.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>23.000000</td>
      <td>2016.000000</td>
      <td>43.000000</td>
      <td>20.000000</td>
      <td>42.000000</td>
      <td>70.000000</td>
      <td>360.000000</td>
      <td>442.000000</td>
      <td>806.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>31.000000</td>
      <td>2016.000000</td>
      <td>87.000000</td>
      <td>32.000000</td>
      <td>2171.000000</td>
      <td>3578.000000</td>
      <td>15096.000000</td>
      <td>42978.000000</td>
      <td>58074.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Look at differnt data types in the dataframe
df.dtypes
```




    Date                object
    Day                  int64
    Month               object
    Year                 int64
    Customer_Age         int64
    Age_Group           object
    Customer_Gender     object
    Country             object
    State               object
    Product_Category    object
    Sub_Category        object
    Product             object
    Order_Quantity       int64
    Unit_Cost            int64
    Unit_Price           int64
    Profit               int64
    Cost                 int64
    Revenue              int64
    dtype: object




```python
# Pie graph of the age group range 
age_group = df['Age_Group'].value_counts()

fig, ax = plt.subplots(figsize = (12,4))

ax = age_group.plot(kind = 'pie')

plt.show()
```


    
![png](output_10_0.png)
    



```python
# Show percent of gender
fig, ax = plt.subplots(figsize = (5,5))

ax = df['Customer_Gender'].value_counts().plot(kind = 'pie')
```


    
![png](output_11_0.png)
    



```python
# Lets look at the correlation
correlation = df.corr()
correlation
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Day</th>
      <th>Year</th>
      <th>Customer_Age</th>
      <th>Order_Quantity</th>
      <th>Unit_Cost</th>
      <th>Unit_Price</th>
      <th>Profit</th>
      <th>Cost</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Day</th>
      <td>1.000000</td>
      <td>-0.007435</td>
      <td>-0.015186</td>
      <td>-0.002845</td>
      <td>0.003520</td>
      <td>0.003578</td>
      <td>0.004714</td>
      <td>0.003493</td>
      <td>0.003995</td>
    </tr>
    <tr>
      <th>Year</th>
      <td>-0.007435</td>
      <td>1.000000</td>
      <td>0.040879</td>
      <td>0.124091</td>
      <td>-0.217431</td>
      <td>-0.213538</td>
      <td>-0.181349</td>
      <td>-0.215449</td>
      <td>-0.208514</td>
    </tr>
    <tr>
      <th>Customer_Age</th>
      <td>-0.015186</td>
      <td>0.040879</td>
      <td>1.000000</td>
      <td>0.027376</td>
      <td>-0.021401</td>
      <td>-0.020301</td>
      <td>0.004388</td>
      <td>-0.016012</td>
      <td>-0.009303</td>
    </tr>
    <tr>
      <th>Order_Quantity</th>
      <td>-0.002845</td>
      <td>0.124091</td>
      <td>0.027376</td>
      <td>1.000000</td>
      <td>-0.516289</td>
      <td>-0.516387</td>
      <td>-0.238770</td>
      <td>-0.340386</td>
      <td>-0.312880</td>
    </tr>
    <tr>
      <th>Unit_Cost</th>
      <td>0.003520</td>
      <td>-0.217431</td>
      <td>-0.021401</td>
      <td>-0.516289</td>
      <td>1.000000</td>
      <td>0.997891</td>
      <td>0.740623</td>
      <td>0.829557</td>
      <td>0.817544</td>
    </tr>
    <tr>
      <th>Unit_Price</th>
      <td>0.003578</td>
      <td>-0.213538</td>
      <td>-0.020301</td>
      <td>-0.516387</td>
      <td>0.997891</td>
      <td>1.000000</td>
      <td>0.749450</td>
      <td>0.825965</td>
      <td>0.818176</td>
    </tr>
    <tr>
      <th>Profit</th>
      <td>0.004714</td>
      <td>-0.181349</td>
      <td>0.004388</td>
      <td>-0.238770</td>
      <td>0.740623</td>
      <td>0.749450</td>
      <td>1.000000</td>
      <td>0.902109</td>
      <td>0.956508</td>
    </tr>
    <tr>
      <th>Cost</th>
      <td>0.003493</td>
      <td>-0.215449</td>
      <td>-0.016012</td>
      <td>-0.340386</td>
      <td>0.829557</td>
      <td>0.825965</td>
      <td>0.902109</td>
      <td>1.000000</td>
      <td>0.988748</td>
    </tr>
    <tr>
      <th>Revenue</th>
      <td>0.003995</td>
      <td>-0.208514</td>
      <td>-0.009303</td>
      <td>-0.312880</td>
      <td>0.817544</td>
      <td>0.818176</td>
      <td>0.956508</td>
      <td>0.988748</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a correlation matrix to uncover some correlations
fig = plt.figure(figsize = (9,9))
plt.matshow(correlation, cmap = 'RdBu_r', fignum = fig.number)
plt.xticks(range(len(correlation)), correlation.columns, rotation = 'vertical')
plt.yticks(range(len(correlation)), correlation.columns)
plt.show()
```


    
![png](output_13_0.png)
    



```python
df.plot(kind = 'scatter', x = 'Revenue', y = 'Cost', figsize = (12,6))
plt.title("Revenue vs Cost")
plt.show()
```


    
![png](output_14_0.png)
    



```python
# Lets look at the calculated cost of the units
df.plot(kind = 'scatter', x = 'Cost', y = 'Profit', figsize = (12,6))
plt.title("Cost vs Profit")
plt.show()
```


    
![png](output_15_0.png)
    



```python
# Now lets look at the average revenue of each state
state_revenue = df.groupby('State', as_index=False).Revenue.mean().sort_values('Revenue', ascending = False)
state_revenue[:10].plot(kind = 'bar', x = 'State', title = 'Average State Revenue')
plt.show()
```


    
![png](output_16_0.png)
    



```python
# Average revenue by age group
age_revenue = df.groupby('Age_Group', as_index=False).Revenue.mean().sort_values('Revenue', ascending = False)
age_revenue.plot(kind = 'bar', x = 'Age_Group', title = "Average Age Group Revenue")
plt.show()
```


    
![png](output_17_0.png)
    



```python
# Average revenue per country
country_revenue = df.groupby('Country', as_index = False).Revenue.mean().sort_values('Revenue', ascending = False)
country_revenue[:10].plot(kind = 'bar', x = 'Country', title = 'Average Country Revenue')
plt.show()
```


    
![png](output_18_0.png)
    



```python
# Average revenue per product
product_revenue = df.groupby('Product', as_index = False).Revenue.mean().sort_values('Revenue', ascending = False)
product_revenue[:10].plot(kind = 'bar', x = 'Product', title = 'Average Product Revenue')
plt.show()
```


    
![png](output_19_0.png)
    



```python
# Average profit per state
state_profit = df.groupby('State', as_index=False).Profit.mean().sort_values('Profit', ascending = False)
state_profit[:10].plot(kind = 'bar', x = 'State', title = 'Average State Profit')
plt.show()
```


    
![png](output_20_0.png)
    



```python
# Average profit per Age Group
age_profit = df.groupby('Age_Group', as_index = False).Profit.mean().sort_values('Profit', ascending = False)
age_profit.plot(kind = 'bar', x = 'Age_Group', title = 'Average Age Group Profit')
plt.show()
```


    
![png](output_21_0.png)
    



```python
# Average profit per Country
country_profit = df.groupby('Country', as_index = False).Profit.mean().sort_values('Profit', ascending = False)
country_profit[:10].plot(kind = 'bar', x = 'Country', title = 'Average Country Profit')
plt.show()
```


    
![png](output_22_0.png)
    



```python
# Average profit per Product
product_revenue = df.groupby('Product', as_index = False).Profit.mean().sort_values('Profit', ascending = False)
product_revenue
product_revenue[:10].plot(kind = 'bar', x = 'Product', title = 'Average Product Profit')
plt.show()
```


    
![png](output_23_0.png)
    



```python

```
