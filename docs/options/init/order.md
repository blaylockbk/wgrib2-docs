# -order

## Introduction

Grids can be stored in GRIB files in one of 16 different orders.
The default configure is for wgrib2 to convert all data into WE:SN order.
That is, the first point is at the bottom left, the second point is
to the right and so on until it finishes up the row. Then the next point
is slightly north of the first point and the process repeats until
the NE point of the grid is reached.

The -order raw option keeps the data in
the original order as specified the grib header. Note, latitude,
longitude is available when the data are in we:sn order.

You must use the default -order we:sn option
in order to get latitude, longitude information.

```
-order raw
      data is the order as specified by the grib file
      this was the order that wgrib used

-order we:ns
      data is in WE:NS order

-order we:sn
      data is in WE:SN order (default)
```

See also:
[-scan](./scan.md),

---

> Description: init X decoded data in X (raw|we:sn|we:ns) order, we:sn is default

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/order.html>_
