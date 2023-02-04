# wgrib2: -set_ijval

## Introduction

The -set_ijval option is used to change one grid point value
of the decoded grid. After changing the grid value, one usually writes out the grid using
-grib_out FILE.
The -set_ijval option only works when the grid is
a rectangular array. For example, staggered and thinned grids are not stored an an array.

## Usage

```

-set_ijval I J VAL
    grid(I,J) = VAL
    I = 1..NX
    J = 1..NY

```

### Example

```

$ wgrib2 small.grb2 small.grb2 -set\_ijval 1 1 91 -set\_ijval 2 1 92 -set\_ijval 1 2 93 -set\_ijval 2 2 94 -grib\_out new.grb2
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
$ wgrib2 new.grb2 -csv new.csv
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
$ cat new.csv
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",0,20,91
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",10,20,92
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",0,28,93
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",10,28,94

```

See also:
[-csv](./csv.html)
[-grib_out](./grib_out.html)
[-set_ival](./set_ival.html)

---

> Description: misc X Y Z sets grid point value X=ix Y=iy Z=val

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ijval.html>_
