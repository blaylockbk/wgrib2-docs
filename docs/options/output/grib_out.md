# -grib_out

## Introduction

The -grib_out option writes the decoded grid to a
specified file in grib2 format. Normally you would use the
-grib option as this option just copies the original grib (sub)message.
The -grib_out option has to repack or compress the decoded grid which
is much slower.
You would only use the
-grib_out option when you have modified
the decoded grid by
the -undefine, -import,
-rpn, or some other option that modifies the
decoded grid.
Use the -set_grib_type option
to specify the grib packing and -set_scaling to specify the scaling.
When the -set_grib_type option is not used, the packing defaults
to simple.
When the -set_scaling option is not used, the scaling is retained
from the input grib message unless a -rpn or -import
option is executed. These two options reset the scaling to the default because they
can alter the range of grid point values.

## Usage

```
-grib_out file_name
```

### Example

```
$ wgrib2 new.grb2 -undefine out-box -10:10 20:40 -grib\_out small.grb2
1:0:d=2005082812:HGT:1000 mb:78 hour fcst:
```

The above routine sets all the grid points outside the 10W-10E 20N-40N to
undefined and then writes the resultant field as a grib file
in small.grb2. This file will be much smaller than the original field.

Hint: by proper use of the -undefine and -grib_out options, one
should be able to send horizontal boundary conditions for
regional models very very quickly. Imagine just sending
4 lines of horizontal boundary conditions.

Hint: gzip

See also: [-text](./text.md),
[-netcdf](./netcdf.md)
[-spread](./spread.md)
[-bin](./bin.md)
[-ieee](./ieee.md)
[-import_bin, -import_grib, -import_ieee, import_grib](./import_bin.md)
[-grib](./grib.md)
[-rpn](./rpn.md)
[-set_grib_type](./set_grib_type.md)
[-undefine](./undefine.md)

---

> Description: out X writes decoded/modified data in grib-2 format to file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grib_out.html>_
