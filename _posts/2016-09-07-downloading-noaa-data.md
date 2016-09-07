---
layout: post
title: "Downloading publicly available real-time NWP data"
date: 2016-09-07
---

Numerical weather prediction models run on supercomputers.
The US weather service makes their NWP forecast data publicly available in real-time (well, almost).
I will show a simple example of how that data can be accessed.

Let's say I want to get the latest temperature forecast for London produced by their high-resolution model.
A good starting point for exploring the available data is [here](https://www.ncdc.noaa.gov/data-access/model-data/model-datasets).

We go to Global Forecast System [GFS](https://www.ncdc.noaa.gov/data-access/model-data/model-datasets/global-forcast-system-gfs) and find the GFS forecasts on the 0.5 degree grid. 
There are different data access links, and I prefer downloading data through the THREDDS Data Server [TDS](http://nomads.ncdc.noaa.gov/thredds/gfs4.html). 

We follow the link to `GFS 004 Half degree...` all the way to the directory [20160905](http://nomads.ncdc.noaa.gov/thredds/catalog/gfs-004/201609/20160905/catalog.html) where we find a bunch of files with names such as `GFS_Grid_4_2016-09-05_18:00_UTC_fct:384.grb2`.
The file name encodes the grid (Grid 4 is 0.5 by 0.5 degrees), the initialisation time (2016-09-05 18:00 UTC), and the forecast lead time (384 hours i.e. the 16 days ahead forecast). 
The file contains global fields of many different variables simulated by the model (temperature, pressure, humidity, wind speed, etc) and is about 60Mb large. 

We could download the entire file in grib format, or use the OPeNDAP Data Access Form to extract only the data we need. 
To get there we follow the link called [1.OPENDAP](http://nomads.ncdc.noaa.gov/thredds/dodsC/gfs-004/201609/20160905/gfs_4_20160905_1800_384.grb2.html).
Scrolling waaay down, we find the variable `Temperature:Grid` and can specify the desired index for time, pressure (i.e. height), latitude, and longitude in `from:by:to` format starting at index 0.

For time there is only one possible value (0:1:0, the default) because different times are saved in different files.
For pressure we use the lowest level (0:1:0, the default).
`lat` runs from 90 (North Pole) to -90 (South Pole) in increments of 0.5 degrees so there are 360 possible latitudes.
`lon` runs from 0 (Greenwhich) to 359.5 in eastward direction so there are 720 possible longitudes.
To get the value at the grid point closest to London (51.50N, 0.13W) we select `lat = (77:1:77)` and `lon = (0:1:0)` corresponding to (51.5N, 0W) in model world.

The automatically generated link to the ASCII file is `http://nomads.ncdc.noaa.gov/thredds/dodsC/gfs-004/201609/20160905/gfs_4_20160905_1800_384.grb2?Temperature[0:1:0][0:1:0][77:1:77][0:1:0]` and the link returns the requested data as plain text:


```
Dataset {
  Grid {
   ARRAY:
     Float32 Temperature[time = 1][pressure = 1][lat = 1][lon = 1];
   MAPS:
     Int32 time[time = 1];
     Float64 pressure[pressure = 1];
     Float64 lat[lat = 1];
     Float64 lon[lon = 1];
  } Temperature;
} gfs-004%2f201609%2f20160905%2fgfs_4_20160905_1800_384%2egrb2;
---------------------------------------------
Temperature.Temperature[1][1][1][1]
[0][0][0], 288.1

Temperature.time[1]
384

Temperature.pressure[1]
100000.0

Temperature.lat[1]
51.5

Temperature.lon[1]
0.0
```

**So, for the 21 September 2016 18:00 UTC, the temperature forecast for London, issued by the high-resolution NOAA model, is 14.95 Celsius.**



