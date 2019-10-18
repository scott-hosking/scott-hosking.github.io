---
permalink: /code
title: "Code:Tools"
excerpt: "here are some tools I have built to improve work efficiency"
toc: True
---

Here I have listed some Python tools I have developed to improve research efficiency. Please feel free to get in touch if you have any suggestions, or if you found them useful

## Get **weather station** data (Global) 
* Can run from your laptop etc
* [GitHub repo](https://github.com/scott-hosking/get_station_data)
* Notebooks for working with [monthly](/notebooks/ghcn_monthly) and [daily](/notebooks/ghcn_daily) averaged data

```python
from get_station_data import ghcnd
from get_station_data.util import nearest_stn

### Read station metadata
stn_md = ghcnd.get_stn_metadata()

### Choose a location & number of nearest neighbours
lon_lat = -0.1278, 51.5074 # i.e., London
my_stns = nearest_stn(stn_md, lon_lat[0], lon_lat[1],
                        n_neighbours=5 )

### Download and extract data into a pandas DataFrame
df = ghcnd.get_data(my_stns)
```

## Get **climate model** output (CMIP5/6)
* Needs to run on systems where data is stored (e.g. [JASMIN](http://www.jasmin.ac.uk/))
* [GitHub repo](https://github.com/scott-hosking/baspy)

```python
import baspy as bp

### Retrieve a filtered catalogue as a Pandas DataFrame
df = bp.catalogue(dataset='cmip5', Model='HadGEM2-CC', 
                    RunID='r1i1p1', Experiment='historical', 
                    Var=['tas', 'pr'], Frequency='mon')

### Iterate over rows in catalogue
for index, row in df.iterrows():
    ds = bp.open_dataset(row) ### In Xarray
    cubes = bp.get_cube(row)  ### Or... In Iris
```