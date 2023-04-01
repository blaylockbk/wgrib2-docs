# -RT

## Introduction

The reference time is usually the analysis time or the time of the start of
the forecast (forecast time=0). However, the grib standard also allows the reference
time to be verifying time of the forecast or the observation time as
indicated by Grib Table 1.2. While legal grib, you should think twice
before setting the reference time to the verifying time of the forecast.
The -RT option prints the type (signficance) of
the reference time (Grib Table 1.2).

## Usage

```
-RT
```

### Example

```
$ wgrib2 png.grb2 -RT
1:4:RT=Start of fcst
```

See also:

---

> Description: inv type of reference Time

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/RT.html>_
