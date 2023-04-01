# -set_bitmap

## Introduction

Complex packed grib files can have undefined grid values stored in
the grib files as either a bitmap or a special value. You should use
the special value method because it results in significantly smaller
files. If software is a concern, you should use whatever is compatible
with the decoding software. Note: only complex packing allows for
using a special value for undefined grid values.

Wgrib2's will store undefined grib values using a special
value when using complex packing unless the
-set_bitmap 1 option is used.
The option -set_grib_type same will not
enable the bitmap option.

### Software Concerns

NDFD (National Digital Forecast Database) which is produced by NOAA has
used complex packing with a special value since the beginning (many years ago).
Consequently many decoders will be able to handle special-value undefineds.
Many codes will handle bitmaps because bitmaps were used in
jpeg2000 packed grib2 files which have been used by NCEP for many
years. At least one organization has codes that will not read
special-value undefineds.

## Usage

```
-set_bitmap X   X=0, 1   0=do not use bitmap (default) 1=use bitmap
                when packing undefined values with complex packing
```

### Example

```
$ wgrib2 in.grb -set\_bitmap 1 -set\_grib\_type complex3 -grib\_out out.grb
```

The above line rewrites a file using complex3 packing and a bitmap.
A bitmap is not used if all the grid values are defined.

See also: [-grib_out](./grib_out.md),
See also: [-set_grib_type](./set_grib_type.md),

---

> Description: misc X use bitmap when creating complex packed files X=1/0

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_bitmap.html>_
