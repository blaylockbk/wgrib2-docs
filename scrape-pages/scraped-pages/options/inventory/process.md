# wgrib2: -process

## Introduction

The -process option prints out
[Code Table 4.3](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_table4-3.shtml). Code Table 4.3 is important because it defines
the product such as analysis, forecast, or forecast error. The
table also change the units of the field as in a probabilty forecast.
You may encounter products that have identical metadata
except for this table (analysis vs analysis error). In the default
wgrib2 inventory, values of Code Table 4.3 which are not
probabiity forecasts, analysis errors or forecast errors are noted.

## Usage

```
-process
```

### Example

```
$ wgrib2 -process png.grb2
1:4:code table 4.3=4 ens fcst
```

See also:

---

> Description: inv Process type (code table 4.3)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/process.html>_
