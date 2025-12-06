# wgrib2: -ncep_uv

## Introduction

Operatinal NCEP grib files often have the U and corresponding V fields in the same grib message.
(U and V are submessages.) Typically wgrib2 converts all the submessages into individual messages.
In order to combine the U and V fields together, you replace the -grib option
with -ncep_uv. Options like
-grib_out and -new_grid do not have the capability
to combine the U and V fields. For theses cases, you run the output through wgrib2.

Note that -ncep_uv is more like -grib than
-grib_out. The option
-ncep_uv uses the compressed grid point values from the original file.
Neither the grid point values nor the packing is changed.

## Usage

```
-ncep_uv output_file
```

### Example

```
$ wgrib2 test.grb2 -match ":500 mb:" -ncep\_uv output.grb

$ wgrib2 test.grb2 -inv /dev/null -new\_grid\_winds earth -new\_grid ncep grid 221 - | wgrib2 - -ncep\_uv output.grb

In the above example, the first wgrib2 regrids the file and writes it into stdout.
The second wgrib2 reads the new grib file from stdout and combines the U and V
records together into one message.



```

See also:
[-tosubmsg](./tosubmsg.md)
[-submsg](./submsg.md)
[-submsg_uv](./submsg_uv.md)
[-grib](./grib.md)
[-grib_out](./grib_out.md)
[-GRIB](./GRIB.md)

---

> Description: out X combine U and V fields into one message like NCEP operations

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ncep_uv.html>_
