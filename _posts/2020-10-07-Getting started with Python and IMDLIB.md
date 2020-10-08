---
layout: post
title: Getting started with Python and IMDLIB
author: Pratiman Patel
---

This is intended for those who have never used python or just starting to work in python. Here are the steps which will help them with getting started with IMDLIB and Python.

## Setting up the Python in Windows
There are multiple options you can choose for Python distribution. I like the use of miniconda which is light weight and does not come with unnecessary libraries.

### Steps to install miniconda
1. Download the installer from [here](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe).
2. Install the miniconda.
![Install miniconda](/uploads/2020/10/07/FIG1.png)
![Install miniconda](/uploads/2020/10/07/FIG2.png)
3. Once you have installed the check by clicking in the start button.
![Check miniconda](/uploads/2020/10/07/FIG3.png)

### Install an IDE (Spyder) and library packages
1. Open Anaconda Prompt (miniconda3)
![Open miniconda prompt](/uploads/2020/10/07/FIG4.png)
2. Write the following code in the anaconda prompt window
```
conda install -c conda-forge numpy scipy rioxarray xarray netcdf4 proplot matplotlib cartopy spyder
pip install imdlib
```
![Install packages](/uploads/2020/10/07/FIG8.png)
#### Output:
```
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Solving environment: failed with repodata from current_repodata.json, will retry with next repodata source.
Collecting package metadata (repodata.json): done
Solving environment: done
...
...
...
Proceed ([y]/n)? y
...
...
...
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```
> You might get extra warnings. You can ignore them.

3. Check the installation.
![Check packages](/uploads/2020/10/07/FIG6.png)

## Running your Python code
1. Open Spyder.
![Open Spyder](/uploads/2020/10/07/FIG7.png)

2. Test your first imdlib code. Copy the following code to the to editior window of the spyder and run.
```python
import imdlib as imd
import rioxarray as rio

# Downloading 1 years of rainfall data for India
start_yr = 2018
end_yr = 2018
variable = 'rain' # other options are ('tmin'/ 'tmax')
#file_dir = (r'C:\Users\imdlib\Desktop\') #Path to save the files
imd.get_data(variable, start_yr, end_yr, fn_format='yearwise')

# Opeining the downloaded dataset
data = imd.open_data(variable, start_yr, end_yr,'yearwise')
ds = data.get_xarray()

#Plot the mean
ds.mean('time').plot()

#Set the CRS:
pr = ds.rio.set_crs("epsg:4326")

# Transpose according to GeoTIFF conventions:
pr = pr.transpose('time', 'lat', 'lon')

#Define lat/long 
pr = pr.rio.set_spatial_dims('lon', 'lat')

#Saving the file
pr.rio.to_raster(r"IMD_Rain_2010_2018.tif")
```
![Run Code](/uploads/2020/10/07/FIG9.png)

#### Output:
![Output Plot](/uploads/2020/10/07/FIG10.png)
![Output Plot](/uploads/2020/10/07/FIG11.png)

