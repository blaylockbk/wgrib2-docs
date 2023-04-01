# -rpn_rcl

## Introduction

The -rpn_rcl N is option that behaves like
-rpn "rcl_N". The only difference is the
the latter will automatically compute the longitudes and latitudes
of all the grid points.
The -rpn_rcl and
-rpn_sto do not initiate the geolocation
calculations which make them ideal for CW2 applications.

## Usage

```
-rpn_rcl N     N=0..9
   replaces the memory-copy of the grid values with register N
```

See also:
[-rpn](./rpn.md),
[-rpn_sto](./rpn_sto.md),

---

> Description: misc X data = register X .. same as -rpn rcl_X .. no geolocation calc needed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/rpn_rcl.html>_
