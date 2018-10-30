
# NYC 311 API

You've gotten a chance to explore API basics through the Yelp API. In preparation for your final project, we will investigate another API from scratch. This should provide you with another familiar dataq option as well as practice for applying the same process to new unfamiliar APIs.

To start, go over to the API documentation at: 

https://dev.socrata.com/foundry/data.cityofnewyork.us/fhrw-4uyv


<img src="311_api_docs.png">

## Make an initial API call to retrieve 311 complaints from a neighborhood or zip code of your choice.


```python
# Your code here
```


```python
# Formulation 1



#!/usr/bin/env python

# make sure to install these packages before running:
# pip install pandas
# pip install sodapy

import pandas as pd
from sodapy import Socrata

# Unauthenticated client only works with public data sets. Note 'None'
# in place of application token, and no username or password:
client = Socrata("data.cityofnewyork.us", token)

# Example authenticated client (needed for non-public datasets):
# client = Socrata(data.cityofnewyork.us,
#                  MyAppToken,
#                  userame="user@example.com",
#                  password="AFakePassword")

# First 2000 results, returned as JSON from API / converted to Python list of
# dictionaries by sodapy.
results = client.get("fhrw-4uyv", incident_zip = '11204', limit=2000)
```


```python
# Formulation 2
import requests
import pandas as pd

zip_code = '11204'

# can't figure out date ranges at the moment...
start_date = '2018-01-01T12:00:00'
end_date = '2018-02-01T12:00:00'

# create pull request based on parameters
url = "https://data.cityofnewyork.us/resource/fhrw-4uyv.json?incident_zip={}".format(zip_code)

# do the pull
response = requests.get(url)
if response.status_code == 200:
    data = response.json()
else:
    print('Hit an error.')
```

## Briefly Explore the Structure of the Response You Received.


```python
#Formulation 1
type(results)
```




    list




```python
len(results)
```




    2000




```python
results[0]
```




    {'address_type': 'ADDRESS',
     'agency': 'NYPD',
     'agency_name': 'New York City Police Department',
     'bbl': '3061590036',
     'borough': 'BROOKLYN',
     'city': 'BROOKLYN',
     'closed_date': '2018-10-28T12:16:45.000',
     'community_board': '11 BROOKLYN',
     'complaint_type': 'Blocked Driveway',
     'created_date': '2018-10-28T11:21:12.000',
     'cross_street_1': '16 AVENUE',
     'cross_street_2': '17 AVENUE',
     'descriptor': 'Partial Access',
     'due_date': '2018-10-28T19:21:12.000',
     'facility_type': 'Precinct',
     'incident_address': '1674 69 STREET',
     'incident_zip': '11204',
     'latitude': '40.6187013953729',
     'location': {'type': 'Point',
      'coordinates': [-73.995627099806, 40.618701395373]},
     'location_type': 'Street/Sidewalk',
     'longitude': '-73.9956270998064',
     'open_data_channel_type': 'PHONE',
     'park_borough': 'BROOKLYN',
     'park_facility_name': 'Unspecified',
     'resolution_action_updated_date': '2018-10-28T12:16:45.000',
     'resolution_description': 'The Police Department responded and upon arrival those responsible for the condition were gone.',
     'status': 'Closed',
     'street_name': '69 STREET',
     'unique_key': '40685123',
     'x_coordinate_state_plane': '985464',
     'y_coordinate_state_plane': '164686'}




```python
# Formulation 2
print(type(data))
```

    <class 'list'>



```python
len(data)
```




    1000




```python
data[0]
```




    {'address_type': 'ADDRESS',
     'agency': 'DSNY',
     'agency_name': 'Department of Sanitation',
     'bbl': '3054640061',
     'borough': 'BROOKLYN',
     'city': 'BROOKLYN',
     'community_board': '12 BROOKLYN',
     'complaint_type': 'Request Large Bulky Item Collection',
     'created_date': '2018-10-28T17:21:00.000',
     'cross_street_1': '20 AVENUE',
     'cross_street_2': '51 STREET',
     'descriptor': 'Request Large Bulky Item Collection',
     'facility_type': 'N/A',
     'incident_address': '961 DAHILL ROAD',
     'incident_zip': '11204',
     'latitude': '40.62436991521529',
     'location': {'type': 'Point',
      'coordinates': [-73.976991692011, 40.624369915215]},
     'location_type': 'Sidewalk',
     'longitude': '-73.97699169201135',
     'open_data_channel_type': 'PHONE',
     'park_borough': 'BROOKLYN',
     'park_facility_name': 'Unspecified',
     'resolution_action_updated_date': '2018-10-30T00:00:00.000',
     'resolution_description': 'The Department of Sanitation has sent this complaint to the appropriate district garage or bureau for further action.',
     'status': 'Assigned',
     'street_name': 'DAHILL ROAD',
     'unique_key': '40682665',
     'x_coordinate_state_plane': '990637',
     'y_coordinate_state_plane': '166752'}



## Create a Pandas DataFrame of the Data From the Response


```python
# Formulation 1 
df = pd.DataFrame(results)

print(len(df))
print(df.columns)
df.head()
```

    2000
    Index(['address_type', 'agency', 'agency_name', 'bbl', 'borough', 'city',
           'closed_date', 'community_board', 'complaint_type', 'created_date',
           'cross_street_1', 'cross_street_2', 'descriptor', 'due_date',
           'facility_type', 'incident_address', 'incident_zip',
           'intersection_street_1', 'intersection_street_2', 'latitude',
           'location', 'location_type', 'longitude', 'open_data_channel_type',
           'park_borough', 'park_facility_name', 'resolution_action_updated_date',
           'resolution_description', 'status', 'street_name',
           'taxi_company_borough', 'unique_key', 'x_coordinate_state_plane',
           'y_coordinate_state_plane'],
          dtype='object')





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
      <th>address_type</th>
      <th>agency</th>
      <th>agency_name</th>
      <th>bbl</th>
      <th>borough</th>
      <th>city</th>
      <th>closed_date</th>
      <th>community_board</th>
      <th>complaint_type</th>
      <th>created_date</th>
      <th>...</th>
      <th>park_borough</th>
      <th>park_facility_name</th>
      <th>resolution_action_updated_date</th>
      <th>resolution_description</th>
      <th>status</th>
      <th>street_name</th>
      <th>taxi_company_borough</th>
      <th>unique_key</th>
      <th>x_coordinate_state_plane</th>
      <th>y_coordinate_state_plane</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3061590036</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-28T12:16:45.000</td>
      <td>11 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-28T11:21:12.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-28T12:16:45.000</td>
      <td>The Police Department responded and upon arriv...</td>
      <td>Closed</td>
      <td>69 STREET</td>
      <td>NaN</td>
      <td>40685123</td>
      <td>985464</td>
      <td>164686</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3066020060</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-28T20:18:46.000</td>
      <td>11 BROOKLYN</td>
      <td>Blocked Driveway</td>
      <td>2018-10-28T18:24:46.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-28T20:18:46.000</td>
      <td>The Police Department responded and upon arriv...</td>
      <td>Closed</td>
      <td>WEST 5 STREET</td>
      <td>NaN</td>
      <td>40685099</td>
      <td>990124</td>
      <td>161156</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3055130072</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-29T00:27:09.000</td>
      <td>12 BROOKLYN</td>
      <td>Noise - Residential</td>
      <td>2018-10-28T22:43:32.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:27:09.000</td>
      <td>The Police Department responded to the complai...</td>
      <td>Closed</td>
      <td>60 STREET</td>
      <td>NaN</td>
      <td>40685075</td>
      <td>988536</td>
      <td>165307</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ADDRESS</td>
      <td>DEP</td>
      <td>Department of Environmental Protection</td>
      <td>3062260028</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>Water System</td>
      <td>2018-10-28T14:45:00.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Open</td>
      <td>BAY RIDGE PARKWAY</td>
      <td>NaN</td>
      <td>40684921</td>
      <td>985084</td>
      <td>162969</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>NaN</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-29T00:17:02.000</td>
      <td>11 BROOKLYN</td>
      <td>Illegal Parking</td>
      <td>2018-10-28T22:16:51.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:17:02.000</td>
      <td>The Police Department issued a summons in resp...</td>
      <td>Closed</td>
      <td>71 STREET</td>
      <td>NaN</td>
      <td>40684027</td>
      <td>987988</td>
      <td>162028</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>




```python
# Formulation 2 
df = pd.DataFrame(data)

print(len(df))
print(df.columns)
df.head()
```

    1000
    Index(['address_type', 'agency', 'agency_name', 'bbl', 'borough', 'city',
           'closed_date', 'community_board', 'complaint_type', 'created_date',
           'cross_street_1', 'cross_street_2', 'descriptor', 'due_date',
           'facility_type', 'incident_address', 'incident_zip',
           'intersection_street_1', 'intersection_street_2', 'latitude',
           'location', 'location_type', 'longitude', 'open_data_channel_type',
           'park_borough', 'park_facility_name', 'resolution_action_updated_date',
           'resolution_description', 'status', 'street_name',
           'taxi_company_borough', 'unique_key', 'x_coordinate_state_plane',
           'y_coordinate_state_plane'],
          dtype='object')





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
      <th>address_type</th>
      <th>agency</th>
      <th>agency_name</th>
      <th>bbl</th>
      <th>borough</th>
      <th>city</th>
      <th>closed_date</th>
      <th>community_board</th>
      <th>complaint_type</th>
      <th>created_date</th>
      <th>...</th>
      <th>park_borough</th>
      <th>park_facility_name</th>
      <th>resolution_action_updated_date</th>
      <th>resolution_description</th>
      <th>status</th>
      <th>street_name</th>
      <th>taxi_company_borough</th>
      <th>unique_key</th>
      <th>x_coordinate_state_plane</th>
      <th>y_coordinate_state_plane</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADDRESS</td>
      <td>DSNY</td>
      <td>Department of Sanitation</td>
      <td>3054640061</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>12 BROOKLYN</td>
      <td>Request Large Bulky Item Collection</td>
      <td>2018-10-28T17:21:00.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-30T00:00:00.000</td>
      <td>The Department of Sanitation has sent this com...</td>
      <td>Assigned</td>
      <td>DAHILL ROAD</td>
      <td>NaN</td>
      <td>40682665</td>
      <td>990637</td>
      <td>166752</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADDRESS</td>
      <td>DEP</td>
      <td>Department of Environmental Protection</td>
      <td>3055620009</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>11 BROOKLYN</td>
      <td>Water System</td>
      <td>2018-10-28T15:53:00.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Open</td>
      <td>19 AVENUE</td>
      <td>NaN</td>
      <td>40682760</td>
      <td>987367</td>
      <td>164122</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ADDRESS</td>
      <td>DEP</td>
      <td>Department of Environmental Protection</td>
      <td>3055620009</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-28T15:05:00.000</td>
      <td>11 BROOKLYN</td>
      <td>Water System</td>
      <td>2018-10-28T15:03:00.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-28T15:05:00.000</td>
      <td>The Department of Environmental Protection det...</td>
      <td>Closed</td>
      <td>19 AVENUE</td>
      <td>NaN</td>
      <td>40682831</td>
      <td>987367</td>
      <td>164122</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ADDRESS</td>
      <td>DSNY</td>
      <td>Department of Sanitation</td>
      <td>3054370009</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>NaN</td>
      <td>12 BROOKLYN</td>
      <td>Request Large Bulky Item Collection</td>
      <td>2018-10-28T15:14:00.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-31T00:00:00.000</td>
      <td>The Department of Sanitation has sent this com...</td>
      <td>Assigned</td>
      <td>16 AVENUE</td>
      <td>NaN</td>
      <td>40682579</td>
      <td>988735</td>
      <td>169761</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ADDRESS</td>
      <td>NYPD</td>
      <td>New York City Police Department</td>
      <td>3055007501</td>
      <td>BROOKLYN</td>
      <td>BROOKLYN</td>
      <td>2018-10-29T00:41:22.000</td>
      <td>12 BROOKLYN</td>
      <td>Illegal Parking</td>
      <td>2018-10-28T21:38:32.000</td>
      <td>...</td>
      <td>BROOKLYN</td>
      <td>Unspecified</td>
      <td>2018-10-29T00:41:22.000</td>
      <td>The Police Department issued a summons in resp...</td>
      <td>Closed</td>
      <td>58 STREET</td>
      <td>NaN</td>
      <td>40682564</td>
      <td>989756</td>
      <td>165018</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 34 columns</p>
</div>



## Create a Histogram of the Complaint Types From Your Dataset


```python
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# Your code here 
df.complaint_type.value_counts().plot(kind='barh', figsize=(8,12))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x11e586be0>




![png](index_files/index_17_1.png)

