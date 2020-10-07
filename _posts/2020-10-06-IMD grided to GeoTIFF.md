---
layout: post
title: India Meteorological Department (IMD) grided data to GeoTIFF
author: Saswata Nandi and Pratiman Patel
---

## Description
IMDLIB is a python package to download and handle binary grided data from India Meteorological Department (IMD). For more information about the IMD datasets, follow the following link: http://imdpune.gov.in/Clim_Pred_LRF_New/Grided_Data_Download.html

## Installation
IMDLIB can be installed via pip. It is tested for both Windows and Linux platforms with 64-bit architecture only.

```bash
pip install imdlib
```

## Downloading data from IMD

IMDLIB is capable of downloading gridded rainfall and temperature data (min and max).
Here is an example of downloading rainfall dataset from 2010 to 2018.

```python
import imdlib as imd

# Downloading 8 years of rainfall data for India
start_yr = 2010
end_yr = 2018
variable = 'rain' # other options are ('tmin'/ 'tmax')
file_dir = r'C:\Users\imdlib\Desktop\' #Path to save the files
imd.get_data(variable, start_yr, end_yr, fn_format='yearwise', file_dir=file_dir)

# Opeining the downloaded dataset
data = imd.open_data(variable, start_yr, end_yr,'yearwise', file_dir)
```
### Output:

```python
Downloading: rain for year 2010
Downloading: rain for year 2011
Downloading: rain for year 2012
Downloading: rain for year 2013
Downloading: rain for year 2014
Downloading: rain for year 2015
Downloading: rain for year 2016
Downloading: rain for year 2017
Downloading: rain for year 2018
Download Successful !!!
```

## Processing with xarray
```python
ds = data.get_xarray()
ds 
```

### Output:
```
<xarray.DataArray 'rain' (lat: 129, lon: 135, time: 3287)>
...
...
...
Coordinates:
  * lat      (lat) float64 6.5 6.75 7.0 7.25 7.5 ... 37.5 37.75 38.0 38.25 38.5
  * lon      (lon) float64 66.5 66.75 67.0 67.25 67.5 ... 99.25 99.5 99.75 100.0
  * time     (time) datetime64[ns] 2010-01-01 2010-01-02 ... 2018-12-31
Attributes:
    long_name:  rainfall
    units:      mm/day
```

## Saving to GeoTIFF
```python
import rioxarray as rio

#Set the CRS:
pr = ds.rio.set_crs("epsg:4326")

# Transpose according to GeoTIFF conventions:
pr = pr.transpose('time', 'lat', 'lon')

#Define lat/long 
pr = pr.rio.set_spatial_dims('lon', 'lat')

#Saving the file
pr.rio.to_raster(r"C:\Users\imdlib\Desktop\IMD_Rain_2010_2018.tif")
```