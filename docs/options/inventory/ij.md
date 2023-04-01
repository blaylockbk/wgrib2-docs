# -ij

## Introduction

The -ij option prints the value of the grid point (i,j)
where i = 1..nx and j = 1..ny. Note, by default the grid is converted to a
WE:SN order. This means that (1,1) is at the South-West corner.

## Usage

```
-ij i j
      i = 1 .. nx
      j = 1 .. ny
```

### Example

```
sh-2.05b$ wgrib2 new.grb2 -s -ij 1 1
1:0:d=2005082812:HGT:1000 mb:78 hour fcst:val=162.3
```

See also: [-ijlat](./ijlat.md), [-ilat](./ilat.md), [-lon](./lon.md)

---

> Description: inv X Y value of field at grid(X,Y) X=1,..,nx Y=1,..,ny (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ij.html>_
