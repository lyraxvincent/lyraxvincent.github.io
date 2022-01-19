---
title: "Monitoring machine learning model performance the right way to avoid overfitting the leaderboard in a competition"
date: 2022-01-13T23:39:00+00:00
lastmod: 2022-01-13T23:39:00+00:00
draft: false
tags: ["model-monitoring", "model-performance", "machine-learning", "overfitting"]
categories: ["Machine Learning"]
---

[# **Monitoring machine learning model performance the right way to avoid overfitting the leaderboard in a competition**]::

<p align='center'>
    <img src="/img/blog/charles-deluvio-unsplash.jpg", alt="jpg" width="600" height="350"/>
    <br>
    Photo by <a href="https://unsplash.com/@charlesdeluvio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Charles Deluvio</a> on <a href="https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  
</p>

## **Introduction**

There are several explanations of what overfitting or underfitting is, whatever works for you, add the following idea to it:
- An overfitting model/algorithm is too complex for the task or dataset at hand while
- An underfitting model is too simple for the task

If overfitting has ever cost you downward jumps on a machine learning competition, you might agree with me that the experience was somewhat humiliating especially if you went from top 3 downwards, when you had better and stronger non-overfitting submissions that you didn't select to be scored on. This is what happens when you rely too much on public leaderboard placement instead of trusting your local cross validation, or local CV for short. 
I understand evaluating algorithms and tracking their performance can be tiresome sometimes but what is the point of building models in the first place if you're not going to make sure they are efficient for the work they are intended to perform? In this article, we are going to define a simple method to monitor your machine learning model performance to avoid such cases.

## **Sample problem**
We're going to use a dataset from a competition on [zindi.](https://zindi.africa/hackathons/airqo-air-sensor-calibration-challenge) The competition entails predicting air quality to help in air quality monitor calibration.  
Let us make the necessary imports and have a look at a preview of the dataset:


```python
import numpy as np
import pandas as pd
pd.set_option("display.max_columns", None)
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set_palette("bright")

from sklearn.cluster import KMeans
from sklearn.ensemble import RandomForestRegressor
from sklearn.preprocessing import LabelEncoder
from sklearn.decomposition import PCA
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split, KFold

from catboost import CatBoostRegressor
from xgboost import XGBRegressor
from lightgbm import LGBMRegressor

import pytz
from tqdm import tqdm, trange
import warnings
warnings.filterwarnings("ignore")
```


```python
# Load and preview data
data = pd.read_csv("../data/Train.csv", parse_dates=['created_at'])
test = pd.read_csv("../data/Test.csv", parse_dates=['created_at'])
data.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>created_at</th>
      <th>site</th>
      <th>pm2_5</th>
      <th>pm10</th>
      <th>s2_pm2_5</th>
      <th>s2_pm10</th>
      <th>humidity</th>
      <th>temp</th>
      <th>lat</th>
      <th>long</th>
      <th>altitude</th>
      <th>greenness</th>
      <th>landform_90m</th>
      <th>landform_270m</th>
      <th>population</th>
      <th>dist_major_road</th>
      <th>ref_pm2_5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ID_0038MG0B</td>
      <td>2020-04-23 17:00:00+03:00</td>
      <td>USEmbassy</td>
      <td>6.819048</td>
      <td>7.313810</td>
      <td>6.794048</td>
      <td>7.838333</td>
      <td>0.807417</td>
      <td>22.383333</td>
      <td>0.299255</td>
      <td>32.592686</td>
      <td>1199</td>
      <td>4374</td>
      <td>21</td>
      <td>14</td>
      <td>6834</td>
      <td>130</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ID_008ASVDD</td>
      <td>2020-02-23 19:00:00+03:00</td>
      <td>USEmbassy</td>
      <td>57.456047</td>
      <td>67.883488</td>
      <td>55.643488</td>
      <td>70.646977</td>
      <td>0.712417</td>
      <td>25.350000</td>
      <td>0.299255</td>
      <td>32.592686</td>
      <td>1199</td>
      <td>4374</td>
      <td>21</td>
      <td>14</td>
      <td>6834</td>
      <td>130</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ID_009ACJQ9</td>
      <td>2021-01-23 04:00:00+03:00</td>
      <td>Nakawa</td>
      <td>170.009773</td>
      <td>191.153636</td>
      <td>165.308636</td>
      <td>191.471591</td>
      <td>0.907833</td>
      <td>20.616667</td>
      <td>0.331740</td>
      <td>32.609510</td>
      <td>1191</td>
      <td>5865</td>
      <td>31</td>
      <td>-11</td>
      <td>4780</td>
      <td>500</td>
      <td>149.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ID_00IGMAQ2</td>
      <td>2019-12-04 09:00:00+03:00</td>
      <td>USEmbassy</td>
      <td>49.732821</td>
      <td>61.512564</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.949667</td>
      <td>21.216667</td>
      <td>0.299255</td>
      <td>32.592686</td>
      <td>1199</td>
      <td>4374</td>
      <td>21</td>
      <td>14</td>
      <td>6834</td>
      <td>130</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ID_00P76VAQ</td>
      <td>2019-10-01 01:00:00+03:00</td>
      <td>USEmbassy</td>
      <td>41.630455</td>
      <td>51.044545</td>
      <td>41.725000</td>
      <td>51.141364</td>
      <td>0.913833</td>
      <td>18.908333</td>
      <td>0.299255</td>
      <td>32.592686</td>
      <td>1199</td>
      <td>4374</td>
      <td>21</td>
      <td>14</td>
      <td>6834</td>
      <td>130</td>
      <td>39.0</td>
    </tr>
  </tbody>
</table>



### **Feature Engineering**
Normally to get good results, you must consider feature engineering. I was lucky to participate in this hackathon and after vigorous data exploration, I came up with some good features.  
Let’s define a feature engineering function to handle feature generation:


```python
# First we impute missing values in the dataset
# It has very few NaNs so we'll do this with mean
all_data = pd.concat([data, test]); all_data.reset_index(drop=True, inplace=True)
all_data.humidity.fillna(all_data.humidity.mean(), inplace=True); all_data.temp.fillna(all_data.temp.mean(), inplace=True)
```


```python
def feat_eng(df):
    le = LabelEncoder()
    kmeans = KMeans(n_clusters=3)
    
    # date features
    df['creation_year'] = pd.to_datetime(df.created_at).dt.year
    df['creation_month'] = pd.to_datetime(df.created_at).dt.month
    df['creation_day'] = pd.to_datetime(df.created_at).dt.day
    df['creation_hour'] = pd.to_datetime(df.created_at).dt.hour
    df['creation_weekday'] = pd.Series(pd.to_datetime(df.created_at).dt.weekday).apply(lambda x: 1 if x<5 else 0) # if weekday or weekeend
    df['days_todate'] = pd.to_datetime(pd.Timestamp.now().date()).tz_localize(tz=pytz.UTC) - pd.to_datetime(df.created_at.dt.tz_convert(tz=pytz.UTC))
    df.days_todate = df.days_todate.apply(lambda x: int(str(x).split(' days')[0])) # remove string 'days'
    df['years_todate'] = df.days_todate.apply(lambda x: int(np.round(x/364)))
    
    # time difference(number of hours to next recording)
    timediff = df.sort_values('created_at').created_at.diff(); timediff.fillna(pd.Timedelta(seconds=0), inplace=True)
    timediff = timediff.apply(lambda val: str(val).split('days')[-1].split(':')[0])
    df['timediff'] = 0
    df.sort_values('created_at', inplace=True)
    df['timediff'] = timediff.astype(int); df.sort_index(inplace=True)
    
    # label encoded features
    df['site_le'] = le.fit_transform(df.site)
    df['creation_year_le'] = le.fit_transform(df.creation_year)
    
    
    # kmeans features
    kmeans.fit(df[['lat', 'long']])
    df['latlong_cluster'] = kmeans.labels_
    
    kmeans.fit(df[['landform_90m', 'landform_270m']])
    landform_labels = kmeans.labels_
    df['landforms_cluster'] = landform_labels
    
    kmeans.fit(pd.concat([pd.Series(landform_labels), pd.Series(df['greenness'].values)], axis=1))
    df['landforms_greenness_clusters'] = kmeans.labels_
    
    kmeans.fit(df[['altitude', 'greenness', 'population']])
    df['altgreenpop_cluster'] = kmeans.labels_
    
    kmeans.fit(df[['site_le', 'humidity', 'temp', 'lat', 'long', 'altitude', 'landform_90m',
                   'landform_270m', 'population', 'dist_major_road']])
    df['master_cluster'] = kmeans.labels_
    
    # dewpoint
    df['dewpoint'] = 0
    for i, _ in tqdm(enumerate(df.temp)):
        temp = df.loc[i, 'temp']
        humidity = df.loc[i, 'humidity']
        df.loc[i, 'dewpoint'] = temp - ((1 - humidity)/5)
        
    
    # temperature and humidity categories
    # temp
    conditions = [
        (df['temp'] <= 20),
        (df['temp'] > 20) & (df['temp'] <= 25),
        (df['temp'] > 25)]
    
    values = ['2', '3', '1']
    df['temp_cat'] = np.select(conditions, values).astype(int)
    
    # temp
    conditions = [
    (df['humidity'] <= 0.5),
    (df['humidity'] > 0.5) & (df['humidity'] <= 0.8),
    (df['humidity'] > 0.8)]

    values = ['1', '2', '3']
    df['humidity_cat'] = np.select(conditions, values).astype(int)
    
    # combination features
    df['humixtemp'] = df.humidity * df.temp
    df['humixtemp2'] = (df.humidity*100) / df.temp
    df['altxhumi'] = df.altitude * df.humidity
    df['altxtemp'] = df.altitude * df.temp
    
    # differences between pm2_5:s2_pm2_5, pm10:s2_pm10 and between pm2_5's:pm10's
    df['pm2_5_diff'] = np.abs(df.pm2_5 - df.s2_pm2_5)
    df['pm10_diff'] = np.abs(df.pm10 - df.s2_pm10)
    df['pm10xpm25'] = np.abs(df.pm10 - df.pm2_5)
    df['s2_pm10xpm25'] = np.abs(df.s2_pm10 - df.s2_pm2_5)
    
    df['pm_total_diffs'] = df.pm2_5_diff + df.pm10_diff + df.pm10xpm25 + df.s2_pm10xpm25
    df['pm_diffs_mean'] = (df.pm2_5_diff + df.pm10_diff + df.pm10xpm25 + df.s2_pm10xpm25)/4
    
    # humixtemp categories
    conditions = [
    (df['humixtemp'] <= 8),
    (df['humixtemp'] > 13) & (df['humixtemp'] <= 18),
    (df['humixtemp'] > 18)]

    values = ['2', '1', '3']
    df['humixtemp_cat'] = np.select(conditions, values).astype(int)
    
    # altxhumi categories
    conditions = [
    (df['altxhumi'] <= 600),
    (df['altxhumi'] > 600) & (df['altxhumi'] <= 900),
    (df['altxhumi'] > 900)]
    values = ['3', '1', '2']
    df['altxhumi_cat'] = np.select(conditions, values).astype(int)
    
    # altxtemp categories
    conditions = [
    (df['altxtemp'] <= 25000),
    (df['altxtemp'] > 25000) & (df['altxtemp'] <= 31000),
    (df['altxtemp'] > 31000)]
    values = ['3', '1', '2']
    df['altxtemp_cat'] = np.select(conditions, values).astype(int)
    
    # pca feature
    pca = PCA(n_components=1, random_state=101)
    vals = pca.fit_transform(df[['pm2_5', 'pm10', 's2_pm2_5', 's2_pm10',
       'humidity', 'temp', 'lat', 'long', 'altitude', 'greenness',
       'landform_90m', 'landform_270m', 'population', 'dist_major_road',
       'creation_month', 'creation_day',
       'creation_hour', 'creation_weekday', 'days_todate', 'years_todate',
       'timediff', 'site_le', 'creation_year_le', 'latlong_cluster',
       'landforms_cluster', 'landforms_greenness_clusters',
       'altgreenpop_cluster', 'master_cluster', 'dewpoint', 'temp_cat',
       'humidity_cat', 'humixtemp', 'humixtemp2', 'altxhumi', 'altxtemp',
       'pm2_5_diff', 'pm10_diff', 'pm10xpm25', 's2_pm10xpm25',
       'pm_total_diffs', 'pm_diffs_mean', 'humixtemp_cat', 'altxhumi_cat',
       'altxtemp_cat']])
    df['component'] = pd.Series(vals.reshape(vals.shape[0],))
    
    # component categories
    # altxtemp categories
    conditions = [
    (df['component'] <= -1000),
    (df['component'] > -1000) & (df['component'] <= 5000),
    (df['component'] > 5000)]
    values = ['2', '1', '3']
    df['component_cat'] = np.select(conditions, values).astype(int)
    
    return df
```

That was one hell of a process! You can imagine the extensive exploratory data analysis (EDA) I did to come up with such a piece!  
Let us see how many more features we add to our initial dataset:


```python
initial_columns = all_data.shape[1]
all_data = feat_eng(all_data)

# separate back to train and test splits
data = all_data[all_data.ref_pm2_5.notna()]
test = all_data[all_data.ref_pm2_5.isna()]; test.drop('ref_pm2_5', axis=1, inplace=True)

# free memory
del all_data

print(f"We created {data.shape[1] - initial_columns} more features!")
```

    13665it [00:04, 2915.07it/s]


    We created 33 more features!


33 more features! No one should beat our place in the leaderboard now if we combine our excellent feature engineering with performance monitoring to make sure we trust our local CV.

### **Modeling: Monitoring model performance**
This is now where we start to define model performance steps.  
Let's declare objects first. We'll be using the able **CatBoost** algorithm wrapped in **KFold** cross validation spanning 10 folds for our modeling.


```python
# Features
feat_cols = ['pm2_5', 'pm10', 's2_pm2_5', 's2_pm10',
       'humidity', 'temp', 'lat', 'long', 'altitude', 'greenness',
       'landform_90m', 'landform_270m', 'population', 'dist_major_road',
       'creation_month', 'creation_day',
       'creation_hour', 'creation_weekday', 'days_todate', 'years_todate',
       'timediff', 'site_le', 'creation_year_le', 'latlong_cluster',
       'landforms_cluster', 'landforms_greenness_clusters',
       'altgreenpop_cluster', 'master_cluster', 'dewpoint', 'temp_cat',
       'humidity_cat', 'humixtemp', 'humixtemp2', 'altxhumi', 'altxtemp',
       'pm2_5_diff', 'pm10_diff', 'pm10xpm25', 's2_pm10xpm25',
       'pm_total_diffs', 'pm_diffs_mean', 'humixtemp_cat', 'altxhumi_cat',
       'altxtemp_cat', 'component', 'component_cat']

X = data[feat_cols]
y = data.ref_pm2_5
```

First, define two variables:
- out of folds predictions (those predictions made on the holdout set during the resampling procedure)
- errors (the errors recorded in every fold - for these we’ll calculate the mean error) The error metric for this competition was root mean squared error (RMSE).

The second variable is not too important, and must not be used, but I like to include it to strengthen my belief that the model performance across folds is reliable compared to the out of fold score if it does not deviate so much.  
We’ll use both in comparison to gauge our model performance. Naturally they should not be far from each other.


```python
oofs_preds = np.zeros(len(data))
errors = []
```

Second, define your cross validation strategy. In this case, we’re using KFold with 10 folds.  
This is where we populate these variables with values from our model training. In this loop, the two variables we created above get updated as the model trains and predicts on each fold.


```python
# these are test predictions, those that we submit to be scored on
pred_test = np.zeros(len(test))

NFOLDS = 10
fold = KFold(n_splits=NFOLDS)

for train_index, test_index in fold.split(X,y):
    
    X_train, X_test = X.iloc[train_index], X.iloc[test_index]
    y_train, y_test = y.iloc[train_index], y.iloc[test_index]
    
    cat  = CatBoostRegressor(verbose=False, random_seed=101, use_best_model=True, loss_function='RMSE', n_estimators=1500, learning_rate=0.05)
    cat.fit(X_train,y_train,eval_set=[(X_train,y_train),(X_test, y_test)])
    preds = cat.predict(X_test)
    
    # update out of folds predictions
    oofs_preds[test_index] = preds
    
    # update errors
    print(f"RMSE: {np.sqrt(mean_squared_error(y_test,preds))}")
    errors.append(np.sqrt(mean_squared_error(y_test,preds)))
    p2 = cat.predict(test[feat_cols])
    pred_test += p2
```

    RMSE: 11.147193104786583
    RMSE: 11.542680896491985
    RMSE: 14.893402844224648
    RMSE: 14.109007552380556
    RMSE: 9.74267734452024
    RMSE: 11.78783134709677
    RMSE: 8.893336931729067
    RMSE: 8.760814543657926
    RMSE: 10.588465788639557
    RMSE: 15.59393007990298


Now comes time to compare between the two variables:
We obtain the RMSE in out of folds predictions and we average errors across folds then compare.  
**REMEMBER:** The difference between these values should be minimal. Otherwise, you could be overfitting.


```python
print(f"RMSE: {np.sqrt(mean_squared_error(y, oofs_preds))}\nMean error: {np.mean(errors)}")
```

    RMSE: 11.930416509510236
    Mean error: 11.895585944533348


The two values are closer to each other. This is promising so we should trust our CV score regardless of what leaderboard score we are ranked with. If anything we should retain our place in the private leaderboard or secure upward jumps, not downwards because we have done our due diligence in tracking and monitoring our model performance.
