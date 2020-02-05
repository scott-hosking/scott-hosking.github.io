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

# Notebooks

Here are some Python notebooks I use for research and teaching. Please feel free to get in touch if you have any comments, _even if just to say you found them useful_

## Getting started
* Using [**BASpy** with **Xarray**](/notebooks/baspy_using_xarray) to read in climate model data (CMIP5)
* Getting weather station data from around the globe (both [monthly](/notebooks/ghcn_monthly) and [daily](/notebooks/ghcn_daily) averaged data) 

## Research
* Automatic detection of low pressure systems from pressure or geopotential height fields. Used here in this example to detect the [_Amundsen Sea Low_](/notebooks/asl_detection)
* Analysing simulated Antarctic preciptation from the [RACMO regional climate model](/notebooks/racmo_with_xarray)
* Working with model output from the the [WRF regional climate model](/notebooks/wrf_with_xarray)

## Useful scripts
* Smoothing a timeseries [using a low-pass filter](/notebooks/smooth_timeseries)

<sub>_I have other notebooks [here](https://nbviewer.jupyter.org/github/scott-hosking/notebooks/tree/master/) which I may or may not migrate over time to this site_</sub>