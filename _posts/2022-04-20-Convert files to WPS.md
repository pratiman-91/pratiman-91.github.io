---
layout: post
title: Convert spatial data to WRF Pre-Processing System (WPS)
author: Pratiman Patel
---

## Description
Let's try to convert a GeoTIFF to WRF Pre-Processing System (WPS). 

We will be using `GIS4WRF` for conversion. For more information about the plugin, please refer to [https://gis4wrf.github.io/documentation/](https://gis4wrf.github.io/documentation/). 

1. Launch the `GIS4WRF` dock from `Plugins` > `GIS4WRF` > `GIS4WRF` in QGIS.
2. Go to GIS4WRF Dock and select `Datasets` > `Process`.
3. Select the layer in the `Layers` Dock to be converted.
4. Click on the `Convert active layer to WPS binary`. 
5. If your data is categorical (i.e., Land Use/ Land Cover), then click on `Yes` else `No`.
6. Choose an empty folder to save the WPS binary files.
7. Verify the results by importing the WPS binary into the QGIS by selecting the `Layer` > `Add Layer` > `Add WPS Binary Layer`

### Just in case if there is a problem with the convertion.

This may happen to the float files. A work around is to convert the files to the 4 decimal place. 

1. Use `Raster` > `Raster Calculator` to multiply the raster by 10,000. 
2. Use `Raser` > `Conversion` > `Translate (Convert Format)` to change the raster `Output data type` to `UInt16`. 
3. Now, save to the WPS binary (follow steps 3 to 6 shown above).
4. Once files are converted, go to the folder where you have saved the WPS binary files and open `index` in a text editor.
5. Add following line in the last of the `index` file:

    `scale_factor = 1e-04`

6. Verify the converted files by importing into the QGIS.
