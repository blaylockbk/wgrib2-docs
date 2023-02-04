
### wgrib2: -ll2i



### Introduction



The -ll2i and
 -ll2ij options are in alpha status.

The -ll2i has been redefined (v2.0.5) so that it returns
the value 1..ndata. This change was to support grids from 1..2^32 - 1 grid points.
With this change, 0 denotes a lat-lon that is outside of the domain.


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




The -ll2i option takes a given latitude and
longitude, finds the grid point that is closest to that specified
latitude and longitude and prints out the index of the grid point.
The -ll2i option uses the gctpc library
and only supports a grids supported by gctpc.


Note that updated -ll2i uses the Fortran convention
which has the index starting at one. Note that the index is for
the output grid which may be different from the input grid.
For example, the GFS usually writes the grib files in WE:NS order.
By default, wgrib2 will read a WE:NS input grid and convert it to
to a WE:SN (output) grid. This is explained in the page for the
[-scan](./scan.html) option.

### Usage




```

-ll2i LON LAT
      LON is the longitude -180..360
      LAT is the latitude from -90..90
      option prints out the index,  0..number of grid points - 1

```

### Example



```

-sh-4.1$ wgrib2 png.grb2 -ll2i 11 22
1:4:11.000000 22.000000 -> (40332)
-sh-4.1$ wgrib2 png.grb2 -ilat 40332
1:4:grid pt 40331,lon=10.000000,lat=22.000000,val=3.3

```



See also: [-ilat](./ilat.html),
[-scan](./scan.html),















----

>Description: inv   X Y    x=lon y=lat, converts to (i), 1..ndata

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ll2i.html>_