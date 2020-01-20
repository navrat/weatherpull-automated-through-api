# weatherpull-automated-through-api

import requests
import json

token = 'bNZFFAXltTxrecSAIVhsfokhkGKFCIHj'
r = requests.get('https://www.ncdc.noaa.gov/cdo-web/api/v2/data?datasetid=GHCND&datatypeid=TAVG&limit=1000&stationid=GHCND:USW00023129&startdate=2015-01-01&enddate=2015-12-31', headers={'token':token})
d = json.loads(r.text)

# First getting all location IDs for which we need the stations
token = 'bNZFFAXltTxrecSAIVhsfokhkGKFCIHj'
r = requests.get('https://www.ncdc.noaa.gov/cdo-web/api/v2/locations?locationcategoryid=ST&limit=52', headers={'token':token})
locations = json.loads(r.text)
import pandas as pd
us_state_locations = pd.DataFrame.from_dict(r.json()['results'])
us_state_locations.columns

# Getting all the stations for a US State and picking those that are alive and have high coveragestartdate = '2015-01-01'
enddate = '2019-12-31'
datasetid = 'GHCND' # datset id for "Daily Summaries
base_url = 'https://www.ncdc.noaa.gov/cdo-web/api/v2/stations'
stations = 'locationid='+str(us_state_locations['id'][0])+'&'+'datasetid='+str(datasetid)+'&'+'units=standard'+'&'+'limit=1000'
r = requests.get(base_url, headers={'token':token}, params=stations)
print("Request status code: "+str(r.status_code))
stations = pd.DataFrame.from_dict(r.json()['results'])
valid_stations = stations.loc[(stations.datacoverage>0.95) & (stations.mindate<=startdate) & (stations.maxdate>=enddate)]

# Pulling weather for the us state using location idparams = 'datasetid='+str(datasetid)+'&'+'locationid='+str(us_state_locations['id'][0])+'&'+'startdate='+str(startdate)+'&'+'enddate='+str(enddate)+'&'+'limit=1000'+'&'+'units=standard'
r = requests.get(base_url, params = params, headers={'token':token})
print("Request status code: "+str(r.status_code))
weatherdata = pd.DataFrame.from_dict(r.json()['results'])
weatherdata.shape

# Pulling data by stationid(s)params = 'datasetid='+str(datasetid)+'&'+'stationid='+str(pd.Series(valid_stations['id'][1:2]).str.cat(sep='&'))+'&'+'startdate='+str(startdate)+'&'+'enddate='+str(enddate)+'&'+'limit=1000'+'&'+'units=standard'
r = requests.get(base_url, params = params, headers={'token':token})
print("Request status code: "+str(r.status_code))
weatherdata = pd.DataFrame.from_dict(r.json()['results'])
weatherdata.shape
