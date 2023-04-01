# -center

## Introduction

The -center option prints out the center (ex. NCEP, ECMWF) that created
the grib file. If your center is printed as a number rather than a name, feel welcome to send
an email requesting an update to the tables.

## Usage

```
-center
```

### Example

```
$ wgrib2 test.grb2 -center
1:0:center=US National Weather Service - NCEP (WMC)
2:46042:center=US National Weather Service - NCEP (WMC)
3:63079:center=US National Weather Service - NCEP (WMC)
...
```

See also: [-set_center](./set_center.md),
[-set](./set.md),
[-subcenter](./subcenter.md)

---

> Description: inv center

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/center.html>_
