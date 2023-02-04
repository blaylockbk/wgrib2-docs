# wgrib2: -nxny

## Introduction

The -nxny option prints the size of a "rectangular" grid.
Of course not all grids are rectangular.

```
$ ./wgrib2 test.grb2 -s -nxny -d 1
1:0:d=2005090200:HGT:1000 mb:60 hour fcst:(720 x 361)
```

The above grid is a 0.5 x 0.5 degree global grid.

```
$ ./wgrib2 ../ecmwf/gaussian\_reduced.grib2 -s -nxny -d 1
1:0:d=2006081712:var discipline=0 master_table=4 parmcat=0 parm=0:500 mb:12 hour
fcst:(-1 x 800)
```

Here, the thinned Gaussian grid has 800 latitudes and a varying number
of longitude points.

## Usage

```
-nxny
```

See also: [-npts](./npts.md)

---

> Description: inv nx and ny of grid

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/nxny.html>_
