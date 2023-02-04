
### wgrib2: -ll2ij



### Introduction



The -ll2ij and
 -ll2ij options are in alpha status.

The -lon option uses a brute-force
method to find the closest grid point to a specified latitude
and longitude. It finds the minimum distance to each grid
point. This slow procedure is more-or-less necessary when
your geolocation routines can transfrom from (X,Y) -> (lon, lat)
but not (lon, lat) -> (X, Y). The gctpc/Proj4 geolocation
libraries have both the forward and inverse transformation so
improved geolocation routines can be added to wgrib2 such
as a fast -lon option and a 
bilinear interpolation option.

 Some grids only have an (i,j) -> (lon,lat) transformation.
(I.e., find that lat-lon of the grid points.) Examples include:
staggered grids, thinned Gaussian grids and irregular grids.




The -ll2ij option takes a given latitude and
longitude, finds the grid point that is closest to that specified
latitude and longitude and prints out the ix and iy of the grid point.
The -ll2i option uses the gctpc library
and only supports a grids supported by gctpc.



See also: [-ijlat](./ijlat.html),
[-ll2i](./ll2i.html)












----

>Description: inv   X Y    x=lon y=lat, converts lon-lat to (i,j) using gctpc

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ll2ij.html>_