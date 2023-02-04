
### wgrib2: -h



### Introduction



The -h option lists the common options.


```

-sh-2.05b$ ./wgrib2 -h
wgrib2 v0.1.6 1/2008 wesley ebisuzaki, Jaakko Hyvätti, Karl Pfeiffer, Manfred Schwarb, Kristian Nilssen, Sergey Varlamov
 -0xSec           inv  X      Hex dump of section X (0..8)
 -bitmap          inv         bitmap mode
 -center          inv         center
 -ctl_inv         inv         ctl inventory dump (for g2ctl)
 -disc            inv         discipline (code table 0.0)
 -domain          inv         max limit for n/s/e/w
 -ens             inv         ensemble information
 -ftime           inv         forecast time
 -grid            inv         grid definition
 -ij              inv  X Y    value of field at grid(X,Y) X=1,..,nx Y=1,..,ny
 -ijlat           inv  X Y    lat,lon and grid value at grid(X,Y) X=1,..,nx Y=1,..,ny
 -ilat            inv  X      lat,lon and grid value at Xth grid point, X=1,..,npnts
 -lev             inv         level (code table 4.5)
 -lev0            inv         level (for g2ctl)
 -lola            inv  X..Z,A lon-lat grid values X=lon0:nlon:dlon Y=lat0:nlat:dlat Z=file A=[bin|text|spread]
 -lon             inv  X Y    value at grid point nearest lon=X lat=Y
 -max             inv         print maximum value
 -min             inv         print minimum value
 -MM              inv         month
 -N_ens           inv         number of ensemble members
 -nl              inv         inserts new line into inventory
 -nlons           inv         number of longitudes for each latitude
 -npts            inv         number of grid points
 -nxny            inv         nx and ny of grid
 -packing         inv         data packing mode
[rest is deleted]

```

### Usage




```

-h

```







----

>Description: misc         help, shows common options

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/h.html>_