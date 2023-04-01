# -cress_lola

## Introduction

The -cress_lola option is similar to the
-lola option in that it creates a regular LOngitude-LAtitude grid.
The former uses a Cressman analysis and the latter option uses a nearest-neighbor interpolation.

You need to specify the lower
left corner of the grid, the number of points in the zonal and meridional directions
and the latitude/longitude increments. Finally you need to specify the output file
and the format. WARNING: winds and other vector fields will not be
rotated. If the vector fields use a grid relative orientation,
then your interpolated winds will be using the original grid.

### Interpolation scheme

The interpolation to the lola grid is by a Cressman analysis. The Cressman analysis
is a multipass system which uses a user-specified "radius" for each pass.
A Cressman analysis can be computationally expensive so you may want to explore
[multiprocessing techniques](./for_n.md).

## Usage

```
-cress_lola LonSW:#lon:dlon LatSW:#lat:dlat file radius1:radius2:..:radiusN

LonSW        Longitude of the South-West point, values from 0 .. 360
#lon         number of longitude points
dlon         spacing of the points in the zonal direction in degrees

LatSW        Latitude of the South-West point, values from -90 .. 90
#lat         number of latitude points
dlat         spacing of the points in the meridional direction in degrees

file         name of the output grib file

radiusM      The radius in km for M-th pass.

```

### Prelim - Cressman Analysis

```
defintion: input grid = observations = grid from the input grib file
           output grid = grid as defined by  LonSW:#lon:dlon LatSW:#lat:dlat

(1) compute mean of observations,save mean on background grid (output)
    background(i,j) = average(observation)

(2) Repeat N times:  PASS-M

   (a) find background value for observations by bilinear interpolation of background grid
   (b) remove the background from the observations: obs' = obs - interpolated_background
   (c) inc'(i,j)  = weighted average of obs'
           inc'(i,j) is increment on output grid
           weighted average of obs' depends on the distance between grid point and obsservation
   (d) new background = background + inc'
```

Warning, this scheme doesn't handle handle rotated winds in a useful manner.
There will also be problem with analyzing winds near the poles.

See alse:
[-lon](./lon.md)
[-lola](./lola.md)

---

> Description: out X..Z,A lon-lat grid values X=lon0:nlon:dlon Y=lat0:nlat:dlat Z=file A=radius1:radius2:..:radiusN

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/cress_lola.html>_
