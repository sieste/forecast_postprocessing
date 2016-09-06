---
layout: post
title: "Statistical post-processing at NOAA" 
date: 2016-09-06
---

[This white paper](http://www.esrl.noaa.gov/psd/people/tom.hamill/WhitePaperImplementationofReforecastsandNationalBlend.pdf) from [NOAA](noaa.gov) suggests that to produce statistically post-processed weather forecasts they have to


> (a) determining what new data are to be produced; what new reanalyses,
> reforecasts (at what resolution, frequency, duration, etc.), high­resolution
> surface reanalyses, additional NDFD elements, and so forth; 

> (b) determining the specifications for an upgraded computational and storage
> system for reanalysis, reforecasting, and statistical post­processing;  

> (c) identifying what additional WFO­based data storage, communications
> bandwidth, forecaster workstation capability, and software improvements may
> be needed if the NDFD is to be augmented with additional probabilistic
> information;  

> (d) procuring, realigning, and/or maintaining the hardware needed for (a) --
> (c) above;  

> (e) developing the capacity and then regularly producing global reanalyses
> and reforecasts, and making this data easily available in a timely fashion
> for post­processing system development inside and external to NOAA; 

> (f) improving the quality of the high­resolution surface analyses, and
> generating high­resolution surface reanalyses to match the period of
> reforecasts; 

> (g) exploring the merging of the North American Ensemble Forecast System with
> the National Blend; 

> (h) updating and sometimes redesigning the post­processing software to use
> the greatly expanded training data and to produce a broader range of
> higher­quality, high­resolution deterministic and probabilistic guidance for
> the NDFD or other archives. 


For researchers and users of the raw data, point (e) seems most important. 
NOAA is doing a much better job with publishing their raw data than European weather services.


> The basic concept behind post­processing is simple; use a training data set
> of past forecasts and observations, ideally from a fixed
> assimilation/forecast system, to determine the adjustments to the current
> forecast.  ...

An old problem in post-processing is that, whenever the forecast or observation system changes, a new training data set has to be produced and the old one goes in the bin.
I am wondering if (a) this is really necessary, i.e. if the changes are so big that you can really see a drop in performance of the post-processed forecast; and (b) if we can use a method dynamically adjusts the post-processing parameters to changes in the system.
In dynamic linear modeling, such intervention effects can be included by temporarily increasing the error variance to account for changes in the data.


