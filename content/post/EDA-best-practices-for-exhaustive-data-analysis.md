---
title: "Exploratory Data Analysis Best Practices for Exhaustive Data Analysis"
date: 2022-01-31T22:43:22+03:00
lastmod: 2022-01-31T22:43:22+03:00
draft: false
tags: ["EDA", "pandas", "plots"]
categories: ["Data Analysis", "Visualizations"]
author: "Vincent N."
---

<p align="center">
    <img src="/img/blog/luke-chesser-JKUTrJ4vK00-unsplash.jpg" alt="jpg" width="600" height="350"/>

</p>

## **Introduction**

EDA, short for Exploratory Data Analysis, or as I like to call it, **Data Reveal Party** 😊 (because it's fun playing around
and getting your hands dirty with datasets) is one of the most important steps towards a successful data project. You get to
uncover hidden data insights that only this process can unearth. It is therefore good practice to make sure you exhaust on it
by investigating and looking at the dataset from every possible angle. In this article we are going to look at the steps to
follow to accomplish this when doing EDA.

## **EDA Steps**

### **1. General dataset statistics**

Here you investigate the structure of your dataset before doing any visualizations. General dataset stats may include but are not limited to:
- small overview of dataset - head(), tail() or even sample()
- size and shape of dataset
- number of unique data types
- number of numerical and non-numerical columns
- number of missing values per column
- presence of duplicate values
- column names - correct naming of column names to one word for ease of calling columns from the dataframe object

This is just to familiarise yourself with the dataset.
Let's wrap all these steps in a single python function:


```python
# Necessary imports
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set_style("darkgrid")


# Load sample dataset
data = pd.read_csv("../data/ix-mobile-banking.csv")

def get_stats(df):

    display(df.head(3))
    print(f"""
        \nshape of dataset: {df.shape}\nsize: {df.size}
        \nNumber of unique data types: {df.dtypes.nunique()}
        \n\t{df.dtypes}
        \nNumber of numerical columns: {len(df.select_dtypes(np.number).columns)}
        \nNon-numerical columns: {df.shape[1] - len(df.select_dtypes(np.number).columns)}
        \nMissing values per column:\n\t{df.isnull().sum()}
        \nDuplicates: {df.duplicated().any()}
        \nColumn names: {list(df.columns)}
    """)

get_stats(data)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>country_code</th>
      <th>region</th>
      <th>age</th>
      <th>FQ1</th>
      <th>FQ2</th>
      <th>FQ3</th>
      <th>FQ4</th>
      <th>FQ5</th>
      <th>FQ6</th>
      <th>...</th>
      <th>FQ27</th>
      <th>FQ28</th>
      <th>FQ29</th>
      <th>FQ30</th>
      <th>FQ31</th>
      <th>FQ32</th>
      <th>FQ33</th>
      <th>FQ34</th>
      <th>FQ37</th>
      <th>Target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ID_000J8GTZ</td>
      <td>1</td>
      <td>6</td>
      <td>35.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ID_000QLXZM</td>
      <td>32</td>
      <td>7</td>
      <td>70.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ID_001728I2</td>
      <td>71</td>
      <td>7</td>
      <td>22.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 42 columns</p>




    shape of dataset: (108446, 42)
    size: 4554732

    Number of unique data types: 3

    	ID               object
    country_code      int64
    region            int64
    age             float64
    FQ1               int64
    FQ2             float64
    FQ3             float64
    FQ4               int64
    FQ5             float64
    FQ6             float64
    FQ7             float64
    FQ8               int64
    FQ9               int64
    FQ10              int64
    FQ11            float64
    FQ12              int64
    FQ13              int64
    FQ14              int64
    FQ15              int64
    FQ16              int64
    FQ17            float64
    FQ18              int64
    FQ19            float64
    FQ20            float64
    FQ21            float64
    FQ22              int64
    FQ23              int64
    FQ24            float64
    FQ35            float64
    FQ36            float64
    FQ25              int64
    FQ26              int64
    FQ27            float64
    FQ28            float64
    FQ29            float64
    FQ30            float64
    FQ31            float64
    FQ32            float64
    FQ33            float64
    FQ34            float64
    FQ37              int64
    Target            int64
    dtype: object

    Number of numerical columns: 41

    Non-numerical columns: 1

    Missing values per column:
    	ID                   0
    country_code         0
    region               0
    age                322
    FQ1                  0
    FQ2              59322
    FQ3              62228
    FQ4                  0
    FQ5              87261
    FQ6              47787
    FQ7              47826
    FQ8                  0
    FQ9                  0
    FQ10                 0
    FQ11             24570
    FQ12                 0
    FQ13                 0
    FQ14                 0
    FQ15                 0
    FQ16                 0
    FQ17             97099
    FQ18                 0
    FQ19             47407
    FQ20             24679
    FQ21             24635
    FQ22                 0
    FQ23                 0
    FQ24             70014
    FQ35             82557
    FQ36             96963
    FQ25                 0
    FQ26                 0
    FQ27            105246
    FQ28            106940
    FQ29             24534
    FQ30            106331
    FQ31            107577
    FQ32             47650
    FQ33                 2
    FQ34             31794
    FQ37                 0
    Target               0
    dtype: int64

    Duplicates: False

    Column names: ['ID', 'country_code', 'region', 'age', 'FQ1', 'FQ2', 'FQ3', 'FQ4', 'FQ5', 'FQ6', 'FQ7', 'FQ8', 'FQ9', 'FQ10', 'FQ11', 'FQ12', 'FQ13', 'FQ14', 'FQ15', 'FQ16', 'FQ17', 'FQ18', 'FQ19', 'FQ20', 'FQ21', 'FQ22', 'FQ23', 'FQ24', 'FQ35', 'FQ36', 'FQ25', 'FQ26', 'FQ27', 'FQ28', 'FQ29', 'FQ30', 'FQ31', 'FQ32', 'FQ33', 'FQ34', 'FQ37', 'Target']



After familiarising yourself with the dataset, you can then do some data preparation; data imputation, correcting column names and data types, removing duplicates etc...
For our case we see that there are no duplicates but you may find duplicates by subseting columns, say investigating if there are duplicates in the first 10 columns as follows:


```python
data[data.duplicated(subset=list(data.columns[1:12]))]
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>country_code</th>
      <th>region</th>
      <th>age</th>
      <th>FQ1</th>
      <th>FQ2</th>
      <th>FQ3</th>
      <th>FQ4</th>
      <th>FQ5</th>
      <th>FQ6</th>
      <th>...</th>
      <th>FQ27</th>
      <th>FQ28</th>
      <th>FQ29</th>
      <th>FQ30</th>
      <th>FQ31</th>
      <th>FQ32</th>
      <th>FQ33</th>
      <th>FQ34</th>
      <th>FQ37</th>
      <th>Target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3354</th>
      <td>ID_14YEHJ8Y</td>
      <td>4</td>
      <td>0</td>
      <td>25.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4133</th>
      <td>ID_1DZ64UE0</td>
      <td>13</td>
      <td>0</td>
      <td>38.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4328</th>
      <td>ID_1G9BR3SF</td>
      <td>79</td>
      <td>0</td>
      <td>60.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5766</th>
      <td>ID_1X42EWSA</td>
      <td>10</td>
      <td>0</td>
      <td>30.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6152</th>
      <td>ID_21OECVBW</td>
      <td>133</td>
      <td>3</td>
      <td>55.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>107939</th>
      <td>ID_ZUKB8ZC6</td>
      <td>60</td>
      <td>4</td>
      <td>37.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>107988</th>
      <td>ID_ZUZ6QUHK</td>
      <td>60</td>
      <td>0</td>
      <td>60.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>107994</th>
      <td>ID_ZV2EK93X</td>
      <td>45</td>
      <td>4</td>
      <td>49.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>108313</th>
      <td>ID_ZYKSG3XG</td>
      <td>131</td>
      <td>1</td>
      <td>46.0</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>108379</th>
      <td>ID_ZZ7M49OG</td>
      <td>45</td>
      <td>3</td>
      <td>53.0</td>
      <td>1</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>645 rows × 42 columns</p>



That way we find that there are 645 duplicate rows in the dataset. You can use this convention to flag columns that say, shouldn't have any repetitions as a requirement.

### **2. Univariate Analysis**
Univariate; analysis of one variable. Here we investigate trends per single column of interest. Keyword here is uni/one/single. We take a column of interest and investigate patterns in it before going to the next.
For example we may want to know how the target variable in our sample dataset above is distributed, or the youngest and oldest customer and age group thresholds(definite age groups as seen in histogram bars).
We do analysis column by column.


```python
# Distribution of target variable
print(data.Target.value_counts())
plt.figure(figsize=(10,5))
sns.countplot(x=data.Target)
```

    0    78735
    1    29711
    Name: Target, dtype: int64





    <AxesSubplot:xlabel='Target', ylabel='count'>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_8_2.png" alt="png" width="600" height="350"/>

</p>



```python
# age into 6 groups
plt.figure(figsize=(10,5))
plt.hist(x=data.age, bins=6)
```




    (array([30748., 30169., 22129., 17240.,  6919.,   919.]),
     array([15., 29., 43., 57., 71., 85., 99.]),
     <BarContainer object of 6 artists>)



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_9_1.png" alt="png" width="600" height="350"/>

</p>


This information from the age column can be used to create a categorical variable as a feature engineering practice.

### **3. Bivariate Analysis**
You guessed it! Here we do analysis of two variables. We try to determine the relationship between them.
One type of plot that I never miss to use under bivariate analysis is the boxplot, especially in helping one to detect and remove outliers. It also helps one to visually see quartiles and their ranges.


```python
# Boxplot
sns.boxplot(y='age', x='Target', data=data)
```




    <AxesSubplot:xlabel='Target', ylabel='age'>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_12_1.png" alt="png" width="600" height="350"/>

</p>



```python
# Investigating correlation of country code and region
print(data[['country_code', 'region']].corr())
sns.heatmap(data[['country_code', 'region']].corr())
```

                  country_code    region
    country_code      1.000000  0.002045
    region            0.002045  1.000000





    <AxesSubplot:>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_13_2.png" alt="png" width="500" height="250"/>

</p>



```python
# Two variable regression joint plot
sns.jointplot(x='FQ28', y='FQ29', data=data, kind='reg')
```




    <seaborn.axisgrid.JointGrid at 0x7f48697e3400>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_14_1.png" alt="png" width="600" height="350"/>

</p>



```python
# add an imaginary continuous variable to show scatterplot between two variables
data['img'] = (data.age / data.country_code) + data.region
plt.scatter(x=data.img, y=data.age)
```




    <matplotlib.collections.PathCollection at 0x7f48684f0370>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_15_1.png" alt="png" width="600" height="350"/>

</p>


### **4. Multivariate Analysis**
This involves observation and analysis of more than two variables at a time. We can also add onto bivariate analysis observations at this step by including the hue parameter to take care of a third distingushing feature between the two.


```python
# Adding hue to the plot above
plt.scatter(x=data.img, y=data.age, c=data.Target, cmap='viridis')
```




    <matplotlib.collections.PathCollection at 0x7f48681d6520>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_17_1.png" alt="png" width="600" height="350"/>

</p>



```python
# Observing variable quartiles separated by variable FQ1
sns.violinplot(x='region', y='age', hue='FQ1', data=data)
```




    <AxesSubplot:xlabel='region', ylabel='age'>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_18_1.png" alt="png" width="600" height="350"/>

</p>


To finish off multivariate analysis, there is one, many in one, plot that carries a lot of information from all the variables; the pairplot.
For ease of readibility we'll include only the first few columns together with the target variable since our dataset has a lot of variables:


```python
sns.pairplot(data[list(data.columns[:10])+['Target']], hue='Target')
```




    <seaborn.axisgrid.PairGrid at 0x7f4861c7c160>



<p align="center">
    <img src="/img/blog/EDA_best_files/EDA_best_20_1.png" alt="png" width="750" height="550"/>

</p>


Most of the plots inside the pairplot don't tell a lot of information in our case because these many variables are categorical. You should try it with a more receptive dataset.

These are the steps for exhaustive data analysis. We went through each giving a few examples. This guide does not cover everything, but it outlays a good procedure to follow for exhaustive EDA. It is good practice to go into details and make as many beatiful visualizations at each step, cheers!

