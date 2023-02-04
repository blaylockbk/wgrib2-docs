# wgrib2: -ijlat

## Introduction

The -ijlat option prints the latitude, longitude and
value of the grid point (i,j) where i = 1..nx and j = 1..ny. Note,
by default the grid is converted to a WE:SN order which puts (1,1)
in the South-West corner. Note, multiple -ijlat can be on the command line.

## Usage

```

-ijlat i j
      i = 1 .. nx
      j = 1 .. ny

```

### Example

```

-sh-2.05b$ wgrib2 new.grb2 -s -ijlat 1 1
1:0:d=2005082812:HGT:1000 mb:78 hour fcst:(1,1),lon=0,lat=-90,val=162.3

-sh-2.05b$ wgrib2 new.grb2 -s -ijlat 1 1 -ijlat 2 2
1:0:d=2005082812:HGT:1000 mb:78 hour fcst:(1,1),lon=0,lat=-90,val=162.3:(2,2),lon=1,lat=-89,val=183.7


```

See also: [-ij](./ij.md), [-ilat](./ilat.md), [-lon](./lon.md)

---

> Description: inv X Y lat,lon and grid value at grid(X,Y) X=1,..,nx Y=1,..,ny (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ijlat.html>_
