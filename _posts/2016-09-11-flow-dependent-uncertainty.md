---
layout: post
title: Flow-dependent predictability
date: 2016-09-11
---

On the ECMWF website on [Quantifying forecast uncertainty](http://www.ecmwf.int/en/research/modelling-and-prediction/quantifying-forecast-uncertainty), we learn

> Despite the increasing accuracy of weather forecasts, there is an element of
> uncertainty in all predictions. In 1992, ECMWF pioneered an ensemble
> prediction system, which now provides a vast range of products to help
> forecasters deal quantitatively with the day-to-day variations in the
> predictability of the atmosphere.

The limited predictability of nonlinear systems is well-known, often under the name *butterfly effect*.
The not-so-well-known idea is the concept of *flow-dependent predictability*, called "day-to-day variations in the predictability of the atmosphere" above.



My favourite illustration of flow-dependent predictability is in the 2011 paper [Uncertainty in weather and climate prediction](http://rsta.royalsocietypublishing.org/content/369/1956/4751.short) by Slingo and Palmer.
They use the system of nonlinear differential equations commonly known as the Lorenz63 system to illustrate the fact that, dependending on today's weather, tomorrow's weather can be more or less predictable.

The [Lorenz63 system](http://journals.ametsoc.org/doi/abs/10.1175/1520-0469%281963%29020%3C0130%3ADNF%3E2.0.CO%3B2) is described by three variables $X$, $Y$, and $Z$ whose evolution is described by the differential equations

$\frac{d}{dt} X = -\sigma (Y - X)$

$\frac{d}{dt} Y = -XZ + \rho X - Y$

$\frac{d}{dt} Z = XY - \beta Z$

The standard parameter settings are $\sigma = 10$, $\rho = 28$, and $\beta = 8/3$.

Simulating the system in R can be done quite easily with the package [deSolve](https://cran.r-project.org/web/packages/deSolve/index.html).

```r
library(deSolve)

#differential equations of the Lorenz'63 system
l63 = function(t, state, parms=NA) {
  list(c(
    10 * (state['Y'] - state['X']),  
    state['X'] * state['Z'] * (-1) + 28 * state['X'] - state['Y'], 
    state['X'] * state['Y'] - 8 / 3 * state['Z'])
  )
}

lorenz63 = deSolve::ode(y=c(X=8,Y=13,Z=18), times=seq(0,100,0.01), func=l63)

##      time         X        Y        Z
## [1,] 0.00  8.000000 13.00000 18.00000
## [2,] 0.01  8.508145 13.66712 18.61278
## [3,] 0.02  9.030802 14.31912 19.33448
## [4,] 0.03  9.564408 14.94015 20.16870
## [5,] 0.04 10.104105 15.51187 21.11655
## [6,] 0.05 10.643642 16.01380 22.17574
## ...
```


Below I plot a long trajectory of the system in the $X$-$Z$ plane - this 2d representation of the Lorenz63 system is often called the butterfly attractor.

I have chosen two points in the $X$-$Z$ plane to illustrate flow-dependent predictability similar to Slingo and Palmer (2011).
Solutions that are initially close to the point (12, 21) remain close for a period of time.
On the other hand, solutions that are initially close to the point (-9.5, 36) quickly diverge from each other over the same period of time.


```r
# plot the butterfly attractor
plot(lorenz63[,'X'], lorenz63[,'Z'],type='l', col='#99999999')

# stable 
x0 = 12
z0 = 21
inds = which( abs(lorenz63[,'X'] - x0) < 0.5 & abs(lorenz63[, 'Z'] - z0) < 0.5 )
for (i in inds) {
  lines(lorenz63[i:(i+60), 'X'], lorenz63[i:(i+60), 'Z'], col='blue')
}
points(x0, z0, pch=21, cex=2, lwd=3)

# unstable 
x0 = -9.5
z0 = 36
inds = which( abs(lorenz63[,'X'] - x0) < 0.5 & abs(lorenz63[, 'Z'] - z0) < 0.5 )
for (i in inds) {
  lines(lorenz63[i:(i+60), 'X'], lorenz63[i:(i+60), 'Z'], col='orange', lwd=2)
}
points(x0, z0, pch=21, cex=2, lwd=3)
```

![Flow-dependent predictability a la Slingo and Palmer (2011)]({{ site.baseurl }}/images/l63.png)


So if you live in Lorenz63 world, and today's weather is $X=12$ and $Z=21$, you know that a small measurement error will not effect the short-term forecast too much.
On the other hand, if the weather today is $X=-9.5$ and $Z=36$, the forecast is extremely sensitive.
A small error in measuring the initial state can produce a forecast that is far far away from the truth.


This is what [Lorenz](http://onlinelibrary.wiley.com/doi/10.1111/j.2153-3490.1969.tb00444.x/abstract) meant when he wrote 

> one flap of a sea gull’s wings would forever change the future course of the weather

The sea gull was later replaced by a butterfly, probably because "butterfly effect" sounds nicer than "sea gull effect".

The essence of flow-dependent predictability is that depending on what the weather is like today, the flap of the wing can change the course of the weather slowly or quickly. 

The goal of ensemble weather forecasting is to estimate how sensitive today's weather forecast is to uncertainties in the initial conditions.
Due to flow-dependent predictability, the weather on any given day can be more or less predictable.

