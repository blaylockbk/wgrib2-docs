
### wgrib2: -lola



### Introduction



The -lola option was not named after a girl but for extracting data 
on a LOngitude-LAtitude grid. You need to specify the lower
left corner of the grid, the number of points in the zonal and meridional directions
and the latitude/longitude increments. Finally you need to specify the output file 
and the format. WARNING: winds and other vector fields will not be
rotated. If the vector fields use a grid relative orientation,
then your interpolated winds will be using the original grid.


### Interpolation scheme



The interpolation to the lola grid is by nearest neighbor. Sure there are more
accurate schemes and people are welcome to do better. Warning: the interpolation
scheme simply picks up the value of the nearest neighbor. This can be very
inaccurate for winds and other vectors near the pole. 

### Usage




```

-lola LonSW:#lon:dlon LatSW:#lat:dlat file format

LonSW        Longitude of the South-West point, values from 0 .. 360
#lon         number of longitude points
dlon         spacing of the points in the zonal direction in degrees

LatSW        Latitude of the South-West point, values from -90 .. 90
#lat         number of latitude points
dlat         spacing of the points in the meridional direction in degrees

file         name of the output file

format       format of the output file: bin, text, spread
               bin = binary
               text = simple "text" format, one value per line
               spread = spread-sheet format, latitude, longitude and value of each grid point
               grib = grib2

The order of the data points is WE:SN (wgrib2 standard).


```

### Comments


I dislike this routine. It is slow, uses a simplistic interpolation
scheme and doesn't handle rotated winds in a useful manner. 

The grib file support allows you to make lat-lon templates which is used by
g2grb.gs. As much as I dislike this routine, I keep using it for interpolations.
Keep wanting to modify it to do bilinear interpolations for interpolating from
lat-lon grids.


5/2010: interpolation from regular lat-lon grids is now handled as special case.
This speeds up the interpolation and paves the way for a bilinear interpolation
for the special cases.


See alse: [-lon](./lon.html)














----

>Description: out   X..Z,A lon-lat grid values X=lon0:nlon:dlon Y=lat0:nlat:dlat Z=file A=[bin|text|spread|grib]

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/lola.html>_