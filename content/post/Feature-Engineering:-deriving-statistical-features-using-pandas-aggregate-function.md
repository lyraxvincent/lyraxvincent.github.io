---
title: "Feature Engineering: deriving statistical features using pandas aggregate function"
date: 2022-01-09T20:58:49+05:30
draft: false

categories: ["Feature Engineering", "Pandas"]
icon: "fas fa-industry"
---

# **Feature Engineering: deriving statistical features using pandas aggregate function**

<p align="center">
<img src="images/blog/pandas_agg.png" width="600" height="350" alt="png"/>
</p>

![png](images/blog/pandas_agg.png)

Many times when dealing with anonymized or machine-generated datasets, you find yourself out of ideas to come up 
with new features because it is unclear of what the dataset variables at hand represent. Take for example the 
following dataframe:


```
import pandas as pd

df = pd.read_csv("../datasets/Updated_Test.csv")
data = df[df.columns[2:13]].head(10)
data
```


<table>
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>absorbance0</th>
      <th>absorbance1</th>
      <th>absorbance2</th>
      <th>absorbance3</th>
      <th>absorbance4</th>
      <th>absorbance5</th>
      <th>absorbance6</th>
      <th>absorbance7</th>
      <th>absorbance8</th>
      <th>absorbance9</th>
      <th>absorbance10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.517951</td>
      <td>0.520508</td>
      <td>0.526852</td>
      <td>0.531611</td>
      <td>0.536816</td>
      <td>0.543828</td>
      <td>0.547761</td>
      <td>0.554379</td>
      <td>0.565622</td>
      <td>0.575762</td>
      <td>0.590253</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.517839</td>
      <td>0.522367</td>
      <td>0.525186</td>
      <td>0.534661</td>
      <td>0.541900</td>
      <td>0.546180</td>
      <td>0.551687</td>
      <td>0.556753</td>
      <td>0.566446</td>
      <td>0.578208</td>
      <td>0.591039</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.517702</td>
      <td>0.522018</td>
      <td>0.527237</td>
      <td>0.534374</td>
      <td>0.541155</td>
      <td>0.547152</td>
      <td>0.549837</td>
      <td>0.557513</td>
      <td>0.566793</td>
      <td>0.580574</td>
      <td>0.592258</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.525008</td>
      <td>0.527439</td>
      <td>0.536871</td>
      <td>0.539636</td>
      <td>0.546555</td>
      <td>0.553183</td>
      <td>0.558826</td>
      <td>0.563549</td>
      <td>0.575675</td>
      <td>0.587214</td>
      <td>0.597155</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.520532</td>
      <td>0.522683</td>
      <td>0.526842</td>
      <td>0.534634</td>
      <td>0.539676</td>
      <td>0.547488</td>
      <td>0.552688</td>
      <td>0.558355</td>
      <td>0.568959</td>
      <td>0.578905</td>
      <td>0.591207</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.526705</td>
      <td>0.532114</td>
      <td>0.536730</td>
      <td>0.541363</td>
      <td>0.549652</td>
      <td>0.553074</td>
      <td>0.558868</td>
      <td>0.564017</td>
      <td>0.576104</td>
      <td>0.583493</td>
      <td>0.598938</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.520870</td>
      <td>0.531424</td>
      <td>0.534101</td>
      <td>0.538626</td>
      <td>0.543272</td>
      <td>0.551315</td>
      <td>0.555033</td>
      <td>0.563571</td>
      <td>0.571861</td>
      <td>0.583686</td>
      <td>0.596506</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.527200</td>
      <td>0.525328</td>
      <td>0.532838</td>
      <td>0.537154</td>
      <td>0.543959</td>
      <td>0.549961</td>
      <td>0.557294</td>
      <td>0.559478</td>
      <td>0.572084</td>
      <td>0.584293</td>
      <td>0.597304</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.525830</td>
      <td>0.532805</td>
      <td>0.535670</td>
      <td>0.539697</td>
      <td>0.546112</td>
      <td>0.551254</td>
      <td>0.556557</td>
      <td>0.564790</td>
      <td>0.575007</td>
      <td>0.583733</td>
      <td>0.598626</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.533108</td>
      <td>0.530202</td>
      <td>0.537924</td>
      <td>0.540190</td>
      <td>0.548308</td>
      <td>0.553694</td>
      <td>0.558700</td>
      <td>0.562952</td>
      <td>0.574196</td>
      <td>0.584925</td>
      <td>0.597308</td>
    </tr>
  </tbody>
</table>



This data is most probably machine generated, that is true because what we're seeing here is 10 rows of blood spectroscopy readings. (There are well over 150 absorbance columns but since our focus is on generating features let's just work with these 11 columns.)  
We can use pandas aggregate function to map various statistical measures such as mean, median, variance etc of our data to come up with more features that can aid in improving our machine learning model performance when using this data in modeling a possible solution to the problem at hand. The modeling bit aside, for now let's dive straight to designing our extra features.  
The various statistical measures we're going to aggregate are:

- mean
- median
- sum
- count
- max
- min
- standard deviation
- variance
- skewness
- kurtosis

Let's quickly wrap them all up in a loop:


```
# To avoid including an aggregated feature to calculations of another aggregated feature
# we create a list of features
features = data.columns.to_list()

print("Dataframe before adding aggregate features:\n\n"); display(data.head(3))

for funct in ['mean', 'median', 'sum', 'count', 'max', 'min', 'std', 'var', 'skew', 'kurt']:
    data[f'agg_{funct}'] = data[features].agg(f'{funct}', axis=1)
    
print("\n\nDataframe after adding aggregate features:\n\n"); display(data.head(3))
```

    Dataframe before adding aggregate features:
    
    


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>absorbance0</th>
      <th>absorbance1</th>
      <th>absorbance2</th>
      <th>absorbance3</th>
      <th>absorbance4</th>
      <th>absorbance5</th>
      <th>absorbance6</th>
      <th>absorbance7</th>
      <th>absorbance8</th>
      <th>absorbance9</th>
      <th>absorbance10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.517951</td>
      <td>0.520508</td>
      <td>0.526852</td>
      <td>0.531611</td>
      <td>0.536816</td>
      <td>0.543828</td>
      <td>0.547761</td>
      <td>0.554379</td>
      <td>0.565622</td>
      <td>0.575762</td>
      <td>0.590253</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.517839</td>
      <td>0.522367</td>
      <td>0.525186</td>
      <td>0.534661</td>
      <td>0.541900</td>
      <td>0.546180</td>
      <td>0.551687</td>
      <td>0.556753</td>
      <td>0.566446</td>
      <td>0.578208</td>
      <td>0.591039</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.517702</td>
      <td>0.522018</td>
      <td>0.527237</td>
      <td>0.534374</td>
      <td>0.541155</td>
      <td>0.547152</td>
      <td>0.549837</td>
      <td>0.557513</td>
      <td>0.566793</td>
      <td>0.580574</td>
      <td>0.592258</td>
    </tr>
  </tbody>
</table>


    
    
    Dataframe after adding aggregate features:
    
    


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>absorbance0</th>
      <th>absorbance1</th>
      <th>absorbance2</th>
      <th>absorbance3</th>
      <th>absorbance4</th>
      <th>absorbance5</th>
      <th>absorbance6</th>
      <th>absorbance7</th>
      <th>absorbance8</th>
      <th>absorbance9</th>
      <th>...</th>
      <th>agg_mean</th>
      <th>agg_median</th>
      <th>agg_sum</th>
      <th>agg_count</th>
      <th>agg_max</th>
      <th>agg_min</th>
      <th>agg_std</th>
      <th>agg_var</th>
      <th>agg_skew</th>
      <th>agg_kurt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.517951</td>
      <td>0.520508</td>
      <td>0.526852</td>
      <td>0.531611</td>
      <td>0.536816</td>
      <td>0.543828</td>
      <td>0.547761</td>
      <td>0.554379</td>
      <td>0.565622</td>
      <td>0.575762</td>
      <td>...</td>
      <td>0.546486</td>
      <td>0.543828</td>
      <td>6.011343</td>
      <td>11</td>
      <td>0.590253</td>
      <td>0.517951</td>
      <td>0.023236</td>
      <td>0.000540</td>
      <td>0.622384</td>
      <td>-0.477046</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.517839</td>
      <td>0.522367</td>
      <td>0.525186</td>
      <td>0.534661</td>
      <td>0.541900</td>
      <td>0.546180</td>
      <td>0.551687</td>
      <td>0.556753</td>
      <td>0.566446</td>
      <td>0.578208</td>
      <td>...</td>
      <td>0.548388</td>
      <td>0.546180</td>
      <td>6.032264</td>
      <td>11</td>
      <td>0.591039</td>
      <td>0.517839</td>
      <td>0.023451</td>
      <td>0.000550</td>
      <td>0.465583</td>
      <td>-0.609624</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.517702</td>
      <td>0.522018</td>
      <td>0.527237</td>
      <td>0.534374</td>
      <td>0.541155</td>
      <td>0.547152</td>
      <td>0.549837</td>
      <td>0.557513</td>
      <td>0.566793</td>
      <td>0.580574</td>
      <td>...</td>
      <td>0.548783</td>
      <td>0.547152</td>
      <td>6.036613</td>
      <td>11</td>
      <td>0.592258</td>
      <td>0.517702</td>
      <td>0.023911</td>
      <td>0.000572</td>
      <td>0.520019</td>
      <td>-0.569977</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 21 columns</p>


Quick and efficient! We have added ten more features to our dataset.  
Other function names that can be passed to pandas aggregate function are listed in the documentation [here.](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.core.groupby.GroupBy.pipe.html#) Note however that not all of them return a pandas series (column).
