---
layout: post
title: Plotting GeoTIFF in python
author: Pratiman
---
Usually working with geographic data, you have to use GIS software. For plotting multiple plots with same quality might be cumbersome. Hence, some people choose to automate the process, I was also struggling to do the same. So here is a simple example to plot a GeoTIFF file.

First of all load your dataset using ```xarray.open_rasterio```.

```python
import xarray as xr
dem = xr.open_rasterio('https://download.osgeo.org/geotiff/samples/pci_eg/latlong.tif')
dem = dem[0] #getting the first band
```

The easiest way to plot is using plot function of xarray

```dem.plot()```

![Dem Plot](/uploads/2020/06/30/Fig1.png)

This plot cannot be used publication. Check the x and y axis labels and the colorbar. We need to change it to make suitable for publishing.

I am using ProPlot. This is an awesome library for the publication quality plots.

```python
import proplot as plot
import xarray as xr

dem = xr.open_rasterio('https://download.osgeo.org/geotiff/samples/pci_eg/latlong.tif')
dem = dem[0]

dem.plot()

# Define extents
lat_min = dem.y.min()
lat_max = dem.y.max()
lon_min = dem.x.min()
lon_max = dem.x.max()

#Starting the plotting
fig, axs = plot.subplots(proj=('cyl'))

#format the plot
axs.format(
    lonlim=(lon_min, lon_max), latlim=(lat_min, lat_max),
    land=False, labels=True, innerborders=False
)

#Plot
m = axs.pcolorfast(dem, cmap='batlow')
cbar = fig.colorbar(m, loc='b', label='whatever') #Adding colorbar with label

#Saving the Figure
fig.savefig(r'geotiff.png')  
```

![{Pro Plot](/uploads/2020/06/30/Fig2.png)