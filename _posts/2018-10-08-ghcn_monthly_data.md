---
title: 'Get monthly average weather station data (Global)'
date: 2018-10-17
permalink: /notebooks/ghcn_monthly/
tags:
  - python
  - weather station
  - ghcn
  - notebook
  - jupyter
---

Extract countries of interest (along with their coordinates) from the
[Global Historical Climatology Network - Monthly (GHCNM) Version 3](https://www.ncdc.noaa.gov/ghcnm/v3.php)

A README file can be found [here](ftp://ftp.ncdc.noaa.gov/pub/data/ghcn/v3/README)

To run this script you will need to download the .dat and .inv files stored in the compressed files (ghncm.*.tar.gz) found [here](ftp://ftp.ncdc.noaa.gov/pub/data/ghcn/v3/)

The code can be downloaded from the [_get_station_data_](https://github.com/scotthosking/get-station-data) github repository

```python
import numpy as np
import pandas as pd
from get_station_data import ghcnm
```

### Name of original data file from GHCN-M
ftp://ftp.ncdc.noaa.gov/pub/data/ghcn/v3/


```python
data_version = 'v3.3.0.20180404'
data_fname   = 'raw_data/ghcnm.tavg.'+data_version+'.qca.dat'
stn_md_fname = 'raw_data/ghcnm.tavg.'+data_version+'.qca.inv'

stn_md = ghcnm.get_stn_metadata(stn_md_fname)

stn_md.head()
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
      <th>station</th>
      <th>lat</th>
      <th>lon</th>
      <th>elev</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10160355000</td>
      <td>36.93</td>
      <td>6.95</td>
      <td>7.0</td>
      <td>SKIKDA</td>
      <td>ALGERIA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10160360000</td>
      <td>36.83</td>
      <td>7.82</td>
      <td>4.0</td>
      <td>ANNABA</td>
      <td>ALGERIA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10160390000</td>
      <td>36.72</td>
      <td>3.25</td>
      <td>25.0</td>
      <td>DAR-EL-BEIDA</td>
      <td>ALGERIA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10160395001</td>
      <td>36.52</td>
      <td>4.18</td>
      <td>942.0</td>
      <td>FT. NATIONAL</td>
      <td>ALGERIA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10160400001</td>
      <td>36.80</td>
      <td>5.10</td>
      <td>230.0</td>
      <td>CAP CARBON</td>
      <td>ALGERIA</td>
    </tr>
  </tbody>
</table>
</div>



### Specify countries to extract data from


```python
country_names = ['Egypt', 'Libya', 'Sudan', 'ISRAEL', \
                    'SAUDI ARABIA', 'Chad', 'Jordan']

my_stns = ghcnm.extract_countries(stn_md, country_names)

my_stns.head()
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
      <th>station</th>
      <th>lat</th>
      <th>lon</th>
      <th>elev</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>132</th>
      <td>11064700000</td>
      <td>12.13</td>
      <td>15.03</td>
      <td>295.0</td>
      <td>NDJAMENA</td>
      <td>CHAD</td>
    </tr>
    <tr>
      <th>133</th>
      <td>11064701000</td>
      <td>14.12</td>
      <td>15.32</td>
      <td>355.0</td>
      <td>MAO</td>
      <td>CHAD</td>
    </tr>
    <tr>
      <th>134</th>
      <td>11064702000</td>
      <td>13.43</td>
      <td>14.73</td>
      <td>292.0</td>
      <td>BOL-BERIM</td>
      <td>CHAD</td>
    </tr>
    <tr>
      <th>135</th>
      <td>11064705000</td>
      <td>10.48</td>
      <td>16.72</td>
      <td>336.0</td>
      <td>BOUSSO</td>
      <td>CHAD</td>
    </tr>
    <tr>
      <th>136</th>
      <td>11064706000</td>
      <td>8.62</td>
      <td>16.07</td>
      <td>422.0</td>
      <td>MOUNDOU</td>
      <td>CHAD</td>
    </tr>
  </tbody>
</table>
</div>



### Extract data for specified stations into a Pandas DataFrame


```python
df = ghcnm.get_data(data_fname, my_stns)
df.columns
```




    Index(['country', 'name', 'station', 'lat', 'lon', 'elev', 'year', 'month',
           'variable', 'value', 'dmflag', 'qcflag', 'dsflag'],
          dtype='object')




```python
df.drop(columns=['dmflag', 'qcflag', 'dsflag']).tail(n=10)
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
      <th>country</th>
      <th>name</th>
      <th>station</th>
      <th>lat</th>
      <th>lon</th>
      <th>elev</th>
      <th>year</th>
      <th>month</th>
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>79298</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>3</td>
      <td>TAVG</td>
      <td>18.9</td>
    </tr>
    <tr>
      <th>79299</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>4</td>
      <td>TAVG</td>
      <td>24.3</td>
    </tr>
    <tr>
      <th>79300</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>5</td>
      <td>TAVG</td>
      <td>26.9</td>
    </tr>
    <tr>
      <th>79301</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>6</td>
      <td>TAVG</td>
      <td>30.5</td>
    </tr>
    <tr>
      <th>79302</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>7</td>
      <td>TAVG</td>
      <td>32.4</td>
    </tr>
    <tr>
      <th>79303</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>8</td>
      <td>TAVG</td>
      <td>31.7</td>
    </tr>
    <tr>
      <th>79304</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>9</td>
      <td>TAVG</td>
      <td>28.9</td>
    </tr>
    <tr>
      <th>79305</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>10</td>
      <td>TAVG</td>
      <td>26.8</td>
    </tr>
    <tr>
      <th>79306</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>11</td>
      <td>TAVG</td>
      <td>23.2</td>
    </tr>
    <tr>
      <th>79307</th>
      <td>JORDAN</td>
      <td>AQABA AIRPORT</td>
      <td>62440340000</td>
      <td>29.63</td>
      <td>35.02</td>
      <td>51</td>
      <td>1990</td>
      <td>12</td>
      <td>TAVG</td>
      <td>18.0</td>
    </tr>
  </tbody>
</table>
</div>



### Save to file


```python
df.to_csv('Egypt_surrounding_ghcnm.csv', index=False)
```
