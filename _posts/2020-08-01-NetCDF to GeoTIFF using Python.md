---
layout: post
title: NetCDF to GeoTIFF using Python
author: Pratiman
---

A simple and elegant method to convert NetCDF to GeoTIFF using python.

# Python Script
```python
import xarray as xr
import rioxarray as rio

#Open the NetCDF
#Download the sample from https://www.unidata.ucar.edu/software/netcdf/examples/sresa1b_ncar_ccsm3-example.nc
ncfile = xr.open_dataset('sresa1b_ncar_ccsm3-example.nc')

#Extract the variable
pr = ncfile['pr']

#(Optional) convert longitude from (0-360) to (-180 to 180) (if required)
pr.coords['lon'] = (pr.coords['lon'] + 180) % 360 - 180
pr = pr.sortby(pr.lon)

#Define lat/long 
pr = pr.rio.set_spatial_dims('lon', 'lat')

#Check for the CRS
pr.rio.crs

#(Optional) If your CRS is not discovered, you should be able to add it like so:
pr.rio.set_crs("epsg:4326")

#Saving the file
pr.rio.to_raster(r"GeoTIFF.tif")
```

# Explanation

1. Basic import stuff for Python
```python
import xarray as xr
import rioxarray as rio
```

2. Loading the dataset
```python
ncfile = xr.open_dataset('sresa1b_ncar_ccsm3-example.nc')
```

3. Extracting the varibale of your choice. Here it is precipitation (pr).
```python
pr = ncfile['pr']
```

4. (Optional) In this example the longitude is from 0-360. GIS software do not treat well this kind of system. Therefore, we onvert it in -180 to 180. This might be optional for your dataset.
```python
pr.coords['lon'] = (pr.coords['lon'] + 180) % 360 - 180
pr = pr.sortby(pr.lon)
```

5. It's a good idea to provide the x and y axis for the GeoTIFF. NetCDF sometimes have non-conventional naming schemes.
```python
pr = pr.rio.set_spatial_dims('lon', 'lat')
```

6. Check for projection system
```python
pr.rio.crs
```

7. (Optional) If there is no output of step 6. Then define the projection system.
```python
pr.rio.set_crs("epsg:4326")
```

8. Saving the GeoTIFF file.
```python
pr.rio.to_raster(r"GeoTIFF.tif")
```

Hope you like it!
