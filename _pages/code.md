---
permalink: /code
title: "Code: Software and Tools"
excerpt: "code"
toc: True
---

### Easily grab weather station data from around the globe ([GitHub repo](https://github.com/scott-hosking/get_station_data))

```python
import numpy as np
import pandas as pd
from get_station_data import ghcnd
from get_station_data.util import nearest_stn

### Read station metadata
stn_md = ghcnd.get_stn_metadata()

### Choose a location (lon/lat) and number of nearest neighbours
london_lon_lat = -0.1278, 51.5074
my_stns = nearest_stn(stn_md, london_lon_lat[0], london_lon_lat[1], n_neighbours=5 )

### Download and extract data into a pandas DataFrame
df = ghcnd.get_data(my_stns)
```

### Making it far easier to read in and work with large volumes of climate model output from CMIP5/6 ([GitHub repo](https://github.com/scott-hosking/baspy))

```python
import baspy as bp

### Retrieve a filtered version of the CMIP5 catalogue as a Pandas DataFrame
df = bp.catalogue(dataset='cmip5', Model='HadGEM2-CC', RunID='r1i1p1', 
                    Experiment='historical', Var=['tas', 'pr'], 
                    Frequency='mon')

### Iterate over rows in catalogue
for index, row in df.iterrows():
    ds = bp.open_dataset(row) ### In Xarray
    cubes = bp.get_cube(row)  ### Or... In Iris
```