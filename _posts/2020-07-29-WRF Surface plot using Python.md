---
layout: post
title: WRF surface plots using Python
author: Pratiman
---

Here is an example of generating a surface plot from WRF output file.
I find this method to be more elegent.

# Python Script
```python
import wrf
from netCDF4 import Dataset
import proplot as plot
import cartopy.crs as crs

# Download the dataset from https://www.unidata.ucar.edu/software/netcdf/examples/wrfout_v2_Lambert.nc
# Load the dataset
ncfile = Dataset('wrfout_v2_Lambert.nc')

# Get the variable
slp = wrf.getvar(ncfile, "slp")

# Get the latitude and longitude points
lats, lons = wrf.latlon_coords(slp)

# Get the cartopy mapping object
cart_proj = wrf.get_cartopy(slp)

#Plotting
fig, axs = plot.subplots(proj=cart_proj)

# Define extents
lat = [lats.min(), lats.max()]
lon = [lons.min(), lons.max()]

#format the plot
axs.format(
    lonlim=lon, latlim=lat,
    labels=True, innerborders=True
)

#Plot using contourf
m = axs.contourf(wrf.to_np(lons), wrf.to_np(lats), wrf.to_np(slp), 
                      transform=crs.PlateCarree(), cmap='roma_r')

#Adding colorbar with label
cbar = fig.colorbar(m, loc='b', label='Sea Level Pressure (hPa)') 

#Saving the figure
fig.savefig('slp.png')
```
## Output
![{WRF SLP](/uploads/2020/07/29/Fig1.png)

# Explanation

1. Basic import stuff for Python
```python
import wrf
from netCDF4 import Dataset
import proplot as plot
import cartopy.crs as crs
```

2. Loading the dataset
```python
ncfile = Dataset('wrfout_v2_Lambert.nc')
```

3. Extracting the varibale of your choice. Here it is sea level pressure (SLP).
```python
# Get the variable
slp = wrf.getvar(ncfile, "slp")
```

4. Get the lat/lon and projection of the WRF file. Here the projection is LCC.
```python
# Get the latitude and longitude points
lats, lons = wrf.latlon_coords(slp)

# Get the cartopy mapping object
cart_proj = wrf.get_cartopy(slp)
```

5. Create the plot and define projection using ```proj```

```python
#Plotting
fig, axs = plot.subplots(proj=cart_proj)
```

6. Setting up the extent of the plot by getting the maximum and minimum lat and long.
```python
# Define extents
lat = [lats.min(), lats.max()]
lon = [lons.min(), lons.max()]
```

7. Format the plot aesthetics.
```python
#format the plot
axs.format(
    lonlim=lon, latlim=lat,
    labels=True, innerborders=True
)
```

8. Plot the filled contour of the variable.  
```python
#Plot using contourf
m = axs.contourf(wrf.to_np(lons), wrf.to_np(lats), wrf.to_np(slp), 
                      transform=crs.PlateCarree(), cmap='roma_r')
```
9. Adding the colorbar at the bottom of the plot.
```python
#Adding colorbar with label
cbar = fig.colorbar(m, loc='b', label='Sea Level Pressure (hPa)') 
```

10. Finally, saving the figure.
```python 
#Saving the figure
fig.savefig('slp.png')
```

# Additional Information

If you do not like the LCC projection then you can change the following line:

```python
#Plotting
fig, axs = plot.subplots(proj='cyl')
```

## Output
![{WRF SLP 1](/uploads/2020/07/29/Fig2.png)