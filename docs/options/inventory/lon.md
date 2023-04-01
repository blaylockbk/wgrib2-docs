# -lon

## Introduction

The -lon option prints the value of the grid point
closest to the specified longitude latitude. The latitude-longitude of the grid
point are also printed. If you use the verbose mode, the grid
coordinates (i,j) and the number of the element are also printed.
The -lon option can be repeated to save processing
time.

```
-sh-2.05b$ wgrib2 eta.t00z.awphys18.grb2 -d 1 -s -lon 249 39 -lon 255 33
1:0:d=2003090300:MSLET:mean sea level:18 hour fcst:lon=249.035,lat=38.9912,val=101685:
lon=254.964,lat=32.9671,val=101668

-sh-3.00$ wgrib2 rtma.t12z.2dvaranl\_ndfd.grb2.c2 -d 1 -v -lon -120 30
1:0:lon=240.008805,lat=29.988418,i=220037,ix=72,iy=206,val=0
```

In the latter example, the verbose mode has been set and
the inventory includes both the value, location and its
grid coordinates; i.e., the 220037th element in the array and
its coordinates are (72,206). Note that these coordinates
are after the data has been converted into a WE:SN scan order.
Both the i, ix and iy start with a value of one.

## Usage

```
-lon LONGITUDE LATITUDE
      LONGITUDE = 0 .. 360
      LATITUDE = -90 .. 90

      If the verbosity is 0, the print out the longitude and
        latitude of the nearest grid point as well as the grid value.

      If the verbosity is 1 or higher, the prints out the longitude and
        latitude of the nearest grid point, the index (i) to the data,
        the grid coordinates (ix,iy) as well as the grid point value.

        i = 1..number of grid points
        ix = 1..nx
        iy = 1..ny
```

### Example

```
$ wgrib2 test.grb2 -s -lon -90 20
1:0:d=2005090200:HGT:1000 mb:60 hour fcst:lon=270,lat=20,val=121.3
2:133907:d=2005090200:HGT:975 mb:60 hour fcst:lon=270,lat=20,val=344.4
3:263511:d=2005090200:HGT:950 mb:60 hour fcst:lon=270,lat=20,val=573
4:389058:d=2005090200:HGT:925 mb:60 hour fcst:lon=270,lat=20,val=806.5
...
```

### Old vs New

The original code for the
-lon option used the internal geolocation package.
This package could compute the lat/lon of the grid points but
nothing else. To find the grid point closest to a specified
lat/lon, the distance to every grid point had to be calculated.
Later, a short cut was added for lat-lon grids. Finally the
geolocation packages gctpc and Proj4 have inverse functions
which allow you to find the closest grid point to specified point
for the the supported grids. For unsupported grids like the
Gaussian grid and staggered grids, the original brute force
code is used. You can turn off the new code by the
-gctpc 0 option. The old code did
not know about grid domains and would find the closest point
even if the point were outside of the grid domain. The
gctpc-based closest will return a lat=lon=999 to signal an
intial point outside of the domain.

### Want Speed?

You want extract the values for a 1000 different points.
So you call wgrib2 1000 times and complain that wgrib2 is slow.
Well decoding a jpeg2000 compressed file 1000 times does take time.
It's better to add a 1000 -lon options to the
command line and only decode the file once.

The number of -lon options on a
command line is limited by a compile-time option. Try
running wgrib2 -config and look for the line
"maximum number of arguments on command line:". The current
value is 5000 which allows you 5000 words on the command line. Each
-lon option takes 3 words, so that gives
you about 1600 -lon options you can run
on one line. Of course, limitations such as the maximum line
length or maximum number of continuations may stop you first.

### Text, Binary and CSV Output

The -lon option writes the grid value
to the inventory. What happens if you want the output written
to a file. You could write the output of
-lon to a file by using the
-last option.

```
$ wgrib2 gep19.aec -lon 10 20  -last junk -nl_out junk -for 1:3
1:0:lon=10.000000,lat=20.000000,val=12391.6
2:70707:lon=10.000000,lat=20.000000,val=219.5
3:96843:lon=10.000000,lat=20.000000,val=85
$ cat junk
lon=10.000000,lat=20.000000,val=12391.6
lon=10.000000,lat=20.000000,val=219.5
lon=10.000000,lat=20.000000,val=85
```

You can also use the -lola option which can
write a 1x1 grid to binary, text or a grib file.

```
$ wgrib2 gep19.aec -no_header -lola "10:1:1" "20:1:1" out.txt text -for 1:3
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
$ cat out.txt
12391.6
219.5
85
```

You can make a CSV file by first converting the grib file and running wgrib2 on that
grib file.

```
$ wgrib2 gep19.aec -no_header -lola "10:1:1" "20:1:1" out.grb grib -for 1:3
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
$ wgrib2 out.grb -csv out.csv
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:182:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:364:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
$ cat out.csv
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",10,20,12391.6
"2009-06-05 00:00:00","2009-06-12 12:00:00","TMP","200 mb",10,20,219.5
"2009-06-05 00:00:00","2009-06-12 12:00:00","RH","200 mb",10,20,85
```

See also: [-last](./last.md),
[-lola](./lola.md),
[-config](./config.md)

---

> Description: inv X Y value at grid point nearest lon=X lat=Y (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/lon.html>_
