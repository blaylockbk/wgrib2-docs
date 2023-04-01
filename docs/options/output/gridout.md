# -gridout

## Introduction

Many grib message contain grids of the following form: grid_val(NX,NY),
longitude(NX,NY) and latitude(NX,NY). Common exceptions are spectral
data, thinned grids and staggered grids. If the grib message is
of the first form, then you can use the -gribout
option to print out i, j, latitude(i,j), longitude(i,j)

## Usage

```
-gridout FILE
   FILE is an output of the command
   if grid is of the form: grid_val(nx,ny), lat(nx,ny), lon(nx,ny)
     print ((i, j, lat(i,j), lon(i,j), i=1,nx), j=1,ny)
     using the format "%10i,%10i, %.3f, %.3f\n"

     FILE will be a CSV file with the latitudes and longitudes of the grid points
```

### Example

```
$ wgrib2 small.grb2 -grid
1:0:grid_template=0:winds(N/S):
	lat-lon grid:(2 x 2) units 1e-06 input WE:SN output WE:SN res 48
	lat 20.000000 to 28.000000 by 8.000000
	lon 0.000000 to 10.000000 by 10.000000 #points=4
$ wgrib2 small.grb2 -gridout grid.txt
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
$ cat grid.txt
         1,         1, 20.000, 0.000
         2,         1, 20.000, 10.000
         1,         2, 28.000, 0.000
         2,         2, 28.000, 10.000
```

See also:
[-grid](./grid.md),

---

> Description: out X text file with grid: i j lat lon (1st record)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/gridout.html>_
