# wgrib2: -fix_ncep

## Introduction

-fix_ncep is a "set" option that replaces
certain NCEP-defined time ranges by the WMO equivalents. The current (7/2008)
wgrib2 needs this option in order to correctly print out the
time ranges for NCEP-defined time ranges.

The -fix_ncep option changes
the memory image of the grib message. Therefore, if you
want the NCEP time range, you must have the
-grib or -grib_out option
before the -fix_ncep option. Otherwise
you will get the WMO time range.

Caution: expect this option to change as more NCEP time ranges
are added

## Usage

```
-fix_ncep
```

### Example

```
wgrib2 -fix_ncep in_grib -grib out_grib
```

The above line replaces NCEP time ranges by the WMO equivalents
and writes out the new grib data into out_grib.

---

> Description: misc fix ncep PDT=8 headers produced by cnvgrib

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/fix_ncep.html>_
