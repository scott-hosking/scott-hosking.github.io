
# Using Baspy + Xarray to work with CMIP data


```python
import baspy as bp
import matplotlib.pyplot as plt
import cartopy.crs as ccrs
import xarray as xr

%matplotlib inline 
```


```python
catlg = bp.catalogue(dataset='cmip5', Experiment='historical', Frequency='mon', Var='tas', Model='HadGEM2-ES')
catlg.drop(columns=['Path','DataFiles'])
```

    Updating cached catalogue...
    >> Current cached values from catalogue (this can be extended by specifying additional values) <<
    {'Experiment': ['rcp26', 'rcp45', 'piControl', 'rcp85', 'historical'], 'Frequency': ['mon']}
    





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
      <th>Centre</th>
      <th>Model</th>
      <th>Experiment</th>
      <th>Frequency</th>
      <th>SubModel</th>
      <th>CMOR</th>
      <th>RunID</th>
      <th>Version</th>
      <th>Var</th>
      <th>StartDate</th>
      <th>EndDate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>486768</th>
      <td>MOHC</td>
      <td>HadGEM2-ES</td>
      <td>historical</td>
      <td>mon</td>
      <td>atmos</td>
      <td>Amon</td>
      <td>r2i1p1</td>
      <td>v20110418</td>
      <td>tas</td>
      <td>185912</td>
      <td>200512</td>
    </tr>
    <tr>
      <th>486814</th>
      <td>MOHC</td>
      <td>HadGEM2-ES</td>
      <td>historical</td>
      <td>mon</td>
      <td>atmos</td>
      <td>Amon</td>
      <td>r4i1p1</td>
      <td>v20110418</td>
      <td>tas</td>
      <td>185912</td>
      <td>200511</td>
    </tr>
    <tr>
      <th>486860</th>
      <td>MOHC</td>
      <td>HadGEM2-ES</td>
      <td>historical</td>
      <td>mon</td>
      <td>atmos</td>
      <td>Amon</td>
      <td>r3i1p1</td>
      <td>v20110418</td>
      <td>tas</td>
      <td>185912</td>
      <td>200512</td>
    </tr>
    <tr>
      <th>486908</th>
      <td>MOHC</td>
      <td>HadGEM2-ES</td>
      <td>historical</td>
      <td>mon</td>
      <td>atmos</td>
      <td>Amon</td>
      <td>r1i1p1</td>
      <td>v20120928</td>
      <td>tas</td>
      <td>185912</td>
      <td>200511</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Extract one model run
catlg = catlg.iloc[3]
catlg
```




    Centre                                                     MOHC
    Model                                                HadGEM2-ES
    Experiment                                           historical
    Frequency                                                   mon
    SubModel                                                  atmos
    CMOR                                                       Amon
    RunID                                                    r1i1p1
    Version                                               v20120928
    Var                                                         tas
    StartDate                                                185912
    EndDate                                                  200511
    Path          /badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES...
    DataFiles     tas_Amon_HadGEM2-ES_historical_r1i1p1_185912-1...
    Name: 486908, dtype: object



## Using Xarray


```python
### Read in multiple files as a Dataset
files = bp.get_files(catlg)
print('files =', files, '\n')
ds = xr.open_mfdataset(files)

'''
Alternatively, if you don't have access to the CMIP5 archive, you can download 
and use some CMIP6 sample data by running this line:
>>> ds = bp.XR.eg_Dataset()
'''

print(ds)
```

    files = ['/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_185912-188411.nc', '/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_188412-190911.nc', '/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_190912-193411.nc', '/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_193412-195911.nc', '/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_195912-198411.nc', '/badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/atmos/Amon/r1i1p1/v20120928/tas/tas_Amon_HadGEM2-ES_historical_r1i1p1_198412-200511.nc'] 
    
    <xarray.Dataset>
    Dimensions:    (bnds: 2, lat: 145, lon: 192, time: 1752)
    Coordinates:
      * lat        (lat) float64 -90.0 -88.75 -87.5 -86.25 -85.0 -83.75 -82.5 ...
      * lon        (lon) float64 0.0 1.875 3.75 5.625 7.5 9.375 11.25 13.12 15.0 ...
        height     float64 1.5
      * time       (time) datetime64[ns] 1859-12-16 1860-01-16 1860-02-16 ...
    Dimensions without coordinates: bnds
    Data variables:
        time_bnds  (time, bnds) float64 dask.array<shape=(1752, 2), chunksize=(300, 2)>
        lat_bnds   (time, lat, bnds) float64 dask.array<shape=(1752, 145, 2), chunksize=(300, 145, 2)>
        lon_bnds   (time, lon, bnds) float64 dask.array<shape=(1752, 192, 2), chunksize=(300, 192, 2)>
        tas        (time, lat, lon) float32 dask.array<shape=(1752, 145, 192), chunksize=(300, 145, 192)>
    Attributes:
        institution:            Met Office Hadley Centre, Fitzroy Road, Exeter, D...
        institute_id:           MOHC
        experiment_id:          historical
        source:                 HadGEM2-ES (2009) atmosphere: HadGAM2 (N96L38); o...
        model_id:               HadGEM2-ES
        forcing:                GHG, SA, Oz, LU, Sl, Vl, BC, OC, (GHG = CO2, N2O,...
        parent_experiment_id:   piControl
        parent_experiment_rip:  r1i1p1
        branch_time:            0.0
        contact:                chris.d.jones@metoffice.gov.uk, michael.sanderson...
        history:                MOHC pp to CMOR/NetCDF convertor (version 1.5.1) ...
        references:             Bellouin N. et al, (2007) Improved representation...
        initialization_method:  1
        physics_version:        1
        tracking_id:            1b8bb1c9-899b-4669-bfe1-8cce30b7c5c8
        mo_runid:               ajhoh
        product:                output
        experiment:             historical
        frequency:              mon
        creation_date:          2010-12-03T16:28:10Z
        Conventions:            CF-1.4
        project_id:             CMIP5
        table_id:               Table Amon (12 November 2010) 6e535ddfacb41fb7a25...
        title:                  HadGEM2-ES model output prepared for CMIP5 histor...
        parent_experiment:      pre-industrial control
        modeling_realm:         atmos
        realization:            1
        cmor_version:           2.5.0



```python
# read a DataArray (e.g., a single variable) from the Dataset
da = ds.tas
da
```




    <xarray.DataArray 'tas' (time: 1752, lat: 145, lon: 192)>
    dask.array<shape=(1752, 145, 192), dtype=float32, chunksize=(300, 145, 192)>
    Coordinates:
      * lat      (lat) float64 -90.0 -88.75 -87.5 -86.25 -85.0 -83.75 -82.5 ...
      * lon      (lon) float64 0.0 1.875 3.75 5.625 7.5 9.375 11.25 13.12 15.0 ...
        height   float64 1.5
      * time     (time) datetime64[ns] 1859-12-16 1860-01-16 1860-02-16 ...
    Attributes:
        standard_name:     air_temperature
        long_name:         Near-Surface Air Temperature
        comment:           near-surface (usually, 2 meter) air temperature.
        units:             K
        original_name:     mo: m01s03i236
        cell_methods:      time: mean
        cell_measures:     area: areacella
        history:           2010-12-03T16:28:10Z altered by CMOR: Treated scalar d...
        associated_files:  baseURL: http://cmip-pcmdi.llnl.gov/CMIP5/dataLocation...



# Plotting


```python
print('shape =', da.shape, '\n')

da.plot()
```

    shape = (1752, 145, 192) 
    





    (array([1.4756000e+04, 6.9845800e+05, 1.2227680e+06, 2.6831180e+06,
            3.4822860e+06, 3.5055740e+06, 7.7460610e+06, 1.0382627e+07,
            1.7711495e+07, 1.3285370e+06]),
     array([193.73047, 205.6619 , 217.5933 , 229.52473, 241.45615, 253.38757,
            265.319  , 277.25043, 289.18182, 301.11325, 313.04468],
           dtype=float32),
     <a list of 10 Patch objects>)




![png](/images/output_8_2.png)



```python
# select first time index
da.isel(time=0).plot()
```




    <matplotlib.collections.QuadMesh at 0x7fa3b534b8d0>




![png](/images/output_9_1.png)



```python
### Plotting
crs = ccrs.SouthPolarStereo(central_longitude=0.0)
ax = plt.subplot(projection=crs)
ax.set_extent([-180,180,-90,-60], ccrs.PlateCarree() )
ax.gridlines(ylocs=range(-90,-30,5))
da.isel(time=0).plot.contourf(ax=ax, transform=ccrs.PlateCarree(), cmap=plt.cm.Blues_r, extend='both')
ax.coastlines('110m', color='k')
```




    <cartopy.mpl.feature_artist.FeatureArtist at 0x7fa3b55bbbe0>




![png](/images/output_10_1.png)

