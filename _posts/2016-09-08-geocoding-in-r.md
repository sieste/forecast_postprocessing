---
layout: post
title: Geocoding with R
date: 2016-09-09
---

To download weather forecasts from external archives, e.g. through [rNOMADS]({{ site.baseurl }}{% post_url 2016-09-08-rNOMADS %}}), it is often useful to know the geographical coordinates of a city or place.

In R, the package [ggmap](https://cran.r-project.org/web/packages/ggmap/index.html) provides a useful geocoding function to get latitude and longitude of a city, using the Google Maps API:


```r
ggmap::geocode('London')
```

```
## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=London&sensor=false
##          lon      lat
## 1 -0.1277583 51.50735
```

It doesn't have to be a city: 

```r
ggmap::geocode('Dartmoor National Park')
```

```
## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=Dartmoor%20National%20Park&sensor=false
##         lon      lat
## 1 -3.920688 50.57189
```


