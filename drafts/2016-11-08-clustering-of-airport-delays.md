---
tags:
  - DSI
  - projects
  - data-science
  - python
  - sql
  - eda
  - visualizations
  - scipy
  - clustering
  - unsupervised-learning
---
Running principal component analysis and clustering on airport delays, cancellations and diversions.

{% include toc %}

# Introduction

Who hates it when their flight gets delayed? \*hand waves\* Or worse, when it gets cancelled?

This week, we look at airport information for Project 7 of the General Assembly , and try to cluster delay, cancellation and/ or diversion data to see if we can find any patterns.

# Data Sets
We were provided with three datasets:

- Airport information (mainly location information)
- Flight delay information for the years 2004 to 2014
- Flight cancellation and diversion information for the years 2004 to 2014

We have location information for more than 5,000 airports, but the other two datasets only included information for a little over 70 airports. Since our features are going to be the delay, cancellation and diversion information, we're limited to those airports. This reduced the size our data considerably, but we still had a sizable dataset as we had observations for 11 years.

# Set up notebook

## Import libraries


```python
import pandas as pd
import numpy as np
import scipy
from sklearn import decomposition, manifold, preprocessing, cluster

import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import seaborn as sns

import psycopg2 as psy
import sqlalchemy

from ipywidgets import interact

%matplotlib inline
```

## Plot style


```python
plt.style.use("fivethirtyeight")
```

# Load data


```python
airports = pd.read_csv("../assets/airports.csv")
```


```python
cancellations = pd.read_csv("../assets/airport_cancellations.csv")
```


```python
operations = pd.read_csv("../assets/Airport_operations.csv")
```

# Exploratory data analysis


```python
airports.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Key</th>
      <th>LocID</th>
      <th>AP_NAME</th>
      <th>ALIAS</th>
      <th>Facility Type</th>
      <th>FAA REGION</th>
      <th>COUNTY</th>
      <th>CITY</th>
      <th>STATE</th>
      <th>AP Type</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Boundary Data Available</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3443.0</td>
      <td>STX</td>
      <td>HENRY E ROHLSEN</td>
      <td>Henry E Rohlsen Int'l Airport</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>-VIRGIN ISLANDS-</td>
      <td>CHRISTIANSTED</td>
      <td>VI</td>
      <td>Public Use</td>
      <td>17.701556</td>
      <td>-64.801722</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5088.0</td>
      <td>X64</td>
      <td>PATILLAS</td>
      <td>NaN</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>#NAME?</td>
      <td>PATILLAS</td>
      <td>PR</td>
      <td>Public Use</td>
      <td>17.982189</td>
      <td>-66.019330</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2886.0</td>
      <td>PSE</td>
      <td>MERCEDITA</td>
      <td>Aeropuerto Mercedita</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>#NAME?</td>
      <td>PONCE</td>
      <td>PR</td>
      <td>Public Use</td>
      <td>18.008306</td>
      <td>-66.563028</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>




```python
cancellations.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Airport</th>
      <th>Year</th>
      <th>Departure Cancellations</th>
      <th>Arrival Cancellations</th>
      <th>Departure Diversions</th>
      <th>Arrival Diversions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004.0</td>
      <td>242.0</td>
      <td>235.0</td>
      <td>71.0</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005.0</td>
      <td>221.0</td>
      <td>190.0</td>
      <td>61.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006.0</td>
      <td>392.0</td>
      <td>329.0</td>
      <td>71.0</td>
      <td>124.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
operations.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>year</th>
      <th>departures for metric computation</th>
      <th>arrivals for metric computation</th>
      <th>percent on-time gate departures</th>
      <th>percent on-time airport departures</th>
      <th>percent on-time gate arrivals</th>
      <th>average_gate_departure_delay</th>
      <th>average_taxi_out_time</th>
      <th>average taxi out delay</th>
      <th>average airport departure delay</th>
      <th>average airborne delay</th>
      <th>average taxi in delay</th>
      <th>average block delay</th>
      <th>average gate arrival delay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004</td>
      <td>53971</td>
      <td>53818</td>
      <td>0.8030</td>
      <td>0.7809</td>
      <td>0.7921</td>
      <td>10.38</td>
      <td>9.89</td>
      <td>2.43</td>
      <td>12.10</td>
      <td>2.46</td>
      <td>0.83</td>
      <td>2.55</td>
      <td>10.87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005</td>
      <td>51829</td>
      <td>51877</td>
      <td>0.8140</td>
      <td>0.7922</td>
      <td>0.8001</td>
      <td>9.60</td>
      <td>9.79</td>
      <td>2.29</td>
      <td>11.20</td>
      <td>2.26</td>
      <td>0.89</td>
      <td>2.34</td>
      <td>10.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006</td>
      <td>49682</td>
      <td>51199</td>
      <td>0.7983</td>
      <td>0.7756</td>
      <td>0.7746</td>
      <td>10.84</td>
      <td>9.89</td>
      <td>2.16</td>
      <td>12.33</td>
      <td>2.12</td>
      <td>0.84</td>
      <td>2.66</td>
      <td>11.82</td>
    </tr>
  </tbody>
</table>
</div>



## Standardize column names


```python
airports.columns = [i.replace("_", " ").lower().replace(" ", "_") for i in airports.columns]
cancellations.columns = [i.replace("_", " ").lower().replace(" ", "_") for i in cancellations.columns]
operations.columns = [i.replace("_", " ").replace("-"," ").lower().replace(" ", "_") for i in operations.columns]
```


```python
airports.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>locid</th>
      <th>ap_name</th>
      <th>alias</th>
      <th>facility_type</th>
      <th>faa_region</th>
      <th>county</th>
      <th>city</th>
      <th>state</th>
      <th>ap_type</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>boundary_data_available</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3443.0</td>
      <td>STX</td>
      <td>HENRY E ROHLSEN</td>
      <td>Henry E Rohlsen Int'l Airport</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>-VIRGIN ISLANDS-</td>
      <td>CHRISTIANSTED</td>
      <td>VI</td>
      <td>Public Use</td>
      <td>17.701556</td>
      <td>-64.801722</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5088.0</td>
      <td>X64</td>
      <td>PATILLAS</td>
      <td>NaN</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>#NAME?</td>
      <td>PATILLAS</td>
      <td>PR</td>
      <td>Public Use</td>
      <td>17.982189</td>
      <td>-66.019330</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2886.0</td>
      <td>PSE</td>
      <td>MERCEDITA</td>
      <td>Aeropuerto Mercedita</td>
      <td>Airport</td>
      <td>ASO</td>
      <td>#NAME?</td>
      <td>PONCE</td>
      <td>PR</td>
      <td>Public Use</td>
      <td>18.008306</td>
      <td>-66.563028</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>




```python
cancellations.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>year</th>
      <th>departure_cancellations</th>
      <th>arrival_cancellations</th>
      <th>departure_diversions</th>
      <th>arrival_diversions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004.0</td>
      <td>242.0</td>
      <td>235.0</td>
      <td>71.0</td>
      <td>46.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005.0</td>
      <td>221.0</td>
      <td>190.0</td>
      <td>61.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006.0</td>
      <td>392.0</td>
      <td>329.0</td>
      <td>71.0</td>
      <td>124.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
operations.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>year</th>
      <th>departures_for_metric_computation</th>
      <th>arrivals_for_metric_computation</th>
      <th>percent_on_time_gate_departures</th>
      <th>percent_on_time_airport_departures</th>
      <th>percent_on_time_gate_arrivals</th>
      <th>average_gate_departure_delay</th>
      <th>average_taxi_out_time</th>
      <th>average_taxi_out_delay</th>
      <th>average_airport_departure_delay</th>
      <th>average_airborne_delay</th>
      <th>average_taxi_in_delay</th>
      <th>average_block_delay</th>
      <th>average_gate_arrival_delay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004</td>
      <td>53971</td>
      <td>53818</td>
      <td>0.8030</td>
      <td>0.7809</td>
      <td>0.7921</td>
      <td>10.38</td>
      <td>9.89</td>
      <td>2.43</td>
      <td>12.10</td>
      <td>2.46</td>
      <td>0.83</td>
      <td>2.55</td>
      <td>10.87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005</td>
      <td>51829</td>
      <td>51877</td>
      <td>0.8140</td>
      <td>0.7922</td>
      <td>0.8001</td>
      <td>9.60</td>
      <td>9.79</td>
      <td>2.29</td>
      <td>11.20</td>
      <td>2.26</td>
      <td>0.89</td>
      <td>2.34</td>
      <td>10.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006</td>
      <td>49682</td>
      <td>51199</td>
      <td>0.7983</td>
      <td>0.7756</td>
      <td>0.7746</td>
      <td>10.84</td>
      <td>9.89</td>
      <td>2.16</td>
      <td>12.33</td>
      <td>2.12</td>
      <td>0.84</td>
      <td>2.66</td>
      <td>11.82</td>
    </tr>
  </tbody>
</table>
</div>



## What's in each dataset?


```python
airports.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5167 entries, 0 to 5166
    Data columns (total 13 columns):
    key                        5164 non-null float64
    locid                      5152 non-null object
    ap_name                    5164 non-null object
    alias                      3498 non-null object
    facility_type              5164 non-null object
    faa_region                 5164 non-null object
    county                     5164 non-null object
    city                       5164 non-null object
    state                      5164 non-null object
    ap_type                    5164 non-null object
    latitude                   5164 non-null float64
    longitude                  5164 non-null float64
    boundary_data_available    5164 non-null object
    dtypes: float64(3), object(10)
    memory usage: 524.8+ KB



```python
airports["locid"].nunique()
```




    5152




```python
cancellations.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 805 entries, 0 to 804
    Data columns (total 6 columns):
    airport                    805 non-null object
    year                       805 non-null float64
    departure_cancellations    805 non-null float64
    arrival_cancellations      805 non-null float64
    departure_diversions       805 non-null float64
    arrival_diversions         805 non-null float64
    dtypes: float64(5), object(1)
    memory usage: 37.8+ KB



```python
cancellations["airport"].nunique()
```




    74




```python
operations.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 841 entries, 0 to 840
    Data columns (total 15 columns):
    airport                               841 non-null object
    year                                  841 non-null int64
    departures_for_metric_computation     841 non-null int64
    arrivals_for_metric_computation       841 non-null int64
    percent_on_time_gate_departures       841 non-null float64
    percent_on_time_airport_departures    841 non-null float64
    percent_on_time_gate_arrivals         841 non-null float64
    average_gate_departure_delay          841 non-null float64
    average_taxi_out_time                 841 non-null float64
    average_taxi_out_delay                841 non-null float64
    average_airport_departure_delay       841 non-null float64
    average_airborne_delay                841 non-null float64
    average_taxi_in_delay                 841 non-null float64
    average_block_delay                   841 non-null float64
    average_gate_arrival_delay            841 non-null float64
    dtypes: float64(11), int64(3), object(1)
    memory usage: 98.6+ KB



```python
operations["airport"].nunique()
```




    77



### Observations

- There are a lot more instances in the airports dataset
- airports["locid"] maps to cancellations["airport"] and operations["airport"]
    - There are a lot more unique "locid"s than "airport"s

### What each dataset is about:

- airports: list of airports and their descriptions
- cancellations: cancellation details of some airports from 2004 to 2014
- operations: delay details of some airports from 2004 to 2014

## Looking only at the airports that appear in the cancellations dataset


```python
df = operations.merge(cancellations)
df = df.merge(airports, left_on="airport", right_on="locid")
```


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>year</th>
      <th>departures_for_metric_computation</th>
      <th>arrivals_for_metric_computation</th>
      <th>percent_on_time_gate_departures</th>
      <th>percent_on_time_airport_departures</th>
      <th>percent_on_time_gate_arrivals</th>
      <th>average_gate_departure_delay</th>
      <th>average_taxi_out_time</th>
      <th>average_taxi_out_delay</th>
      <th>...</th>
      <th>alias</th>
      <th>facility_type</th>
      <th>faa_region</th>
      <th>county</th>
      <th>city</th>
      <th>state</th>
      <th>ap_type</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>boundary_data_available</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004</td>
      <td>53971</td>
      <td>53818</td>
      <td>0.8030</td>
      <td>0.7809</td>
      <td>0.7921</td>
      <td>10.38</td>
      <td>9.89</td>
      <td>2.43</td>
      <td>...</td>
      <td>Albuquerque Int'l Sunport</td>
      <td>Airport</td>
      <td>ASW</td>
      <td>BERNALILLO</td>
      <td>ALBUQUERQUE</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005</td>
      <td>51829</td>
      <td>51877</td>
      <td>0.8140</td>
      <td>0.7922</td>
      <td>0.8001</td>
      <td>9.60</td>
      <td>9.79</td>
      <td>2.29</td>
      <td>...</td>
      <td>Albuquerque Int'l Sunport</td>
      <td>Airport</td>
      <td>ASW</td>
      <td>BERNALILLO</td>
      <td>ALBUQUERQUE</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006</td>
      <td>49682</td>
      <td>51199</td>
      <td>0.7983</td>
      <td>0.7756</td>
      <td>0.7746</td>
      <td>10.84</td>
      <td>9.89</td>
      <td>2.16</td>
      <td>...</td>
      <td>Albuquerque Int'l Sunport</td>
      <td>Airport</td>
      <td>ASW</td>
      <td>BERNALILLO</td>
      <td>ALBUQUERQUE</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ABQ</td>
      <td>2007</td>
      <td>53255</td>
      <td>53611</td>
      <td>0.8005</td>
      <td>0.7704</td>
      <td>0.7647</td>
      <td>11.29</td>
      <td>10.34</td>
      <td>2.40</td>
      <td>...</td>
      <td>Albuquerque Int'l Sunport</td>
      <td>Airport</td>
      <td>ASW</td>
      <td>BERNALILLO</td>
      <td>ALBUQUERQUE</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ABQ</td>
      <td>2008</td>
      <td>49589</td>
      <td>49512</td>
      <td>0.8103</td>
      <td>0.7844</td>
      <td>0.7875</td>
      <td>10.79</td>
      <td>10.41</td>
      <td>2.41</td>
      <td>...</td>
      <td>Albuquerque Int'l Sunport</td>
      <td>Airport</td>
      <td>ASW</td>
      <td>BERNALILLO</td>
      <td>ALBUQUERQUE</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 32 columns</p>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 799 entries, 0 to 798
    Data columns (total 32 columns):
    airport                               799 non-null object
    year                                  799 non-null int64
    departures_for_metric_computation     799 non-null int64
    arrivals_for_metric_computation       799 non-null int64
    percent_on_time_gate_departures       799 non-null float64
    percent_on_time_airport_departures    799 non-null float64
    percent_on_time_gate_arrivals         799 non-null float64
    average_gate_departure_delay          799 non-null float64
    average_taxi_out_time                 799 non-null float64
    average_taxi_out_delay                799 non-null float64
    average_airport_departure_delay       799 non-null float64
    average_airborne_delay                799 non-null float64
    average_taxi_in_delay                 799 non-null float64
    average_block_delay                   799 non-null float64
    average_gate_arrival_delay            799 non-null float64
    departure_cancellations               799 non-null float64
    arrival_cancellations                 799 non-null float64
    departure_diversions                  799 non-null float64
    arrival_diversions                    799 non-null float64
    key                                   799 non-null float64
    locid                                 799 non-null object
    ap_name                               799 non-null object
    alias                                 799 non-null object
    facility_type                         799 non-null object
    faa_region                            799 non-null object
    county                                799 non-null object
    city                                  799 non-null object
    state                                 799 non-null object
    ap_type                               799 non-null object
    latitude                              799 non-null float64
    longitude                             799 non-null float64
    boundary_data_available               799 non-null object
    dtypes: float64(18), int64(3), object(11)
    memory usage: 206.0+ KB



```python
df.describe(include=["O"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>locid</th>
      <th>ap_name</th>
      <th>alias</th>
      <th>facility_type</th>
      <th>faa_region</th>
      <th>county</th>
      <th>city</th>
      <th>state</th>
      <th>ap_type</th>
      <th>boundary_data_available</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
      <td>799</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>1</td>
      <td>9</td>
      <td>63</td>
      <td>69</td>
      <td>36</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>top</th>
      <td>LAS</td>
      <td>LAS</td>
      <td>THEODORE FRANCIS GREEN STATE</td>
      <td>Honolulu Int'l Airport</td>
      <td>Airport</td>
      <td>AWP</td>
      <td>ORANGE</td>
      <td>NEW YORK</td>
      <td>CA</td>
      <td>Federalized/Commercial</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>11</td>
      <td>799</td>
      <td>177</td>
      <td>33</td>
      <td>33</td>
      <td>122</td>
      <td>748</td>
      <td>799</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove "duplicate" columns
del df["key"]
del df["locid"]

# Remove columns that may not be useful
del df["ap_name"]
del df["alias"]
del df["facility_type"]
del df["county"]
del df["city"]
del df["boundary_data_available"]
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 799 entries, 0 to 798
    Data columns (total 24 columns):
    airport                               799 non-null object
    year                                  799 non-null int64
    departures_for_metric_computation     799 non-null int64
    arrivals_for_metric_computation       799 non-null int64
    percent_on_time_gate_departures       799 non-null float64
    percent_on_time_airport_departures    799 non-null float64
    percent_on_time_gate_arrivals         799 non-null float64
    average_gate_departure_delay          799 non-null float64
    average_taxi_out_time                 799 non-null float64
    average_taxi_out_delay                799 non-null float64
    average_airport_departure_delay       799 non-null float64
    average_airborne_delay                799 non-null float64
    average_taxi_in_delay                 799 non-null float64
    average_block_delay                   799 non-null float64
    average_gate_arrival_delay            799 non-null float64
    departure_cancellations               799 non-null float64
    arrival_cancellations                 799 non-null float64
    departure_diversions                  799 non-null float64
    arrival_diversions                    799 non-null float64
    faa_region                            799 non-null object
    state                                 799 non-null object
    ap_type                               799 non-null object
    latitude                              799 non-null float64
    longitude                             799 non-null float64
    dtypes: float64(17), int64(3), object(4)
    memory usage: 156.1+ KB



```python
df.to_csv("combined_data.csv")
```

## Visualizations

### Correlations


```python
# Create a mask so that only one half of our heatmap shows up
# This takes out duplicates in the grid and also the spots where a variable is being compared to itself
mask = np.zeros_like(df.corr(), dtype=np.bool)
mask[np.triu_indices_from(mask)] = True
sns.heatmap(df.corr(), mask=mask.T);
```


![png](output_38_0.png)


#### Observations

- Cancellations and diversions are highly correlated
- Percentage of on time gate departures, airport departures, gate arrivals, average departure delay, and average arrival delay are highly correlated (both positively and negatively)
- Average taxi out time and taxi out delay has medium correlation with other delays as well as cancellations and diversions
- Average taxi in time has high correlation with cancellations and diversions


```python
sns.pairplot(df[[i for i in df.columns if "average" in i or "percent" in i or "cancel" in i or "diver" in i]],
             size=5);
plt.xticks(fontsize=5);
```


![png](output_40_0.png)


### Helper function for swarm plots


```python
def plot_swarm(x, y, hue, data, xlab=None, ylab=None, title=None):
    plt.subplots(figsize=(16,8));
    ax = sns.swarmplot(x=x, y=y, hue=hue, data=data);
    plt.xticks(fontsize=15);
    plt.yticks(fontsize=15);
    if ylab is not None:
        plt.ylabel(ylab, fontsize=20);
    if xlab is not None:
        plt.xlabel(xlab, fontsize=20);
    if title is not None:
        plt.title(title);
```

### Distribution of data

#### By year


```python
plot_cols = list(df.columns)[4:-5]
plot_cols
```




    ['percent_on_time_gate_departures',
     'percent_on_time_airport_departures',
     'percent_on_time_gate_arrivals',
     'average_gate_departure_delay',
     'average_taxi_out_time',
     'average_taxi_out_delay',
     'average_airport_departure_delay',
     'average_airborne_delay',
     'average_taxi_in_delay',
     'average_block_delay',
     'average_gate_arrival_delay',
     'departure_cancellations',
     'arrival_cancellations',
     'departure_diversions',
     'arrival_diversions']




```python
for i in plot_cols:
    plot_swarm("year", i, hue=None, data=df, xlab="Year",
           ylab=i, title= i + " by year")
```


![png](output_46_0.png)



![png](output_46_1.png)



![png](output_46_2.png)



![png](output_46_3.png)



![png](output_46_4.png)



![png](output_46_5.png)



![png](output_46_6.png)



![png](output_46_7.png)



![png](output_46_8.png)



![png](output_46_9.png)



![png](output_46_10.png)



![png](output_46_11.png)



![png](output_46_12.png)



![png](output_46_13.png)



![png](output_46_14.png)

plot_swarm("year", "average_gate_departure_delay", hue=None, data=df, xlab="Year",
           ylab="Average gate departure delay", title="Distribution of gate departure delays by year")
##### Observations

- No apparent pattern visible by year except for block delay and gate arrival delay

#### By FAA region


```python
for i in plot_cols:
    plot_swarm("faa_region", i, hue=None, data=df, xlab="faa_region",
           ylab=i, title= i + " by region")
```


![png](output_50_0.png)



![png](output_50_1.png)



![png](output_50_2.png)



![png](output_50_3.png)



![png](output_50_4.png)



![png](output_50_5.png)



![png](output_50_6.png)



![png](output_50_7.png)



![png](output_50_8.png)



![png](output_50_9.png)



![png](output_50_10.png)



![png](output_50_11.png)



![png](output_50_12.png)



![png](output_50_13.png)



![png](output_50_14.png)


##### Observations

- Swarm plot shows that certain regions have higher delays (AEA - Eastern)

### Plotting latitude and longitude


```python
plt.scatter("longitude", "latitude", data=df)
```




    <matplotlib.collections.PathCollection at 0x119212e10>




![png](output_53_1.png)


# Principal component analysis

## Define X and Y


```python
X_scaler = preprocessing.StandardScaler()
X = df[plot_cols[3:]]
del X["average_taxi_out_time"]
X = pd.DataFrame(X_scaler.fit_transform(X), columns = X.columns)
```


```python
X.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 799 entries, 0 to 798
    Data columns (total 11 columns):
    average_gate_departure_delay       799 non-null float64
    average_taxi_out_delay             799 non-null float64
    average_airport_departure_delay    799 non-null float64
    average_airborne_delay             799 non-null float64
    average_taxi_in_delay              799 non-null float64
    average_block_delay                799 non-null float64
    average_gate_arrival_delay         799 non-null float64
    departure_cancellations            799 non-null float64
    arrival_cancellations              799 non-null float64
    departure_diversions               799 non-null float64
    arrival_diversions                 799 non-null float64
    dtypes: float64(11)
    memory usage: 68.7 KB


Different parts of X:

- departure delay
    - average_gate_departure_delay
    - average_taxi_out_delay
    - average_airport_departure_delay
- arrival delay
    - average_airborne_delay
    - average_taxi_in_delay
    - average_gate_arrival_delay
- overall delay
    - average_block_delay
- cancellations
    - departure_cancellations
    - arrival_cancellations
- diversions
    - departure_diversions
    - arrival_diversions

## Fit PCA and plot the cumulative sum of the explained variance ratio


```python
### Helper function for plotting cumulative sum
def plot_cumulative(x, title=None):
    cumsum = list(np.cumsum(x))
    cumsum.insert(0,0)
    plt.plot(cumsum);
    plt.title("Explained variance ratio for "+title);
    plt.xlabel("Number of components");
    plt.ylabel("Cumulative explained variance ratio");
    plt.ylim(0,1.1);
```

### Delay columns


```python
pca = decomposition.PCA()
delay = pca.fit_transform(X[[i for i in X.columns if "delay" in i]])
```


```python
plot_cumulative(pca.explained_variance_ratio_, "delays")
```


![png](output_63_0.png)


#### Observations

- With 3 components we can explain 85% of the variance in the variables


```python
pca = decomposition.PCA(n_components=3)
delay = pca.fit_transform(X[[i for i in X.columns if "delay" in i]])
```

### Cancellations and Diversions


```python
pca2 = decomposition.PCA()
candd = pca2.fit_transform(X[[i for i in X.columns if "cancel" in i or "diver" in i]])
```


```python
plot_cumulative(pca2.explained_variance_ratio_, "\ncancellations and diversions")
```


![png](output_68_0.png)


#### Observations

- With just 1 component we can explain more than 80% of the variance in the variables


```python
pca2 = decomposition.PCA(n_components=1)
candd = pca2.fit_transform(X[[i for i in X.columns if "cancel" in i or "diver" in i]])
```

### In combination


```python
pca3 = decomposition.PCA()
X_decomp = pca3.fit_transform(X)
```


```python
plot_cumulative(pca3.explained_variance_ratio_, "all")
```


![png](output_73_0.png)


#### Observations

- With 3 components we can explain more than 80% of the variance in the variables


```python
pca3 = decomposition.PCA(n_components=3)
X_decomp = pca3.fit_transform(X)
```

## 3D plots of our principal components

### Helper function for 3D plots


```python
def plot_3d(x, c=None, cmap=None):
    fig = plt.figure(figsize=(10,10))
    ax = fig.add_subplot(111, projection='3d')
    ax.scatter(x["pc1"], x["pc2"], x["pc3"], c=c, cmap=cmap)
    ax.set_xlabel("pc1");
    ax.set_ylabel("pc2");
    ax.set_zlabel("pc3");
    ax.set_title("3D plot of principal components");
```

### Just delay components


```python
delay = pd.DataFrame(delay, columns=["pc1", "pc2", "pc3"])
```


```python
plot_3d(delay)
```


![png](output_81_0.png)


### Combined


```python
pc = pd.DataFrame(X_decomp, columns=["pc1", "pc2", "pc3"])
```


```python
plot_3d(pc)
```


![png](output_84_0.png)


# Clustering

## Adopting the code from our DBSCAN lecture


```python
def plot_dbscan(db, X):
    fig = plt.figure(figsize=(10,8))

    core_samples_mask = np.zeros_like(db.labels_, dtype=bool)
    core_samples_mask[db.core_sample_indices_] = True
    labels = db.labels_

    # Number of clusters in labels, ignoring noise if present.
    n_clusters_ = len(set(labels)) - (1 if -1 in labels else 0)

    # Black removed and is used for noise instead.
    unique_labels = set(labels)
    colors = plt.cm.Spectral(np.linspace(0, 1, len(unique_labels)))
    for k, col in zip(unique_labels, colors):
        if k == -1:
            # Black used for noise.
            col = 'k'

        class_member_mask = (labels == k)

        xy = X[class_member_mask & core_samples_mask]
        plt.plot(xy[:, 0], xy[:, 1], 'o', markerfacecolor=col,
                 markeredgecolor='k', markersize=5)

        xy = X[class_member_mask & ~core_samples_mask]
        plt.plot(xy[:, 0], xy[:, 1], 'o', markerfacecolor=col,
                 markeredgecolor='k', markersize=5)

    plt.title('Number of clusters: %d' % n_clusters_);

```


```python
def plot_others(db, X):
    fig = plt.figure(figsize=(10,8))
    labels = list(db.labels_)

    # Number of clusters in labels, ignoring noise if present.
    n_clusters_ = len(set(labels))
    plt.scatter(X[:,0], X[:,1], c= list(labels),s=50, cmap="rainbow")
    plt.title('Number of clusters: %d' % n_clusters_);    
```


```python
def tweaker(data, eps, min_samples, model, k):
    if data == "delay":
        X = delay.as_matrix()
    elif data == "all":
        X = pc.as_matrix()
    if model == "db":
        db = cluster.DBSCAN(eps=eps, min_samples=min_samples)
        db.fit(X)
        plot_dbscan(db, X)
    elif model == "kmeans":
        db = cluster.KMeans(n_clusters=k)
        db.fit(X)
        plot_others(db, X)
    elif model == "hierarchical":
        db = cluster.AgglomerativeClustering(n_clusters=k)
        db.fit(X)
        plot_others(db, X)

interact(tweaker, data=["delay", "all"],eps=(0.1,2.0,0.1), min_samples=(5,20,1),
        model = ["db", "kmeans", "hierarchical"], k=(2,5,1));
```


![png](output_89_0.png)


## Using SciPy

### Just delay information


```python
hc = scipy.cluster.hierarchy.linkage(delay, "complete")
```


```python
plt.subplots(figsize=(16,8));
scipy.cluster.hierarchy.dendrogram(hc);
plt.xticks([]);
```


![png](output_93_0.png)



```python
labels = scipy.cluster.hierarchy.fcluster(hc, 12, criterion="distance")

fig, ax = plt.subplots(figsize=(12,4));
ax.scatter(delay.iloc[:, 0], delay.iloc[:, 1], c=labels, s=50, cmap='rainbow');
plt.show();

plot_3d(delay, c=labels, cmap="rainbow")
```


![png](output_94_0.png)



![png](output_94_1.png)


### Using delay, cancellation and diversion information


```python
hc2 = scipy.cluster.hierarchy.linkage(pc, "complete")
```


```python
plt.subplots(figsize=(16,8));
scipy.cluster.hierarchy.dendrogram(hc2);
plt.xticks([]);
```


![png](output_97_0.png)



```python
labels2 = scipy.cluster.hierarchy.fcluster(hc2, 12, criterion="distance")

fig, ax = plt.subplots(figsize=(12,4));
ax.scatter(pc.iloc[:, 0], pc.iloc[:, 1], c=labels2, s=50, cmap='prism');
plt.show();

plot_3d(pc, c=labels2, cmap="prism")
```


![png](output_98_0.png)



![png](output_98_1.png)


# Interpreting our results

## Adding the cluster labels to the data


```python
df2 = df.copy()
```


```python
df2.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>airport</th>
      <th>year</th>
      <th>departures_for_metric_computation</th>
      <th>arrivals_for_metric_computation</th>
      <th>percent_on_time_gate_departures</th>
      <th>percent_on_time_airport_departures</th>
      <th>percent_on_time_gate_arrivals</th>
      <th>average_gate_departure_delay</th>
      <th>average_taxi_out_time</th>
      <th>average_taxi_out_delay</th>
      <th>...</th>
      <th>average_gate_arrival_delay</th>
      <th>departure_cancellations</th>
      <th>arrival_cancellations</th>
      <th>departure_diversions</th>
      <th>arrival_diversions</th>
      <th>faa_region</th>
      <th>state</th>
      <th>ap_type</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ABQ</td>
      <td>2004</td>
      <td>53971</td>
      <td>53818</td>
      <td>0.8030</td>
      <td>0.7809</td>
      <td>0.7921</td>
      <td>10.38</td>
      <td>9.89</td>
      <td>2.43</td>
      <td>...</td>
      <td>10.87</td>
      <td>242.0</td>
      <td>235.0</td>
      <td>71.0</td>
      <td>46.0</td>
      <td>ASW</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ABQ</td>
      <td>2005</td>
      <td>51829</td>
      <td>51877</td>
      <td>0.8140</td>
      <td>0.7922</td>
      <td>0.8001</td>
      <td>9.60</td>
      <td>9.79</td>
      <td>2.29</td>
      <td>...</td>
      <td>10.24</td>
      <td>221.0</td>
      <td>190.0</td>
      <td>61.0</td>
      <td>33.0</td>
      <td>ASW</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ABQ</td>
      <td>2006</td>
      <td>49682</td>
      <td>51199</td>
      <td>0.7983</td>
      <td>0.7756</td>
      <td>0.7746</td>
      <td>10.84</td>
      <td>9.89</td>
      <td>2.16</td>
      <td>...</td>
      <td>11.82</td>
      <td>392.0</td>
      <td>329.0</td>
      <td>71.0</td>
      <td>124.0</td>
      <td>ASW</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ABQ</td>
      <td>2007</td>
      <td>53255</td>
      <td>53611</td>
      <td>0.8005</td>
      <td>0.7704</td>
      <td>0.7647</td>
      <td>11.29</td>
      <td>10.34</td>
      <td>2.40</td>
      <td>...</td>
      <td>12.71</td>
      <td>366.0</td>
      <td>304.0</td>
      <td>107.0</td>
      <td>45.0</td>
      <td>ASW</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ABQ</td>
      <td>2008</td>
      <td>49589</td>
      <td>49512</td>
      <td>0.8103</td>
      <td>0.7844</td>
      <td>0.7875</td>
      <td>10.79</td>
      <td>10.41</td>
      <td>2.41</td>
      <td>...</td>
      <td>11.48</td>
      <td>333.0</td>
      <td>300.0</td>
      <td>79.0</td>
      <td>42.0</td>
      <td>ASW</td>
      <td>NM</td>
      <td>Federalized/Commercial</td>
      <td>35.040194</td>
      <td>-106.609194</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>




```python
df2["clustered_labels"] = np.array(labels)
```


```python
df2["clustered_labels"].value_counts()
```




    2    748
    1     51
    Name: clustered_labels, dtype: int64




```python
df2.to_csv("clustered_data.csv")
```

## Comparing clusters


```python
cluster1 = df2[df2["clustered_labels"]==1].reset_index()
```


```python
cluster2 = df2[df2["clustered_labels"]==2].reset_index()
```
for i in plot_cols:
    plotting = []
    for j in range(2004, 2015):
        plotting.append(cluster2[i][cluster2["year"]==j].median())
    plt.plot(plotting, '-', marker="o", markersize=10);
    plt.xlabel("year");
    plt.ylabel(i);
    plt.title(i);
    plt.xticks(range(0,11), [str(i) for i in range(2004, 2015)]);
    plt.show();

```python
for i in plot_cols:
    plt.subplots(figsize=(16,8))
    plot1 = []
    plot2 = []
    for k in range(2004, 2015):
        plot1.append(cluster1[i][cluster1["year"]==k].median())
        plot2.append(cluster2[i][cluster2["year"]==k].median())
    plt.plot(plot1, '-', marker="o", markersize=10, label="cluster1", alpha=0.5);
    plt.plot(plot2, '-', marker="o", markersize=10, label="cluster2", alpha=0.5);
    plt.xlabel("year");
    plt.ylabel(i);
    plt.title(i);
    plt.xticks(range(0,11), [str(i) for i in range(2004, 2015)]);
    plt.legend();
    plt.show();
```


![png](output_110_0.png)



![png](output_110_1.png)



![png](output_110_2.png)



![png](output_110_3.png)



![png](output_110_4.png)



![png](output_110_5.png)



![png](output_110_6.png)



![png](output_110_7.png)



![png](output_110_8.png)



![png](output_110_9.png)



![png](output_110_10.png)



![png](output_110_11.png)



![png](output_110_12.png)



![png](output_110_13.png)



![png](output_110_14.png)



```python
for i in plot_cols:
    plt.subplots(figsize=(16,8))
    plt.scatter(cluster1["year"], cluster1[i], c='r', s=100, label="cluster1");
    plt.scatter(cluster2["year"], cluster2[i], c='k', alpha=0.3, s=50, label="cluster2");
    plt.xlabel("year");
    plt.ylabel(i);
    plt.title(i);
    plt.legend();
    plt.show();
```


![png](output_111_0.png)



![png](output_111_1.png)



![png](output_111_2.png)



![png](output_111_3.png)



![png](output_111_4.png)



![png](output_111_5.png)



![png](output_111_6.png)



![png](output_111_7.png)



![png](output_111_8.png)



![png](output_111_9.png)



![png](output_111_10.png)



![png](output_111_11.png)



![png](output_111_12.png)



![png](output_111_13.png)



![png](output_111_14.png)


## Observations

- In our plots, we can see that in general, cluster 1 has more delays, cancellations and diversions than cluster 2.
- When we look at DBSCAN clustering, with using our principal components as features, we only get one cluster, with the rest being labeled as noise
    - Maybe KMeans and Agglomerative clustering are taking some of the noise and labelling them as a separate class


```python

```
