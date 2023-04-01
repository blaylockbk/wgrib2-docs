# -rpn_sto

## Introduction

The -rpn_sto N is option that behaves like
-rpn "sto_N". The only difference is the
the latter will automatically compute the longitudes and latitudes
of all the grid points.
The -rpn_rcl and
-rpn_sto do not initiate the geolocation
calculations which make them ideal for CW2 applications.

## Usage

```
-rpn_sto N     N=0..9
   saves the memory-copy of the grid values in to register N
```

See also:
[-rpn](./rpn.md),
[-rpn_rcl](./rpn_rcl.md),

---

> Description: misc X register X = data.. same as -rpn sto_X .. no geolocation calc needed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/rpn_sto.html>_
