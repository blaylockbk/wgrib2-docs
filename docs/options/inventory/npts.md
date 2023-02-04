# wgrib2: -npts

## Introduction

The -npts option prints the number of grid points
in the grid. The total include both defined and undefined grid points.

```

-sh-2.05b$ ./wgrib2 test.grb2 -s -npts -d 1
1:0:d=2005090200:HGT:1000 mb:60 hour fcst:npts=259920

```

## Usage

```

-npts

```

See also: [-nxny](./nxny.html)

---

> Description: inv number of grid points

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/npts.html>_
