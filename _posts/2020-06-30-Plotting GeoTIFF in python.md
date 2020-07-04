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

The easiest way to plot is using plot function of xarray ```dem.plot()```.

![Dem Plot](/uploads/2020/06/30/Fig1.png)

This plot has certain drawbacks. First of all, x and y labels are too small, title is "band 1", and colorbar need to be changed. You can change it to make suitable for publishing.

I am using ProPlot. This is an awesome library for the publication quality plots.
This is the whole script and output.

## Pyhton Script

```python
import proplot as plot
import xarray as xr

dem = xr.open_rasterio('https://download.osgeo.org/geotiff/samples/pci_eg/latlong.tif')
dem = dem[0]

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

## Explaination

1. Basic import stuff for python
```python
import proplot as plot
import xarray as xr
```

2. Open the GoeTIFF dataset using ```xarray.open_rasterio``` and get the first band of the image. You can select a different band if you have multi-band GoeTIFF.
```python
dem = xr.open_rasterio('https://download.osgeo.org/geotiff/samples/pci_eg/latlong.tif')
dem = dem[0]
```

3. Get the extent of the dataset, if you have global dataset then there is no need.
```python
lat_min = dem.y.min()
lat_max = dem.y.max()
lon_min = dem.x.min()
lon_max = dem.x.max()
```

4. Create the figure and axes with projection system. Here, ```proj=('cyl')``` means the Cartopy ```PlateCarree``` projection
```python
fig, axs = plot.subplots(proj=('cyl'))
```

5. Format the plot. Here, ```lonlim``` and ```latlim``` are the zoom locations of the axes. If you are using Global dataset then you can ignore it. Turn on/off the various geographic features such as ```land, ocean, coast, river, lakes, innerborders, etc.```. To turn on the lat and long labels use ```labels=True```.
```python
axs.format(
    lonlim=(lon_min, lon_max), latlim=(lat_min, lat_max),
    land=False, labels=True, innerborders=False
)
```

6. Finally, plot the dem using ```pcolorfast```. It is good enough for fast processing of the GeoTIFF files. ```pcolormesh``` takes a longer time to process but can be used. The colormap used is ```batlow```. For more colormaps you can check using ```plot.show_cmaps()```. Save the figure to your deired location.

```python
m = axs.pcolorfast(dem, cmap='batlow')
cbar = fig.colorbar(m, loc='b', label='whatever') #Adding colorbar with label

#Saving the Figure
fig.savefig(r'geotiff.png')  
```


